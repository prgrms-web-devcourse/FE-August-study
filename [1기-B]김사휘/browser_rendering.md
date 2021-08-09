---
title: "브라우저 렌더링 (Browser Rendering)"
date: "2021-08-08"
tags: ["TIL", "브라우저", "렌더링", "DOM", "CSSOM"]
---
### Browser.

브라우저의 주요한 역할은 사용자가 서버에 특정 리소스를 요청하였을 때 서버가 응답해준 리소스를 화면에 표시해주는 것이다. 서버가 넘겨준 HTML, CSS, JavaScript 파일을 브라우저가 어떤 과정을 거쳐서 시각적으로 그려내는 지에 대해서 알아보자.

그 전에 브라우저를 구성하는 요소들에 대해 간략히 살펴보면 다음과 같다.

+ 사용자 인터페이스  
  주소 표시줄, 이전/다음 버튼, 북마크 등등 뷰포트를 제외한 나머지 부분을 가리킨다.

+ 브라우저 엔진  
  사용자 인터페이스와 렌더링 엔진 사이의 동작을 제어하는 역할. URL을 로드 하거나 새로고침, 뒤로가기 등을 실행하는 메서드를 제공한다.

+ 통신  
  HTTP, FTP 등 브라우저 내부 계층에서 이루어지는 통신

+ 자바스크립트 해석기  
  자바스크립트 코드를 해석하고 실행하는 역할. 크롬의 V8.

+ UI 백엔드  
  select box, input box 등과 같은 기본 위젯을 그리는 역할. OS의 사용자 인터페이스 체계를 이용한다. 

+ 자료 저장소  
  쿠키, 로컬 스토리지, IndexedDB 등 브라우저 내에서 자료가 저장되는 영역.

+ 그리고 **'렌더링 엔진'**이 있다.   
  사용자가 요청한 리소스를 파싱하여 화면에 표시해주는 역할을 담당하는, 이 글의 주제와 가장 관련 깊은 부분이다.

<img src="https://user-images.githubusercontent.com/75300807/128510906-105bd8d6-2b7c-4747-8e2a-4b9e9fc61436.png" alt="렌더링 엔진-50px" style="zoom:50%;" />  

(파이어폭스의 Gecko, 사파리의 Webkit, 크롬의 Blink 등이 렌더링 엔진이다)


### 렌더링 엔진의 동작 원리.

브라우저가 서버에 요청을 보낸다. 서버는 브라우저가 요청을 보낸 HTML 파일을 읽어 들여 메모리에 저장한 다음, 메모리에 저장된 바이트(2진수)를 응답한다.

응답된 2진수 형태의 HTML 문서는 meta 태그의 charset 어트리뷰트에 의해 지정된 인코딩 방식(UTF-8)을 기준으로 브라우저가 문자열로 변환한다. 이때 meta 태그의 인코딩 방식은 응답 header의 content-type에 담겨 전달된다.

이제 본격적인 렌더링 과정이 수행된다.

#### Critical Rendering Path. 중요 렌더링 경로

렌더링 엔진의 동작은 크게 4단계로 구분할 수 있다. 

1. 파싱(을 통한 DOM Tree 구축)
2. Render Tree 구축
3. Render Tree 배치
4. REnder Tree 그리기

이 과정을 Critical Rendering Path라고 부른다.

#### 1. 파싱

파싱을 통해 개발자가 작성한 HTML 마크업을 '브라우저가 이해할 수 있는 객체 구조'로 변환하는데 그것이 바로 **DOM, Document Object Model**이다. DOM은 HTML 마크업이 각각의 노드로 변환된 트리 자료구조를 뜻한다.

![DOM tree](https://poiemaweb.com/img/HTMLElement.png)


각각의 element, attribute, text 심지어 주석까지, HTML 파일 안에 작성된 문자열이 노드로 변환된다.

이제 화면에 표시될 콘텐츠들이 브라우저가 이해할 수 있는 형태로 변환됐으니 이를 '어떻게' 보여줄지에 대한 시각 정보가 필요하다.

이를 위해 브라우저는 HTML 파일을 읽어 내려가다가 CSS 파일을 참조하는 \<link>나 \<style>태그를 만나면, HTML을 파싱한 것과 마찬가지로 CSS 파싱을 시작한다. DOM을 생성했듯, CCS를 파싱하여 생성한 객체 모델을 CSSOM, CSS Object Model이라고 한다.

#### 2. 렌더 트리 구축

이제 브라우저가 화면에 표시할 콘텐츠인 DOM과 스타일 정보인 CSSOM이 각각 생성되었다.  다음 단계로는 화면을 그리기 위해 어떤 콘텐츠에 어떤 스타일이 적용되어야 하는지를 알아야 할 것이다. 이를 위해 DOM 트리와 CSSOM 트리가 순서에 맞게 결합되어 렌더 트리를 형성한다.

<img src="https://developers.google.com/web/fundamentals/performance/critical-rendering-path/images/render-tree-construction.png?hl=ko" alt="렌더 트리" style="zoom: 60%;" />


이때, 렌더 트리에는 레이아웃에서 공간을 차지하지 않는 요소들은 포함되지 않는다. \<meta>태그, \<script>태그, 그리고 display: none 인 요소 등이 이에 해당된다.

반면 visibility가 hidden인 요소들은 화면에 보이지 않지만 레이아웃에서는 공간을 차지하기 때문에 렌더 트리에 포함이 된다.

#### 3. 렌더 트리 배치

이제 어떤 콘텐츠에 어떤 스타일이 적용되는지를 알았으니, 화면의 어떤 위치에 어떤 크기로 콘텐츠를 그려줄지를 계산하는 과정이 필요하다. 이 과정을 **레이아웃** 또는 **리플로우**라고 한다.

CSS에서 사용된 ```width: 100%```, ```1.2em```, ```100vh``` 같은 상대적 단위를 px 값으로 계산하는 과정이 여기에 포함된다.

#### 4. 렌더 트리 그리기

레이아웃 계산이 끝나면, 브라우저는 Paint 이벤트를 발생시켜 렌더 트리를 화면의 픽셀로 변환하여 사용자의 뷰포트에 그려준다. 



이렇게 브라우저의 렌더링 단계인 Critical Rendering Path에 대해서 간략히 알아봤다. 작동 과정을 알게 되었으니 이제 요소에 변화가 생길 때마다 렌더링 과정에서 어떤 일이 발생하는지에 대해서 고려해볼 수 있을 것이다.

+ repaint

  예를 들어 요소의 위치나 크기는 그대로인데, 버튼 클릭에 따라 배경 색만 바뀌는 상황이 발생한다면 마지막 단계인 페인팅 과정만 다시 수행하면 된다.

+ reflow + repaint

  브라우저 창의 크기가 바뀌거나, 요소의 크기가 조작되거나, 새로운 요소가 생성되거나 하는 경우는 요소들의 위치와 크기 값도 다시 계산해야 하고, 계산이 반영된 내용을 화면에 다시 그려야한다. 리플로우는 비용이 큰 작업이니 최소화 하는 것이 효율적이다.

이런 부분을 염두해둔다면 CSS를 사용할 때 렌더링 비용이 적게 드는 방법을 고를 수 있는 근거를 마련할 수 있을 것이다. (ex. ```fixed```는 다른 요소들의 관계에 영향을 받지 않으니 리플로우의 대상이 아니다)



### HTML 속의 CSS와 JavaScript

#### 점진적 렌더링

렌더링 엔진은 사용자 경험을 향상시키기 위해 가능하면 화면에 내용을 빠르게 표시하고자 한다. 이를 위해 모든 HTML이 파싱될 때까지 기다리지 않고, 파싱이 완료된 것부터 배치와 그리기 과정을 수행한다. 이를 점진적 렌더링이라고 한다.

#### CSS

그러나 CSS는 이것이 불가능하다. CSS는 규칙을 덮어쓸 수 있기 때문에 파싱된 부분을 먼저 렌더링하기 시작했다가 이를 변경하는 스타일이 추후에 등장한다면 문제가 될 것이기 때문이다.

따라서 렌더링 엔진은 HTML을 파싱하다가 CSS를 만나면 처리가 다 끝날 때까지 모든 콘텐츠의 렌더링을 일시적으로 '중단'한다. 그렇기 때문에 CSS를 참조하는 \<link> 태그가 \<head>에 포함된다. 만약 \<body> 중간에 CSS가 있다면, 점진적 렌더링을 중단시킬 것이기 때문이다.

#### JavaScript

자바스크립트 엔진은 자바스크립트 코드를 CPU가 이해할 수 있는 low-level-language로 변환하고 실행하는 역할을 담당한다. DOM과 CSSOM 처럼 자바스크립트 엔진은 자바스크립트를 해석하여 추상적 구문 트리 AST (Abstract Syntax Tree)를 생성한다. 그리고 AST를 기반으로 [바이트코드](https://ko.wikipedia.org/wiki/%EB%B0%94%EC%9D%B4%ED%8A%B8%EC%BD%94%EB%93%9C)를 생성하여 실행한다.

렌더링 엔진이 HTML 파싱 중에 \<script> 태그를 만나는 경우에는 스크립트가 실행 종료될 때까지 DOM 생성이 일시 중지된다. 이는 렌더링 엔진의 제어권한이 자바스크립트 엔진으로 넘어가기 때문이다.

그렇기 때문에 자바스크립트 파일을 \<body> 태그의 가장 하단에 배치한다. 자바스크립트를 통해서 DOM 요소를 조작하려고 하는데 만약 해당 요소가 \</script> 이후에 위치하고 있다면, 스크립트 실행 시점에 아직 파싱되지 않은 상태라는 뜻이다. 따라서 존재하지 않는 요소를 조작하려고 시도하여 원치 않는 결과가 발생한다.

또한 자바스크립트로 CSS를 조작해야 하는 경우도 있다. 따라서 스크립트가 실행되는 시점에 CSSOM 생성이 완료되지 않았다면 스크립트는 실행되지 않고, CSSOM 완료가 될 때까지 '대기'한다. 

HTML5에서는 즉각적인 스크립트 태그의 실행이 야기하는 문제를 해결하기 위해 HTML 파싱과 자바스크립트 파일 로드를 비동기적으로 처리하도록 돕는 ```async```와 ```defer```가 도입되었다. [👉링크](https://ko.javascript.info/script-async-defer)



### 렌더링 과정과 관련된 유용한 기능.

브라우저의 렌더링과 관련된 정보들을 확인해볼 수 있는 몇 가지 방법을 찾아봤다.

#### csstriggers.com

<img src="https://user-images.githubusercontent.com/75300807/128514534-00ebbfde-8c6b-4c50-a452-2617d0153d3d.PNG" alt="csstriggers" style="zoom: 48%;" />

[이곳](https://csstriggers.com/ )에서는 특정 CSS를 수정하면 각 브라우저의 엔진별로 어떤 렌더링 과정을 다시 수행해야하는지를 알려준다.
#### 크롬 개발자 도구

크롬 개발자 도구의 기능들을 잘 활용한다면, 웹페이지의 렌더링 성능과 효율성에 관한 다양한 정보를 얻을 수 있다.

<img src="https://user-images.githubusercontent.com/75300807/128514950-52cc8aaf-b276-428f-8a91-2908c94e2cb7.PNG" alt="network" style="zoom: 69%;" />

[개발자 도구] - [Performance] 탭의 Record 기능을 활용하면, 녹화된 시간 동안 브라우저에서 발생한 이벤트, 렌더링 단계를 각 프레임 별로 확인할 수 있다.

<img src="https://user-images.githubusercontent.com/75300807/128519046-6586e6da-8d48-4b04-ab5f-124f263d72f4.PNG" alt="콘솔" style="zoom: 68%;" />

또한 [console drawer] - [Rendering]의 Paint flashing 기능을 사용하면, 페이지에서 리페인트 해야하는 영역을 실시간으로 표시해주기도 한다. 

이밖에도 DOMContentLoaded 시간을 통해 DOM이 생성되는 데에 걸린 시간 등을 체크해볼 수도 있다.

#### 더하여.

모던 브라우저에서 CSS나 애니메이션의 렌더링 과정에 대해 더욱 깊게 이해하기 위해서는 레이아웃 단계 이후에 등장하는 'Layer Model' 개념에 대해서 자세히 공부해보면 좋을 것 같다. 



#### 참고

[How Browsers Work: Behind the scenes of modern web browsers](https://www.html5rocks.com/en/tutorials/internals/howbrowserswork/)  
[브라우저는 어떻게 동작하는가?](https://d2.naver.com/helloworld/59361)  
[Critical Rendering Path](https://developers.google.com/web/fundamentals/performance/critical-rendering-path?hl=ko)  
[FrontEnd 개발자가 수행하는 성능 개선 작업](https://sculove.github.io/slides/improveBrowserRendering/#/)  
[Accelerated Rendering in Chrome](https://www.html5rocks.com/ko/tutorials/speed/layers/)  
[문서 객체 모델](https://poiemaweb.com/js-dom)  
[Reflow or Repaint(or ReDraw)과정 설명 및 최적화 방법](https://webclub.tistory.com/346)  
[[자바스크립트] 브라우저 렌더링](https://12bme.tistory.com/140)  

**이미지 출처**  
[브라우저 렌더링 엔진](https://medium.com/@jonbiro/browser-engines-chromium-v8-blink-gecko-webkit-98d6b0490968)  
[DOM Nodes](https://poiemaweb.com/)  
[Render Tree](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/)  