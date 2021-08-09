
# 🤔웹 스토리지 (Web Storage)가 뭐야?

기본적인 웹 구성은 **Client-Server**로 나뉘게 되며 서버단에는 **Database**가 존재한다.

보통 정보 저장은 Database에 하게 된다.

그러나 일부 정보를 상태 유지 하기 위해서 **Client 단에 저장 할 수 있는 기능**을 만들었고,

이러한 기술을 **Web Storage**라고 한다.

---

## 😒 Web Storage은 왜 생겼을까?

생긴 이유는 **간단**하다.

> 1. 일부 정보를 상태 유지를 하여 **Server**와의 불필요한 통신 감소.
2. **Session**의 Server 데이터 저장 문제점 극복
3. 기존 클라이언트에 저장하는 **Cookie의 문제점**을 극복

먼저, 간단하게 **Cookie와 Session**에 대해 알아보자.


---


### 😐Cookie는 왜 쓸까?
![](https://images.velog.io/images/minsgy/post/4dff44ca-1eea-4811-953f-a631ca7ce654/image.png)
> **로그인 상태, 장바구니** 등 클라이언트가 
정보를 유지하는 Stateful한 성격의 서비스 확대로, **클라이언트 단에 정보를 저장**하기 위해 도입.

```js
// Server - Cookie 방식의 로그인 구현 
if (request.method === "GET" && request.url === "/login") {
      // 키는 login 값은 으로 설정
      if (!request.headers.cookie) {
        // Request 헤더에 쿠키 정보가 없는 경우
        response.writeHead(200, {
          "Set-Cookie": "login=value",
        });
      }
    } 
```

그리고 여러 가지 **단점**들이 존재했다.

1. 쿠키에 대한 정보를 HTTP Header**(Set-Cookie)**에 매번 추가해서 보내서 **상당한 트래픽 발생**으로 성능이 다운 됨.

2. 쿠키를 스크립트를 통한 해킹 시, **보안에 대한 위험 발생함.**

---

이러한 **트래픽**과 **쿠키 변경 보안적 이슈 해결**을 위해서 나온 것이 **Session** 이다.

### 🤪Session은 왜 쓸까?
![](https://images.velog.io/images/minsgy/post/5924fc6c-9728-445d-94c4-eff62b825073/image.png)

> Cookie의 여러 문제를 보완했고, 
**추가적으로 사용자 인증**까지 사용 할 수 있다.

- **일시적인 데이터를 저장 할 때 사용**한다. 브라우저 종료 시, 삭제된다. 
>요즘 홈페이지 방식(장바구니, 자동 로그인 등 여러 서비스 추가)으로는 용도에 ~~**알맞지 않음.**~~

- **관리자/사용자 여부 판단**으로 맞는 서비스 제공한다.
> **Client-Server** 인증에서는 **좀 더 범용적인 Token 방식**을 사용하게 됨.

- 그러나, 세션은 사용자수만큼 **서버 메모리를 차지하게 되는 문제**가 발생했고, 
사용자 수만큼 **데이터 용량 차지하는 결과**가 나타났다.
> 그래서 요즘은 **Token 기반의 인증 방식**을 쓴다는 점!

그럼 이번에는 문제점을 보완해서 사용가능한 **WebStorage**를 살펴보자.

---


## 💻WebStorage

![](https://images.velog.io/images/minsgy/post/62ebebf5-0912-49cf-b419-1040f9620238/image.png)

WebStorage를 다루는 **WebStorage API**는 **서버가 아닌**,
**클라이언트에 데이터를 저장**할 수 있도록 지원하는 기능이다.

기본적으로 **약 5MB까지 저장 공간을 사용**할 수 있고, 최신 버전에 따라 계속 변경 된다고 한다.
![](https://images.velog.io/images/minsgy/post/fddfb833-5d23-4b28-a141-973b33d506e8/image.png)
> 현재도 그렇다고 한다. 참고로 ~~**Cookie는 4KB**~~

### 그렇다면 왜 사용할까?

쿠키와 비교해보자.

**1. 데이터 저장 방식 차이**

- 쿠키 - `String`으로 `Value`를 다룸. 
 `ex) Cookie:name=minseok` 

- WebStorage - `key-value`방식으로 데이터를 다룸. 
`ex) localstorage['name'] = minseok`
> `String`으로 데이터를 다룬다는 자체가 안정적이지 않다.

**2. 한정적인 데이터 저장 공간
**
- 기본적인 데이터 저장 공간이 Cookie(~~**4KB**~~), **WebStorage(5MB)**
- 쿠키의 경우 너무 한정적인 저장 공간.

**3. 쿠키는 모든 HTTP Request가 포함되어 성능에 영향을 줌.**
- Cookie는 서버와 계속 통신을 하면서 성능에 영향을 준다.
- 그렇지만 **WebStorage**는 서버와 통신을 아예 하지 않음!

또한 세션도 비슷한 상황으로 **서버에 데이터 저장 과부하 문제**가 있다.


그래서 우리가 **왜? WebStorage를 사용**하는지 알 수 있었다.

그러면 **이유**를 알았으니 실제로 사용해보자!
![](https://images.velog.io/images/minsgy/post/dbcbf588-3ea0-445c-9945-6e32d41c2988/image.png)

---

### ⚡ WebStorage 사용 사례

- **LocalStorage**를 활용한 **자동 로그인** 기능
> 실제론 이렇게 쓰면 안된다고 한다..


- 웹 브라우저 **첨부 파일 - JSON 데이터로 변환 후 저장하기**
  
  - 웹에서 파일 변환 및 업로드 과정에서 실제로 많이 사용하고 있는 용도.
  - 서버에 저장하게되면 내용을 읽을 때마다 서버에 접속해야하니 부하가 생김.
  - >![img](https://blog.kakaocdn.net/dn/R3tql/btqvkxVP7VI/bKfwMLneB5EhHUkllrGcOK/img.png)
  
그런데 **WebStorage**에 대해 공부하면서 새로운 글을 봤다.

---

### 😥 WebStorage를 쓰지말라고?


[Chrome 개발자](https://web.dev/storage-for-the-web/)가 명세한 글로, 요즘은 잘 사용하지 않는 기능이라고 한다.

**사용하지 말라는 이유는?**
> 오프라인에서도 동작하고 안정적인 서비스를 위해서이다. 
**인터넷이 끊겨도 사용자 경험이 중요하기 때문에..**

최근에는 PWA(Progressive WebApp) 등, 
기술을 통한 **사용자 경험 성능**을 개선하고 있다.

그러면 이를 대체할 Resource 저장법은 무엇이 있을까?

### 😏 WebStorage 대체하기!

대표적으로 2가지 방법이 있다.

1. **Service Worker**가 제공하는 Cache Storage API를 활용한다.
2. **IndexedDB**를 활용하자. <- 이 내용은 **Day2** 진행 할 때 언급한 내용..

최근 브라우저는 두 가지 방법 다 지원한다고 하여 많이 사용하고 있다고 한다.

다음 포스팅은 **WebStorage를 대체하는 스토리지**에 대해서 조사해보겠다.

---

## QnA
Q. LocalStorage를 활용해 자동 로그인을 구현하면 안되는 이유는?
> A. LocalStorage는 Javascript를 활용해 조작의 위험이 있어서 보안적으로 위험하기 때문에 로그인 정보로 사용하지 않습니다.  

Q. LocalStorage가 어디 저장되어있고, 삭제되는 건 어떻게 이루어지나요?
> A. LocalStorage는 사용자 별 어플리케이션 데이터 저장소에 들어가있으며, WebStorage API인 remove() 메소드를 사용하여 로컬 자원을 삭제할 수 있습니다.

---


## Reference 

- [WebStorage - MDN](https://developer.mozilla.org/ko/docs/Web/API/Web_Storage_API) 
- [Google Chrome 개발팀](https://web.dev/storage-for-the-web)
