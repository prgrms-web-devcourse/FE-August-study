![](https://images.velog.io/images/younoah/post/eb614ffb-e7df-4bad-8fb2-68677319d022/myJavaScript.png)

## intro

사용자가 브라우저에서 url을 입력하고 엔터를 첬을 때!!

브라우저는 어떻게, 어떤 과정을 통해서 브라우저 화면을 띄워줄까?? 🤔

차근 차근 알아보자!!



## 브라우저의 기본 구성 요소

우선 브라우저가 어떻게 구성이 되어 있어서 어떤 일들을 하는지 알아보자.

![](https://images.velog.io/images/younoah/post/cdf1a0c3-2542-4ed0-9b4e-e1de6784e35e/%E1%84%87%E1%85%B3%E1%84%85%E1%85%A1%E1%84%8B%E1%85%AE%E1%84%8C%E1%85%A5%E1%84%8B%E1%85%B4%E1%84%80%E1%85%B5%E1%84%87%E1%85%A9%E1%86%AB%E1%84%80%E1%85%AE%E1%84%89%E1%85%A5%E1%86%BC%E1%84%8B%E1%85%AD%E1%84%89%E1%85%A9.png)

### 사용자 인터페이스

- 브라우저의 윈도우를 제외한 나머지 모든 부분을 말한다.

- 가령 주소 표시줄, 뒤로가기/앞으로가기 버튼, 북마크 등과 같은 아이들이 브라우저의 사용자 인터페이스라고 할 수 있다.

- 이름 그대로 사용자와 상호작용하는 역할을 한다.



### 브라우저 엔진

- **사용자 인터페이스**와 **렌더링 엔진** 사이의 **동작을 제어**한다.



### 렌더링 엔진 ⬅️ 오늘의 주인공 🥳

- 브라우저 엔진으로부터 요청받은 URI를 받아서 서버에 요청을 보낸다. (네트워킹, 통신)
- 서버로부터 URI에 해당하는 데이터(HTML, CSS, JS, 이미지파일들 등등)를 받아서 파싱한 후 브라우저 화면에 렌더링을 한다.

> 브라우저별 렌더링 엔진
>
> | 브라우저 |         렌더링 엔진         |
> | :------: | :-------------------------: |
> |    IE    |           Trident           |
> |   Edge   |       EdgeHTML, Blink       |
> |  Chrome  | Webkit, Blink(버전 28 이후) |
> |  Safari  |           Webkit            |
> | FireFox  |            Gecko            |



### 네트워크(통신)

- 렌더링 엔진으로부터 HTTP요청을 받아서 네트워크 요청을하고 응답을 전달한다.
- 브라우저와 독립적으로 브라우저 하부, 즉 OS단에서 실행된다. (OSI 7계층 나와주세요~)



### 자바스크립트 해석기

- 자바스크립트를 파싱(코드를 해석하고 실행)한다.

>브라우저별 자바스크립트 해석기(엔진)
>
>| Browser, Headless Browser, or Runtime | JavaScript Engine |
>| :------------------------------------ | :---------------- |
>| Mozilla                               | Spidermonkey      |
>| Chrome                                | V8                |
>| Safari                                | JavaScriptCore*   |
>| IE and Edge                           | Chakra            |
>| Node.js                               | V8                |



### 자료 저장소

- Local Storage, 쿠키 등 클라이언트 사이드에서 데이터를 저장한다.



### 디스플레이 백엔드(UI 백엔드)

- 체크박스나 버튼같은 기본적인 위젯을 그린다.


</br>


>  이 글은 브라우저가 어떻게 렌더링을 하는지에 대해 알아보고자 하는 것이니 렌더링 엔진이 **어떻게 렌더링 하는지 과정**에 대해 집중적으로 알아보자!!!



## 렌더링 엔진의 목표

- HTML, CSS, JS, 이미지 등 웹 페이지에서 포함된 모든 요소를 화면에 보여준다.
- 업데이트가 필요할 때 효율적으로 렌더링을 할 수 있도록 한다.



## 브라우저 렌더링 과정 (Critical Rendering Path)

![](https://images.velog.io/images/younoah/post/fbbc1d5a-842f-4271-a9ef-baa05c9e5e16/critical-rendering-path.png)



#### 과정 요약

- 렌더링 엔진은 **HTML문서를 파싱**해서 **DOM Tree**를 만들고, (HTML 파싱)

- **CSS 문서를 파싱**해서 **CSSOM Tree**를 만든다. (CSS 파싱)

- DOM Tree와 CSSOM Tree를 이용하여 **Render Tree**를 만들고, (Attachment)

- Render Tree를 이용하여 요소들을 브라우저 화면에 **어디에 배치**할지, **어떤 크기**를 가질지 계산한다. (Layout)

- 다시 Render Tree와 Layout 정보를 이용해서 요소들을 화면에 그릴 **픽셀**로 준비한다. (paint)

- 최종적으로 브라우저 윈도우 화면에 **픽셀이 렌더링**이 된다. (조금 더 정확하게 분류하자면, Composite)



### 1. HTML 파일 파싱

![](https://images.velog.io/images/younoah/post/b4d77c47-c61d-4a9f-961a-e3d0e48f19c4/critical-rendering-path-1.png)

서버로 부터 받은 HTML파일을 불러와서 파싱을한다.

렌더링 엔진이 HTML을 파싱하는 과정에서 필요한 자바스크립트, CSS파일을 요청하기도 한다.

- CSS는 CSSOM Tree로
- 자바스크립트는 자바스크립트 해석기로



### 2. HTML에서 DOM Tree로

![](https://images.velog.io/images/younoah/post/1f0acf37-ee78-4b1a-ab4a-4905c2e8db02/critical-rendering-path-2.png)

렌더링 엔진이 HTML파일로 부터 DOM Tree를 생성한다.



#### 2-1. DOM Tree란?

![](https://images.velog.io/images/younoah/post/461d7049-af4b-4b7c-b32e-84f88ff94e9f/HTML%E1%84%91%E1%85%A1%E1%84%89%E1%85%B5%E1%86%BC.png)

쉽게 말해서 HTML 페이지에서 브라우저가 이해할 수 있도록 브라우저만의 언어(렌더트리)로 바꾼것이다.

- HTML 코드를 어휘분석을 통해 HTML5표준에 지정된 **토큰**으로 변환된다.

- 브라우저의 **렉싱 과정**을 통해 토큰을 **속성과 규칙을 정의한 DOM Node객체**로 변환한다.

- <u>각 노드가 **서로의 연관성**을 나타내도록 트리를 형성하는데 이것이 **DOM Tree**이다.</u>

> HTML태그가 부모와 자식의 관계가 있기 때문에 트리구조로 구현된다.



### 3. CSS에서 CSSOM Tree로

![](https://images.velog.io/images/younoah/post/2450e0a5-a60d-4fcc-b5cf-135c4e6eca75/critical-rendering-path-3.png)

HTML코드를 파싱 하는 과정에서 css파일을 불러오는 `link` 태그를 만나면 CSS파일을 파싱하고 CSSOM Tree를 생성한다.

#### 3-1. CSSOM Tree란?

![](https://images.velog.io/images/younoah/post/a9b300d8-f3c3-4629-a97b-950f38582f17/css%E1%84%91%E1%85%A1%E1%84%89%E1%85%B5%E1%86%BC.jpg)

DOM Tree를 생성했던 방식과 유사하게 **CSSOM Tree**를 생성하는데

CSSOM Tree는 DOM이 어떻게 화면에 표시해줄지를 알려주는 역할을 한다.

> CSS는 [스타일 적용우선순위(cascading)](https://velog.io/@younoah/css-Cascading) 이 있기 때문에 트리구조 형태로 구현된다.



### 4. DOM Tree + CSSOM Tree ➡️ Render Tree

![](https://images.velog.io/images/younoah/post/cfaa2af5-cd0f-420f-8693-ba7c814234fe/critical-rendering-path-4.png)

렌더링 엔진은 DOM Tree와 CSSOM Tree를 합처서 Render Tree를 생성한다. Render Tree는 화면에 표시되어야 하는 노드의 컨텐츠와 스타일 정보 정보만을 포함한다. 

(`head`, `meta` 태그나, `display: none` 요소는 렌더트리에서 제외)

> 어떤식으로 만드는 건데?
>
> Render Tree는 DOM노드를 순회하면서 CSSOM Tree에 매칭되는 스타일 규칙을 적용한다. 그럴 때 화면에 표시되어야 할 노드는 렌더트리에 포함시킨다.



#### 4-1. Render Tree란?

![](https://images.velog.io/images/younoah/post/4f609a24-e152-4a2e-8717-f00e801cf758/render-tree.png)

DOM Tree와 CSSOM Tree를 합처서 화면에 표시되어야 하는 노드의 컨테츠, 스타일 정보를 포함하는 Tree이다.



### 5. Layout

![](https://images.velog.io/images/younoah/post/e82c5c80-eb67-45a1-8deb-1d6266278450/critical-rendering-path-5.png)

브라우저 윈도우(뷰포트)에서 요소들의 위치와 크기를 계산하여 요소가 어디에 어떻게 그려질지 정보를 준비한다.



### 6. paint

![](https://images.velog.io/images/younoah/post/36d29964-c924-4d6e-b4f3-a7b90b0a9b5a/critical-rendering-path-6.png)

화면에 실제 픽셀로 그리도록 변환하는 과정이다. 

이 과정에서 렌더트리에 포함된 요소들이나 텍스트, 이미지들이 실제 픽셀들로 그려진다.

최종적으로 브라우저 윈도우 화면에 픽셀들을 렌더링한다. (Composition)



## 렌더링 성능 개선하기

브라우저 렌더링 과정을 통해서 빠르게 렌더링이 되려면 어떻게 해야할까? 🧐


![](https://images.velog.io/images/younoah/post/3592e23e-5c25-4c6e-b8dd-ee4ba72c6bb0/criticalRenderingPath.png)


- 쓸데 없는 태그, CSS를 줄임으로써 렌더트리가 빠르게 만들어지도록 한다.

- 리렌더링이 될 때 최대한 컴포지션만 일어나도로 하자 (**layout** ➡️ **paint** ➡️ **composition**)
  - 레이아웃부터 다시 만들어서 컴포지션까지 가거나 (가장 최악의경우임)
  - 페인트부터 시작해서 컴포지션을 하게된다면 작업이 오래 걸리기 때문이다.
  - 즉, **컴포지션**만 다시 발생하면 **베스트**, **페인트**부터 발생하면 **쏘쏘**, **레이아웃**부터 발생하면 **최악**이다.



### 브라우저별로 렌더링 엔진은 다르다

따라서 각 렌더링 엔진별로 동작하는 방식이 약간은 다르다.

아래 사이트에서 렌더링 엔진별로 어떻게 DOM을 구성할지 참고하면 좋다.

https://csstriggers.com/



## 참고

> **참고자료**
>
> [이웅모님의 모던자바스크립트 딥다이브](https://poiemaweb.com/js-browser)
>
> [David, WebDeveloper](https://davidhwang.netlify.app/Developments/browser-rendering-process/)
>
> [우아한테크 유튜브](https://www.youtube.com/watch?v=sJ14cWjrNis)
>
> [Beomy 블로그- 설명 역대급](https://beomy.github.io/tech/browser/browser-rendering/)
>
> [개발괴발 네이버포스트](https://post.naver.com/viewer/postView.nhn?volumeNo=8431285&memberNo=34176766)
>
> [Santos의 개발브로그](https://sangcho.tistory.com/entry/%EB%B8%8C%EB%9D%BC%EC%9A%B0%EC%A0%80%EC%9D%98Rendering2Operation?category=740188)
>
> [CHANTEONG 블로그](https://chanyeong.com/blog/post/43)
>
> [마광휘님 블로그](https://vallista.kr/2019/05/06/%EB%B8%8C%EB%9D%BC%EC%9A%B0%EC%A0%80-%EB%A0%8C%EB%8D%94%EB%A7%81-%EA%B3%BC%EC%A0%95)
>
> https://www.html5rocks.com/en/tutorials/internals/howbrowserswork/
>
> [브라우저는 웹페이지를 어떻게 그리나요? - Critical Rendering Path](https://m.post.naver.com/viewer/postView.nhn?volumeNo=8431285&memberNo=34176766)
>
> https://dev.to/starkiedev/how-the-browser-renders-a-web-page-1ahc



> **이미지 출처**
>
> https://developers.google.com/web/fundamentals/performance/critical-rendering-path/constructing-the-object-model?hl=ko
>
> https://post.naver.com/viewer/postView.nhn?volumeNo=8431285&memberNo=34176766
>
> https://dev.to/starkiedev/how-the-browser-renders-a-web-page-1ahc
>
> https://sangcho.tistory.com/entry/%EB%B8%8C%EB%9D%BC%EC%9A%B0%EC%A0%80%EC%9D%98Rendering2Operation?category=740188
>
> https://beomy.github.io/tech/browser/browser-rendering/