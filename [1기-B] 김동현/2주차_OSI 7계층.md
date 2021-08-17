## **응용 계층 ( Application layer )**

\- 데이터를 받는 "엔드 시스템"이 이해 할 수 있는 방식으로 데이터 형식으로 변환합니다.

\- 대표적인 프로토콜로는 \[ HTTP, SMTP, FTP \] 등이 존재합니다.

\- 대표적인 어플리케이션으로는 \[ 크롬 ,카카오톡, 줌  \] 등이 있습니다.

\* 카카오톡 메세지를 받는 상황을 예시로 들어 보겠습니다.

1) 친구가 저에게 메세지를 보냈다고 가정하면, 서버에는 새로운 대화 메세지가 오게 될 것입니다.

2) 서버는 저에게 "신규 메세지"를 보내 주어야 하는데, 이 때 상태를 파악 할 수 있도록 데이터에 \[ status code : 500 \] 와 같은 정보들을 붙여서 보내줍니다.

3) 여러 계층을 거쳐서, 제 응용계층으로 오게 되면 추가 정보를 파악하고, 메세지와 분리합니다.

[##_Image|kage@83cWK/btrbV0vGXmG/cG7RhUm48pwFjb732wKs90/img.png|alignCenter|data-origin-width="762" data-origin-height="936" data-filename="스크린샷 2021-08-12 오후 8.58.12.png" width="711" height="873" data-ke-mobilestyle="widthOrigin"|||_##]

## **프레젠테이션 계층 ( Presentation Layer )**

프레젠테이션 계층에서는 \[ 데이터의 변환 -> 압축 -> 암호화 \] 기능을 수행합니다.

이렇게 데이터를 암호화 하는 이유는, 통신을 하는 기기마다 다른 인코딩 방법을 채택 할 수 있기 때문입니다bb.

[##_Image|kage@8wYqg/btrb1bwceYW/jJdfGwQKeWZnx8poO688b1/img.png|alignCenter|data-origin-width="1506" data-origin-height="444" data-filename="스크린샷 2021-08-12 오후 8.55.03.png" data-ke-mobilestyle="widthOrigin"|||_##]

## **세션 계층 ( Session Layer )**

세션 계층에서는 \[ 세션 관리 및 복구 \] 기능을 수행합니다.

\* 잠시만 복구는 어떻게 하나요?

세션 계층에서는 임의로 **체크포인트**를 설정합니다.

가령 100mb 데이터를 다른 기기로 전송 할 때, 체크포인트를 5mb로 설정 했다고 가정을 해보겠습니다.

47mb 쯤 전송을 하던 도중, 커넥션이 끊겼을 때 처음부터 시작하는 것이 아닌 45mb에서부터 다시 전송을 시작합니다.

## **전송 계층 ( Transport Layer )**

전송 계층은 \[ 세그멘테이션, 오류 / 흐름 제어, Port  \] 기능을 수행합니다.

대표적인 프로토콜로는 \[ TCP / UDP \] 가 있습니다.  

**\[1\] 세그멘테이션 ( Segmentation )**

세그멘테이션은 한국말로 '분리' 라는 의미를 가지고 있는데요. 실제로도 이 뜻처럼, 위에서 받은 데이터를 특정 사이즈로 나누는 기능을 수행 합니다.

그럴 경우, 완전한 전송이 되지 않더라도 일부 일부를 볼 수 있게 할 수 있고 데이터 손실률도 줄일 수 있다는 장점이 생깁니다. 

**\[2\] 흐름 제어**

흐름 제어는 두 컴퓨터 간의 전송 속도를 맞추어 주는 것을 의미합니다.

예를 들어서 \[A\] 컴퓨터는 50mb 속도로 송/수신 가능하고, \[B\] 컴퓨터는 20mb 로 송/수신이 가능하다고 해보겠습니다.

그럴 경우 A에서는 B에게는 50mb의 속도로 보낸다고 하더라도, B에서는 이 속도를 온전히 처리 할 수 없기에 속도를 20mb로 맞춰줍니다.

**\[3\] 오류제어**

오류제어는 오류가 생긴 데이터를 다시 보내주는 것을 의미합니다. 

**\[4\] Port**

Port 번호는 하나의 컴퓨터 안에서 수행되는 프로세스들이 가지는 고유한 번호를 의미하는데요.

프레젠테이션 계층에서는 이 번호를 통해, 어떤 작업에게 일을 할당하고 요청 받을지 결정을 하게 됩니다. 

데이터 + 출발지와 도착지의 Port +  TCP || UDP 정보를 캡슐화 해줍니다. => 세그먼트

## **네트워크 계층 ( Network-Layer )**

네트워크 계층은 실제 데이터의 전송을 담당하는 계층입니다.

네트워크 계층에서는 전송 계층에서 전달된 세그먼트 헤더에 출발/도착지의 IP 주소를 붙이고, 이를 패킷이라 부릅니다.

또한, 최적으로 경로를 찾는 라우팅 기능까지 담당을 합니다.

라우팅 프로토콜과 알고리즘은 이전에 작성한 포스팅을 참조 해주세요 :D

1) 프로토콜

[https://kimbangg.tistory.com/263](https://kimbangg.tistory.com/263)

[

\[ Network \] 라우팅 프로토콜

머릿말 이전 게시물을 통해, 거리벡터 & 링크상태 알고리즘에 대해서 다루었습니다. 이번에는 1) IGP 에서 사용되는 라우팅 프로토콜인 RIP & OSPF 2) EGP 에서 사용되는 BGP 프로토콜까지 알아 보겠습

kimbangg.tistory.com



](https://kimbangg.tistory.com/263)

2) 알고리즘

[https://kimbangg.tistory.com/262](https://kimbangg.tistory.com/262)

[

\[ Network \] 라우팅 알고리즘 ( Link State 편 )

머릿말 지난 게시물에서 라우터에 대한 설명과 라우팅 알고리즘 중 거리벡터 알고리즘을 다루었습니다. https://kimbangg.tistory.com/261 \[ Network \] 라우팅 알고리즘 ( 라우터 & Link State 편 ) 1. 라우터(Rou..

kimbangg.tistory.com



](https://kimbangg.tistory.com/262)

[https://kimbangg.tistory.com/261](https://kimbangg.tistory.com/261)

[

\[ Network \] 라우팅 알고리즘 ( 라우터 & Distance Vertor 편 )

1\. 라우터(Router) 란? Router는 1) 최적의 길을 찾아주는 Path Determine 과 2) 패킷을 전송하는 Switching 이렇게 2가지의 기능을 수행합니다. 우리가 여기서 다룰 내용은 최적의 길을 찾는 방법인 라우팅 프

kimbangg.tistory.com



](https://kimbangg.tistory.com/261)

## **데이터 링크 계층 ( Data Link Layer )**

\-  동일한 네트워크 내부에서의 담당을 제어합니다.

\-  이전에서 넘어온 패킷에 출발지/인접 맥어드레스를 붙여 프레임을 만들어줍니다.

#### **\- 흐름 제어**

네트워크 계층에서 했던 것과 같이 \[송/수신자\] 간의 속도를 맞추기 위한 것 입니다.

관련된 방법으로는 \[ Stop & wait, Sliding Window\] 기법이 있습니다.

#### **\- Framing **

실제로 한 대의 컴퓨터는 여러 대의 컴퓨터로부터 정보를 받습니다.

예를 들어서 A는 1100 0001을 보냈고, B는 0011 1100 을 보냈다고 가정해봅시다.

그렇다면 이 데이터를 수신 받은 컴퓨터는 1100 0001 0011 1100 과 같이 데이터를 받을 것입니다.

지금은 2대의 컴퓨터가 각각 하나의 데이터만을 보내서 구별이 가능하다고 치더라도, 이것이 늘어난다면 구별하기가 어려워지겠죠?

그렇기 때문에 여기서는 데이터의 첫과 끝에 데이터를 붙여주는 "프레이밍"  수행합니다.

예를 들어서  \[ 시작 지점 \] 에는 1111을 \[끝 지점\] 에는 0000을 붙여준다고 가정해봅시다.

그럴 경우에는 데이터가 아무리 길어져도 \[1111\] 1100 0011 \[0000\] 과 같이 전송이 되기 때문에 구별 할 수 있겠죠? :D

#### **\- 데이터 전송**

상위 계층인 네트워크 계층에서 이미 라우팅이 완료가 되어있습니다.

전체적인 이동경로는 확정이 되었지만, 실제로 데이터를 전달 할 때는 이웃에게 전달하는 방식만을 이용하는데요.

그 이유는 IP는 논리적 주소이기에 변동이 가능해서, 데이터가 올바르게 전달이 되지 않을 수 있기 때문입니다. 

또한, 모든 맥주소를 가지고 있으며 라우터에 부하가 많이 가기 때문에 실제로는 주위에 있는 기기의 맥주소 정보만이 담긴 ARP 테이블이 있기 때문입니다.

## **물리 계층 (  Physical Layer )**

비트 단위를 전기 신호로 변환 해주고, 전송을 하는 기능을 수행합니다.

## **Reference**

[https://aws-hyoh.tistory.com/entry/ARP-%EC%89%BD%EA%B2%8C-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0](https://aws-hyoh.tistory.com/entry/ARP-%EC%89%BD%EA%B2%8C-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0)

[

ARP 쉽게 이해하기

주소 결정 프로토콜(Address Resolution Protocol, ARP)은 네트워크 상에서 IP 주소를 물리적 네트워크 주소로 대응(bind)시키기 위해 사용되는 프로토콜이다. 여기서 물리적 네트워크 주소는 이더넷 또는

aws-hyoh.tistory.com



](https://aws-hyoh.tistory.com/entry/ARP-%EC%89%BD%EA%B2%8C-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0)

[https://www.youtube.com/watch?v=1pfTxp25MA8&t=999s](https://www.youtube.com/watch?v=1pfTxp25MA8&t=999s) 

[https://www.youtube.com/watch?v=Ilk7UXzV\_Qc&t=365s](https://www.youtube.com/watch?v=Ilk7UXzV_Qc&t=365s)