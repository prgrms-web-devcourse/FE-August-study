# [오늘의 주제] TCP/IP 데이터 전송과정에 대한 이해 - 소켓

## 👀데이터는 어디서 주고받는걸까?-소켓에서!
저번 스터디 포스팅에서([링크](https://velog.io/@seungrok-yoon/how-we-travel-internet-1))는 3-Way handshake와 4-Way Handshake 를 알아보았다. 데이터 전송 준비를 위한 세션 생성과 세션 종료를 통한 자원 반납이 그 핵심이었고, 그 과정은 클라이언트와 호스트가 세그먼트에 flag를 표시하여 주고 받으며 진행된다고 설명했었다.

그런데 3-Way Handshake와 4-Way Handshake도 이미 서버와 데이터를 주고 받은 셈이다. 그렇다면 애초에 두 호스트는 어디를 거쳐서 데이터를 주고받았을까? 바로 소켓(Socket)이다. 

>**소켓이란?**
소켓은 버클리 소켓(Berkeley Sockets)의 준말이며, 버클리 소켓은 프로세스 간 커뮤니케이션(IPC, Inter-Process Communication)에 사용되는 인터넷 소켓과 유닉스 도메인 소켓을 위한 API(Application Programming Interface)이다. 연결된 네트워크 상에서 데이터 송수신에 사용할 수 있는 운영체제가 제공하는 소프트웨어적인 장치이며 프로세스 간 통신의 종착점이다.[참조](https://en.wikipedia.org/wiki/Berkeley_sockets)

소켓은 응용 프로그램에서 전송계층의 TCP/IP를 이용하는 인터페이스역할을 한다. 다시 말해 응용프로그램 내에서 소켓 API를 통해 구현된 소켓이 통신 접속점 역할을 하여 이를 통해 다른 호스트 소켓과의 데이터 교환을 가능케 하는것이다. 

집전화의 수화기에 비유하면 좀 더 이해가 쉬울 듯 한다. 가령,내가 친구 집에 전화를 하는 경우를 생각해보자. 그 경우, 내 입에서 나오는 말(데이터) 는 우리집 수화기와 친구 집 수화기 간 통화가 연결되어야 전송이 가능하다. 이 때 양 측의 수화기가 소켓이라고 생각하면 된다.


![](https://media.vlpt.us/images/seungrok-yoon/post/531f1d49-b1f0-48ec-9dbe-65d7e5b61310/image.png)[출처](https://www.ibm.com/docs/en/ssw_ibm_i_72/rzab6/rzab6pdf.pdf)


![](https://media.vlpt.us/images/seungrok-yoon/post/e28cf4f9-0b65-42f1-9d9f-4016d1fc40bd/image.png)

## 🚅소켓의 특징!
###  1) 5-Tuple
통신을 통해 전달되는 모든 데이터 포맷은 5-tuple 이라는 규격에 맞추어 흐르게 되는데, 소켓 역시 이 5-Tuple의 정보를 가지고 있다.

1. 프로토콜 (TCP, UDP)
2. 자신의 IP address
3. 자신의 Port number
4. 목적지 IP address
5. 목적지 Port number

### 2) 운영체제나 언어에 종속적이다
소켓을 구현하는 방법은 프로그래밍 언어마다, 그리고 운영체제 마다 다르다.

### 3) 네트워크 프로그래밍 인터페이스이다
TCP/IP의 관점에서 소켓은 하나의 네트워크 프로그래밍 인터페이스일 뿐, TCP/IP 표준은 아니다. UDP 프로토콜로 소켓으로 네트워크 프로그래밍을 할 수도 있다.


## 🚋 HTTP와 Socket

그렇다면 **TCP/IP기반으로 만들어진 application 계층 프로토콜인 HTTP는 소켓으로 통신하는가?** 에 대하여 한 번 생각해보자. 


>Socket 프로그래밍은 Server와 Client가 특정 Port를 통해 연결을 유지하고 있어 실시간으로 양방향 통신을 할 수 있는 방식. 예를 들면, 실시간 Streaming 중계나 실시간 채팅과 같이 즉각적으로 정보를 주고받는 경우에 사용합니다. 예를 들어 실시간 동영상 Streaming 서비스를 Http 프로그래밍으로 구현하였다고 가정하겠습니다. 이러한 경우에 사용자가 서버로 동영상을 요청하기 위해서는 동영상이 종료되는 순간까지 계속해서 Http Request를 보내야 하고 이러한 구조는 계속 연결을 요청하기 때문에 부하가 걸리게 됩니다. 그러므로 이러한 경우에는 Socket 프로그래밍을 통해 구현하는 것이 적합합니다.
>
[출처] (https://mangkyu.tistory.com/48)


>...socket is just an endpoint. it could follow http protocol to come in a communication in www as a client requesting a page or it could act as a server listening to connections.

정답은 HTTP도 Socket을 이용한 통신을 한다! 라는 것! 다만 그 차이는, 서버와 클라이언트의 소켓 간 연결이 단발적이냐, 연속적이냐의 차이. HTTP 통신 같은 경우에는 서버 측에서 클라이언트의 요청에 대한 답을 해주고 나서 연결을 닫아버린다. 하지만 TCP 소켓통신 같은 경우에는 그 연결을 read()/write()를 하며 계속 유지하고 있는 것

- HTTP Communication Using Sockets>[출처](https://www.oreilly.com/library/view/http-the-definitive/1565925092/httpatomoreillycomsourceoreillyimages96908.png)
  
![](https://media.vlpt.us/images/seungrok-yoon/post/13323450-bbea-424d-9572-83a76921400a/image.png)

- Connection-Oriented Communication Using Stream Sockets[출처](https://docs.oracle.com/cd/E19120-01/open.solaris/817-4415/sockets-12/index.html)


![](https://media.vlpt.us/images/seungrok-yoon/post/084953d3-08d3-4ee5-b913-e56086334c38/image.png)
## 🚌 Socket API 함수

컴퓨터와 컴퓨터 프로그램 간의 시스템 콜의 작동원리를 API를 통해 가려놓았기 때문에 사용자는 네트워크 처리에 관해 일일이 프로그래밍 할 필요 없이 TCP,UDP통신 등 구조적인 부분을 함수 및 라이브러리 형태로 쉽게 가져다가 사용만 하면 된다.

Berkeley socket API는 대개 아래와 같은 함수들을 제공하고 있는데, 가볍게 한 번 살펴보자.

- **socket()**:
소켓을 생성(TCP는 stream)
- **bind()**:
서버사이드에서 사용되는 함수이며, 소켓 구조(IP주소와port 번호)를 소켓에 등록한다.
- **listen()**:
서버사이드에서 사용되는 함수이며,아직 연결되지 않은 소켓을 요청 수신대기모드로 전환한다.
- **connect()**:
클라이언트사이드에서 사용되는 함수이며, 비어있는 로컬 포트 번호를 소켓에 할당한다. TCP소켓의 경우에는 서버사이드의 소켓에 클라이언트 사이드의 소켓과 목적지 IP주소, 포트 주소를 지정해준다. 대한 새로운 TCP연결을 개설한다.
- **accept()**:
서버사이드에서 사용되는 함수이며, 클라이언트사이드의 소켓과 통신을 위해 1:1로 새로운 TCP 연결을 생성한다.
- **send(), recv(), sendto(), and recvfrom()**:
데이터를 주고 받는데 쓰인다.
- **close()**:
소켓에 할당된 자원을 반환한다. TCP의 경우 연결이 종료됨.


## 😲TCP/IP 소켓 통신을 다시 살펴보기
 이제 소켓이 좀 더 잘 보이는 느낌이죠?
 
-  [TCP를 이용하는 소켓 통신 플로우 다이어그램](https://upload.wikimedia.org/wikipedia/commons/thumb/a/a1/InternetSocketBasicDiagram_zhtw.png/330px-InternetSocketBasicDiagram_zhtw.png)


![](https://media.vlpt.us/images/seungrok-yoon/post/6e7d4759-c354-4a09-983f-b910f3c7c47c/image.png)



## 생각할거리
- 웹소켓은 무엇이고 어떻게 동작하는 것일까?
- 브라우저는 웹소켓을 반드시 이용하는 것일까?
- 소켓을 통해 전송한 데이터가 반드시 호스트에게 도달할 것인가? 그렇지 않다면 어떤 처리가 필요한가?

## Reference 
- [네트워크 [브라우저에 URL 입력 후 일어나는 일들] 4_TCP 소켓 통신](https://wangin9.tistory.com/entry/브라우저에-URL-입력-후-일어나는-일들-4TCP-소켓-통신)
- [Berkeley Socket](https://en.wikipedia.org/wiki/Berkeley_sockets)
- [API에 대한 이해](https://en.wikipedia.org/wiki/API)