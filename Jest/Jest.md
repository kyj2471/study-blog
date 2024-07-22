# Jest란?

<aside> 💡 `jest`는 페이스북에서 만든 Javascript 테스팅 라이브러리 입니다. Jest는 이전의 자바스크립트 테스팅 라이브러리와는 차별점을 가지고있는데 전에는 test를 진행하는데 있어서 test runner, matcher, mock등을 다른 라이브러리르 조합하여서 사용했었는데 `jest`라는 라이브러리가 나옴으로서 이런 모든 것 들을 지원해 줌으로 상당히 편리하게 접근할 수 있고 사용할 수 있습니다.

</aside>

# jest 설치

```
npm install --save -dev jest

```

# jest를 사용하는 이유

**test란 우리가의 코드가 정상적으로 우리가 의도한 방향으로 작동하는지 검증하는 작업입니다.**

jest와 같은 테스팅 라이브러리가 없었다면 우리는 우리가 구현한 기능을 직접 사용해 봄 으로서 테스트를 할 것 입니다. 하지만 프로젝트의 규모가 커지다 보면 이러한 모든 기능을 수동으로 일일히 체크 한다는 것은 시간도 많이 소모되고 번거로울 것 입니다. 또한 미처 체크를 하지 못하고 배포가 되어 버그가 있는 상태로 사용자가 서비스를 이용하는 위험성 또한 있을 겁니다.

우리는 이러한 사고를 막기위해 테스트 코드를 작성할 것이고 이로서 우리의 프로젝트는 더욱더 견고해 질 것이며 더욱 안정적인 서비스를 제공할 것입니다. 이러한 이유 뿐 아니라 내가 작성한 코드를 나중에 시간이 지나 다른 개발자가 보았을 때 이러한 테스트 코드를 보면서 하나하나의 유닛테스트를 보며 어떤기능을 하는 것인지 빠르게 파악을 할 수 있으며 통합테스트 코드를 보면서 어떠한 의도를 가진 프로젝트인지 빠르게 파악이 가능하다.

이러한 장점들 때문에 우리는 jest를 사용하고 jest는 위에 언급했듯 사람이 직접 테스트하는게 아니라 _테스트 시스템이 자동으로 확인_을 해 줄 것이고 우리는 이것을 _**테스트 자동화**_ 라고 합니다.

### unit 테스트

유닛 테스트란 우리의 소스 코드의 특정 모듈이 의도한 대로 정확히 작동하는지 검증하는 절차입니다.

모든 함수와 메서드에 대한 테스트 케이스를 작성하는 절차를 의미하는데 이런 유닛테스트의 특성 덕분에 우리는 언제라도 코드가 변경되거나 문제가 발생할 때 빠른시간내로 이를 파악 할 수 있게 됩니다.

그렇다면 우리는 이러한 유닛 테스트를 어떤 식으로 활용할까?

1. 컴퍼넌트가 문제없이 렌더링 되는가?
2. 컴퍼넌트의 특정함수가 실행 될 때 상태값이 우리가 의도한 대로 바뀌는가?
3. 리덕스에서 액션 생성함수가 액션 객체를 잘 만들어 내는가?
4. 리덕스에서 리듀서를 실행했을때 해당 리듀서가 이전 상태값과 액션을 참조하여 의도한 대로 새로운 상태값을 반환 하는가?

위와 같은 방식으로 우리는 유닛테스트를 사용할겁니다. 하지만 하나하나의 유닛테스트에서의 함수는 의도한대로 작동을 하지만 전체적으로 볼때는 의도한대로의 기능이 작동하지 않을 수도 있습니다.

그래서 유닛테스트와 다른 의도의 테스트인 통합테스트가 있습니다.

### 통합 테스트

위의 각자의 기능들이 전체적으로 잘 작동하는지 확인하기 위한 것이 통합테스트 입니다.

어떤 목적을 가진지는 이름만으로 알 수 있지만 자세히 알아보자면 통합테스트는 모듈을 통합하는 단계에서 수행하는 테스트이며 위와같은 단위테스트를 우선 수행하여 각각 정상적으로 작동되는게 확인이 된 후 이러한 모듈을 연동하여 테스트를 수행하는 것입니다. 이러한 통합테스트에는 어떤게 있을까?

1. 여러 컴퍼넌트를 렌더링했을때 이상이 없으며 서로 상호작용을 잘 하는가요?
2. DOM에 이벤트 발생시 우리의 UI에 의도한대로 변화가 생기나요?
3. 리덕스와 연동된 컴포넌트의 DOM에 특정 이벤트 발생시 우리가 의도한 액션이 디스패치 되는가요?

위와같은 통합테스트를 통해 우리는 우리 프로젝트 결과물을 보다더 견고하게 만들어 갈 수 있습니다.

# Jest 사용해보기

우선 jest 공식홈페이지에서 제공하는 코드를 보면서 jest를 사용해보겠습니다.

```jsx
// sum.js

function sum(a, b) {
	return a + b;
}

module.exports = sum
```

```jsx
// sum.test.js

const sum = require("sum.js경로")

test("calculate a + b", () => {
	expect(sum(1,2)).toBe(3);
})

```

우리는 component.test.js라는 파일명으로 테스트를 진행할 수있습니다. 또는 __ test __라는 폴더를 이용해 해당 폴더내의 코드는 테스트진행할 수 있습니다 또한 npm test라는 명령어를 통해 test를 진행할 수 있습니다.

위의 코드에서 `expect`는 특정값이 ~일 것이다라고 사전에 정의를 하고 테스트 결과가 우리의 예상과 같게 나오면 테스트를 성공 시키고 만약 그렇지 않다면 테스트를 실패할 것입니다.

그리고 `expect`뒤에나오는 `toBe`라는 함수는 `matchers`라고 부릅니다.

우리는 이 `matchers`를 통해 특정 값이 어떠한 조건을 만족 시키는지 또는 어떤 함수가 실행이 되었는지 에러가 났는지 등을 확인할 수 있습니다.

우리가 사용한 `toBe`는 기본값을 비교하거나 개체 인스턴스 참조 id를 확인하는데 주로 사용하는 `matcher`인데

이렇듯 jest에서는 우리에게 테스트의 목적이나 상황에 맞게 제공해주는 다양한 `matcher`가 있습니다.

- [링크](https://jestjs.io/docs/expect)를 통해서 우리는 사용할 matcher함수를 선택할 수 있습니다.

마지막으로 우리는 테스트를 진행하는데 있어 test라는 키워드를 사용했습니다. 우리는 `test`라는 키워드 말고 `it`이란 키워드를 사용해서 테스트를 진행할 수 있습니다.

이는 테스트를 진행하는데 있어 가장작은 단위를 의미하는 것 입니다. 여기서 우린 둘중 어떤 것을 사용해야 하는지 궁금할텐데 사실 아무 키워드를 사용해서 이용하면 됩니다.

test, it 모두 작동방식이 같으며 코드의 가독성을위해 사용하는 것 입니다.

## jest 비동기 테스트

### Callback

우린 자바스크립트 코드를 비동기적으로 실행되는 상황을 자주 맞이합니다.

아래의 코드를 통해서 Jest 비동기테스트를 알아보겠습니다.

```jsx

// userData.js

function getUser(id, callback){
	setTimeout(() => {
		const user = {
			id : id,
			name : "user" + id,
			email : id + "@test.com"
		}
	callback(user)
	},3000)
}

//userData.test.js
it("getUser", () => {
	console.log("jest asynchronous")
	getUser(1, (user) => {
		expect(user).toEqual({
			id:1,
			name:"user1",
			<email:"1@test.com>"
		})
	}
}

```

위와 같은 상황에서 우리는 테스트를 진행해 보았습니다. 결론적으로 테스트는 통과합니다. 하지만 우리는 분명 3초뒤에 실행되게했는데 결과는 3초뒤가 아닌 바로 실행 됩니다.

![https://images.velog.io/images/kyj2471/post/a24db949-3acf-4179-8b5a-00e1168a6e46/_2021-05-31__11.28.04.png](https://images.velog.io/images/kyj2471/post/a24db949-3acf-4179-8b5a-00e1168a6e46/_2021-05-31__11.28.04.png)

위에서의 결과처럼 3초가아닌 바로 결과가 도출되는 현상이 나타납니다.

이 이유는 jest runner는 기본적으로 테스트가 최대한 빨리 호출해 줍니다. 그래야 프로젝트 규모가 커짐으로 테스트 코드가 많아져도 빠르고 좋은 성능을 유지할 수 있기 때문인데 위의 상황은 성공 케이스지만 만약 name, email값에 다른값을 주어도 통과가 되버리는 일이 발생합니다. 어떠한 값이 들어가도 통과가 되는데 이는 콜백함수에서 toEqual이라는 Matcher마저도 호출되지 못한 것입니다.

해결법으로는 jest Runner에 이테스트가 비동기함수라고 가르쳐주면 되는데요 빈인자가 아닌 `done`이라는 함수

인자를 받도록하여 `done`함수를 콜백함수의 가장 마지막에 호출되도록 해주면 됩니다.

```jsx
//userData.test.js

it("getUser", (done) => {
	console.log("jest asynchronous")
	getUser(1, (user) => {
		expect(user).toEqual({
			id:1,
			name:"user1",
			<email:"1@test.com>"
		})
	done()
	}
}

```

![https://images.velog.io/images/kyj2471/post/c3d5b527-f6b2-4608-aed7-1f11c1eeb2e6/_2021-05-31__11.35.51.png](https://images.velog.io/images/kyj2471/post/c3d5b527-f6b2-4608-aed7-1f11c1eeb2e6/_2021-05-31__11.35.51.png)

이제 다시 npm test를 통해 테스트를 해보겠습니다.

그러면 위와같이 우리가 의도한 결과로 테스트를 통과할 수 있습니다.

### Promise

이번엔 Promise를 사용해 구현된 비동기 테스트를 보겠습니다.

```jsx
//userData.js

function getUser(id){
	return new Promise((resolve) => {
		setTimeout(() => {
			console.log("wait 3 seconds use Promise")
			const user = {
				id:id,
				name:"user"+id,
				email:id + "@test.com"
		};
		resolve(user);
		},3000)
	});
}

// userData.test.js

test or it("fetch a user use Promise", () => {
	getUser(7).then((user) => {
		expect(user).toEqual({
			id:7,
			name:"user999",
			<email:"7@test.com>"
		});
	});
});

```

위의 테스트는 통과하지 말아야합니다. name에 "user7"이 우리가 예상한 값인데 다른 값을 주었으니 당연히 테스트는 통과하면 안됩니다. 하지만 결과는 아래와 같습니다.

![https://images.velog.io/images/kyj2471/post/0d72a46f-7606-4c83-97eb-61017a35925e/_2021-05-31__11.51.36.png](https://images.velog.io/images/kyj2471/post/0d72a46f-7606-4c83-97eb-61017a35925e/_2021-05-31__11.51.36.png)

테스트 시간도 1ms인점을 보아서 getUser()함수에서 리턴된 promise의 then()메서드가 실행할 기회조차 얻지 못한겁니다.

해결법은 간단합니다. 테스트코드에서 getUser앞에 return을 넣어주는 것 입니다. 그러면 테스트 함수가 Promise리턴시 jest runner는 리턴된 promise가 resolve될 때 까지 기다려줍니다.

이제 name값을 맞게 수정해주고 테스트를 해보면 해당테스트는 아래와 같이 우리가 원하는 값을 줄 것 입니다.

![https://images.velog.io/images/kyj2471/post/a98a9f08-1308-44fc-89a5-b9812c7c8c2e/_2021-05-31__11.56.04.png](https://images.velog.io/images/kyj2471/post/a98a9f08-1308-44fc-89a5-b9812c7c8c2e/_2021-05-31__11.56.04.png)

### async / await

이제 마지막으로 위의 테스트 코드를 async / await를 활용해 더 가독성이 좋은 테스트코드를 작성할 수 있습니다.

```
// userData.test.js

test("fetch a user use async/await", () => {
	const user = await getUser(1)
	expect(user).toEqual({
		id:1,
		name:"user1",
		<email:"1@test.com>"
	});
});

```

![https://images.velog.io/images/kyj2471/post/e2f4bb7a-0fa0-44dd-bb35-d86a67d9711e/_2021-05-31__12.05.53.png](https://images.velog.io/images/kyj2471/post/e2f4bb7a-0fa0-44dd-bb35-d86a67d9711e/_2021-05-31__12.05.53.png)

위의 코드를 통해 위와같은 결과를 가져올 수 있습니다.

## Mock Function

jest를 사용하면서 장점중 하나는 다른 라이브러리의 도움없이 바로 mock기능을 사용할 수 있다는 점이다.

mock함수 관련 공부를 하다보면 mocking이라는 개념이 나오게 되는데 mocking에 대해 알아보겠다.

### mocking

결론부터 말하면 mocking이란 우리가 단위 테스트를 진행할 때 해당 코드가 의존하는 부분을 가짜로 대체하는 기법 입니다. 여기서 mock이 가짜의 뜻을 가지는데 우리는 주로 "모킹하다" "목객체"라고 말합니다.

우리가 테스트를 진행할 때 실제 데이터베이스를 사용한다면 당연히 데이터 접속과같이 네트워크 작업이 포함된 테스트를 진행하면 실행속도가 느려질 것이고 프로젝트의 규모가 커지면서 이러한 테스트 코드가 많아지면 이런 속도 저하때문에 큰 이슈가 될 수 있습니다.

또한 테스트를 위한 코드보다 데이터베이스와 연결을 위한 코드가 더 길어질 수 있고 테스트를 진행할 때 데이터 베이스가 오프라인이라면 해당테스트는 실패를 합니다 즉, 테스트가 환경에 영향을 받는겁니다.

하지만 어떠한 이유던 결론적으로 특정 기능을 분리해서 테스트를 하는 유닛테스트의 근본에 적합하지 않습니다.

위에서 말씀드렸듯 mocking은 실제 객체인척하는 가짜 객체를 생성해줍니다. 그리고 테스트 진행중에 이 가짜 객체가 어떤 일이 있었는지를 기억하기에 가짜 객체가 테스트에서 어떻게 사용되는지 검증이 가능합니다.

### jest.fn()

jest.fn()은 jest에서 가짜함수를 생성할수있게 해주는 함수입니다. fn()을 더 자세히 보면 아래와 같이 나옵니다.

```jsx
function fn(): Mock;
    /**
     * Creates a mock function. Optionally takes a mock implementation.
     */

```

아래의 코드를 보고 mock함수를 생성해주는 jest.fn()에 대해 더 알아봅시다.

```jsx
// example

const mockFn = jest.fn()

mockFn(1)
mockFn(3)
mockFn("Tony")
mockFn([1,2,3],{a:"b"})

```

위의 코드에서 호출 결과는 undefined입니다. 리턴값 설정을 하지 않아서입니다.

결론부터 말하자면 설정에따라 이 가짜함수가 어떤 값을 리턴해줄지 설정할 수 있으며 어떤 함수를 적용하냐에 따라 가짜 비동기함수 또한 생성할 수 있습니다.

```jsx
const mockFn = jest.fn()

mockFn
	.mockReturnValueOnce(true)
	.mockReturnValueOnce(false)
	.mockReturnValueOnce(true)
	.mockReturnValueOnce(false)
	.mockReturnValueOnce(true)

const result = [1,2,3,4,5].filter((item) => mockFn(item))
console.log(result)

test("result number is 1,3,5",() => {
	expect(result).toStrictEqual([1,3,5]) //pass
})

```

우리는 mockReturnValue를 통해 가짜 함수가 호출될 때마다 반환되는 리턴값을 지정할 수 있습니다.

위에서 사용된 mockReturnValueOnce는 이 가짜 함수가 한번 호출할 때 리턴값을 받습니다. 이렇게 연속으로 호출시 각각 다른 값을 반환하도록 연결할 수 있습니다.

이제 가짜 비동기함수를 만들어 보겠습니다.

```jsx
//example

const mockFn = jest.fn()
mockFn.mockResolvedValue({ name : "Tony" })

test("my name is", () => {
	mockFn().then((res) => {
	expect(res.name).toBe("Tony")
	});
})

```

위와같이 mockResolvedValue(Promise에서 resolve하는 값)이란 함수를 이용해 가짜 비동기함수를 만들 수 있으며 다른 함수를 이용하면 해당함수를 아래와 같이 통째로 즉석으로 재 구현할 수도 있습니다.

```jsx
//example

const mockFn = jest.fn()

test("Change name", () => {
	mockFn.mockImplementation((name) => `My name is ${name}`);
	console.log(mockFn("Tony") // My name is Tony
});

```

우리는 이렇게 생성한 가짜함수를 테스트할 때 toBeCalled 함수이자 jest matcher를 이용해 가짜함수가 몇번 호출 되었고 인자로 어떤 것이 넘어왔는지 검증할 수 있습니다.

관련 jest matcher와 상황에따라 사용될 다양한 함수는 [_공식문서 링크_](https://jestjs.io/docs/mock-function-api#mockfnmockreturnvaluevalue)를 통해 확인할 수 있습니다.

### jest.spyOn()

우리가 007과 같은 스파이를 다루는 영화를 볼 때 스파이는 어떤 역할을 하나요? 주로 어떤 곳에 몰래 잠입해 정보를 빼오는 역할을 수행합니다. 실제 jest.spyOn역시도 이와 비슷하다 생각하면 이해하기 편합니다.

첫번째로 객체를 두번째로 methodName을 받아 사용하는 jest.spyOn(object, method)을 이해하기 위해 아래 코드를 보도록 하겠습니다.

```jsx
const calculator = {
	add: (a, b) => a + b,
}

const spy = jest.spyOn(calculator, "add")
const result = calculator.add(2,3)
expect(spy).toBe(5) // pass
```

위의 코드처럼 우리는 spyOn함수를 사용해 calculator라는 객체에 접근하여 methodName이 "add"를 가져와 사용하였습니다. 또 다른 몇가지 상황을 보면서 어떤 상황에서 spyOn을 주로 사용하는지 알아보겠습니다.

```jsx
const initialState = {
	id: Date.now(),
	user: "Tony",
	gender: "male"
}

test("Date.now() is working?", () => {
	jest.spyOn(Date,"now").mockReturnValueOnce(1000);
	const checkDate = Date.now();
	expect(checkDate).toBe(1000);
}

```

위와같이 현재시간을 알아볼 수있는 Date.now()를 테스트 할 때에도 우리는 spyOn으로 접근하고 위에서 설명한 mockReturnValueOnce를 이용해 해당 리턴값을 원하는 값으로 바꿔서 테스트를 할 수 있습니다.

이제 아래 코드를 통해 데이터 통신에서 활용하는 예제를 보도록 하겠습니다.

```jsx
// fetchData.js

const axios = require("axios");
const API = "<https://jsonplaceholder.typicode.com>";

module.exports = {
  findOne(id) {
    return axios.get(`${API}/users/${id}`).then((res) => res.data);
  },
};

//fetchData.test.js

test('find fetched data from the API endpoint', async () => {
  const spyGet = jest.spyOn(axios, 'get');
  await findOne(1);
  expect(spyGet).toBeCalledTimes(1);
  expect(spyGet).toBeCalledWith(`https://jsonplaceholder.typicode.com/users/1`);
});

```

위와 같은 비동기 통신 상황을 보겠습니다. 우리는 spyOn을 활용해 axios에 접근해 그중 get이라는 method를 테스트합니다. 이런 방법을 통해 우리는 데이터 통신을 하는 상황에서의 테스트를 할 수 있습니다.

### jest.mock()

지금까지 우린 jest.fn(), jest.spyOn()을 활용해서 특정 모듈에 접근해 한가지 메서드를 모킹하거나 함수를 목킹 하는등의 작업을 통해서 테스트를 진행했었다. jest에서는 jest.mock()이라는 강력한 목킹 함수를 제공한다.

이는 모듈을 통째로 mocking시켜버리는 함수인데 위의 예제처럼 axios의 get에 접근하는등의 작업을 할 필요가 없다. 이유는 mock()을 사용하면 axios의 모든 함수가 자동으로 목함수가 되기 때문이다.

사용법은 목킹해야하는 모듈을 Import(require)를 이용해서 받아온후 jest.mock("모듈경로")를 해준후 사용하면된다. mock과 관련한 내용은 실제 프로젝트에 jest를 더하면서 다음 jest와react-testing-libarary리뷰에서 다시 자세하게 써보겠다.

**[참고자료]**

- [https://jestjs.io/](https://jestjs.io/) ⇒ jest 공식문서
- [https://github.com/YeThor/JEST_GUIDE_KR/blob/master/SUMMARY.md](https://github.com/YeThor/JEST_GUIDE_KR/blob/master/SUMMARY.md) ⇒ jest문서 한글번역본
- [https://www.daleseo.com/](https://www.daleseo.com/) ⇒ jest관련 기술 블로그