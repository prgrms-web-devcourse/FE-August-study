![image](https://user-images.githubusercontent.com/57757719/128895854-c88b9cfb-5af6-43bb-ac52-ef330e225e4d.png)

<br><br>

## HTTP

- **HyperText Transfer Protocol**의 약자로 하이퍼텍스트 문서를 교환하기 위하여 사용된 통신 규약

- 모든 프로그램이 이 규약에 맞춰 개발해서 서로 정보를 교환할 수 있게 된 것이다.

- **HTTP는 웹에서만 사용하는 프로토콜**로 **TCP/IP 기반으로 클라이언트와 서버 사이에 요청과 응답을 전송**합니다.

---

### 특징

- **HTTP 메시지**는 클라이언트와 서버에서 각자 해석이 이루어지며 메시지의 종류는 <b>요청(request)</b>과 <b>응답(response)</b>이 존재합니다.

- **요청**은 **클라이언트가 서버로 전달하는 메시지**이고, **응답**은 **서버가 클라이언트의 요청에 대해 답변하는 메시지**입니다.

- **TCP/IP**를 이용하는 **응용 계층 프로토콜**입니다. 사용자는 보통 TCP/IP에 직접 접속하지 않고 응용 계층의 프로그램들을 통하여 서비스를 사용합니다. 흔히 브라우저가 응용 계층의 프로그램이고 통신 서비스를 사용할 수 있게 해줍니다.

- HTTP는 연결 상태를 유지하지 않는 **비연결성 프로토콜**입니다.

  > 비연결성(Connectionless) ? <br><br>
  > 비연결성은 클라이언트와 서버가 한 번 연결을 맺은 후, 클라이언트 요청에 대해 서버가 응답을 마치면 맺었던 연결을 끊어 버리는 성질을 말한다. 이로인해 요청/응답 방식으로 동작한다.

- HTTP는 인터넷 상에서 **불특정 다수의 통신 환경을 기반으로 설계**되었습니다. <br><br> 만약 서버에서 다수의 클라이언트와 연결을 계속 유지해야 한다면, 이에 따른 **많은 리소스가 발생**하게 됩니다. <br><br>따라서 **연결을 유지하기 위한 리소스를 줄이면 더 많은 연결**을 할 수 있으므로 비연결적인 특징을 갖는다는 장점이 있습니다. <br><br>하지만 **서버는 클라이언트를 기억하고 있지 않으므로** 동일한 클라이언트의 **모든 요청에 대해, 매번 새로운 연결을 시도/해제의 과정**을 거쳐야하므로 **연결/해제에 대한 부하가 발생한다**는 단점이 있습니다. <br><br>이러한 단점을 해결하기 위해 쿠키, 세션, 웹 스토리지 등의 등장하게 되었습니다.

---

### Request (요청)

요청은 **클라이언트가 서버에게 보내는 메시지**라고 하였는데, 요청에 대하여 좀 더 자세히 알아보자(**Http/1.1 기준**).

#### Request Method (요청의 종류)

- GET : 서버에게 데이터를 **요청**할 때 사용

- POST : 서버에게 데이터의 **생성을 요청**할 때 사용
- PUT : 서버에게 데이터의 **수정을 요청**할 때 사용
- DELETE : 서버에게 데이터의 **삭제를 요청**할 때 사용

#### Request 메시지

```
GET https://velog.io/@codenmh0822 HTTP/1.1	 // 시작줄
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) ...  // 헤더
Upgrade-Insecure-Requests: 1 // 헤더

(본문 없음) // GET 요청
```

**1. 시작줄**

첫 라인은 시작줄로 현재 요청 메서드 종류와 주소 그리고 http 버전으로 나타내어진다.

```
GET https://velog.io/@codenmh0822 HTTP/1.1
```

**2. 헤더**

두 번째 라인 부터는 요청 메시지의 헤더에 해당하며 요청에 대한 다양한 정보들이 담겨 있다.

```
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) ...  // 헤더
Upgrade-Insecure-Requests: 1 // 헤더
```

**3. 본문**

헤더로부터 한 라인의 뛰어진 다음 나오는 정보가 본문에 해당된다.

본문은 요청을 할 때 함께 보낼 데이터를 담는 부분이며, 위 예시로는 단순 주소로 요청을 보내고 있기에 함께 보낼 데이터가 존재하지 않아 본문이 비어있다.

---

### Response (응답)

응답은 **서버가 클라이언트에게 보내는 메시지**라고 하였는데, 응답에 대하여 좀 더 자세히 알아보자(**Http/1.1 기준**).

```
HTTP/1.1 200 OK						// 시작줄
Connection: keep-alive		        // 헤더
Content-Encoding: gzip
Content-Length: 35653
Content-Type: text/html;

<!DOCTYPE html><html lang="ko" data-reactroot=""><head><title...
```

**1. 시작줄**

첫 줄은 버전과 상태코드 그리고 상태메시지로 구성되어 있다.

**2. 헤더**

두 번째 줄부터는 헤더로 응답에 대한 정보를 담고 있다.

**3. 본문**

응답에는 대부분의 경우 본문이 있다. 보통 데이터를 요청하고 응답 메시지에는 요청한 데이터를 담아서 보내주기 때문이다. 응답 메시지에 HTML이 담겨 있는데 이 HTML을 받아 브라우저가 화면에 렌더링한다.

#### Response Status Code (응답 상태 코드)

- Client에서 Server에 HTTP request를 보내면, Server는 **HTTP 응답 상태 코드(HTTP response status code)를 이용해 요청이 성공적으로 완료되었는지를 알려준다.**

- 상태 코드에는 굉장히 많은 종류가 있다. 모두 숫자 세 자리로 이루어져 있으며, 아래와 같이 크게 다섯 부류로 나눌 수 있다.

1. **1XX (조건부 응답)** : 요청을 받았으며 작업을 계속한다.
2. **2XX (성공)** : 클라이언트가 요청한 동작을 수신하여 이해했고 승낙했으며 성공적으로 처리했음을 가리킨다.
3. **3XX (리다이렉션 완료)** : 클라이언트는 요청을 마치기 위해 추가 동작을 취해야 한다.
4. **4XX (요청 오류)** : 클라이언트에 오류가 있음을 나타낸다.
5. **5XX (서버 오류)** : 서버가 유효한 요청을 명백하게 수행하지 못했음을 나타낸다.

<br><br>

#### 참고자료

- https://velog.io/@sejong202/HTTP%EB%8A%94-%EB%AC%B4%EC%97%87%EC%9D%BC%EA%B9%8C%EC%9A%94

- https://www.zerocho.com/category/HTTP/post/5b344f3af94472001b17f2da

- https://developer.mozilla.org/ko/docs/Web/HTTP/Overview

- https://medium.com/@lunay0ung/protocol-http%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%BC%EA%B9%8C-84a896c5fc93

- https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=adh3305&logNo=220970960532

- https://developer.mozilla.org/ko/docs/Web/HTTP/Messages

- https://reakwon.tistory.com/68

- https://victorydntmd.tistory.com/286
