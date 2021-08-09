## 1) HTTP 요청 메서드 정의

클라이언트가 웹서버에게 요청하는 목적/종류를 알리는 수단
HTTP 요청 메시지의 첫째 줄, 맨 앞에 위치

- HTTP 요청 메시지?
  서버와 클라이언트 간의 데이터를 교환하는 방식
  - 종류
    요청(request): 클라이언트 -> 서버
    응답(response): 서버 -> 클라이언트
  - 형식
    ![](https://images.velog.io/images/94chl/post/c1f697b1-42f5-4e0d-a4aa-565cb6ada0b3/image.png)
    ![](https://images.velog.io/images/94chl/post/676c352b-57ff-4e28-9d41-6bd9b566db81/image.png)

</br>

## 2) HTTP 요청 메서드 종류

### - GET

식별자를 갖고 있으며, 이를 통해 특정 리소스의 표시를 요청(읽기)

특징

- 요청 메시지의 body가 있을 수도 없을 수도 있음
  > 요청 메시지의 body 有? 無?
  > HTTP 요청 메서드 상 GET은 body가 필요없으나, 이는 서버에 따라 결과가 다르다.
  > 관련 포스트:
  > https://if1live.github.io/posts/http-get-request-with-body-and-http-library/
- 보안성 X: 요청 시 URI에 데이터 저장
  </br>

### - POST

웹 서버에 데이터를 전송(생성)

특징

- 보안성 O: 요청 시 body에 데이터 저장
- 안전성 X:웹 서버의 리소스에 영향을 끼침  
  </br>

### - PUT

식별자 정보를 갖고 있으며, 이를 통해 목적 리소스를 HTTP 요청 메시지의 정보로 교체(수정)

특징

- 멱등성(Idempotent) : 식별자를 갖고 있기에, 요청 실패 등으로 웹 서버에 여러 번 요청해도 결과는 늘 동일
  > vs POST
  > POST는 식별자가 없다. 즉, 멱등성이 보장되지 않기에, 동일한 내용의 요청이 다수 이루어지면, 중복되어 반영되는 결과가 나타난다.
  > </br>

### - DELETE

식별자 정보를 갖고 있으며, 이를 통해 목적 리소스를 삭제(삭제)
</br>

### - HEAD

응답 메시지의 header 정보만을 요청
body 정보가 없기에 GET 보다 빠름
</br>

### - TRACE

요청 리소스가 수신되는 경로 표시
목적 리소스의 경로를 따라서 자신에게 메시지를 반환(loop-back)하는 테스트(디버깅)
</br>

### - OPTIONS

웹 서버측 제공 가능한 메서드 옵션에 대한 질의
목적 리소스의 통신을 설정

> Request
>
> ```
> OPTIONS /example.html HTTP/1.1
> ```
>
> RESPONSE
>
> ```
> HTTP/1.1 204 No Content
> Allow: OPTIONS, GET, HEAD, POST
> ...
> ```
>
> </br>

### - PATCH

리소스의 일부분을 수정

> vs PUT

- 부분 수정: 수정하지 않는 부분은 입력 불필요
- 멱등성 X: 함수 등을 통한 동기적인 수정을 진행할 경우, 요청이 많으면 결과값이 달라짐
- 호환성 X
  </br>

### - CONNECT

요청한 목적 리소스에 대해 양방향 연결을 시작하는 메서드
클라이언트는 원하는 목적지와 TCP 연결을 HTTP 프록시 서버에 요청
-> 서버는 클라이언트를 대신하여 연결을 진행
-> 한 번 서버에 의해 연결이 수립되면, 프록시 서버는 클라이언트에 오가는 TCP 스트림을 계속해서 프록시한다.
</br>
</br>

## HTTP 요청 메서드 특징 요약

![](https://images.velog.io/images/94chl/post/0fe0ce8e-a952-4326-a3fe-386ed351b4ea/image.png)

</br>
</br>

## Q&A

### TRACE의 호환성이 ?인 이유

...모르겠다.
PATCH 메서드의 경우에는 확실하게 구버전 브라우저 지원불가능, 일부 브라우저 지원 불가능 등의 레퍼런스를 찾을 수 있었지만, TRACE만큼은 아무런 자료를 찾을 수 없었다.
그저 MDN에서 자료를 긁어왔던 폐해다.
나름 추측을 해보자면, TRACE는 디버깅을 위해 자기자신에게 메세지를 요청하고 응답하는 형태이기에, 호환성을 걱정할 필요가 없는게 아닐까? 호환성이 발신자와 수신자간의 차이가 있을 때 발생하는 것이니까...

### 멱등성이란 무엇인가?

설명 능력의 부족을 느꼈다. 발표를 준비할 때도 헷갈려서 여러번 찾아봤던 부분인데, 발표를 시작하자 어렵게 내렸던 결론은 뒤로하고, 헤메던 부분을 잘못 설명했다.

**멱등성**이란 **여러번의 요청에도 항상 동일한 응답** 결과를 내야한다.

### PATCH와 PUT의 차이는?

PATCH와 PUT의 차이는 멱등성의 차이인데, PATCH는 멱등성을 보장하지 않고, PUT은 멱등성을 보장한다.

PATCH는 요청 형식이 자유롭다. 서버 리소스를 교체하는 것 뿐만이 아니라, 리소스에 값을 더하는 등의 조작을 할 수 있다. 이 경우, 여러번의 리소스에 값을 더하는 요청이 보내지면, 결과적으로 여러번의 덧셈이 행해지는 것이다.

> 참고: https://oen-blog.tistory.com/211

반면, PUT은 리소스를 동적으로 조작할 수 없기에, 항상 똑같은 결과를 내는 것이다.

**+추가) PATCH는 멱등성을 보장할 수도 있다.**
이는 PATCH를 PUT처럼 리소스의 값을 대체하는 형식으로 사용했을 때이다. 하지만 앞서 말했듯 값을 조작하는 형식으로도 쓰일 수 있기 때문에, 멱등성을 보장한다고 확신할 수는 없다.

결론적으로, 리소스를 수정한다는 기본적인 틀은 동일하나,
**PATCH는 데이터를 조작 및 교체**하고,
**PUT은 데이터를 교체만** 한다고 말 할 수 있다.

참고자료
https://developer.mozilla.org/ko/docs/Web/HTTP/Methods
https://giju.gitbook.io/rfc7231/4.request-methods
https://pro-self-studier.tistory.com/126
https://velog.io/@rosewwross/Http-and-Request-and-Response-hok6exbnfb
