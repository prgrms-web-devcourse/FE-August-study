>요청한 데이터를 응답해주는 **서버의 구조**를 보며 **데이터의 흐름**을 파악해봅시다!

# # <span style = "color: #0d47a1;">웹 서버</span> = 컴퓨터?

웹 서버는 **웹 서버 하드웨어**와 **웹 서버 소프트웨어** 두 가지를 뜻할 수 있습니다.
일반적으로는 **웹 서버 = 웹 서버 소프트웨어**  라고 생각하면 됩니다.

<br>

### Web

- 인터넷을 기반으로, 정보를 공유하고 검색할 수 있게 하는 서비스
    - `URL` - 주소
    - `HTTP` - 통신규칙
    - `HTML` - 내용

### Server

- 클라이언트에게 네트워크를 통해 정보나 서비스를 제공하는 컴퓨터 시스템

## <span style = "color: #0d47a1;">Web Server</span>

= 인터넷을 기반으로 + 클라이언트에게 웹 서비스를 제공하는 프로그램

<br><br>

## 웹 서버의 역할

![](https://images.velog.io/images/young18/post/47ce39ca-f03f-489f-a7ab-d7106de6e951/Untitled.png)

>**기존의 HTTP를 사용한 웹 구조**

- 웹 서버는 클라이언트의 요청을 기다리고,,
요청에 대한 데이터를 만들어서 응답
- 응답하는 데이터는 웹에서 처리할 수 있는 
`HTML` `CSS` `image` 등 **정적 콘텐츠**로 한정
- 정적(Static) 콘텐츠 = **사전에 준비된 콘텐츠**

>**웹이 보급되면서..** 

- 🤷‍♀️: "*정적 콘텐츠만으로는 부족한데..?"*
- 🤷‍♂️: *"웹에서 보여주는 것만 봐야해?"*
- 👶: *"HTML로 어떻게든 해봐!!" ← ???
~~(HTML은 프로그래밍 언어가 아닙니다)~~*

**⇒ 웹 서버만으로는 불가능**


<br><br><br><br>

# # <span style = "color: #fbc02d;">WAS</span> = Web server + <span style = "color: #fbc02d;">Web container</span>

### **Web Application**
- **웹**을 사용해서 기능을 제공하는 **응용프로그램**

### **W**eb **A**pplication **S**erver 
- **웹 애플리케이션을 실행**시켜 필요한 기능을 수행하고 그 **결과를 웹 서버에게 전달**하는 미들웨어

<br>

## WAS의 역할

  - 클라이언트 요청에 따라 동적 콘텐츠를 생성할 수 있는 서버(Web Container)
  - jsp, php, asp와 같은 언어를 사용해 프로그램(jsp, servlet) 실행 환경과 데이터베이스 접속 기능 제공
  - 비즈니스 로직 수행
  
<br>

### 🙄 웹 서버가 있는데 왜 WAS를 쓰는거지?

> Web Server만 이용한다면 **사용자가 요청하는 모든 정보를 사전에 준비**해두고 서비스를 해야할 것입니다.
하지만 모든 요청에 사전대응하기란 **현실적으로 불가능**합니다.<br>
따라서 **정적 페이지는 Web Server에서 처리**하고, 
**WAS를 통해 상황에 맞는 비즈니스 로직을 수행하면서 DB로부터 데이터를 가져오는 것**이 자원 효율성을 높일 수 있습니다.

<br>
<br>

# # 웹 서비스 구조(Web Service Architecture)

- **Client ↔ <span style = "color: #0d47a1;">Web server</span> ↔ DB**

- **Client ↔ <span style = "color: #fbc02d;">WAS</span> ↔ DB**

![](https://images.velog.io/images/young18/post/73ff3c6e-5cc8-4ff7-99d7-0990c4f752d4/%E1%84%8B%E1%85%B0%E1%86%B8%E1%84%89%E1%85%A5%E1%84%87%E1%85%B5%E1%84%89%E1%85%B3%E1%84%8B%E1%85%A1%E1%84%8F%E1%85%B5%E1%84%90%E1%85%A6%E1%86%A8%E1%84%8E%E1%85%A51.png)

- **Client ↔ <span style = "color: #0d47a1;">Web server</span> ↔ <span style = "color: #fbc02d;">WAS</span> ↔ DB**

![](https://images.velog.io/images/young18/post/21ce214b-246c-4513-83b4-213cb7e887bf/%E1%84%8B%E1%85%B0%E1%86%B8%E1%84%89%E1%85%A5%E1%84%87%E1%85%B5%E1%84%89%E1%85%B3%E1%84%8B%E1%85%A1%E1%84%8F%E1%85%B5%E1%84%90%E1%85%A6%E1%86%A8%E1%84%8E%E1%85%A52.png)

<br>

### 🙄 그럼 WAS로 정적 콘텐츠까지 처리하면 Web Server를 안써도 되나?

답은 `네니오!!`

> 예전의 WAS는 정적인 웹서버 기능을 제공하지 않았지만 최근 WAS들은 동적인 컨텐츠 뿐만 아니라 정적인 컨텐츠도 제공해줄 수 있고, 성능도 나쁘지 않습니다.
그렇기 때문에, Web server 없이 **WAS만 존재하는 것도 가능**합니다.<br>
**그럼에도 Web server를 따로 두는 이유는 무엇일까요?**

#### 1.  WAS는 바쁘다

따라서 **단순한 정적 콘텐츠는 Web server가 처리**하도록 **기능을 분리**하여 **과부하를 방지**할 수 있습니다.

#### 2. 보안 강화

WAS에는 Web Application이 올라가 있고, DB서버와 연결되어있는 경우가 많습니다.

따라서 외부에 노출될 경우 설정 파일이나 리소스가 노출될 위험이 있습니다.

보통 WAS는 내부에 두고 앞단에서 Web server가 외부와 통신하도록하여 보안을 강화할 수 있습니다. 
>![](https://images.velog.io/images/young18/post/247b9a2a-c77a-49ea-b512-2e093149755a/WAS%E1%84%87%E1%85%A9%E1%84%8B%E1%85%A1%E1%86%AB1.png) 
<br><span style = "color: gray;">_외부에서 WAS로 접근할수 있는 상황. 
WAS는 DB서버에도 연결되어 있으므로 2차 방화벽이 의미가 없다._</span>

>![](https://images.velog.io/images/young18/post/00b18527-14db-4bce-a2c2-47cd155d1e12/WAS%E1%84%87%E1%85%A9%E1%84%8B%E1%85%A1%E1%86%AB2.png)
<br><span style = "color: gray;">_Web server를 DMZ에 두고 WAS는 내부 영역에 두었기 때문에 해킹당하더라도 Web server를 통해 DB서버까지 접근하는 것은 불가능하다._</span>

#### 3. Load Balancing
여러 대의 WAS를 연결할 수 있기 때문에 Web server가 앞단에서 WAS들에게 업무를 적절히 분배함으로써 가용성 및 응답시간을 최적화 시킬 수 있습니다.

#### 4. 여러 웹 어플리케이션 서비스 가능
예를들어 PHP 언어로된 Application과 Java 언어로된 Application을 하나의 Web server에 연결하여 사용할 수 있습니다.

<br><br>

## 👏 마무리

웹 애플리케이션을 만드는 방법은 다양합니다. <br>
예를 들어 자바스크립트를 사용하는 node.js 자체는 server가 아니지만, node.js의 framework를 통하여 web server를 만들 수 있습니다. 로직 처리도 가능하고, 여기에 express등의 프레임워크를 사용하여 DB와 연동할 수 있는 하나의 서버 환경을 구축할 수 있습니다. 이러한 웹 애플리케이션은 실시간으로 대량의 데이터를 처리해야하는 경우 유용할 수 있습니다. 반대로 데이터 처리 별로 CPU 성능을 많이 차지하게되는 서비스라면 다른 서버환경을 구축하는 것이 유리할 것입니다.
<br>
구축하고자하는 웹사이트가 정적 콘텐츠 위주인지 동적 콘텐츠 위주인지, 대용량 데이터를 처리해야하는지 등을 고려하여 적절한 서버 환경을 구성하는 것이 중요합니다.
<br>
결국 서비스를 함에 있어서 甲은 클라이언트이고, 요청들에 대해 고성능으로 응답해주는 것이 개발자의 역할이 아닐까 생각합니다😉
