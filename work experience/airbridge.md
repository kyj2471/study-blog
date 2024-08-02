# 에어브릿지 사용

```jsx
💡 Marketting Tool의 하나로 링크를 통한 앱 인스톨/이벤트 등의 어트리뷰션 분석이 가능한 도구입니다.
```

**Web SDK** 설치 패키지 로드 아래 옵션 3개 중 하나를 선택해서 설치해주세요. 직접 로드 (옵션 1) 아래와 같은 코드를 head 태그 안 끝에 넣어 주세요. NPM 모듈 사용 (옵션 2) 아래 명령을 실행해서

[Download Guide](https://developers.airbridge.io/docs/web-sdk#%EC%95%B1-%EB%8B%A4%EC%9A%B4%EB%A1%9C%EB%93%9C-%EB%B2%84%ED%8A%BC-%EC%84%A4%EC%A0%95)

```html
// html
<button id="app_download">앱으로 이동</button>

```

```jsx
// javascript
airbridge.setDownloads({
    buttonID: "app_download",
    // or ["app_download_1", "app_download_2", ...]
    defaultParams: {
        campaign: 'example_campaign',
        medium: 'example_medium',
        term: 'example_term',
        content: 'example_content'
    }
});

```

### Airbridge 연동 2

[DeepLink Guide]

```jsx
airbridge.openDeeplink({
    type: "redirect",
    deeplinks: {
        ios: "ablog://main",
        android: "ablog://main",
        desktop: "<http://blog.ab180.co>"
    },
    fallbacks: {
        ios: "itunes-appstore",
        android: "google-play"
    },
});

```

실 적용기 custom hooks 를 활용.

```jsx
const useAirbridge = () => {
  const initializedRef = useRef(false);
  const paramRef = useRef();

  // Airbridge Action 에 맞는 분기처리
  const handleAirbridge = () => {
    if (paramRef.current.actionType == Const.ACTION_TYPE_DOWNLOAD) {
      handleDownload();
    }
    if (paramRef.current.actionType == Const.ACTION_TYPE_DEEPLINK) {
      handleDeepLink();
    }
  };

  // 다운로드
  const handleDownload = () => {
    getAirbridge().then((airbridge) => {
      airbridge.default.setDownloads({
        buttonID: Const.DEFAULT_PARAM.BUTTON_ID.DOWNLOAD,
        defaultParams: {
          campaign: Const.DEFAULT_PARAM.CAMPAIGN.WEB_TO_APP,
          medium: Const.DEFAULT_PARAM.TOP
        }
      });
      paramRef.current.initCallback();
    });
  };

  // 딥링크 & 디퍼드 딥링크
  const handleDeepLink = () => {
    getAirbridge().then((airbridge) => {
      airbridge.default.openDeeplink({
        type: 'redirect',
        deeplinks: {
          ios: paramRef.current.deepLink,
          android: paramRef.current.deepLink
        },
        fallbacks: {
          ios: Const.MARKET.IOS,
          android: Const.MARKET.AND
        },
        defaultParams: {
          campaign: Const.DEFAULT_PARAM.CAMPAIGN.WEB_TO_APP,
          medium: Const.DEFAULT_PARAM.POP_UP,
          term: paramRef.current.trackingParams
        }
      });
    });
  };

  /**
   * Airbridge Hooks의 Event 발생시 호출되는 콜백
   */
  const onChangeAction = useCallback(
    (params) => {
      paramRef.current = params;
      if (!initializedRef.current) {
        getAirbridge().then((airbridge) => {
          airbridge.default.init(
            {
              app: Const.APP_NAME,
              webToken: Const.WEB_TOKEN,
              useProtectedAttributionWindow: true
            },
            () => {
              initializedRef.current = true;
              handleAirbridge();
            }
          );
        });
        return;
      }
      handleAirbridge();
    },
    [initializedRef]
  );

  // Dynamic Import (Loadable Component에서는 Library 지원이 안되서 React의 기능활용)
  const getAirbridge = () => {
    return import('airbridge-web-sdk-loader');
  };

  return { onChangeAction };
};
export default useAirbridge;

```

### Airbridge 연동 3

아래 코드를 좀더 살펴보자면 Code Splitting을 활용하였습니다. 웹+모웹에서 에어브릿지 링크를 사용하는 경우는 전체 트래픽에 비해 많지 않습니다. 그런데 사용할지 사용하지 않을지 결정되지 않은 기능을 위해 전체 웹사이트 js 용량을 증가시킬 필요는 없습니다. SEO에도 좋지 않지만 사용자들도 초기 로딩이 늦어지게 되는 문제가 발생합니다.

```jsx
const getAirbridge = () => {
  return import('airbridge-web-sdk-loader');
};

useState를 활용하기보다는 useRef를 활용하였습니다. 리 렌더링이 필요한 상황은 아니였기 때문에…

const initializedRef = useRef(false);
const paramRef = useRef();

```

### Airbridge 연동 4

실제 호출하는 곳에서는 사용법이 두가지로 나뉩니다.

1. 콜백이 필요한 경우. (앱 다운로드의 상황) downloadRef는 `<div id={C.DEFAULT_PARAM.BUTTON_ID.DOWNLOAD} ref={downloadRef}>` 와 같이 실제 클릭이 필요한 dom에 연결시켜 둡니다. 콜백이 발생하면 해당 div의 click 이벤트가 발생해야 airbridge의 download가 수행되게 됩니다.

```jsx
const handleDownload = () => {
  const handleDownloadClick = () => {
    downloadRef.current.click();
  };
  onChangeAction({ actionType: C.ACTION_TYPE_DOWNLOAD, initCallback: handleDownloadClick });
};

```

1. 딥링크가 필요한 경우. 이 경우는 좀더 사용이 간단해 집니다. 콜백이 필요없기 때문에 클릭 이벤트가 발생하면 바로 훅의 onChangeAction function을 호출하면 되겠습니다.

```jsx
const { onChangeAction } = useAirbridge;

onChangeAction({
  actionType: C.ACTION_TYPE_DEEPLINK,
  deepLink: deepLink,
  trackingParams: trackingParams
});

```

**[Class Component에서의 Hook]** Function Component에서는 그냥 사용할 수 있는 hook이지만 Class Component에서는 사용이 불가합니다. 때문에 호환이 가능한 Wrapper Component를 만들어 두었습니다.

```jsx
import useAirbridge from './useAirbridge';
// Airbridge hooks 연동 (Class Component에서 사용가능하도록 Wrapper 생성)
function withUseAirbridge(Compo) {
  return function (props) {
    const airbridge = useAirbridge();
    return <Compo {...props} useAirbridge={airbridge} />;
  };
}
export default withUseAirbridge;
export default withUseAirbridge(withRouter(ItemMobileDownload));

```