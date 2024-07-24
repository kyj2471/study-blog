# ESlint란?

`Lint` 의 뜻은 `보푸라기`다. ESlint란 ECMAScript에서의 `보풀`을 의미하게 되는데 우리가 옷을 입을 때 보풀이 있어도 입고다닐순 있지만 보기에 좋지 않다. 이렇듯 들여쓰기도 일정한 규칙에 맞게 하지 않고 선언한 변수나 함수를 사용하지 않는등 코드에 보풀이 일어나도 작동은 하지만 결과 적으로는 유지보수를 할 때 큰 어려움을 겪게 될 것이고 잠재적인 오류로 나중에는 큰 오류로 인해 개발하는데 어려움을 주게 될 것이다. 이런 것을 방지하고자 ESlint가 나오게 되었다.

# ESlint 기본 개념

`린트`의 목적은 ECMAScript 코드의 가독성을 높이고 잠재적 오류나 버그를 잡아내 더욱 견고한 코드를 만들어 내는 것이다. ESlint는 크게 두가지의 일을 하게 되는데

- 코드 포맷팅
- 코드 품질

이렇게 두가지를 꼽을 수 있다.

`코드 포맷팅`에서는 개발시 일관성있는 코드 스타일을 유지하게 해서 가독성 좋게 만들어주는 역할을 한다.

`코드 품질`에서는 개발시 잠재적인 오류나 버그를 예방해주는 역할을 한다. 위에서 말했듯 쓰지 않는 변수나 함수가 선언 되는 것처럼 잠재적이지만 후에 오류로 이어질 수 있는 것들을 잡아내 주는 역할을 한다.

# 사용방법

eslint를 install한다.

```jsx
npm i -D eslint

```

그러고 난 후 eslintrc.js 파일을 생성해 내가 원하는 설정을 해주면된다.

```jsx
// eslintrc.js

module.exports = {
  env: {
    browser: true,
    node: true
  },
  extends: ['eslint:recommended', 'plugin:prettier/recommended'],
	parser: 'babel-eslint',
  plugins: ['react'],
  rules: {
    'no-console': ['error', { allow: ['warn', 'error'] }]
  }
};

```

실제 린트를 어떻게 설정하고 사용하냐는 사실상 답이없다. 본인이 개발하는데 있어서 원하는 설정을 하고 본인이 편하면 그만이라 생각해도 된다.

하지만 개발은 혼자하는 것이 아니고 모두가 함께 편하게 일관성있는 규칙을 이용해서 개발해야하기 때문에 왜 우리는 저런 규칙의 린트를 사용하는지 정도는 알고 있어야한다.

### rules

`Rules`에서 우리는 규칙 목록을 확인 할 수있다.

이 [_**링크**_](https://eslint.org/docs/rules/)를 타고 들어가면 어떠한 규칙을 정해 줄 수 있는지 일일히 자세히 설명되어있다.

```jsx
//eslintrc.js

module.export = {
	rules:{
		"no-extra-semi": "error",
		...
	}
}
```

위처럼 원하는 룰을 추가해 린트를 설정해 나가면된다.

### extends

이제 린트에서 규칙을 정의해 설정하는 사용하는법을 알았다.

하지만 링크에서 확인했듯 너무 많은 양의 룰이 있고 필요할 때마다 일일히 찾아서 설정해서 사용하는 것은 비효율적이며 힘든일이다.

그래서 나오게된 것이 `extends`이다. 바벨에서 프리셋과 플러그인 관계와 비슷하다.

이러한 많은 룰에서 기본적으로 많이 사용되는 린트 규칙들을 모아서 하나로 캡슐화했다고 생각하면 된다.

```jsx
// eslintrc.js

module.export = {
	extends: [
		"eslint:recommended"
	]
}
```

위의 린트 코드에서 `extends` 설정은 위에서 린트 `rule` 링크 페이지에서 체크가 되어있는 룰은 모두 적용되어 있는 것이다. 만약 다른 규칙이 필요하다면 아래 룰을 추가해 필요한 설정을 정의하면된다.

린트에서는 이런 기본적으로 제공하는 것 외에 airbnb, standard 패키지 설정을 제공해준다.

**airbnb**는 _airbnb스타일 가이드_를 따르는 규칙으로 `eslint-config-airbnb-base` 패키지로

**standard**는 _javascript standard 스타일_을 사용한다 `eslint-confing-standard` 패키지로 제공된다.

### Parser

eslint를 실행하면 parser가 자바스크립트 코드를 분석한다. 실제 parse의 뜻이 _**"구문을 분석하다"**_ 이다.

이렇게 분석한 코드로 `추상화 구문 트리(abstract syntax tree:AST)`를 만들어 낸다.

그런데 만들어낸 결과물은 JS를 바탕으로 분석한 결과물이다.

아무리 호환성을 최대한 문제가 없게 설정을 했어도 다른 것과 깊은 비교를 하다보면 문제가 생길수 있고 이 문제는 린트 오류로 이어질 수 있다.

그래서 만약 JS가 아닌 다른것을 함께 사용한다면 지정을 해줘야 안정적으로 사용할 수있다.

예로들어 TypeScript를 사용한다면 Parser에 @typescript-eslint/parser를 등록 해 줘야 하는 것이다.

```jsx
// eslintrc.js

module.exports = {
  extends: ['eslint:recommended', 'plugin:prettier/recommended'],
	parser: 'babel-eslint',
  rules: {
    'no-console': ['error', { allow: ['warn', 'error'] }]
  }
};

```

우리는 babel-eslint를 parser로 지정을 해주었는데 이는 ESlint와 호환되는 Babel 파서를 둘러싼 래퍼이다.

만약 Parser를 사용하지 않는다면 import, export와 같은 es6문법에서 에러가 날 것이다.

파서중에 babel-eslint, Esprima등을 선택해서 설정할 수 있으며 따로 설정하지 않는 다면 기본적으로 Espree

를 제공하게된다.

eslint [_**공식문서**_](https://eslint.org/docs/user-guide/configuring/plugins#configuring-plugins)에 위의 관련된 사항을 자세히 설명해 놓았는데 한번쯤 읽어보면 사용하는데 큰 도움이 될 것이다.

# Prettier

## Prettier란?

prettier란 코드를 더 예쁘게 만들어 주는 것이다. eslint랑 다르게 프리티어는 코드 포맷팅관련 부분만 역할을

수행하고 코드 품질에 대해서는 영향을 주지 않는다.

단순 코드를 조금 더 일관성있는 스타일을 가지게 해주기 위해 사용한다.

## 사용 방법

```
npm install -D prettier

```

우선 프리티어 패키지를 설치한다.

settings.json에서 설정해 줄 수도 있지만 .prettierrc라는 파일을 만드는 것을 추천한다.

```jsx
// .prettierrc

{
  "arrowParens": "always",
  "bracketSpacing": true,
  "htmlWhitespaceSensitivity": "css",
  "jsxBracketSameLine": true,
  "printWidth": 80,
  "proseWrap": "preserve",
  "quoteProps": "as-needed",
  "semi": true,
  "singleQuote": true,
  "tabWidth": 2,
  "trailingComma": "none",
  "useTabs": false
}

```

위의 코드는 현재 제가 사용하고 있는 프리티어 설정이다.

이 [_**링크**_](https://prettier.io/docs/en/options.html)에서 각각의 설정이 어떤 역할을 하는지 확인 할 수있다.

# ESlint & Prettier 함께 사용하기

포맷팅을 Prettier에 맡기더라도 코드 품질을 위해 린트를 사용에대한 필요성이있다.

최상의 방법은 두개를 함께 사용한는 것이다.

크게 두가지가 있다.

- eslint-config-prettier
- eslint-plugin-prettier

### eslint-config-prettier

위의 방법은 프리티어에서 제공하는 린트와 통합방법이다.

이것은 프리티어와 충돌하는 린트 규칙을 꺼버린다. 두개가 함께 사용하면 규칙이 충돌해 에러가 나기 때문이다.

```jsx
npm install -D eslint-config-prettier

```

우선 eslint-config-prettier 패키지를 설치해 준다.

```
// .eslintrc.js
{
  extends: [
    "eslint:recommended",
    "eslint-config-prettier"
  ]
}

```

그러고 난 후 extends에 추가하여 사용하면 된다.

### eslint-plugin-prettier

위의 방법은 prettier 규칙을 린트 규칙에 추가하는 플러그인이다.

결론적으로 프리티어 모든 규칙이 린트에 들어오니 ESlint만 실행하면 된다.

```jsx
npm install -D eslint-plugin-prettier

```

역시 패키지를 설치해야한다.

```jsx
// .eslintrc.js

{
  extends: [
    "eslint:recommended",
    "eslint-config-prettier"
  ],
	plugins:[
		"prettier"
	],
	rules:{
		"prettier/prettier": "error"
	}
}

```

그러고 난 후 위의 플러그인과 룰을 추가하여 설정해주고 사용하면 된다.

그렇다면 어떤 상황에서 config를 사용할지? plugin을 사용할지 고민된다면 감사하게도 고민할 필요가 없다.

ESlint를 처음 소개하는 위에서 린트 코드를 보면

```jsx
module.exports = {
  env: {
    browser: true,
    node: true
  },
  extends: ['eslint:recommended', 'plugin:prettier/recommended'],
	parser: 'babel-eslint',
  plugins: ['react'],
  rules: {
    'no-console': ['error', { allow: ['warn', 'error'] }]
  }
};

```

프리티어는 이러한 두 패키지를 함께 사용하는 단순한 설정을 제공해준다.

그것이 위의 `extends`에서 보이는 `plugin:prettier/recommended`이다.

이제 다 알았으니 우리가 항상 사용하는 린트와 프리티어 코드 셋팅 코드를 보고 원하는 입맛대로 바꿔가면서 사용하면된다.