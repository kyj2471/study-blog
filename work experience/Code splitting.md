# Code Splitting (코드분할) 이란?

`코드 분할(Code Splitting)`은 `SPA`의 성능을 향상시키는 방법입니다. 싱글 페이지 애플리케이션(Single Page Application)은 초기 실행시에 필요한 리소스를 모두 다운로드한후 해당화면에 필요한 스크립트를 실행시키는 특징이 있습니다. 때문에 초기 다운로드 비용이 매우 비싸고 , 로딩속도가 지연될수 있기 때문에 필요한 시점에 분할된 리소스를 다운받아서 실행시키는 코드분할 기술을 적절히 사용하면 속도개선과 `SEO`에 큰 도움이 됩니다.

현재 React와 Next.js의 버전은 아래와 같다.

- React `16.20.0`
- Next.js `9.0.5`

# 번들링

대부분 React 앱들은 `Webpack`, `Rollup` 또는 `Browserify` 같은 툴을 사용하여 여러 파일을 하나로 병합한 “번들 된” 파일을 웹 페이지에 포함하여 한 번에 전체 앱을 로드 할 수 있습니다. 간단하게 예를 들면 아래와 같다.

**App**

```jsx
// app.js
import { add } from './math.js';

console.log(add(16, 26)); // 42

```

```jsx
// math.js
export function add(a, b) {
  return a + b;
}

```

**Bundle**

```jsx
function add(a, b) {
  return a + b;
}

console.log(add(16, 26)); // 42

```

# Dynamic Import

앱에 코드 분할을 도입하는 가장 좋은 방법은 `동적 import()` 문법을 사용하는 방법입니다.

**Before**

```jsx
import { add } from './math';

console.log(add(16, 26));

```

**After**

```jsx
import("./math").then(math => {
  console.log(math.add(16, 26));
});

```

Webpack이 이 구문을 만나게 되면 앱의 코드를 분할합니다. Create React App을 사용하고 있다면 이미 Webpack이 구성이 되어 있기 때문에 즉시 [사용](https://create-react-app.dev/docs/code-splitting/)할 수 있습니다. [Next.js](https://nextjs.org/docs/advanced-features/dynamic-import) 역시 지원합니다.

[코드 분할 가이드](https://webpack.js.org/guides/code-splitting/)를 참조하세요. Webpack 설정은 [가이드](https://gist.github.com/gaearon/ca6e803f5c604d37468b0091d9959269)에 있습니다.

Babel을 사용할 때는 Babel이 동적 import를 인식할 수 있지만 변환하지는 않도록 합니다. 이를 위해 [babel-plugin-syntax-dynamic-import](https://yarnpkg.com/en/package/babel-plugin-syntax-dynamic-import)를 사용하세요.

# React.lazy

> 주의 `React.lazy`와 `Suspense`는 아직 서버 사이드 렌더링을 할 수 없다. 서버에서 렌더링 된 앱에서 코드 분할을 하기 원한다면 `Loadable Components`를 추천한다. 이는 서버 사이드 렌더링과 번들 스플리팅에 대한 좋은 가이드입니다.

**Before**

```jsx
import OtherComponent from './OtherComponent';

```

**After**

```jsx
const OtherComponent = React.lazy(() => import('./OtherComponent'));

```

`MyComponent`가 처음 렌더링 될 때 `OtherComponent`를 포함한 번들을 자동으로 불러옵니다.

`React.lazy`는 동적 `import()`를 호출하는 함수를 인자로 가집니다. 이 함수는 React 컴포넌트를 포함하며 `default` export를 가진 모듈로 결정되는 `Promise`로 반환해야 합니다.

lazy 컴포넌트는 `Suspense` 컴포넌트 하위에서 렌더링되어야 하며, `Suspense`는 lazy 컴포넌트가 로드되길 기다리는 동안 로딩 화면과 같은 예비 컨텐츠를 보여줄 수 있게 해줍니다.

```jsx
import React, { Suspense } from 'react';

const OtherComponent = React.lazy(() => import('./OtherComponent'));

function MyComponent() {
  return (
    <div>
      <Suspense fallback={<div>Loading...</div>}>
        <OtherComponent />
      </Suspense>
    </div>
  );
}

```

`React.lazy`는 아직 서버사이드 렌더링을 지원하지 않습니다. 이와 같은 이유로 인해서 `SSR 스플리팅`을 위해 Loadable Components를 활용하도록 하자. 공식문서에서 언급하고있는 Library입니다. 한동안 `react-loadable` 를 React 공식문서에서는 권장했었습니다만 , 라이센스이슈?로 인해서 `loadable-component` 를 추천하기 시작했습니다. → [이슈확인](https://github.com/reactjs/react.dev/pull/1407/files/29abc46424db8ba67dec0e2fde1a5a35ffd56f84)

# Route-based code splitting

사용자의 경험을 해치지 않으면서 번들을 균등하게 분배할 곳을 찾고자 한다.

이를 시작하기 좋은 장소는 `라우트`입니다. 웹 페이지를 불러오는 시간은 페이지 전환에 어느 정도 발생하며 대부분 페이지를 한번에 렌더링하기 때문에 사용자가 페이지를 렌더링하는 동안 다른 요소와 상호작용하지 않습니다.

`React.lazy`를 [React Router](https://reacttraining.com/react-router/) 라이브러리를 사용해서 애플리케이션에 라우트 기반 코드 분할을 설정하는 예시입니다.

```jsx
import React, { Suspense, lazy } from 'react';
import { BrowserRouter as Router, Route, Switch } from 'react-router-dom';

const Home = lazy(() => import('./routes/Home'));
const About = lazy(() => import('./routes/About'));

const App = () => (
  <Router>
    <Suspense fallback={<div>Loading...</div>}>
      <Switch>
        <Route exact path="/" component={Home}/>
        <Route path="/about" component={About}/>
      </Switch>
    </Suspense>
  </Router>
);

```

# Loadable Components

사용법은 간단합니다.

초기 `import`시에 `loadable` 함수를 이용하여 람다식으로 실제 import할 component를 기입해주면 됩니다.

```jsx
import loadable from '@loadable/component'

const OtherComponent = loadable(() => import('./OtherComponent'))

function MyComponent() {
  return (
    <div>
      <OtherComponent />
    </div>
  )
}

```

### ImageVideoLightbox.js

```jsx
import loadable from '@loadable/component';
const ReactImageVideoLightbox = loadable(() => import('react-image-video-lightbox'));
export default ReactImageVideoLightbox;

```

# Webpack SplitChunks

중복 import된 모듈들을 별도의 `chunk` 파일로 분리하기 위한 설정입니다. 상황에 따라 번들파일을 적절히 분리하여 로딩속도를 개선할수 있습니다. 중복된 모듈들을 반복해서 다운로드할필요는 없다? 빈번하게 사용되는 모듈들을 따로 분리하여 한번만 다운로드하고 `캐싱`된 리소스를 `재사용`하는 형태로 서비스를 구축해두면 퍼포먼스를 향상시킬수 있습니다.

```jsx
if (!isServer && isProduction) {
  config.optimization.splitChunks = {
    chunks: 'all',
    cacheGroups: {
      default: {
        name: 'default',
        minChunks: 2,
        priority: 1,
        reuseExistingChunk: true
      },
      vendors: {
        name: 'vendors',
        test: /[\\\\\\\\/]node_modules[\\\\\\\\/]/,
        minChunks: 3,
        priority: 2
      },
      asyncBundle: {
        name: 'asyncBundle',
        chunks: 'async',
        test: /[\\\\\\\\/]node_modules[\\\\\\\\/]/,
        priority: 5
      }
    }
  };
}

```

총 3가지 번들로 분리하였으며 해당 모듈에 대한 설명은 아래와 같습니다.

1. `name` : 분리될 모듈의 이름입니다.
2. `test` : 분리될 모듈의 조건입니다.
3. `minChunks` : 공통 청크로 이동하기 전에 모듈을 포함해야하는 최소 청크 수입니다.
4. `priority` : 모듈 청크의 우선순위. 값이 높을 수록 우선적으로 판정됩니다.

- `vendors` : node_modules 폴더에서 관리되는 library들입니다.
- `asyncBundle` : Dynamic import , React.lazy 등 동적 임포트가 필요한 모듈을 따로 분리합니다.
- `default` : 그외에 사용되는 library들입니다.

# Code Splitting 적용

code splitting 적용 전후 비교를 하기위해서는 code splitting과 bundling의 상태를 체크해야합니다. 브라우저의 개발자 도구 → 네트워크 탭에서도 확인해볼수 있습니다.

## 적용 전

![https://images.velog.io/images/kyj2471/post/a80ebe56-7d9a-466d-b692-ba88e294601a/_2021-05-21__4.25.04.png](https://images.velog.io/images/kyj2471/post/a80ebe56-7d9a-466d-b692-ba88e294601a/_2021-05-21__4.25.04.png)

![https://images.velog.io/images/kyj2471/post/1c3e8d90-d919-4df7-8305-aa2de7def1fc/_2021-05-21__4.27.39.png](https://images.velog.io/images/kyj2471/post/1c3e8d90-d919-4df7-8305-aa2de7def1fc/_2021-05-21__4.27.39.png)

## 적용 후

![https://images.velog.io/images/kyj2471/post/199d61bd-6ba2-43e4-a64e-a0cc7a5ec5b1/_2021-05-21__4.30.34.png](https://images.velog.io/images/kyj2471/post/199d61bd-6ba2-43e4-a64e-a0cc7a5ec5b1/_2021-05-21__4.30.34.png)

![https://images.velog.io/images/kyj2471/post/7483a1ea-ef99-4948-bd70-dddb5658be2d/_2021-05-21__4.56.57.png](https://images.velog.io/images/kyj2471/post/7483a1ea-ef99-4948-bd70-dddb5658be2d/_2021-05-21__4.56.57.png)

`item.js`의 용량이 많이 줄어들고 다른 컴포넌트에서 사용되는 공통 모듈들이 `vendors`와 `default`로 스플리팅되어 로드된것을 확인할수 있습니다. 총 다운로드수도 줄어들었네요.

이부분에서 아직은 동적 import된 library는 확인할수 없습니다. 웹 상품 상세화면의 경우 상품이미지 영역을 클릭해야 모달창이 뜨면서 해당 컴포넌트가 로드됩니다.

위에서 설명했던 [ImageVideoLightbox.js](https://www.notion.so/fe9cc50ff11249919f10282bb6b3c2e6?pvs=21) 라는 컴포넌트가 그 역할을 하고 있지요.

아래 보시면 asyncBundle 모듈이 다운로드 된것을 확인할수 있습니다.

![https://images.velog.io/images/kyj2471/post/a56e4789-31e6-4ad8-bafc-c21cbbcd2a33/_2021-05-21__5.03.36.png](https://images.velog.io/images/kyj2471/post/a56e4789-31e6-4ad8-bafc-c21cbbcd2a33/_2021-05-21__5.03.36.png)

# Webpack Bundle Analyzer

[Webpack Bundle Analyzer](https://github.com/webpack-contrib/webpack-bundle-analyzer)를 사용하면 자바스크립트 번들의 의존성 그래프(dependency graph)를 살펴보고, 그중에 쉽게 최적화 가능한 것이 있는지 찾을 수 있습니다.

### Website Webpack Bundle Analyzer

웹사이트 프로젝트에서 WebPack Bundle Analyzer를 돌린 화면입니다.

자세히 보면 중복된 모듈들이 수없이 많습니다.

![https://images.velog.io/images/kyj2471/post/490f302f-b71f-4d34-b0e7-4c65395f80c9/_2021-05-21__5.18.12.png](https://images.velog.io/images/kyj2471/post/490f302f-b71f-4d34-b0e7-4c65395f80c9/_2021-05-21__5.18.12.png)

좀더 자세히 살펴볼까요?

500kb 가 넘는 `react-image-video-lightbox` 를 반복해서 사용해왔던것으로 보여집니다.

이런 비슷한 양상이 곳곳에서 나타나고 있습니다. 코드 스플리팅과 웹팩의 스플리트 청크를 이용해서 어떤 결과가 나왔는지 살펴보시죠.

![https://images.velog.io/images/kyj2471/post/ba5df02d-474e-4eee-a1d7-748ea5f0aba7/_2021-05-21__5.20.14.png](https://images.velog.io/images/kyj2471/post/ba5df02d-474e-4eee-a1d7-748ea5f0aba7/_2021-05-21__5.20.14.png)

### Dynamic import & SplitChunks

![https://images.velog.io/images/kyj2471/post/98f404cb-84d0-4137-839b-364e8102109d/_2021-05-21__5.39.41.png](https://images.velog.io/images/kyj2471/post/98f404cb-84d0-4137-839b-364e8102109d/_2021-05-21__5.39.41.png)

asyncBundle로 `react-iamge-video-lightbox` 컴포넌트를 분리했습니다. vendos 번들로 외부 컴포넌트들을 번들링했고 , 내부적으로 사용되는 컴포넌트들은 default 번들로 분리되었습니다.

깔끔해졌네요.

# 참고

[https://ko.reactjs.org/docs/code-splitting.html](https://ko.reactjs.org/docs/code-splitting.html)

[https://nextjs.org/docs/advanced-features/dynamic-import](https://nextjs.org/docs/advanced-features/dynamic-import)

[https://github.com/gregberge/loadable-components](https://github.com/gregberge/loadable-components)

[https://runebook.dev/ko/docs/webpack/plugins/split-chunks-plugin](https://runebook.dev/ko/docs/webpack/plugins/split-chunks-plugin)

[https://github.com/webpack-contrib/webpack-bundle-analyzer](https://github.com/webpack-contrib/webpack-bundle-analyzer)

[https://medium.com/@addyosmani/a-tinder-progressive-web-app-performance-case-study-78919d98ece0](https://medium.com/@addyosmani/a-tinder-progressive-web-app-performance-case-study-78919d98ece0)