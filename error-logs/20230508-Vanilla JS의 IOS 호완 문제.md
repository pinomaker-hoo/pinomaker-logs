# Vanilla JS의 IOS 호완 문제 -> 2023.05.08

(1) 개요

```
회사에서 진행하는 프로젝트 중에서, Vanilla JS를 이용하여 PC와 Mobile View를 만든 후 Mobile은 Unity를 게임과 WebView를 패키징하여 만든 프로젝트다.
```

<br />

프로젝트를 진행하던 중, 내 휴대폰과 PC는 물론 동료들의 휴대폰과 PC에서도 제대로 작동하였으나, 특정 사람(iPhone)에서만 작동이 안 되는 현상이 있었고 이에 나는 캐시 문제 때문에, 작동이 안 되는 것이라고 판단을 해서 캐시를 지우라고 했으나 같은 현상이 일어난다고 했다.

<br />

그래서 그의 휴대폰을 다음 날에 받아서 실행해보았더니 캐시 문제가 아니였다. 이에 휴대폰을 맥북에 연결하여 디버깅한 결과 아래와 같은 결과가 나왔는 데, 이는 JS 버전의 차이 때문이였다. 구 버전의 아이폰은 safari 등의 버전이 낮아 최신 문법을 호완하지 않았기에 오류를 발생한 것이였다.

![스크린샷 2023-05-08 오후 12 00 04](https://user-images.githubusercontent.com/56928532/236769222-f0737896-2de7-4070-b891-2a71085bd899.png)

<br />

(2) 해결 방법

이에 나는 React에서도 이런 상황을 대비하여 JSX라는 최신 문법으로 작성된 코드를 Babel를 이용하여 ES5로 내리는 것을 기억하여 해당 프로젝트를 Babel를 이용하여 코드의 버전을 ES5로 내려 구 버전의 브라우저에서도 호완이 가능하게 해결하였다.

(3) 느낀 점

평소에 개발을 할 때 나는 Babel, Webpack, Vite, SWC... 등 다양한 번들링 및 패키징 툴을 사용하면서 그것들이 어떤 역할을 하는 지에 대해서 조금만 알고 있지 그것에 대한 원리와 필요성에 대한 중요성을 잘 몰랐으나 이번 기회를 통해 Babel과 같은 것들에 대한 관심이 커져 따로 공부를 할 예정이다.
