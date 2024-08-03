React-Query 학습 키워드 기록

React Query는 React 애플리케이션에서 서버 상태를 가져오고, 캐싱하고, 동기화하고 업데이트하는 것을 매우 쉽게 만든다.

```jsx
import { QueryClient, QueryClientProvider, useQuery } from 'react-query';

const queryClient = new QueryClient();

export default function App() {
  return (
    <QueryClientProvider client={queryClient}>
      <Example />
    </QueryClientProvider>
  );
}

function Example() {
  const { isLoading, error, data } = useQuery('repoData', () =>
    fetch('<https://api.github.com/repos/tannerlinsley/react-query>').then(res =>
      res.json()
    )
  );

  if (isLoading) return 'Loading...';
  if (error) return 'An error has occurred: ' + error.message;

  return (
    <div>
      <h1>{data.name}</h1>
      <p>{data.description}</p>
      <strong>👀 {data.subscribers_count}</strong>{' '}
      <strong>✨ {data.stargazers_count}</strong>{' '}
      <strong>🍴 {data.forks_count}</strong>
    </div>
  );
}

```

여기서 `repoData`는 쿼리 키값이다. key-value 매핑 구조이다.

(useQuery Hook으로 수행되는 Query 요청은 HTTP METHOD GET 요청과 같이 서버에 저장되어 있는 “상태”를 불러와 사용할 때 사용)

### useQuery 리턴값

- `data` → resolve된 데이터
- `error` → 에러 객체(발생했을 때)
- `isLoading`, `isSuccess`, `status` → 현재 쿼리 상태
- `refetch` → query를 refetch하는 함수
- `remove` → query cache에서 지우는 함수

### useQuery option값

- `onSuccess`, `onError`, `onSettled` → query fetching시 성공/실패/완료시 실행할 sideEffect 정의
- `enabled` → 자동으로 query를 실행시킬지 말지
- `retry` → 실패 시 자동으로 retry할지 결정하는 옵션
- `select` → 성공 시 데이터 가공해서 전달
- `keepPreviousData` → 새로 fetch 시 이전 데이터 유지

## Mutations

query와 다르게 데이터 update 시 사용함. CRUD에서 useQuery가 Read였다면 나머지 create, update, delete는 mutation으로.

(useMutation Hook으로 수행되는 Mutation 요청은 HTTP METHOD POST, PUT, DELETE 요청과 같이 서버에 Side Effect를 발생시켜 서버의 상태를 변경시킬 때 사용)

### useMutation 리턴값

- `mutate` → mutation 실행시키는 함수
- `mutateAsync` → mutate랑 비슷하나 promise 반환
- `reset` → mutation 내부 상태 clean

나머진 useQuery랑 비슷.

### 캐싱? 동기화?

**stale-while-revalidate**: 백그라운드에서 stale response를 revalidate하는 동안 캐시가 가진 stale response를 반환하는 것. 쉽게 새 물건 들어올 때까지 헌 물건 보여주겠다는 뜻.

```jsx

import { useQuery } from 'react-query';
import * as API from 'api';

export const useGetInquiryList = (query, options) =>
  useQuery('useGetInquiryList', () => API.getInquiriesList(query), options);

```

여기서 CRUD의 read 즉 get 메서드를 호출하기 위해 useQuery를 활용해 `getInquiresList`란 API 호출을 한다. 여기서 key값 `useGetInquiryList`에 response가 value로 매핑될 것이다.

해당 훅스를 필요한 UI 컴포넌트에서 호출하자.

```jsx

const getInquiryList = useGetInquiryList(query, {
  onSuccess: (data) => {
    console.log(data);
  },
  onError: (error) => {
    alert(error.message);
  },
});

```

따로 useEffect를 사용하지 않아도 default로 mount 시 해당 함수가 실행될 것이다. 또한 response에서 성공 시, 에러 시 핸들링을 간단하게 처리할 수 있다.

```jsx

import { useQueryClient } from 'react-query';

const queryClient = useQueryClient();
const testList = queryClient.getQueryData('useGetInquiryList');

console.log('ui compo level', testList);

```

이전에 `useGetInquiryList`라는 키값에 response 값을 매핑했으니 해당 키값을 통해 접근해 값을 받아올 수 있다.

실제 `useQueryClient` 코드를 보면 `queryClient` 내부적으로 Context를 사용합니다.