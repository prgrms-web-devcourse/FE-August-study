# HTTP - Client & Server Communication


**발표의 범위와 목표**
http와 http**s**, REST API, HTTP0.9, HTTP1.0, HTTP/2, HTTP/3 초안(2020년 10월 기준)...
그러나 HTTP 시맨틱스는 이 버전들 모두 동일합니다: 동일 요청 메소드, 상태 코드, 메시지 필드가 일반적으로 모든 버전에 적용됩니다.

⇒ 따라서 이번 발표는 `CORE HTTP`에 초점을 맞춰 진행합니다.

<br>

## 웹의 네가지 핵심 요소

1) **html** : 웹페이지를 만드는 컴퓨터 언어

2) **url** : 원하는 웹페이지에 방문을 도와주는 주소 체계

3) **web browser, web server** : 웹페이지를 주고받는 소프트웨어

4) **HTTP(HyperText Transfer Protocol)** : 웹브라우저와 웹서버가 통신할 때 사용하는 통신 규칙
  -> 통신? 컨텐츠(html, 이미지, 오디오, css, javascript 파일 등)을 주고 받는 것

<br>

## 1. HTTP(**Hypertext Transfer Protocol)란?**

- 인터넷 세계에서 **클라이언트**(웹브라우저를 포함한 사용자를 대신하여 동작하는 모든 도구)**와 서버**(웹서버)**도 정해진 형식에 따라 통신을 주고 받습니다.** 그것이 바로 **Hypertext Transfer Protocol(HTTP)**입니다.

- 그런데 실제로는 브라우저와 서버 사이에는 좀 더 많은 컴퓨터와 머신이 HTTP 메시지를 이어 받고 전달합니다. 여러 계층으로 이루어진 웹 스택 구조에서 이러한 컴퓨터/머신들은 대부분은 전송, 네트워크, 물리 계층에서 동작하며, 성능에 상당히 큰 영향을 주지만 HTTP 계층에서는 이들이 어떻게 동작하는지 눈에 보이지 않습니다.

- HTTP기반 시스템의 구성요소
![구성요소](https://mdn.mozillademos.org/files/13679/Client-server-chain.png)

- [프록시](https://developer.mozilla.org/ko/docs/Web/HTTP/Overview#%ED%94%84%EB%A1%9D%EC%8B%9C)가 궁금하다면?

    웹 브라우저와 서버 사이에서는 수많은 컴퓨터와 머신이 HTTP 메시지를 이어 받고 전달합니다. 이 중에서도 애플리케이션 계층에서 동작하는 것들을 일반적으로 프록시라고 부릅니다. 프록시는 눈에 보이거나 그렇지 않을 수도 있으며(프록시를 통해 요청이 변경되거나 변경되지 않는 경우를 말함) 다양한 기능들을 수행할 수 있습니다.

    - 캐싱 (캐시는 공개 또는 비공개가 될 수 있습니다 (예: 브라우저 캐시))
    - 필터링 (바이러스 백신 스캔, 유해 컨텐츠 차단(자녀 보호) 기능)
    - 로드 밸런싱 (여러 서버들이 서로 다른 요청을 처리하도록 허용)
    - 인증 (다양한 리소스에 대한 접근 제어)
    - 로깅 (이력 정보를 저장)

<br>

## 2. HTTP 구성

- HTTP 통신의 종류
    1. **HTTP 요청 (HTTP Request) :** 클라이언트가 서버에게 주문을 넣을 때 사용하는 규약(Protocol)에 맞게 쓴 글(Hypertext)
    2. **HTTP 응답 (HTTP Response) :** 주문에 따라 서버가 클라이언트에게 답변할 때 사용하는 규약(Protocol)에 맞게 쓴 글(Hypertext)

    - 먼저 요청이 들어와야 응답이 발생한다.
    - 요청 하나에 응답 하나인 1:1 매칭으로 발생한다.

- HTTP 통신의 내용
    - **제목**은 말 그대로, 가장 핵심적인 요약 사항
    - **Header**는 해당 통신에 대한 설정 및 부가 정보를 포함
    - **Body**는 해당 통신과 함께 보내야 하는 자료를 보통 나타냅니다. Body는 존재하지 않는 경우도 있습니다.

<br>

## 3. HTTP Request

HTTP 메시지들은 사람이 읽고 이해할 수 있어, 테스트하기 쉽고 초심자의 진입장벽을 낮췄습니다.

![HTTP Request](https://mdn.mozillademos.org/files/13821/HTTP_Request_Headers2.png)

### 1) 위 HTTP 요청의 제목 `GET /doc/test.html HTTP/1.1`

- GET은 **[요청 메소드](https://developer.mozilla.org/ko/docs/Web/HTTP/Methods)**를 의미합니다.

    HTTP 요청은 주로 GET, POST, PUT, DELETE 등의 요청 메소드가 사용됩니다.클라이언트는 각 요청의 종류에 적합한 요청메소드를 사용합니다.
    - GET : html파일 등 어떠한 **정보 제공** 요청
    - POST : **저장, 생성** 등의 작성하는 작업
    - PUT : 기존에 있는 것을 **수정**
    - DELETE : **삭제** 작업

- `/doc/test.html` 은 해당 서버의 어떤 URL 주소로 요청을 보내는지를 나타냅니다.
- `HTTP/1.1` 은 현재 사용하는 HTTP 규약의 버전을 의미합니다.

### 2) 위 HTTP 요청의 header

- Host : 주문을 받는 쪽의 호스트 정보 (어떤 서버도메인으로 보내는지)
- Accept... : 클라이언트가 받을 수 있는 것들의 정보 즉, 클라이언트에 대한 정보
- Content-Length : body의 길이 (HTTP 요청이 여러개가 동시에 왔을 때, 어디까지가 한 요청의 범위인지를 알기 위한 용도)

<br>

## 4. HTTP Response

![HTTP Response](https://mdn.mozillademos.org/files/13823/HTTP_Response_Headers2.png)

### 1) 위 HTTP 응답의 제목 `HTTP/1.1 200 OK`

- `HTTP/1.1` 은 현재 사용하는 *HTTP 규약의 버전*을 의미합니다.
- `200` 은 현재 발송되는 **[HTTP 응답의 상태 코드](https://developer.mozilla.org/ko/docs/Web/HTTP/Status)**를 나타냅니다. 성공했는지, 실패했는지 등에 대한 여부를 판단할 수 있는 코드입니다.
    - 100대 : 정보응답
    - 200대 : 성공응답
    - 300대 : 리다이렉션 메시지(요청한 자원이 새 주소나 임시 주소에 있을 경우)
    - 400대 : 클라이언트 에러 응답(주문을 잘못 넣음. 404: 요청한 자원이 서버에 없다)
    - 500대 : 서버 에러 응답
- `OK` 는 해당 응답 상태 코드에 대한 요약어입니다.

<br>

## 5. 개발자 도구의 Network 탭

- 메뉴에서 HTTP 요청을 살펴볼 수 있습니다.

    ```jsx
    console.log("Check console or network tab in your dev tool.");

    /*
      fetch는 HTTP 요청을 보낼 수 있는 함수입니다.
     */

    fetch("https://coding.surge.sh/quiz/100.json")
      .then((res) => {
        console.log("Response ▶︎▶︎▶︎", res);
      });
    ```

- `fetch()`함수
    - 원격 API를 간편하게 호출할 수 있도록 브라우저에서 제공하는 함수
    - 첫번째 인자로 URL, 두번째 인자로 옵션 객체를 받고, Promise 타입의 객체를 반환합니다. 반환된 객체는, API 호출이 성공했을 경우에는 응답(response) 객체를 `resolve`하고, 실패했을 경우에는 예외(error) 객체를 `reject`한다.

        ```jsx
        fetch(url, options)
          .then((response) => console.log("response:", response))
          .catch((error) => console.log("error:", error))
        ```

    - `fetch()` 함수는 디폴트로 GET 방식으로 작동하고 GET 방식은 요청 전문을 받지 않기 때문에 옵션 인자가 필요가 없습니다.

<br>

## 6. HTTP Secure

HTTP protocol의 **암호화된 버전**입니다. 즉, 쉽게 말해 보안 기능을 추가한 것이라고 할 수 있습니다.

HTTP는 암호화되지 않았기 때문에 도난이나 변조, 도청이 가능합니다.

HTTPS는 소켓 통신에서 일반 텍스트를 이용하는 대신에, SSL(보안 소켓 계층)이나 TLS 프로토콜을 통해 세션 데이터를 암호화합니다.

<br>

## 7. HTTP 버전

### 1) **HTTP/2**

**HTTP 프로토콜의 두 번째 버전입니다. ▶▶▶** [구글 개발자 페이지 - HTTP/2소개](https://developers.google.com/web/fundamentals/performance/http2/)

- HTTP/2는 HTTP의 애플리케이션 의미 체계를 어떤 식으로도 수정하지 않습니다. 모든 핵심 개념(예: HTTP 메서드, 상태 코드, 헤더 필드 등)은 그대로 유지됩니다.
- 그 대신 HTTP/2는 클라이언트와 서버 간에 데이터 서식(프레임)이 지정되는 방식과 데이터가 전송되는 방식을 수정합니다.
- 성능이 개선, 요청 우선순위 지정, 흐름 제어, 서버 푸시와 같은 새로운 기능들이 제공
    - ***바이너리 프레이밍 계층*** (HTTP 메시지가 캡슐화되어 클라이언트와 서버 사이에 전송)

        ![binaryFramingLayer](https://developers.google.com/web/fundamentals/performance/http2/images/binary_framing_layer01.svg)

    - **Multiplexed Streams** - streams라는 개념 도입. 한 커넥션으로 동시에 여러개의 메세지를 주고 받을 있으며, 응답은 순서에 상관없이 stream으로 주고 받는다.

        ![multiplexedStreams](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRjS4zRzzmSyqe7azR7fo2S01y0lOiUnAPRFw&usqp=CAU)

### 2) HTTP/3

**HTTP 프로토콜의 세 번째 버전입니다.**

기존의 HTTP/1, HTTP/2와는 다르게 UDP 기반의 프로토콜인 QUIC 을 사용하여 통신하는 프로토콜입니다.

참고자료

- [HTTP/3는 왜 UDP를 선택한 것일까?](https://evan-moon.github.io/2019/10/08/what-is-http3/)
- [HTTP/3: the past, the present, and the future](https://blog.cloudflare.com/http3-the-past-present-and-future/)