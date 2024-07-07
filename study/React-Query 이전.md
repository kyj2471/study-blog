# 도입한 이유

회사에서 업무를 하면서 기존 Redux, Redux-Saga를 사용하며 상태 관리를 하던 중 함께 스터디를 하는 팀원들 간 React-Query로 이전에 대한 이야기가 나왔고 결론적으로 API 통신 코드의 복잡성, 많은 양의 보일러플레이트 코드, 직관적이지 않은 에러 처리 방식, 캐싱 및 최적화 처리, Client 상태와 Server 상태 분리등 Redux, Redux-Saga를 사용하며 경험한 문제점을 해결하기위해 React-Query 도입하였습니다.

## Client 상태? Server 상태?

Client 상태, Server 상태를 구분하는건 데이터가 어디서 오너쉽을 가지고 관리됨의 차이가 키워드이다.

### Client 상태

- Client에서 소유하고 제어가능함
- 초기값 설정, 조작에 제약이 없음
- 다른 유저들과 공유되는 상태가 아님
- 항상 Client에서 최신 상태로 관리됨

### Server 상태

- Client가 아닌 원격에서 관리 및 유지됨
- Fetch하고 Update하는데 비동기 API 필요
- 다른 유저와 공유되는 것으로 변경 가능성이 있음
- 신경 쓰지 않으면 out-of-date가 될 가능성이 있음

## Redux, Redux-Saga의 문제

### 문제점

- API 통신을 위한 코드가 많아짐.
- 에러 처리가 복잡하고 직관적이지 못한다.
- 반복적인 구조로 boilerplate코드 증가.

아래의 코드는 Redux, Redux-Saga를 사용했던 구조를 조금 수정한 것이다.

```jsx
// api호출 선언부
export const getNotificationsData = async (query) => {
  const queryData = querystring.stringify(query);
  const { data } = await axios.get(`/api/notifications?${queryData}`);
  return data;
};
```

```jsx
// action type 정의
export const notificationsRequest = (request) => ({
  type: actionTypes.NOTIFICATIONS_REQUEST,
  request
});

// reducer 정의
const initialState = {
	// ...initialState 정의
};

const reducer = (state = initialState, action) => {
  switch (action.type) {
    case actionTypes.NOTIFICATIONS_REQUEST:
      const { request } = action;
      return { ...state, loading: true };

    case actionTypes.GET_NOTIFICATIONS_SUCCESS: {
      const { list, request } = action;
      const { alarmList, page, limit } = state;
      // 추가적인 처리...
    }

    default:
      return state;
  }
};

// saga 정의
export function* handleGetAlarms({ query, request }) {
  try {
    yield put(actions.notificationsRequest(request));
    let {
      status,
      message,
      data: { list }
    } = yield call(getNotificationsData, query);

    if (status === 200) {
      yield put(actions.getNotificationsSuccess(list, request));
    } else {
      yield put(sweetAlertMessage(message));
    }
  } catch (error) {
    yield put(actions.notificationsFailure(error));
  }
}

// alarm 관련 saga
export function* watchAlarm() {
  yield all([
    yield takeEvery(actionTypes.GET_NOTIFICATIONS_LIST, handleGetAlarms),
    yield takeEvery(actionTypes.DELETE_NOTIFICATION, handleDeleteAlarm)
  ]);
}

// 사용 컴퍼넌트
const Example = ({ data, onGetNotificationsList, onDeleteNotification }) => {
  return (
    <>
     // ...생략
    </>
  );
};

const mapStateToProps = ({ alarm }) => ({ data: alarm });
const mapDispatchToProps = (dispatch) => ({
  onGetNotificationsList: (query, request) =>
    dispatch(actions.getNotificationsList(query, request)),
  onDeleteNotification: (alarmId, index) => dispatch(actions.deleteNotification(alarmId, index))
});

export default connect(mapStateToProps, mapDispatchToProps)(Example);

```

이와 같이 상태 변화를 위해 많은 양의 액션 생성 함수, 리듀서, 비동기 API 통신을 위한 Redux-Saga 코드가 필요했다.

### React-Query로 전환

[[React-Query 키워드 간단한 정리 노트]](https://www.notion.so/React-Query-58776f1886e14689a5278d9fda9385cf?pvs=21)

우선, 고차 컴포넌트로 이루어진 부분을 Redux Hook 기반으로 변형했습니다. 이는 현재 서비스 중인 코드에서 Props drilling이 발생하고 있어, 이 문제를 해결하기 위함이었습니다. Props drilling은 컴포넌트 계층 구조에서 데이터를 전달할 때 많은 중간 컴포넌트들을 거쳐야 하는 문제로, 이를 해결하여 API 통신 누락되는 문제를 방지하려 했습니다.

이후, 폴더 구조를 개선했습니다. 기존에는 Redux와 Redux-Saga를 사용할 때 각 API 호출마다 action, actionTypes, reducer, saga 모듈을 분리하여 관리했습니다. 그러나 React-Query로 전환하면서 이러한 복잡한 구조를 단순화했습니다. 이제 hooks 폴더 내에 queries와 mutations 두 개의 폴더만 선언하고, 각 API별로 하나의 파일에 모든 관련 작업을 관심사별로 분리하여 처리하도록 했습니다. 이를 통해 코드의 가독성과 유지 보수성을 크게 향상시켰습니다.

### 전환 후 코드 구조

React-Query로 전환한 후의 코드는 다음과 같은 구조로 변경되었습니다.

### API Fetch 선언부

API 호출을 위한 함수를 정의합니다. 이는 비동기 함수로, axios를 사용하여 데이터를 가져옵니다.

```jsx
// api fetch 선언부
export const fetchList = async (query) => {
  const queryData = queryString.stringify(query);
  const { data } = await axios.get(`${path}?${queryData}`);
  return data;
};

```

### Queries 정의

React-Query의 `useQuery` 훅을 사용하여 데이터를 fetching합니다. `useQuery`는 데이터를 가져오고, 캐싱하고, 동기화하며, 에러를 처리하는 등 다양한 기능을 제공합니다.

```jsx
// queries
export const useFetchData = (query, options) => {
  return useQuery(['fetchData', query], () => fetchList(query), options);
};

```

여기서 `'fetchData'`는 쿼리 키로, 동일한 쿼리 키를 사용하여 React-Query는 캐싱된 데이터를 관리합니다. `options` 매개변수를 통해 추가적인 설정(예: 에러 처리, 쿼리 활성화 조건 등)을 할 수 있습니다.

### 컴포넌트에서의 사용

컴포넌트에서는 `useFetchData` 훅을 호출하여 데이터를 가져옵니다.

```jsx
// example component
const MyComponent = ({ query }) => {
  const { isLoading, data, error } = useFetchData(query, {
    enabled: !!query, // query가 있을 때만 실행
    onError: () => {
      console.error('Error fetching data');
    },
  });

  return (
    <>
      {!isLoading && <Layout data={data} />}
    </>
  );
};

```

이 코드에서는 `useFetchData` 훅을 사용하여 데이터의 로딩 상태(`isLoading`), 데이터(`data`), 에러(`error`)를 관리합니다. `enabled` 옵션은 쿼리가 존재할 때만 데이터를 fetching하도록 설정합니다. 에러 발생 시 `onError` 콜백을 통해 에러를 처리합니다.

### 결론

1. **보일러플레이트 코드 제거**: Redux와 Redux-Saga에서 반복적으로 작성해야 했던 액션 타입, 액션 생성 함수, 리듀서, 사가 등의 보일러플레이트 코드를 더이상 사용할 필요가 없어졌습니다.
2. **직관적인 에러 처리**: `useQuery` 훅의 `onError` 옵션을 통해 에러를 각 컴퍼넌트별 분리하여 상황에 맞게 처리할 수 있습니다.
3. **폴더 구조 간소화**: 기존의 분리된 파일들을 각 관심사별로 하나의 파일로 합쳐서 관리함으로써 코드의 가독성과 유지 보수성이 향상되었습니다.
4. **클라이언트 상태와 서버 상태 분리**: React-Query를 통해 클라이언트 상태와 서버 상태를 명확히 분리하여 관리할 수 있습니다. 이는 상태 관리의 복잡한 구조를 간결히 함으로 추후 유지 보수를 하는데 수월한 구조가 되었습니다.

최종적으로 React-Query로의 전환은 유지 보수성과 개발 생산성을 크게 향상시키는 데 기여했습니다. 결과적으로 더욱 효율적으로 클라이언트와 서버 상태를 분리하여 관리할 수 있게 되었으며, 코드의 간결하고 직관적으로 구현하여 가독성 또한 개선되었습니다.

[참고 자료]

[React-Query 공식문서](https://tanstack.com/query/latest/docs/framework/react/overview)

[우아한테크세미나](https://www.youtube.com/watch?v=MArE6Hy371c&t=5178s)

[if kakao](https://www.youtube.com/watch?v=YTDopBR-7Ac&t=1239s)