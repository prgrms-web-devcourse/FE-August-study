이번주제는 SSR과 CSR이다. SPA와 관련한 강의들을 들으면서 짤막하게 소개된 내용인데 더 깊이 알아보려고 조사했다.

![](https://images.velog.io/images/goum/post/9faee0f3-dcf0-4cf6-83c5-930d8eaf21c9/image.png)

### 정적 웹사이트 (Static Websites)

- 클라이언트가 서버에 요청을 보내면, 서버에 저장된 파일을 받아 브라우저가 그대로 그리는 것을 말한다.
- 화면에 변화를 주려고 하면 그때마다 서버에서 새로운 HTML을 전송받아 다시 렌더링 해야 했다.
- 매번 처음부터 새로 렌더링 하기때문에 성능적인 문제도 있고 화면의 깜박임으로 UX측면에서도 좋지 않았다.

### 동적 웹사이트 (Dynamic WebSites)

- 클라이언트가 서버에 요청을 보내면, 서버에 저장된 html파일이 그대로 브라우저에 그려지는것이 아닌 동적으로 html파일이 만들어 지는것을 말한다.
- 데이터가 실시간으로 변경돼서 유저에게 보여줘야하는 사이트들에 적합하다.

과거에는 대부분 정적인 사이트였지만, 점점 처리해야하는 데이터가 많아지고 실시간으로 사용자와 상호작용하는 UI가 많아지면서 동적인 사이트가 많아지게 되었다.

이러한 사이트를 만들기 위한 렌더링 방식이 SSR 과 CSR인 것이다.

## CSR (Client Side Rendering)

![](https://images.velog.io/images/goum/post/854d5cce-6c28-4998-8c5c-756e7d8ffdba/image.png)

- CSR은 서버로 부터 비어있는 HTML, JS를 받아와 클라이언트에서 JS가 실행되며 콘텐츠를 렌더링 하는것을 의미한다.
- 추가로 필요한 데이터가 있다면 서버에 요청해 JSON같은 포맷으로 받아와 JS를 통해 동적으로 HTML을 생성해서 보여준다.
- CSR 타임라인
  1. 사이트 접속
  2. 서버에서 index.html 받아옴 (html 구조만 담긴 비어있는 파일)
  3. index에 링크된 모든 로직이 있는 자바스크립트를 받아옴
  4. 이때부터 브라우저에서 js파일로 동적으로 html 생성 → TTV TTI

\*\* TTV (Time To View) 사용자가 웹브라우저에서 내용을 볼 수 있는 시점

\*\* TTI (Time To Interact) 사용자가 웹브라우저에서 인턴랙션을 할 수 있는 시점

### 장점

- 첫 로딩이후에는 동적으로 빠르게 렌더링 되기 때문에 더나은 사용자경험을 제공한다.
- 서버에 요청하는 횟수가 줄어 서버의 부담이 덜하다.

### 단점

- 첫화면을 보기까지 시간이 오래 걸릴 수 있다.
  - js로 렌더링 하기때문에 처음 응답에서 html과 js를 다 받아오는데 서비스 사이즈가 커질수록 js코드양도 많아지고 그렇게 파일이 커지게되면 받는시간이 길어질수있고 받아서 그려지는 시간까지의 텀도 길어질수있다.
- SEO에 취약하다.
  - 검색엔진 봇들이 크롤링할 때 html이 비어있어 크롤링이 쉽지 않다. 구글의 경우에는 js까지 읽어 분석을 한다고 하는데 이것도 SSR에 비해 잘된다고 할 수 없다.

### SPA(Single Page Application)

- CSR 렌더링 방식을 이용하여 제작한 웹사이트로 하나의 HTML을 기반으로 자바스크립트를 이용해 동적으로 화면의 콘텐츠를 바꾸는 방식

## SSR (Server Side Rendering)

![](https://images.velog.io/images/goum/post/d296a90f-4ec7-4aa6-bf27-d9730b918f3a/image.png)

- CSR의 문제점으로 인해 다시 떠오른 SSR방식은 static sites 에서 이용하던 방식으로 클라이언트가 서버에 요청하면 서버가 그요청에 대한 결과값을 도출하여 만들어진 새로운 HTML을 클라이언트에 넘겨준다.
- SSR 타임라인
  1. 사이트에 접속
  2. 서버에서 요청에 따라 만들어진 HTML을 받아옴 ⇒ TTV
  3. 서버에서 자바스크립트를 받아옴 ⇒ TTI
  - 자바스크립트를 받아와야 인터렉션이 가능해 TTV와 TTI의 공백이 발생함

### 장점

- HTML에 서버에서 이미 콘텐츠가 담겨오기 때문에 SEO 최적화가 용이하다.
- CSR에 비해 초기로딩이 빠르다.

### 단점

- html을 서버에서 다시받아오기 때문에 페이지 전환시 깜박임이 생긴다.
- 매번 서버와 요청과 응답을 주고받아 서버의 부하가 커진다.
- 초기 화면은 빨리 렌더링이 될지 모르지만 js가 받아지기전에 사용자의 행동이 생긴다면 제대로 반영되지 않을 수있다.

### MPA(Multi Page Application)

- SSR 렌더링 방식을 이용하여 제작한 웹사이트 물리적으로 페이지마다 파일이 각각 존재하는 형태

## 정리

위 두개의 렌더링 방식은 각각 장단점이 존재하고 무엇이 더 좋은 방법이라고 할수없다. 상황에 알맞은 방식을 선택해 이용하면 되고 CSR을 사용한다면 최종적으로 번들링해서 사용자에게 보내주는 js파일을 어떻게 효율적으로 분할해서 첫 로딩시간을 줄일 수 있을지 고민해봐야 한다.

SSR을 사용한다면 TTV와 TTI의 시간차를 줄이기위해 무엇을 할수있고 어떻게 매끄러운 UX를 제공할수있는지 고민해봐야 한다.

또 두가지 방식의 장점을 이용할 수 있는 React의 nextJS와 같은 라이브러리를 이용해보는것도 생각할수 있다.

## 마치며

주제를 준비하는 와중에 온라인세션에서 관련하여 강사님이 발표해주셔서 주제를 변경해야 하나 고민했지만 강사님의 말씀해주신 내용도 제대로 이해하지 못했었기 때문에 계속 이어나가기로 맘먹고 정리했다. 하지만 여전히 흐름을 잘 파악하지 못하겠다 세션도 다시 들어보고 더 찾아보기도 했지만 잘 모르겠어서 일단 이렇게 마무리해 두고 후에 관련한 내용을 더 이해할수 있게 된다면 렌더링의 역사에 관해서 더 디테일하게 파고들어 정리해보고싶다. 아래 참고자료들은 스터디를 위해 참고했던 자료들과 세션이후 더 찾아본 자료들이다.

### 참고

- [https://www.pluralsight.com/blog/creative-professional/static-dynamic-websites-theres-difference](https://www.pluralsight.com/blog/creative-professional/static-dynamic-websites-theres-difference)
- [https://gollumnima.github.io/posts/webpage_type](https://gollumnima.github.io/posts/webpage_type)
- [https://im-developer.tistory.com/227](https://im-developer.tistory.com/227)
- [https://kim-mj.tistory.com/275](https://kim-mj.tistory.com/275)
- [https://developers.google.com/web/updates/2019/02/rendering-on-the-web?hl=ko](https://developers.google.com/web/updates/2019/02/rendering-on-the-web?hl=ko)
- [https://blog.hahus.kr/csr-ssr-spa-mpa-ede7b55c5f6f](https://blog.hahus.kr/csr-ssr-spa-mpa-ede7b55c5f6f)
- [https://velog.io/@ahnjh/Web-웹-서비스의-역사와-발전](https://velog.io/@ahnjh/Web-%EC%9B%B9-%EC%84%9C%EB%B9%84%EC%8A%A4%EC%9D%98-%EC%97%AD%EC%82%AC%EC%99%80-%EB%B0%9C%EC%A0%84)
- [https://tech.weperson.com/wedev/frontend/csr-ssr-spa-mpa-pwa/#ssr-server-side-rendering](https://tech.weperson.com/wedev/frontend/csr-ssr-spa-mpa-pwa/#ssr-server-side-rendering)
- [https://medium.com/walmartglobaltech/the-benefits-of-server-side-rendering-over-client-side-rendering-5d07ff2cefe8](https://medium.com/walmartglobaltech/the-benefits-of-server-side-rendering-over-client-side-rendering-5d07ff2cefe8)
- [https://d2.naver.com/helloworld/2177909](https://d2.naver.com/helloworld/2177909)
- [https://www.youtube.com/watch?v=iZ9csAfU5Os](https://www.youtube.com/watch?v=iZ9csAfU5Os)
- [https://subicura.com/2016/06/20/server-side-rendering-with-react.html](https://subicura.com/2016/06/20/server-side-rendering-with-react.html)
- [https://velog.io/@kdo0129/SSR-CSR](https://velog.io/@kdo0129/SSR-CSR)
