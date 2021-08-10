DOM 설명하기 전에 앞서 브라우저에 대해 간단히 알아보자.

### **브라우저**

브라우저의 사전적 정의는 인터넷상에서 웹에 연결시켜주는 윈도 기반의 소프트웨이이다.

흔히 크롬, 파이어폭스, 사파리, 오페라와 같은 것들이다.
<br><br>

#### **브라우저의 핵심 기능**

브라우저의 핵심 기능은 사용자가 참조하고자 하는 웹 페이지를 서버에 요청을 하고 서버로부터 응답을 받아 브라우저에 표시해 주는 것이다.

브라우저는 서버를 통해 HTML, CSS, JS, 이미지 파일 등을 응답받고

HTML, CSS 파일은 렌더링 엔진의 HTML 파서와 CSS 파서에 의해 파싱(Parsing)되어 DOM, CSSOM 트리로 변환되고

렌더 트리로 결합된다. 이렇게 생성된 렌더 트리를 기반으로 브라우저는 웹페이지를 표시한다.

**이런 과정을 랜더링이라 표현한다.**
  ![랜더링 예시](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbSYA2n%2FbtrbtzRiRj2%2FzHIQsCzwOykovLcxUcKs8K%2Fimg.png)


  ![랜더링 예시1](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FIgKxH%2FbtrbuzX0rbH%2Fuzb90IW8GHgNTYrIcJlel1%2Fimg.png)

  ![랜더링 예시2](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FJLSgK%2FbtrbrVAto1U%2FF2NfHgAAPadKkHLW3DYZK1%2Fimg.png)
  이미지 출처 : https://poiemaweb.com/js-browser

위와 같은 브라우저가 클라이언트에게 화면을 보여주기 위해선 랜더링 과정이 필요하며

**이를 담당하는 랜더링 엔진이 존재한다.**

**크롬 ==> Blink(블링크) 엔진**

**사파리 ==> Webkit(웹킷) 엔진을 사용하며**

**파이어폭스 ==> GecKo(게코) 엔진을 사용한다.**

  ![랜더링 동작 과정](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fbxi0yn%2Fbtrbd0jgLTB%2FpuBLumCTaz15kyiDR2z2rk%2Fimg.png)

브라우저에 따라 다른 랜더링 엔진을 사용한다는 점만 이해하고 넘어가도록 하자.

자세한 정보는 링크 참고(참고 : [https://d2.naver.com/helloworld/59361](https://d2.naver.com/helloworld/59361))

---

#### **브라우저의 흐름**

HTML 파일을 읽고 랜더링 엔진의 HTML 파서가 이를 분석한다. 이때 CSS를 함께 로드하며 CSS 파서가 파싱을 하고

HTML을 분석하여 이를 토대로 DOM 트리를 생성하고, CSS를 분석하여 이를 토대로 CSSSOM 트리를 생성한다.

  ![브라우저의 흐름](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcCGKka%2FbtrborUl2Jj%2FhgI4fjadbBFcahhpUQP1Q1%2Fimg.png)
  이미지 출처 : https://www.youtube.com/watch?v=zyz1eJJjsNE&t=195s

---

**이렇게 생성된 DOM, CSSOM를 이용해 랜더 트리라는 결과물을 만들고 이 결과물을 브라우저에서 실행시켜 클라이언트가 보는 화면(뷰 포트)을 표시하게 된다.**

  ![트리 구조](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcOyuDr%2Fbtrbos60zsF%2FV7v3wX3L93MMNd07Hxscp1%2Fimg.png)
  이미지 출처 : https://poiemaweb.com/js-dom

<br>

> **여기서 CCSOM 이란**  
> **CSS Object Model**  
> **아래에서 DOM을 설명하겠지만, HTML이 아닌 CSS가 대상인 DOM으로**  
> **CSS 스타일을 동적으로 읽고 수정할 수 있게 도와준다.**

---


### **DOM (Document Object Model)**


자바스크립트를 이용해서 **웹 브라우저의 표시되는 문서(HTML, XML, SVG)들을 동적으로 제어할 수 있도록 도와주는 인터페이스**이다.

웹 페이지를 구성하고 있는 문서 안의 다양한 구성요소 (<a> , <div>....) 하나하나를 일종의 객체로 만들어서

자바스크립트가 제어할 수 있도록 하는 것이 DOM이다.
<br><br>

**브라우저가 HTML과 같은 텍스트 파일을 이해하고 출력해주기 위해서는 파싱한 웹 문서를 브라우저가 이해할 수 있는 구조로 구성하여 메모리에 적재해줘야 하는데 그 역할을 DOM이 담당하고 있는 것이다.**

**DOM은 웹 페이지를 핸들링 하기 위한 프로그래밍 인터페이스이며,**

**자바스크립트를 통해 DOM을 제어하여 웹 페이지를 제어한다고 이해하면 될 것 같다.**

여기서, 중요한 것은 DOM은 Javascript 언어의 일부가 아니라는 점이다.

개발자는 단지 DOM을 조작하기 위해 Javascript 언어를 사용할 뿐이다.
  
<br>

#### **그렇다면 DOM을 사용하는 목적은 무엇일까?**

**이유는 동적인 웹 페이지를 만들기 위해서라고 할 수 있다.**

HTML이 정적인 텍스트이기 때문에, 웹 페이지를 하나 만들고 이는 클라이언트와 어떠한 상호작용도 없이 100% 정적인 웹 사이트라면 DOM이 필요 없겠지만 그런 웹 페이지는 이미 도태된 지 오래이다.

사용자가 어떠한 기능을 이용하고 최신화된 정보를 계속 접하는등 사용자와 상호작용 하기 위해서는 동적인 웹 페이지가 필요하기에 이를 DOM이라는 인터페이스를 이용하여 자바스크립트로 제어하는 것이다.

**동적으로 웹페이지를 변경하기 위한 유일한 방법은 메모라 상에 존재하는 DOM을 변경하는 것이고 이는 Javascript언어와 함께 DOM API (getEkenebtById(), getElmentByName()....)과 같은 DOM API로 제어할 수 있다.**
  
<br><br>

#### **Body 요소 가장 아래에 script를 위치시키는 이유**

html 문서를 작성하는 경우 script를 body의 맨 아래에 위치시키는 경우가 많다.

**왜 맨 아래에 위치시키는 걸까?**

**이유는 브라우저와 연관이 있다고 한다.**

브라우저는 **동기적**으로 HTML, CSS, Javascript를 처리한다. 이것은 script 태그의 위치에 따라 블로킹이 발생하여 **DOM 생성이 지연될 수 있음을 의미**한다.
  
<br><br>

> **여기서 블로킹이란**  
> 자바스크립트는 랜더링 엔진이 아닌 자바스크립트 엔진에서 처리한다.   
> 랜더링 엔진의 HTML 파서는 script 태그를 만날 경우 자바스크립트 코드를 실행하기 위해서 DOM 생성 프로세스를 중지하고 자바스크립트 엔진으로 권한을 위임한다.  
>   
> 그 후, 자바스크립트 실행이 완료되면 다시 HTML 파서로 제어 권한을 위임하여 중지되었던 시점부터  
> DOM 생성 프로세스를 재개한다.  
>   
> **이처럼 DOM 생성 프로세스 진행 중 중지 및 재개하는 것을 블로킹이라고 표현한다.**
  
<br><br>

**이처럼 script를 맨 아래에 위치하게 할 경우**

HTML 요소들이 script 로딩 지연으로 인해 랜더링에 지장 받는 일이 발생하지 않아 페이지 로딩 시간이 단축되며

DOM이 완성되지 않은 상태에서 자바스크립트가 DOM을 조작한다면 에러가 발생하는 경우를 막을 수 있다.

---
  
<br>

**추가로**, React와 Vus.JS 등의 라이브러리 or 프레임워크 들은 **Virtual DOM의 개념**을 사용하는데

SPA는 모든 리소스들이 애플리케이션 실행 동안 한번 로드가 되는데 특정 이벤트가 발생했을 경우 페이지를 다시 랜더링 하는 작업을 모두 프런트 단에서 하기 때문에 수시로 DOM을 변경해야 하는 경우가 생긴다.

이때 이러한 요청이 많아 질수록 브라우저에 부하가 걸리기 때문에 이를 Virtual DOM이라는 개념을 사용해 먼저 변경 요청을 인지하여 Virtual DOM에 선 반영을 한다 이때 화면을 그리지는 않으며 요청된 모든 DOM 변경 요청 반영한 후, Virtual DOM과 DOM을 서로 비교한 후 최소한의 내용만 반영하여 브라우저의 부하를 덜어주는 역할을 한다.

추후 React의 가상 DOM 또한 알아보도록 하자.

---

**출처 및 참고 :**

**https://www.youtube.com/watch?v=1ojA5mLWts8**
  
**https://poiemaweb.com/js-dom**
  
**https://poiemaweb.com/js-browser**
  
**https://d2.naver.com/helloworld/59361**
  
**https://opentutorials.org/course/1375/6655**