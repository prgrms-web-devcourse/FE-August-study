# [오늘의 주제] TCP 3-Way Handshake & 4-Way Handshake (작성자: [1기-B]윤승록)

프론트엔드 개발자가 되기 위해서는 브라우저가 어떻게 동작하고 있는지에 대한 이해가 필요하다. 하지만 나는 이러한 지식이 전무하다시피 한 이유로, 앞으로 진행할 CS스터디의 목표을  '브라우저 동작원리 이해'로 설정하고, 이와 관련된 주제를 선정하여 시리즈로 포스팅할 계획이다. 오늘은 TCP 3 way-Handshake & 4-way Handshake에 관하여 포스팅해 보도록 하겠다.


# 1. TCP란 무엇인가?
> TCP란 IP(Internet Protoccol)을 보충하는 네트워크 구현 과정에서 생겨난 인터넷 프로토콜이다. 그래서 TCP/IP 라고 많이들 일컫는 것. WWW, 이메일, 원격 관리, TCP 파일 전송과 같은 인터넷 어플리케이션들은 다 TCP/IP suite의 전송 레이어의 한 부분인 TCP 프로토콜에 의존하고 있다.[[출처 번역]( https://en.wikipedia.org/wiki/Transmission_Control_Protocol#Historical_origin)]

<img src="https://media.vlpt.us/images/seungrok-yoon/post/b8d50590-a277-41af-ad4c-c123d5dc3ca7/image.png">

[[이미지 출처](http://wiki.hash.kr/index.php?title=OSI_7_%EA%B3%84%EC%B8%B5&mobileaction=toggle_view_desktop)]

TCP는  OSI 7 계층 모델과,이 모델을 4계층으로 단순화한 모델(OSI 7계층 모델과 TCP/IP 4계층 모델은 관련은 있으나 서로 완전히 들어맞지는 않으니 주의!)
인 TCP/IP 계층 모델에서 전송 계층에 속하며, TCP 프로토콜을 통해 각각의 시스템을 연결하고, 데이터를 도착지까지 전송하게 된다. 그러니까 TCP는 전송 프로토콜인 것이야! 그러면 IP는? 전송 계층이 아닌 인터넷 계층에 존재한다. 

TCP의 대표적인 특징은 다음과 같다. 
> - 신뢰할 수 있는 프로토콜
- 연결지향
- 데이터 전달이 보증 된다.
- 순서 보장
- TCP는 IP의 단점인 비연결성을 해소한다.
 그리고 패킷 전달 중 누락 될 시, 이를 인지할 수 있다.

TCP 프로토콜에서는 데이터 송수신을 위해 클라이언트와 서버의 소켓이 연결되어 있어야 하는데, 데이터가 유실되면 데이터 재전송을 요청하게 되어있다. 이렇게 데이터 재전송을 요청하게 된다면 데이터 전달이 어느 정도 보증 될 것이고, 그래서 이로 인해 신뢰성이 보장 될 수 있다. 

반대로 연결지향 프로토콜이 아닌 프로토콜에는 UDP 프로토콜이 있다. 비연결지향 프로토콜인 UPD 프로토콜은 전송한 데이터가 잘 전달이 되었는지 확인하지 않고, 단지 데이터만 보낸다. 물론 확인 없이 데이터만 전달하기 때문에, 성능이 빠르다는 특징이 있다.

앞에서 데이터 송수신을 위해 TCP프로토콜은 클라이언트와 서버의 소켓의 연결이 필수적이라고 언급했다. 이러한 연결 작업을 포함하는 TCP의 전반적인 작동 흐름은 다음과 같다.
- 1) 연결 생성 (Connection establishment) = 세션 생성
- 2) 자료 전송 (Data transfer)
- 3) 연결 종료 (Connection Termination) = 세션 종료

그리고 TCP프로토콜에서 클라이언트와 서버 간 연결 생성과 연결 종료 방법이 각각 3-Way Handshake와 4-Way Handshake이며, 이것들이 오늘 내가 다룰 주제이다. 그럼 3-Way Handshake부터 알아보러 가자. 

# 2. "똑똑똑~ 계세요?" 3-Way Handshake

연결지향이란말은 데이타를 전송하는 측과 데이타를 전송받는 측에서 전용의 데이타 전송 선로(Session)을 만든다는 의미이다. 데이타의 신뢰도가 중요하다고 판단될때 주로 사용된다.

<img src="https://media.vlpt.us/images/seungrok-yoon/post/1dbb9016-06f7-4c52-bb3e-5937b15a6d49/image.png"> [[이미지출처]
(https://www.masterraghu.com/subjects/np/introduction/unix_network_programming_v1.3/ch02lev1sec6.html)]

<img src="https://media.vlpt.us/images/seungrok-yoon/post/548c8e86-b589-4f0c-b7ba-96e6e56844c5/image.png"> 

서버간 연결 생성을 위해서 클라이언트와 서버는 CP세그먼트 헤드에 9bit길이의'플래그(Flags)'에 자신의 상태를 기록해서 세그먼트를 전달한다.

1) **[SYN]** 클라이언트는 서버에게 데이터 전송 전에, 먼저 서버 접속요청을 위한 세그먼트(SYN-Synchronize)을 보낸다. 그리고 이 요청과 더불어 1바이트 크기의 Sequence number(SEQ 넘버)를 함께 실어 보낸다. 마치 친구가 내 집에 놀러와 벨을 누르며 "XXX야 오늘 같이 놀래?" 라고 물어보면서 현관문 밑으로 슬쩍 숫자가 적힌 작은 쪽지 하나를 건네주는 것으로 생각하면 된다. (클라이언트 상태는 SYN - SENT 로 변화, 서버는 요청을 받은 후에 SYN -RECEIVED로 변화)

2) **[SYN,ACK]** 서버는 클라이언트로부터 SYN 요청을 받은 다음, 연결을 수락(ACK) 한다. 그러면서 아까 받은 Sequence number에 1을 더한 값을 연결준비 상태 응답으로 주면서, 자신도 클라이언트에 Acknowledge number를 같이 연결 요청 응답으로 보내준다 (SYN,ACK). (클라이언트 상태는 ESTABLISHED 로 변화, 서버는 아직SYN -RECEIVED 상태)

3) **[ACK]** 이제 서버의 연결준비 상태(SYN)을 받은 클라이언트는 서버에게 접속 수락 확인(SYN) 을 보내는데, 이 때도 똑같이 Sequence number에 1을 더한 값을 ACK값으로 보내주면서 패킷 수신 응답을 한다. 
 (현재 클라이언트 상태는 ESTABLISHED, 서버 상태도 ESTABLISHED로 변화 )
<img src="https://media.vlpt.us/images/seungrok-yoon/post/2a1036e5-dfe5-41fe-9ac8-d97a5bcdb9cc/image.png"> 

TCP가 무엇인지 이해하는 것이 어려웠을 뿐이지, 막상 3-Way Handshake를 알아보니 별거 없다는 생각이 든다. 그렇다면 우리가 사용하는 브라우저에서 특정 웹 사이트에 접속 요청을 할 때 실제로 3-Way Handshake가 일어나는지를 눈으로 확인해보고 싶지 않은가? 교수님들이 우리를 공부시키기 위해 거짓말을 하는 것일 수도 있으니 말이다. 그래서 다음으로 WireShark를 이용한 실습을 진행하며 실제로 클라이언트가 서버가 데이터 전송 전 사전 작업을 거치는지 확인해보자!


# 3. Wire Shark를 통해 3-Way Handshake의 과정을 직접 확인해보자!

실습 대상 사이트로는  TCP/IP 프로토콜과 관련된 응용 프로토콜인 HTTP 프로토콜을 사용하는 환경부 사이트(http://me.go.kr/home/web/main.do)를 선정했다. (HTTP와 HTTPS의 차이점도 알아봐야겠다)

즉, 이 실습에서 서버는 환경부 사이트 서버이고, 클라이언트는 내 브라우저가 될 것이다.

먼저 명령 프롬프트에서 **nslookup [사이트 주소]** 를  통해서 환경부 사이트의 IP 주소를 알아낸다.
그리고 Wireshark에서 패킷수집을 켜놓은 상태로 환경부 사이트에 접속한다. 그러면 부다다다 패킷이 쏟아져 들어올 것이다. 그 중에서 내가 원하는 것은 내 브라우저와 환경부 서버와의 연결 내역이기 때문에, Destination IP주소가 환경부 사이트 IP주소인 패킷을 찾아 낸다.
<img src="https://media.vlpt.us/images/seungrok-yoon/post/a87cc2c0-8660-4781-a750-c164403fdbd3/image.png"> 

<img src="https://media.vlpt.us/images/seungrok-yoon/post/faaeee9a-5058-45b7-93fd-6ea525f9e857/image.png"> 

Destination 이 27.101.216.200를 ip.addr ==27.101.216.200 명령어로 필터링 한 다음 TCP스트림을 추적해 본다. 


<img src="https://media.vlpt.us/images/seungrok-yoon/post/436252f7-6c9d-4d62-bdee-de83e5311980/image.png"> 

짜잔~ 우리가 실제로 보기를 원했던 3-Way Handshake 세그먼트 정보가 뙇 하고 나오게 된다. 그럼 이제 하나씩 눌러보며 살펴보도록 하자. TCP세그먼트는 세그먼트 헤더와 데이터의 두 섹션으로 구성되어 있다.

<img src="https://media.vlpt.us/images/seungrok-yoon/post/548c8e86-b589-4f0c-b7ba-96e6e56844c5/image.png"> 

첫 번째 SYN 요청 세그먼트이다. Sequence number가 3151176559 인데, 이 숫자는 특별한 의미가 없는 난수이다. 

<img src= "https://media.vlpt.us/images/seungrok-yoon/post/d01f5501-8872-4398-bcd0-8174f5105bc2/image.png">


두번째 SYN+ACK 세그먼트이다.
ACK number는 클라이언트가 SYN 을 할 때 준 Sequence number(3151176559)에 +1을 하여 보내준다(3151176560). 그리고 서버 역시도 클라이언트에게 이 응답을 잘 들었는지 확인하기 위해서, 서버만의 Sequence number를 또 보내준다.
<img src= https://media.vlpt.us/images/seungrok-yoon/post/83e773ea-ffb4-474d-ada5-b6b5d6ac9225/image.png>

마지막 ACK 세그먼트이다. 

<img src= https://media.vlpt.us/images/seungrok-yoon/post/d465e096-5038-4d7a-aa47-246008a59275/image.png>

이후, 연결에 성립되어 데이터가 전송되고 나서는 연결을 종료해야 한다. 

# 4. 데이터 전송을 했다면 이제 연결을 종료해야지! 4-Way Handshake

이후, 연결에 성립되어 데이터가 전송되고 나서는 연결을 종료해야 한다. 클라이언트와 서버 간 논리적인 접속 상태를 해제하기 위해서는 그간 연결을 위해 사용하였던 리소스의 정리가 필요한데, 이 정리 과정이 4-Way Handshake를 통해서 진행된다.  4-Way Handshake는 서버 간 4번의 세그먼트(segment)전송이 이루어지며 진행이 된다.
<img src= https://media.vlpt.us/images/seungrok-yoon/post/b4598bee-b5f0-41d1-bf61-d2a7c3f4f979/image.png>

<img src= https://media.vlpt.us/images/seungrok-yoon/post/bae6b934-a208-42b5-b378-59a1c0b37c06/image.png>
FIN 메시지는 데이터 전송이 완료되었다고 서버가 판단하였으면 먼저 보낼 수도 있기 때문에, 꼭 위 도식처럼 진행되는 것은 아니다. 

여기서도 마찬가지로 Sequence number와 Acknowledge number를 가지고 서로 통신을 하며, 상태 변화는 위 도식과 같다.

# 5. Wire Shark를 통해 4-Way Handshake의 과정을 직접 확인해보자!

<img src= "https://media.vlpt.us/images/seungrok-yoon/post/2dad7ff1-4202-4be1-ab4a-a80eb6fda953/image.png" >
<img src= "https://media.vlpt.us/images/seungrok-yoon/post/81f39294-e7b7-4f2e-9ea4-681c709380bc/image.png">
<img src= "https://media.vlpt.us/images/seungrok-yoon/post/8c143d34-8557-46d2-a619-fad263baf0ce/image.png">
<img src= "https://media.vlpt.us/images/seungrok-yoon/post/8c08283d-c42a-48d2-97b4-876ce82105ba/image.png">


# 6. 의문점
1) 조사를 하면서 패킷과 세그먼트를 혼용하여 사용하는 경우가 많이 관찰되었다. 
패킷은 무엇이고 세그먼트는 무엇인지, 이 둘을 구분하는 기준은 무엇인지 궁금했다.
오라클 문서에서는 패킷을 다음과 같이 정의하고 있다.

> 데이터 캡슐화 및 TCP/IP 프로토콜 스택
패킷은 네트워크를 통해 전송되는 기본 정보 단위입니다. 기본 패킷은 송신 및 수신 시스템의 주소가 있는 헤더, 본문 또는 전송될 데이터가 있는 페이로드로 구성됩니다. 패킷이 TCP/IP 프로토콜 스택을 통해 이동함에 따라 각 계층의 프로토콜은 기본 헤더에서 필드를 추가하거나 제거합니다. 송신 시스템의 프로토콜이 패킷 헤더에 데이터를 추가하면 이 프로세스를 데이터 캡슐화라고 합니다. 또한 각 계층은 다음 그림과 같이 변경된 패킷에 대해 서로 다른 용어를 사용합니다. [[링크](https://docs.oracle.com/cd/E38901_01/html/E38894/ipov-29.html)]

만약 계층에 따라 패킷을 부르는 명칭이 다르다면, 세그먼트는 전송레이어에서 캡슐화가 진행되어 헤더가 추가된 패킷을 의미하는 것인가? 

그 답은 [[링크1](https://ko.wikipedia.org/wiki/%EC%A0%84%EC%86%A1_%EC%A0%9C%EC%96%B4_%ED%94%84%EB%A1%9C%ED%86%A0%EC%BD%9C)] 과 [[링크2](http://www.ktword.co.kr/test/view/view.php?m_temp1=310)]에서 찾을 수 있었다. 
> TCP는 데이터 스트림으로부터 데이터를 받아 들여 이것을 청크 단위로 분할한 뒤 TCP 헤더를 덧붙여 TCP 세그먼트를 생성한다. TCP 세그먼트는 IP 데이터그램에 캡슐화되어 상대방과 주고 받게 된다.[2]
TCP 패킷이라는 용어가 종종 사용되지만 이는 정확한 표현이 아니다. 세그먼트가 TCP 프로토콜 데이터 유닛(PDU)을 의미하는 정확한 표현이며 데이터그램[3] 은 IP PDU를, 프레임은 데이터 링크 계층 PDU를 의미한다.
프로세스는 TCP를 통해 데이터 버퍼를 인수로 넘겨 줌으로써 데이터를 전송한다. TCP는 이 버퍼들을 묶어 세그먼트를 생성하여 인터넷 모듈(IP 등)을 통해 목적지의 TCP로 각각의 세그먼트들을 전송한다.

>OSI 7계층모델과 같이 계층화된 구조에서  임의의 한 프로토콜과 관련되어
     - 같은 계층에 존재하는 두 통신 개체 간에 (즉, Peer-to-Peer) 서로 주고 받게되는,
     - 헤더(제어정보) 및 데이타(SDU,Payload)가 합쳐진 캡슐화된 운반체(패킷,프레임 등)
     * 즉, 해당 계층에서 만 처리되는 특정한 헤더를 붙이게 됨


그러니까 DataLink 계층(2)에서 PDU(프로토콜 데이터 단위)는 프레임(Frame)으로, Network 계층(3) 에서 PDU는 패킷(Packet)으로, 그리고 Transport 계층(4) 에서 PDU는 세그먼트(Segment)로 명명하게 되는 것이다.

2) 그런데 왜 OSI7 계층이나 TCP/IP 4계층이나 왜 이렇게 계층화를 할까?
답은 **"독립성을 보장하기 위해서"** 이다. 문제가 발생했을 때 문제가 발생한 계층만 파악하면 되니 독립성이 보장됨. 

# 7. 더 알아야 할 내용

- OSI 7 layers VS TCP/IP Protocol Suite
- TCP/IP 4계층에 의한 데이터 전송
- 데이터 캡슐화 및 TCP/IP 프로토콜 스택
- 데이터 패킷
- 4 Layers of the TCP/IP Model
- HTTP와  HTTPS의 차이점


# Reference
- **TCP정의**
https://www.fortinet.com/resources/cyberglossary/tcp-ip
https://www.joinc.co.kr/w/Site/Network_Programing/Documents/IntroTCPIP
https://victorydntmd.tistory.com/288

- **TCP 위키**
https://ko.wikipedia.org/wiki/%EC%A0%84%EC%86%A1_%EC%A0%9C%EC%96%B4_%ED%94%84%EB%A1%9C%ED%86%A0%EC%BD%9C
https://en.wikipedia.org/wiki/Transmission_Control_Protocol#Historical_origin
- **TCP connection establishment and termination **
https://www.masterraghu.com/subjects/np/introduction/unix_network_programming_v1.3/ch02lev1sec6.html
- **패킷과 세그먼트**
http://www.ktword.co.kr/test/view/view.php?m_temp1=310
- **인터넷은 어떻게 동작하는가?**
https://developer.mozilla.org/ko/docs/Learn/Common_questions/How_does_the_Internet_work

- **데이터 캡슐화 및 TCP/IP 프로토콜 스택**
https://docs.oracle.com/cd/E38901_01/html/E38894/ipov-29.html
https://victorydntmd.tistory.com/288
http://wiki.hash.kr/index.php?title=OSI_7_%EA%B3%84%EC%B8%B5&mobileaction=toggle_view_desktop

- **TCP플래그**https://www.keycdn.com/support/tcp-flags
- **OSI 모델과 와 TCP/IP 모델 간 차이**
https://www.guru99.com/difference-tcp-ip-vs-osi-model.html
https://chloe-codes1.gitbook.io/til/network/02_osi_7_layers
https://blog.naver.com/PostView.nhn?isHttpsRedirect=true&blogId=demonicws&logNo=40117378644
- **3-Way handshake**
https://www.ietf.org/rfc/rfc793.txt 에서의 "Basic 3-Way Handshake for Connection Synchronization"
https://tools.ietf.org/html/rfc675 에서의 "4.3.2 ESTABLISHING A CONNECTION"
https://networkengineering.stackexchange.com/questions/24068/why-do-we-need-a-3-way-handshake-why-not-just-2-way/24069
https://sleepyeyes.tistory.com/4
https://youngq.tistory.com/55
https://www.crocus.co.kr/1362

- **WireShark 를 통한 실습**
https://blog.naver.com/agerio100/221948546623

- **브라우저는 어떻게 동작하는가?**
https://d2.naver.com/helloworld/59361 (번역)
https://www.html5rocks.com/en/tutorials/internals/howbrowserswork/ (원문)

