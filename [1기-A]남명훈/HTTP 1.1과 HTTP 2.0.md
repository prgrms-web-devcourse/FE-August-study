<p align="center">
	<img src="https://images.velog.io/images/codenmh0822/post/cc8b1d54-93df-4cab-9eaa-69d9110114cb/image.png" width="50%" height="50%"/>
</p>

<br>

HTTP는 1996년 1.0 버전으로 처음 출시되고 1999년 1.1 버전이 등장하였다. 그리고 1.1 버전은 HTTP 2.0이 등장하기까지 무려 15년 동안 지속되었다.

하지만 시간이 지남에 따라 웹에서 담아야 할 정보는 점점 늘어났고, 지금은 하나의 웹사이트에 수 많은 멀티미디어 리소스들과 비동기 요청들이 발생한다.

이런 상황에서 더 이상 HTTP 1.1으로는 네트워크 통신을 하기 힘들었고 HTTP 2.0이 등장하게 되었습니다.

우리가 네트워크 통신에 있어서 전달 받는 정보가 정확하든 안정확하든 정보를 받는 시간이 길어진다면 확인 조차 힘들 것 입니다. 

그러므로 왜 HTTP 1.1이 통신의 어려움이 있었는지, HTTP 2.0은 어떤 점이 개선되었는지에 대해 네트워크 속도에 우선순위를 두고 생각해봅시다.

---

## HTTP 1.1

<br>

<p align="center">
	<img src="https://images.velog.io/images/codenmh0822/post/dc1bf7da-0db2-49c4-b320-a9220eaf2465/image.png" width="50%" height="50%"/>
</p>

<br>

HTTP 1.0은 일반적으로 한 번의 연결에 **하나의 요청을 처리**할 수 있습니다. 그렇기 때문에 동시 전송이 불가능하고 하나의 요청에 대한 응답을 받아야 다음 요청을 전송하게 됩니다.

수 많은 멀티미디어 리소스들(css, javascript, image 등)이 있는 상황에서 이러한 특징은 **네트워크 지연**을 발생시키게 된다.

이를 위해 HTTP 1.1은 차선책으로 **HTTP Pipelining**을 도입하였습니다. 이는 TCP 안에 두 개 이상의 HTTP 요청을 담아 네트워크 지연을 줄이는 방식입니다.

<br>

#### HTTP Pipelining

<br>

<p align="center">
	<img src="https://images.velog.io/images/codenmh0822/post/c77ec89c-d7ef-4deb-b11b-5a4b7df78e87/image.png" width="50%" height="50%"/>
</p>

<br>

파이프라이닝의 가장 큰 차이점은 그림과 같이 바로 **다수의 요청들에 대한 각 응답을 기다리지 않고, 한 번에 여러개의 요청을 보내는 것**이다. 이를 통해 어느 정도의 네트워크 지연을 줄일 수 있다.

하지만 파이프라이닝 기법 역시 응답에 대한 처리를 미루는 방식이므로 각 응답의 처리는 순차적으로 처리 되어진다. 결국엔 후순위의 응답은 네트워크 지연이 발생할 수 밖에 없다. 이러한 파이프라이닝의 네트워크 지연을 **HOL Bloking**이라 부른다.

<br>

#### HOL Bloking 문제

<br>

HOL Bloking은 Pipelining의 가장 큰 문제점으로 **앞의 요청에 의해 뒤의 요청이 지연되는 것**을 의미한다. 

<br>

<p align="center">
	<img src="https://images.velog.io/images/codenmh0822/post/4e3bd9ca-3156-4ae4-8746-7f5f620c911b/image.png" width="50%" height="50%"/>
</p>

<br>

위 그림 처럼 HTTP Pipelining 을 통해 한 번에 여러 개의 이미지를 요청하는 경우를 생각해봅시다.

가장 앞에 요청한 이미지가 응답이 지연되면 두, 세번째 이미지도 지연이 발생합니다.

TCP 안에 여러 개의 HTTP 요청이 왔으므로 완료된 응답부터 보내면 되지 않을까라고 생각할 수 있지만 서버는 TCP에서 요청을 받은 순서대로 응답을 해야합니다.

<br>

#### RTT(Round Trip Time) 증가 문제

<br>

앞서 말했듯이 HTTP/1.1의 경우 일반적으로 한 번의 연결에 요청 한 개를 처리한다. 이렇다보니 매번 요청 별로 연결의 맺고 끊음이 발생하며, 이는 3-way Handshake가 반복적으로 발생함을 의미하며 불필요한 RTT 증가를 통한 네트워크 지연을 초래하여 성능을 지연시킨다.

<br>

#### 무거운 Header 구조 문제

<br>

HTTP/1.1의 헤더에는 많은 메타 정보들이 저장되어 있다. 클라이언트가 서버로 보내는 HTTP 요청은 매 요청 때마다 중복된 헤더 값을 전송하게 되며 서버의 도메인과 관련된 쿠키 정보도 헤더에 함께 포함되어 전송된다. 이러한 중복적인 헤더 정보가 중복해서 데이터로 전달되므로 네트워크 지연과 자원이 소비됩니다.

<br>

---

<br>

## HTTP 2.0

<br>

HTTP 2.0은 HTTP 1.1을 완전하게 재작성한 것이 아니라 앞서 다루었던 문제인 네트워크 지연, 자원 사용량 등 프로토콜의 성능에 초점을 맞추어 수정한 버전입니다.

<br>

#### Multiplexed Streams

<br>

<p align="center">
	<img src="https://images.velog.io/images/codenmh0822/post/15642d58-33ba-415e-bf90-e519d91bfd57/image.png" width="50%" height="50%"/>
</p>

<br>

HTTP/1.1의 Pipelining의 개선이라고 보면 된다. 하나의 연결에서 여러개의 메세지를 주고받을 수 있으며, 응답은 요청 순서에 상관없이 stream으로 주고 받아서 HOL Blocking 도 발생하지 않습니다.

위 그림 처럼, 하나의 연결에서 여러 병렬 스트림(3개)이 존재 할 수 있다. stream이 뒤섞여서 전송 될 경우, stream number를 이용해 수신측에서 재조합이 이루어지기 때문에 정확한 정보를 볼 수 있다.

<br>

#### Stream Prioritization

<br>

<p align="center">
	<img src="https://images.velog.io/images/codenmh0822/post/ea1afa97-0056-4046-bd8b-5da6403da6af/image.png" width="50%" height="50%"/>
</p>

<br>

응답에 대한 우선순위를 정해 우선순위가 높을수록 응답을 빨리 합니다.

예를 들어 하나의 HTML 문서에 CSS 파일과 여러 IMG 파일이 있다고 가정해봅시다.

만일 여러 IMG 파일을 응답하느라 CSS 파일의 응답이 느려지면 클라이언트는 렌더링을 하지 못하고 기다리게 됩니다.

따라서 CSS 파일의 우선순위를 올려 렌더링을 진행하며 IMG 파일은 도착하는 대로 띄어준다면 더 효율적입니다.

<br>

#### Server Push

<br>

<p align="center">
	<img src="https://images.velog.io/images/codenmh0822/post/062089fe-1d93-4751-858e-28d51df87509/image.png" width="50%" height="50%"/>
</p>

<br>

서버가 클라이언트의 요청없이 응답을 보내는 방법입니다.

위와 마찬가지로 하나의 HTML 문서에 CSS 파일과 여러 IMG 파일이 있다고 가정해봅시다.

기존에는 HTML 문서를 요청한 후 다시 각각의 CSS 파일과 여러 IMG 파일을 위한 요청을 보내야 했습니다.
 
하지만 Server Push 로 인해서 클라이언트의 요청을 최소화하고 서버가 HTML문서에 필요한 리소스를 알아서 보내줍니다.

<br>

#### Header Compression

<br>

HTTP 1.1의 경우 이전 요청과 중복되는 Header도 똑같이 전송하느라 네트워크 자원을 불필요하게 낭비하였습니다.

HTTP 2.0의 경우, Header Table과 Huffman Encoding(문자 빈도수를 이용해서 파일을 압축하는 과정)을 사용하는 HPACK 압축방식으로 이를 개선하였습니다.

<br>

<p align="center">
	<img src="https://images.velog.io/images/codenmh0822/post/2ffee766-2f6e-48d5-b954-244f00549ead/image.png" width="50%" height="50%"/>
</p>

<br>

클라이언트와 서버는 각각 Header Table을 관리하고 이전 요청과 동일한 필드는 table의 index만 보내고, 변경되는 값은 Huffman Encoding 후 보냄으로서 Header의 크기를 경령화 하였습니다.

<br>

<p align="center">
	<img src="https://images.velog.io/images/codenmh0822/post/a268c78a-75a4-4b42-b9ad-72383735b553/image.png" width="50%" height="50%"/>
</p>

<br>

---

<br>

## 글을 마치며

<br>

<p align="center">
	<img src="https://images.velog.io/images/codenmh0822/post/4dd9c9f3-1b20-4641-93e0-4e3bba8ccd1f/image.png" width="50%" height="50%"/>
</p>

<br>

HTTP 1.1과 HTTP 2.0에 대하여 상세 성능을 비교하며 알아보았다. 

아직 이해가 잘 안된다면 다시 글의 처음으로 올라가 내가 이해가 안가는 내용부터 구글링을 하여 학습을 진행하면 된다. 그것만으로도 네트워크 전반에 관하여 많은 내용에 대해 학습할 수 있다.

나 역시 그렇게 이 과정에 대해 학습하며 이해하게 되었다.

<br>

---

<br>

#### 참고자료
- https://velog.io/@junhok82/%ED%97%88%ED%94%84%EB%A7%8C-%EC%BD%94%EB%94%A9Huffman-coding
- https://developer.mozilla.org/ko/docs/Web/HTTP/Headers/Connection
- https://ijbgo.tistory.com/26
- https://velog.io/@hoo00nn/HTTP1.1-vs-HTTP2.0
- https://www.popit.kr/%EB%82%98%EB%A7%8C-%EB%AA%A8%EB%A5%B4%EA%B3%A0-%EC%9E%88%EB%8D%98-http2/
- https://ssungkang.tistory.com/entry/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-HTTP-11-VS-HTTP-20
- https://goodgid.github.io/HTTP-2.0/
- https://seokbeomkim.github.io/posts/http1-http2/#http11-vs-http2
- https://medium.com/@shlee1353/http1-1-vs-http2-0-%EC%B0%A8%EC%9D%B4%EC%A0%90-%EA%B0%84%EB%8B%A8%ED%9E%88-%EC%82%B4%ED%8E%B4%EB%B3%B4%EA%B8%B0-5727b7499b78
- https://jins-dev.tistory.com/entry/HTTP11-%EC%9D%98-HTTP-Pipelining-%EA%B3%BC-Persistent-Connection-%EC%97%90-%EB%8C%80%ED%95%98%EC%97%AC