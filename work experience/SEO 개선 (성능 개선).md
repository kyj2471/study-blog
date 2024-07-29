# 웹 성능 측정 도구

### Lighthouse

Lighthouse는 웹 페이지 성능에 대한 점수 지표를 제공하고 웹 페이지에 개선 필요한 부분에 대한 안내를 제공한다. 평소 작업을 하면서 가장많이 사용했었다 아래 이미지의 왼편이 mobile web, 우측이 web의 결과이다.

![KakaoTalk_Photo_2024-07-20-11-33-46 027.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/ad79c095-c62e-4268-8fe9-c9d202ae92f5/5537a3a3-ce7a-4bf2-aaf6-7dc3e0c0228c/KakaoTalk_Photo_2024-07-20-11-33-46_027.png)

![KakaoTalk_Photo_2024-07-20-11-33-43 003.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/ad79c095-c62e-4268-8fe9-c9d202ae92f5/491e0d98-948f-435e-94c2-83b4aeae51fc/KakaoTalk_Photo_2024-07-20-11-33-43_003.png)

측정 항목 중 Performance는 화면에 콘텐츠가 얼마나 빨리 표시되고 사용자는 얼마나 빠르게 화면을 볼 수 있는지에 초점을 맞춘다.

**- First Contentful Paint (FCP)** 페이지가 로드되기 시작한 시점부터 페이지 콘텐츠의 일부가 화면에 렌더링 될 때까지의 시간을 측정합니다.

**- Speed Index** 페이지를 로드 중에 얼마나 빨리 콘텐츠가 시각적으로 표시되는지 속도를 측정합니다.

**- Largest Contentful Paint (LCP)** 페이지가 처음으로 로드를 시작한 시점부터 가장 큰 이미지나 텍스트의 렌더링 시간을 측정합니다.

**- Time to Interactive (TTI)** 사용자가 페이지와 상호작용이 가능하기까지의 시간을 측정합니다.

**- Total Blocking Time (TBT)** 메인 스레드가 응답하지 못할 정도로 오래 작업을 할 때 FCP와 TTI 사이의 시간을 측정합니다.

**- Cumulative Layout Shift (CLS)** 사용자가 예상치 못한 레이아웃 이동을 경험하는 빈도를 수량화합니다

### Network (리소스 측정)

각 페이지의 요청 횟수 (requests), 전송된 데이터 양 (transferred), 사용한 리소스 (resources), 총 소요 시간(finish)를 갭쳐한 이미지이다. 개선 전과 개선 후의 이미지를 비교하면 전체적으로 줄어든 수치를 볼 수 있다.

**👉 [홈피드] 개선 전**

![3.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/ad79c095-c62e-4268-8fe9-c9d202ae92f5/3559d2c7-be30-46d3-8d8d-966b9ad0b6f7/3.png)

**👉 [홈피드] 개선 후**

![4.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/ad79c095-c62e-4268-8fe9-c9d202ae92f5/17aaf871-c422-418f-a100-166325e29d48/4.png)

**👉 [아이템 상세] 개선 전**

![KakaoTalk_Photo_2024-07-20-11-33-43 008.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/ad79c095-c62e-4268-8fe9-c9d202ae92f5/05baa749-3155-47ef-a1ca-db573cb9c2f2/KakaoTalk_Photo_2024-07-20-11-33-43_008.png)

**👉 [아이템 상세] 개선 후**

![KakaoTalk_Photo_2024-07-20-11-33-44 009.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/ad79c095-c62e-4268-8fe9-c9d202ae92f5/573b8eda-fe2f-4246-853c-260482217b6f/KakaoTalk_Photo_2024-07-20-11-33-44_009.png)

## PageSpeed Insights

PageSpeed Insights는 데스크톱 및 휴대기기 모두에서 웹사이트의 속도와 성능을 분석하는 구글에서 개발한 도구이다. 다양한 성능 메트릭에 대한 자세한 보고서를 제공하고 웹 사이트의 속도와 사용자 경험을 최적화하는 데 도움이 되는 개선 사항을 제안한다. 이 도구는 구글에서 지속적으로 업데이트하는 웹 성능에 대한 모법 사례 모음을 기반으로 한다. PageSpeed Insights는 페이지 로드 시간, 첫 번째 바이트까지 걸리는 시간, 기타 사용자 중심 성능 측정 항목과 같은 다양한 측정항목을 기반으로 웹사이트의 속도와 성능을 분석하고 작동한다. 이미지, JavaScript, CSS에 대한 최적화 제안, 서버 응답 시간 및 브라우저 캐싱과 같은 웹 사이트의 다양한 성능 측면에 대한 자세한 보고서를 제공한다. 또한 이 도구는 데스크톱 및 모바일 장치 모두에 대해 0에서 100까지의 범위에서 웹 사이트의 속도 및 성능에 대한 점수를 제공한다.

### [개선 전 PageSpeed Insights]

![KakaoTalk_Photo_2024-07-20-11-33-43 001.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/ad79c095-c62e-4268-8fe9-c9d202ae92f5/29afe247-a5bf-41d3-97ca-51f61ff7698e/KakaoTalk_Photo_2024-07-20-11-33-43_001.png)

![KakaoTalk_Photo_2024-07-20-11-33-44 012.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/ad79c095-c62e-4268-8fe9-c9d202ae92f5/2af71dbc-b357-4f1e-b18a-f11fe6914c32/KakaoTalk_Photo_2024-07-20-11-33-44_012.png)

### [개선 후 PageSpeed Insights]

![KakaoTalk_Photo_2024-07-20-11-33-44 013.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/ad79c095-c62e-4268-8fe9-c9d202ae92f5/4ab731f4-89c8-46a6-b710-60d097d00eb0/KakaoTalk_Photo_2024-07-20-11-33-44_013.png)

![KakaoTalk_Photo_2024-07-20-11-33-44 015.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/ad79c095-c62e-4268-8fe9-c9d202ae92f5/63941f4b-129d-4474-a425-93b4ca0427df/KakaoTalk_Photo_2024-07-20-11-33-44_015.png)

![KakaoTalk_Photo_2024-07-20-11-33-44 014.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/ad79c095-c62e-4268-8fe9-c9d202ae92f5/4e6f6f93-4ce4-4e24-afb7-90034f16572c/KakaoTalk_Photo_2024-07-20-11-33-44_014.png)

![KakaoTalk_Photo_2024-07-20-11-33-44 016.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/ad79c095-c62e-4268-8fe9-c9d202ae92f5/93055fcb-cb52-445a-b710-5cbbc42b6afa/KakaoTalk_Photo_2024-07-20-11-33-44_016.png)

🔍 사용하지 않는 자바스크립트 줄이기 → 2.1초 🔍 자바스크립트 실행 시간 단축 → 5.3초 🔍 기본 스레드 작업 최소화 → 8.5초

## bundle-analyzer를 통한 분석

@zeit/next-bundle-analyzer 설치 `npm install --save @zeit/next-bundle-analyzer`

next.cpmfig.js 파일에 설정

```jsx
const withBundleAnalyzer = require('@zeit/next-bundle-analyzer');

module.exports = withPlugins(
  ...
  [
    withBundleAnalyzer,
    {
      analyzeServer: ['server', 'both'].includes(process.env.BUNDLE_ANALYZE),
      analyzeBrowser: ['browser', 'both'].includes(process.env.BUNDLE_ANALYZE),
      bundleAnalyzerConfig: {
        server: {
          analyzerMode: 'static',
          reportFilename: '../bundles/server.html'
        },
        browser: {
          analyzerMode: 'static',
          reportFilename: '../bundles/client.html'
        }
      }
    }
  ],
  ...
);

```

실행

`npm run analyze` `"analyze": "BUNDLE_ANALYZE=both npm run production-build”`

결과 확인

왼편은 server 오른편은 client이다.

![KakaoTalk_Photo_2024-07-20-11-33-44 017.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/ad79c095-c62e-4268-8fe9-c9d202ae92f5/5f53f78e-6463-4899-8e89-a9c1da35a9de/KakaoTalk_Photo_2024-07-20-11-33-44_017.png)

![KakaoTalk_Photo_2024-07-20-11-33-45 018.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/ad79c095-c62e-4268-8fe9-c9d202ae92f5/1913d954-4848-4afd-95de-d63d7e391c12/KakaoTalk_Photo_2024-07-20-11-33-45_018.png)

### [번들 사이즈]

기존 번들 사이즈 ([번들 사이즈에 대한 설명](https://github.com/webpack-contrib/webpack-bundle-analyzer/issues/61))

- **stat**: 최적화 되기 전 코드의 번들 사이즈
- **parsed**: minimize된 파일 사이즈 (브라우저에서 파싱된 자바스크립트 코드의 사이즈)
- **gzip**: minimize와 gzip을 거친 후의 사이즈 (네트워크에서 로드될 때의 사이즈)

### 빌드 결과

왼편은 작업 전 오른편은 작업 후 이다.

![KakaoTalk_Photo_2024-07-20-11-33-45 019.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/ad79c095-c62e-4268-8fe9-c9d202ae92f5/b085847b-57cc-4cfa-bb69-2d3b296efe71/KakaoTalk_Photo_2024-07-20-11-33-45_019.png)

![KakaoTalk_Photo_2024-07-20-11-33-45 020.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/ad79c095-c62e-4268-8fe9-c9d202ae92f5/0f574313-ecf5-426a-8d19-f0e743a32969/KakaoTalk_Photo_2024-07-20-11-33-45_020.png)

용량이 큰 페이지만 비교하면

- `/_app` 213KB → 171KB
- `/item/detail` 167KB → 260KB
- `/item/regist` 484KB → 164KB
- `/search` 113KB → 64.7KB

## javaScript와 CSS의 최적화

**- lodash (71K → 3.4K)** loadash 라이브러리에서 debounce 메서드만 사용하고 있기 때문에 default import로 변경하였다.

![KakaoTalk_Photo_2024-07-20-11-33-45 021.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/ad79c095-c62e-4268-8fe9-c9d202ae92f5/7d56860b-498d-4a51-9603-abbaadc27e75/KakaoTalk_Photo_2024-07-20-11-33-45_021.png)

![KakaoTalk_Photo_2024-07-20-11-33-45 022.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/ad79c095-c62e-4268-8fe9-c9d202ae92f5/f9ea313c-96f4-43f0-8e19-e725356eae47/KakaoTalk_Photo_2024-07-20-11-33-45_022.png)

**- lottie-web** 상품 등록시 AI로딩 애니메이션 라이브러리인데 용량이 306k가 넘는다. dynamic import 사용

**- date-fns (31K → 9.4K)** default import가 적용되는 parse, format 적용

![KakaoTalk_Photo_2024-07-20-11-33-45 023.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/ad79c095-c62e-4268-8fe9-c9d202ae92f5/1f6f0f10-06ab-48de-858a-b73fac369e7d/KakaoTalk_Photo_2024-07-20-11-33-45_023.png)

![KakaoTalk_Photo_2024-07-20-11-33-45 024.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/ad79c095-c62e-4268-8fe9-c9d202ae92f5/9f851e7a-544b-4554-91a8-77b18058df55/KakaoTalk_Photo_2024-07-20-11-33-45_024.png)

**- npm dedup** node_modules의 사이즈를 줄이고, 종속성을 평탄화(flatten)하기 위해 `npm dedup`을 통해 중복된 종속성을 정리하였다.

**- webpack 번들 결과 용량 줄이기 위해 treeShaking 코드 추가**

**UghlifyJsPlugin**: uglifyjs-webpack-plugin

```jsx
const UglifyJsPlugin = require('uglifyjs-webpack-plugin');
 // UglifyJsPlugin
  config.optimization.minimizer.push(
    new UglifyJsPlugin({
      cache: true,
      parallel: true,
      sourceMap: false,
      extractComments: true,
      uglifyOptions: {
        compress: true,
        output: {
          comments: false
        }
} })
);
```

**TerserPlugin**: uglifyjs-webpack-plugin → terser webpack-plugin 교체

```jsx
const TerserPlugin = require('terser-webpack-plugin');
 // TerserPlugin
config.optimization.minimizer.push(
        new TerserPlugin({
          terserOptions: {
            compress: true,
            mangle: true,
            output: {
              comments: false
            }
},
          extractComments: true
        })
);
```

**교체를 진행한 이유**

1. **Terser는 더 최신이며 활발히 유지보수된다** : Terser는 더 최근에 개발된 활발히 유지보수되는 JavaScript 최소화 라이브러리입니다. 반면 uglifyjs-webpack-plugin은 UglifyJS를 기반으로 하고 있으며, UglifyJS에는 일부 문제가 있었고 최신 업데이트가 활발하지 않습니다.
2. **더 작은 결과 파일 크기** : Terser는 UglifyJS보다 작은 결과 파일 크기를 생성하여 웹사이트와 애플리케이션의 빠른 로드 시간을 도와줍니다.
3. **병렬 처리** : Terser는 병렬 처리를 지원하여 다중 코어 시스템에서도 빠른 최소화가 가능해집니다. 이로 인해 빌드 시간이 크게 단축될 수 있습니다.
4. **ES 모듈 지원** : Terser는 ES 모듈 형식을 네이티브로 지원하며, 현대적인 JavaScript 프로젝트에서 ES 모듈을 사용하는 경우에 중요합니다.

**CssMinimizerPlugin**: css-minimizer-webpack-plugin

```jsx
const CssMinimizerPlugin = require('css-minimizer-webpack-plugin');
config.optimization.minimizer.push(
        new TerserPlugin({
          terserOptions: {
            compress: true,
            mangle: true,
            output: {
              comments: false
            }
},
          extractComments: true
        }),
     // CssMinimizerPlugin
        new CssMinimizerPlugin()
      );
}
```

## Image Lazy Loading

`Image Lazy Loading`은 페이지 안에 있는 실제 이미지들이 실제로 화면에 보여질 필요가 있을 때 로딩을 할 수 있도록 하는 기술입니다. 웹 페이지 내에서 바로 로딩을 하지 않고 로딩 시점을 뒤로 미루는 것이라 볼 수 있습니다. 이 방식은 웹 성능과 디바이스 내 리소스 활용도 증가, 그리고 연관된 비용을 줄이는데 도움을 줄 수 있습니다.

유사하게 `lazy loading`은 페이지 내에서 실제로 필요로 할 때까지 리소스의 로딩을 미루는 것입니다. 페이지를 로드하자마자 리소스를 로딩하는 일반적인 방식 대신, 실제로 사용자 화면에 보여질 필요가 있을 때까지 이러한 로딩을 지연하는 것입니다.

laze loading을 다루는 방식은 페이지 내의 거의 모든 리소스에 적용할 수 있습니다. 예를 들어, `SPA(Single Page Application)` 내에서 JS 파일이 나중에까지 필요하지 않으면 초기에 로드해서 가져오지 않는 것이 가장 좋습니다. 이처럼 이미지도 바로 보여질 필요가 없다가, 실제로 보여질 필요가 있을 때 로딩을 하는 것입니다.

### Intersection Observer API

이 API는 `element` 요소가 `view port`에 들어가는 것을 감지하고 액션을 취하는 것을 아주 간단하게 만들어 주고 성능면으로도 좋습니다.

`Event Listener`를 사용해서 Image lazy loading을 구현할 수도 있지만 `Intersection Observer`를 사용한 이유는 이 두 방법에 대해 이미지 로딩 시간을 비교해보았습니다. 후자가 이미지를 로드하는 트리거가 빠르며 스크롤할 때 이미지가 느리게 나타나지 않는 것을 확인할 수 있습니다. 이벤트 리스너를 이용한 방식은 성능을 위해 timeout을 추가했었는데, 이 부분은 사용성 측면에서 이미지 로드에 약간의 딜레이가 발생하는 미미한 영향을 끼칩니다.

```jsx
// image lazy loading

import React, { useEffect, useState, useRef } from 'react';
import styled from 'styled-components';
import * as C from '../../constants/index';

const LazyImage = (props) => {
  const { src, alt, handleMouseOver, handleClick } = props;
  const [isLoading, setIsLoading] = useState(false);
  const imgRef = useRef(null);
  const observer = React.useRef();
  const emptyImgUrl = `${IMAGE_URL}${C.URL.IMAGE.EMPTY.THUMBNAIL}`;

  useEffect(() => {
    observer.current = new IntersectionObserver(intersectionObserver);
    imgRef.current && observer.current.observe(imgRef.current);
  }, []);

  const intersectionObserver = (entries, io) => {
    entries.forEach((entry) => {
      if (entry.isIntersecting) {
        io.unobserve(entry.target);
        setIsLoading(true);
      }
    });
  };

  return (
    <S.ItemImg
      ref={imgRef}
      src={isLoading ? `${src}&f=webp` : emptyImgUrl}
      alt={alt}
      onMouseOver={handleMouseOver}
      onClick={handleClick}
    />
  );
};

const S = {
  ItemImg: styled.img`
    ...
  `,
};

export default LazyImage;

```

이미지 로드를 지연시키기 위해 모든 이미지에 옵저버를 부착시킵니다. 엘리먼트가 뷰포트에 들어간 것을 API가 감지했을 때, isIntersecting 속성을 이용해서 이미지 url을 image placeholder url에서 실제 아이템 이미지 url로 로드하도록 트리거를 일으킵니다.

적용 완료하고 테스트 시 최고점을 찍긴 했지만 35~45 정도를 유지하고 있었습니다.

![KakaoTalk_Photo_2024-07-20-11-33-45 025.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/ad79c095-c62e-4268-8fe9-c9d202ae92f5/197275c9-d464-4319-85c0-07f17c6584ff/KakaoTalk_Photo_2024-07-20-11-33-45_025.png)

## code splitting

### module code splitting

react-image-video-splitting 라이브러리의 용량이 513.8K로 큰편이다. 애초에 이런 라이브러리는 사용하지 않아야하는데 레거시로 프로젝트에 많은 영향을 주고있어 dynamic import로 사용하도록 되어있다.

```jsx
import loadable from '@loadable/component';

const ReactImageVideoLightbox = loadable(() => import('react-image-video-lightbox'));

export default ReactImageVideoLightbox;

```

**- swiper 관련 모듈**

```jsx
import loadable from '@loadable/component';

const Swiper = loadable(() => import('swiper/react').then((module) => ({ default: module.Swiper })));
const SwiperSlide = loadable(() => import('swiper/react').then((module) => ({ default: module.SwiperSlide })));
const Navigation = loadable(() => import('swiper').then((module) => ({ default: module.Navigation })));
const Pagination = loadable(() => import('swiper').then((module) => ({ default: module.Pagination })));

export { Swiper, SwiperSlide, Navigation, Pagination };

```

**- React.lazy 사용** React.lazy는 default export만 지원하기 때문에 named export인 경우에는 다른 방식으로 처리 필요

```jsx
import React from 'react';

const Swiper = React.lazy(() => import('swiper/react'));
const SwiperSlide = React.lazy(() => import('swiper/react').then((module) => ({ default: module.SwiperSlide })));
const Navigation = React.lazy(() => import('swiper').then((module) => ({ default: module.Navigation })));
const Pagination = React.lazy(() => import('swiper').then((module) => ({ default: module.Pagination })));

export { Swiper, SwiperSlide, Navigation, Pagination };

```

**- useEffect 훅 사용** 컴포넌트가 마운트될 때 모듈 비동기 import

```jsx
import { useState, useEffect } from 'react';

const SwiperLoader = () => {
  const [Swiper, setSwiper] = useState(null);
  const [SwiperSlide, setSwiperSlide] = useState(null);
  const [Navigation, setNavigation] = useState(null);
  const [Pagination, setPagination] = useState(null);

  useEffect(() => {
    const loadSwiperModules = async () => {
      const SwiperModule = await import('swiper/react');
      const { Swiper, SwiperSlide } = SwiperModule;
      setSwiper(Swiper);
      setSwiperSlide(SwiperSlide);
    };

    const loadSwiperNavigation = async () => {
      const NavigationModule = await import('swiper').then((module) => ({ default: module.Navigation }));
      setNavigation(NavigationModule.default);
    };

    const loadSwiperPagination = async () => {
      const PaginationModule = await import('swiper').then((module) => ({ default: module.Pagination }));
      setPagination(PaginationModule.default);
    };

    loadSwiperModules();
    loadSwiperNavigation();
    loadSwiperPagination();
  }, []);

  return { Swiper, SwiperSlide, Navigation, Pagination };
};

export default SwiperLoader;

```

**- Route Based Code Splitting** 라우터 기반 코드 스플리팅은 페이지별로 코드 스플리팅을 하는 것입니다. 웹 페이지를 불러오는 시간은 페이지 전환에 어느 정도 발생하며 대부분 페이지를 한 번에 렌더링하기 때문에 사용자가 페이지를 렌더링하는 동안 다른 요소와 상호작용하지 않습니다. 하지만 Next.js는 페이지별 코드 스플리팅이 기본 제공되기 때문에 필요가 없습니다.

**- Component Code Splitting**

Loadable Components란? React가 자체적으로 제공하는 `React.lazy`나 `React.suspense`도 있지만 `SSR`까지 커버 가능하고 사용 방법이 거의 동일한 Loadable Components를 페이스북에서도 추천하고 있습니다.

모달들은 페이지에서 처음부터 필요한 컴포넌트가 아니기 때문에, 사용자가 어떤 액션(클릭)했을 때 나타나도록 Code Splitting을 적용할 수 있습니다.

```jsx
import React from 'react';
import styled from 'styled-components';
import * as C from '../../../../../constants';
import loadable from '@loadable/component';
const Category = loadable(() => import('./category'));
const Brand = loadable(() => import('./brand'));
const Price = loadable(() => import('./price'));
const Etc = loadable(() => import('./etc'));
/** *
*/
const Toggle = (props) => {
  const { id, brandDataList, isLoading } = props;
  const isCategory = id === C.FILTER.TOPIC.CATEGORY;
  const isBrand = id === C.FILTER.TOPIC.BRAND;
  const isPrice = id === C.FILTER.TOPIC.PRICE;
  const isEtc = id === C.FILTER.TOPIC.ETC;
  return (
    <S.Wrapper>
      {isCategory && <Category isLoading={isLoading} />}
      {isBrand && <Brand brandDataList={brandDataList} isLoading={isLoa
ding} />}
      {isPrice && <Price />}
      {isEtc && <Etc />}
    </S.Wrapper>
); };
const S = {
  Wrapper: styled.div`
    margin-top: 10px;
  `
};
export default Toggle;
```