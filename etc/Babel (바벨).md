# Babel의 배경

사람이 살아가는데 서로 각자 다른 언어를 사용하면 의사소통에 문제가 생기고 수많은 다른 언어를 배우는 것은 매우 어려운 일이다. 이런 상황과 비슷하게 브라우져 마다 사용되는 언어가 다르고 이런 언어가 다르다. 물론 계속해서 브라우져는 개선되고 있지만 IE는 `promise`를 이해하지 못하며 많은 이용자가 있는 Safari 에서도 2018년도 까지는 `Promise.prototype.finally`라는 메서드는 사용할 수 없었다. 이러한 **크로스 브라우징 이슈**는 많은 프론트엔드 개발에서 _**코드의 일관성을 해치며**_ 주니어 개발자들에게는 개발 자체를 힘들게 하였다. _**Babel이란 히브리어로 "혼돈"을 뜻한다.**_ 즉, 이러한 크로스 브라우징 "혼돈"을 해결을 위해서 Babel이 나왔다.

# Babel의 기본 동작

Babel은 ECMAScript2015 이상의 코드를 하위 버전으로 바꾸어 주는걸 주된 역할로 한다. 위에서 말했듯 서로 다른 브라우저에서 최신 Javascript코드를 이해 못하는 환경에서 동작하게 해주기 위함이다.

Babel은 3가지 일을 해준다.

1. 파싱(Parsing)
2. 변환(Transfroming)
3. 출력(Printing)

**첫번째** Parsing부분에서 Babel은 우리가 작업한 Javascript코드를 한줄한줄 읽어가면서 구문을 분석해 `추상 구문 트리(abstract syntax tree:AST)`를 만들어준다. 위의 작업을 하는 이유는 빌드 작업을 처리하기에 적합한 자료구조인데 컴파일러 이론에 사용되는 개념이다.

**두번째** Transforming부분에서는 실제로 코드를 변환하는 작업을 한다. 현재의 신문법이 적용된 Javascript코드를 과거의 문법으로 크로스브라우징 이슈가 생기지 않게 변환해준다.

**세번째** 출력에서는 변경된 결과물을 출력 해준다.

# Babel 사용해보기

우선 바벨의 최신버전을 아래와 같은 명령어로 설치하고 터미널 사용을위한 도구를 설치한다.

```
npm install -D @babel/core  @babel/cli

```

![https://images.velog.io/images/kyj2471/post/ae62774c-e191-427c-9f3c-d79e0b94fe83/_2021-05-13__10.38.50.png](https://images.velog.io/images/kyj2471/post/ae62774c-e191-427c-9f3c-d79e0b94fe83/_2021-05-13__10.38.50.png)

위의 상황과 같이 우리는 현재 진행중인 최신 Javascript를 사용한 프로젝트가 있을 것이다.

`babelrc.js`라는 파일을 만들어주자.

```jsx
// babelrc.js

{
  "presets": [],
  "plugins": []
}

```

### 플러그인 (plugins)

간단히 플러그인에 대한 설명을 하자면 위에서 말씀 드린 것 처럼 babel은 3가지 역할을 한다.

하지만 정확히 말하면 _**babel은 Parsing과 Printing만 담당**_하고 `Transforming`은 다른 녀석이 처리해 주는데

이것을 **플러그인** 이라고 한다.

`const / let`을 사용한 es6 Javascript를 크로스 브라우징 이슈를 없애기 위해 var로 변경하는 작업을 하게되면

어떻게 해야할까?

```jsx
npm install -D @babel/plugin-transform-block-scoping

```

우선적으로 위와같이 npm install을 해서 해당 패키지를 install해주고 babelrc.js에

```
module.export={
	plugins:[
		"@babel/plugins-transform-block-scoping"
	]
}

```

을 추가 해주면 된다.

그런데 프로젝트의 규모가 커지면 추가해야되는 plugins는 너무 많아질 것이고 그럴 때 마다 계속해서

npm install을 해주고 플러그인에 등록해주고 나중에는 Package json에서 devDependency에는 무슨 동작을

하는지도 모르는 디펜던시가 추가되면 추후에 리팩토링을 진행하거나 유지보수에 있어서 굉장히 어려워 질것이다.

그래서 나오게 된게 presets이다.

### 프리셋 (Presets)

위의 플러그인을 일일이 계속 설정하는 것은 피해야한다.

그래서 _**목적에 맞게 여러가지 플러그인을 세트로 모아 캡슐화 해준 것이 프리셋이다.**_

```
//babelrc.js

{
  "presets": ["next/babel"],
  "plugins": ["babel-plugin-styled-components"]
}

```

## 바벨 실제로 사용해보기

preset과 plugin을 이해했으면 진짜 바벨을 사용할 준비가 되었다.

우선 프리셋과 플러그인을 다시 비워주고 빌드를 진행해보겠다.

![https://images.velog.io/images/kyj2471/post/5d32ebb5-6d5a-4a17-89db-b9c53671e592/_2021-05-13__11.11.44.png](https://images.velog.io/images/kyj2471/post/5d32ebb5-6d5a-4a17-89db-b9c53671e592/_2021-05-13__11.11.44.png)

그리고 변경된 코드를 확인을 위해 package.json에서 build에서 next build로 되어 있는걸(CRA시 자동으로 설정되는 것) 위와 같이 변경 해주겠다.

![https://images.velog.io/images/kyj2471/post/291e90e5-6bda-49ee-9aa2-8375a216ba6a/_2021-05-13__11.13.19.png](https://images.velog.io/images/kyj2471/post/291e90e5-6bda-49ee-9aa2-8375a216ba6a/_2021-05-13__11.13.19.png)

그랬더니 styled-component를 인식을 못하다 보니 스컴을 사용한 component관련된 것은 트랜스 컴파일링이 되지 않고 스컴을 사용하지 않은 store(redux)부분만 변경된걸 확인 할 수 있다.

이번에는 플러그인에 위의 셋팅중에서 스컴설정을 넣어주고 다시 빌드해보자.

이번에는 모든 폴더가 에러없이 트랜스 컴파일링 된 것을 확인 할 수있다.

하지만 public에 생긴 lib 폴더에서 컴파일링된 결과를 보면 아무런 변화가 없다.

이유는 우리가 아무런 셋팅을 해주지 않아서이다.

![https://images.velog.io/images/kyj2471/post/670a6c84-bdc0-40c9-a2d3-d081a1a0d81b/_2021-05-13__11.17.55.png](https://images.velog.io/images/kyj2471/post/670a6c84-bdc0-40c9-a2d3-d081a1a0d81b/_2021-05-13__11.17.55.png)

위에서 처럼 npm 을 통해 바벨 es2015셋팅을 install해주고난 후 등록해 줍시다.

그리고 다시 빌드를 해줍니다.

![https://images.velog.io/images/kyj2471/post/7d5ff03e-f8e4-4b83-8d2a-02614c2ca6c6/_2021-05-13__11.19.16.png](https://images.velog.io/images/kyj2471/post/7d5ff03e-f8e4-4b83-8d2a-02614c2ca6c6/_2021-05-13__11.19.16.png)

그러고 난후 다시 lib폴더를 확인해 보면 모든 component에서 javascript 구버전으로 컴파일링 된것을 확인할 수 있습니다.

한가지만 더 확인을위해 minify 라는 바벨 셋팅을 install 해준후 등록해줍니다.

![https://images.velog.io/images/kyj2471/post/3ffdfa33-2d45-49aa-8fec-11edd48494b7/_2021-05-13__11.20.30.png](https://images.velog.io/images/kyj2471/post/3ffdfa33-2d45-49aa-8fec-11edd48494b7/_2021-05-13__11.20.30.png)

위와 같이 설정이 됬다면 다시 빌드를 해본다.

![https://images.velog.io/images/kyj2471/post/a844e38a-c790-4f6b-8449-c53f33de104c/_2021-05-13__11.22.00.png](https://images.velog.io/images/kyj2471/post/a844e38a-c790-4f6b-8449-c53f33de104c/_2021-05-13__11.22.00.png)

이런식으로 모든 component에서 코드들이 한줄에 압축되어 있는걸 확인 할 수 있다.

이제 프리셋과 플러그인을 이해하고 사용도 해봤으니 babelrc.js와 package.json을 원래상태로 돌려준다.

다시 빌드.

![https://images.velog.io/images/kyj2471/post/950efeee-10df-4900-8862-63245c427445/_2021-05-13__11.24.16.png](https://images.velog.io/images/kyj2471/post/950efeee-10df-4900-8862-63245c427445/_2021-05-13__11.24.16.png)

기본 셋팅과 많은 플러그인들이 모여있는 프리셋을 사용해 빌드를 하였고 결과적으로 우리는 빌드시 모든

바벨이 트랜스컴파일링을 해주어 크로스 브라우징 이슈로 부터 배포 할 준비가 완료되었다.