## **함수 실행 컨텍스트**

- 함수를 호출하면 함수 코드가 평가되어 함수 실행 컨텍스트가 생성된다.
- 생성된 함수 실행 컨텍스트는 '실행 컨텍스트 스택(콜 스택)'에 푸시되고 함수 코드가 실행된다.
- 함수 코드의 실행이 종료되면 함수 실행 컨텍스트는 콜 스택에 팝 되어 제거된다.

<p  align="center">
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FZwqnE%2Fbtrb6sMLDmm%2FBcfzSBuCrFkQMZHXEsepFK%2Fimg.png"/>
</p>
<p  align="center">
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FCu7X6%2FbtrcebJDkWD%2FOuq5XyUjJvYMAjAaAQN1f0%2Fimg.png"/>
</p>

---

<br>
<br>

## **동기 처리**

- JavaScript 엔진은 단 하나의 실행 컨텍스트(콜 스택)를 가진다.
- 즉, 2개 이상의 함수를 동시에 실행할 수 없다.
- 한 번에 하나의 태스크를 실행하기 때문에 JavaScript 엔진은 싱글 스레드 방식으로 동작한다.  
  \-> 블로킹(작업 중단)이 발생한다.
- 태스크가 순서대로 처리되기 때문에 실행 순서가 보장된다.

<p  align="center">
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbaxRKY%2Fbtrb8hjMUeo%2F2ykzEmAf1G3o9Th16EKA31%2Fimg.png"/>
</p>

![](https://drive.google.com/uc?export=view&id=1DSi2_5KyFUhijPfTHetvgGMSq3WfKzmU)

---

## **비동기처리**

- 현재 실행 중인 태스크가 종료되지 않은 상태라 해도 다음 태스크를 바로 실행하는 방식.
- 실행 순서가 보장되지 않는다.
- 전통적으로 콜백 패턴을 사용해 비동기 처리를 수행한다.
- 콜백 패턴은 콜백 헬(Callback Hell)이 발생한다.

<p  align="center">
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcL4OZQ%2FbtrcebQpHNm%2F9BLZWOOkQdU0Rg1j5AtALk%2Fimg.png"/>
</p>

![](https://drive.google.com/uc?export=view&id=1j2gSPyLLKMu7QOIodYCBg8Uz3kf4Aqb1)

<br>
<br>

---

<br>
<br>

## 콜백 헬(Callback Hell)

- 비동기 처리를 위해 콜백 함수를 계속해서 전달한다.
- 이러한 방식은 가독성이 좋지 않으며, 발생하는 예외를 처리하기 힘들다.
- '어디서 에러가 발생하는지? 발생하는 에러마다 예외를 하나하나 만들어서 처리해야 하나?' 와 같은 의문점과 문제점이 발생해 처리하기 어렵다.
- 해결법은 “promise”! => ES6에 등장한 Promise로 콜백 헬을 해결할 수 있다.(콜백 헬은 다음 스터디 때 진행)

<p  align="center">
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcoUPbG%2Fbtrb7yzkbMp%2FLVIef1KhLEu68SjqJvYh01%2Fimg.png"/>
</p>

<br>
<br>

---

<br>
<br>

## **이벤트 루프와 태스크 큐**

- JavaScript의 동시성을 지원하는 이벤트 루프가 브라우저에 내장되어 있다.
- 비동기 처리에서 소스코드의 평가와 실행을 제외한 모든 처리는 JS 엔진을 구동하는 환경인 브라우저 또는  
  Node.js가 담당한다.

<p  align="center">
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F0TGD7%2Fbtrb8UO1Nap%2FCqJEhCaB3cBb8jM7kne1S0%2Fimg.png"/>
</p>

<br>

## **힙**

- 객체가 저장되는 메모리 공간.
- 객체는 원시타입이 아니기 때문에 할당할 때 메모리 공간 크기는 런타임에 결정(동적 할당) 한다.

## **태스크 큐(Task Queue)**

- setTimeout이나 setInterval과 같은 비동기 함수의 콜백 함수 또는 이벤트 핸들러가 일시적으로 보관되는 영역
- 태스크 큐와는 별도로 프로미스의 후속 처리 메소드의 콜백 함수가 일시적을 보관되는 마이크로태스크 큐도 존재

## **이벤트 루프(Event Loop)**

- 콜 스택에 현재 실행 중인 실행 컨텍스트가 있는지, 태스크 큐에 대기 중인 함수(콜백 함수, 이벤트 핸들러 등) 가 있는지 반복해서 확인.
- 콜 스택이 비어있고 태스크 큐에 대기 중인 함수가 있다면 이벤트 루프는 순차적으로(FIFO) 태스크 큐에 대기 중인 함수를 콜 스택으로 이동시킨다.
- 콜 스택으로 이동한 함수는 즉시 실행된다.

![](https://drive.google.com/uc?export=view&id=1hA2hmY3qbdTXfhOxYVa5m32NZ75GCTSj)

---

<br>
<br>

## **결론**

- JS는 싱글 스레드 방식으로 동작한다.
- 싱글 스레드 방식으로 동작하는 것은 브라우저가 아닌 브라우저에 내장된 JS 엔진이다.
- JS 엔진은 싱글 스레드, 브라우저(또는 Node.js)는 멀티 스레드로 동작한다.
- 태스크 큐에서 대기하고 있는 컨텍스트는 콜 스택이 비어야 실행되기 때문에 설정한 딜레이 시간보다 더 늦게 호출된다.
