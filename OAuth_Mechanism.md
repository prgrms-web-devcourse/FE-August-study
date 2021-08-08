## OAuth란 무엇인가
### 정의
>OAuth는 인터넷 사용자들이 비밀번호를 제공하지 않고 다른 웹사이트 상의 자신들의 정보에 대해 웹사이트나 애플리케이션의 접근 권한을 부여할 수 있는 공통적인 수단으로서 사용되는, 접근 위임을 위한 개방형 표준
>  \- 위키피디아 -



## OAuth가 탄생한 배경
OAuth가 탄생한 이유는 우리의 일상에 대입 해보면 간단하다. 내가 이용하고자 하는 서비스 업체에서 내 개인 정보를 요구했을 때 일일이 정보를 직접 제공하기 귀찮고, 내가 신뢰하는 사람이 (내가 허용하는 범위에 한해서만) 정보를 대신 제공해주는 대리인이 있으면 좋겠다는 순간이 있다. 그래서 이러한 순간을 해결하는 매커니즘이 생겨났고, 이를 OAuth라고 부른다.

OAuth에 대해 본격적으로 설명하기에 앞서 OAuth 인증에 참여하는 주체에 대한 명칭을 명확히 할 필요가 있다. 주체는 3개로 다음과 같다.

>* **Resource Owner**: 정보의 원래 소유주이다. 
>* **Client(Service Provider)**: 정보를 이용하고 싶은 주체이다
>* **Resource Server**: Resource Owner의 정보를 가지고 있는 주체로, Resource Owner가 허용하는 범위에 한해 Client에게 정보를 대신 전달해주는 역할을 수행한다.


_(Resource Owner를 나, Client를 서비스, Resource Server를 소셜로그인 기능을 제공해주는 SNS 플랫폼이라고 가정하면 주체의 역할을 쉽게 이해할 수 있다)_
![](https://images.velog.io/images/rlacksals96/post/ca5802a6-d783-44e0-9ed1-e70e35d2cd3a/IMG_014E9F0DD3C1-1.jpeg)  


`Client`가 `Resource Server`로부터 `Resource Owner`의 정보를 받는 가장 간단한 방법은 무엇일까? `Resource Owner`가 직접 `Resource Server`에 등록된 ID, Password를 직접 `Client`에게 전달하면, `Client`가 `Resource Server`에 접속하여 정보를 가져오는 것이다.

그러나 이러한 방식은 3개의 주체 모두에게 불편한 결과를 초래한다. 각각의 입장에서 다음과 같은 문제점을 가지고 있다.
> **Resource Owner**
_"내 ID, Password를 누군지도 모르는 Client에게 넘겨야 하는게 불안하다"_
> **Client**
> _"Resource Owner로부터 ID, Password를 받았지만 이 정보를 해킹당해서 정보를 유출시킬 리스크가 부담스럽다"_
> **Resource Server**
>_"누군지도 모르는 Client에게 내가 가진 Resource Owner의 개인정보를 함부러 제공하기 불편하다"_

<br></br>
이러한 상황에서 모두가 만족하는 방법이 있다. `Resource Owner`가 개인정보를 가지고 있는 `Resource Server`에게 `Client`가 개인 정보를 가져가도 된다고 **'동의'** 하면, `Resource Server`는 **Access Token**을 `client`에게 발급하고, `client`는 Resource Server로부터 개인정보를 가져갈 때마다 **Access Token**을 제시하는 것이다.
![](https://images.velog.io/images/rlacksals96/post/dc34788e-4bdc-4557-bbf1-3fdbe0e3663c/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-08-08%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%207.18.21.png)
## OAuth의 작동 원리
OAuth의 인증 방식을 대략적으로 이해하였으니 이제 자세하게 어떠한 방식으로 인증이 일어나는지 살펴보자
### 1. 등록
![](https://images.velog.io/images/rlacksals96/post/ffed082f-0f65-456f-bc23-6d9788187321/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-08-08%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%207.24.51.png)

`Client`가 `Resource Server`가 가진 정보를 얻기 위해서는, `Resource Server`에 `client`정보를 등록 해야한다. 요청하는 정보를 입력하면 `Resource Server`는 `client id`, `client secret`, `Authorized redirect URL`을 발급해준다.
>**client ID**: ID
>**client Secret**: Resource Server에 접근시 필요한 비밀번호로, 절대 노출되면 안되는 정보이다.
>**Authorized redirect URL**: 접근시 해당 URL을 통해서만 접근할 수 있다 


### 2. 승인
![](https://images.velog.io/images/rlacksals96/post/130b4420-c52f-42a6-9fad-b31598304d80/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-08-08%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%207.30.41.png)

`client`는 `Resource Owner`에게 정보 사용 동의를 위해 위 사진과 같이 `URL`을 전달한다. 해당 `URL`에는 다음과 같은 내용이 들어있다.
> **주소**: Resource Server의 주소
> **Client_ID**: Client ID
> **scope**: 개인정보 사용 범위
> ** redirect_URL**: Client에게 다시 접근하기 위한 URL

주소를 따라가면 `Resource Server`는 개인정보 제공 동의서를 `Resource Owner`에게 전달하고, `Resource owner`는 해당 동의서를 체크한 뒤 다시 `Resource Server`에게 전달한다.

`Resource Server`는 동의서의 내용을 바탕으로 데이터베이스에 `scope`를 저장한다.

![](https://images.velog.io/images/rlacksals96/post/bd59b158-c8be-492a-a333-9f812ecf0106/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-08-08%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%207.25.09.png)
`Resource Server`는 응답으로 `Callback_url?authorization code`를 전달한다. 해당 응답은 `Resource Owner`가 인지하지 못하게 자동으로 `Client`에게 전달한다.

`client`는 `Resource Owner`로부터 전달받은 `authorization code`를 이용하여 요청서를 `Resource Server`에게 전달하는데 내용은 다음과 같다
> **grant_type**: 어떠한 형태의 동의서인지 명시하는 속성이다. OAuth는 인증관련 이므로 'authorization code'로 정의한다.
> **code**:Resource Owner로부터 전달받은 autorization code이다
> ** client_id **: Resource Server에 등록할 때 발급받은 ID
> ** client_secret **:Resource Server에 등록할 때 발급받은 Password

`authorization code`의 경우 `Resource Server`가 `Resource Owner`에게만 발급한 코드이다. 따라서 해당 코드를 `client`가 전달했다는 의미는 `Resource Owner`가 `client`에게 해당 코드를 전달하는 경우 밖에 없으므로 `Resource Server`는 해당 요청을 신뢰할 수 있다.

### 3. 발급
![](https://images.velog.io/images/rlacksals96/post/e6bd61a8-c0b0-4bb5-8da6-74855e7e8870/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-08-08%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%207.52.13.png)
요청을 받은 `Resource Server`는 `client`에게 `Access Token`을 생성하여 해당 토큰이 누구의 개인정보를 얻기 위해 필요한 토큰인지 데이터베이스에 저장하고, `client`에게 전달한다. `client`도 해당 토큰을 데이터베이스에 저장하고, `Resource Owner`의 정보가 필요할 때마다 `Access Token`을 사용하여 정보 제공을 요청한다.



### 마무리
OAuth는 정의에서도 소개되어 있듯 개방형 표준이다. 따라서 많은 서비스 업체(google, facebook, kakao 등)에서 표준에 맞춰 OAuth 인증서비스를 제공하고 있다. 방법이 약간씩 차이가 있을 수 있지만 기본적인 매커니즘은 표준을 따르고 있다. 보다 자세한 내용에 대해 알고 싶다면 [RFC6749](https://datatracker.ietf.org/doc/html/rfc6749) 표준 웹문서를 참고하면 좋을 것 같다. 

_해당 내용은 '생활코딩'의 강의와 'RFC6749'의 내용을 참고하였으며 수정이 필요한 내용, 또는 보충이 필요한 내용이 있으면 덧글에 남기시면 적극적으로 수용하여 수정, 보충하겠습니다._

### 참고자료
생활코딩: [WEB2 - OAuth 2.0](https://opentutorials.org/course/3405)

RFC6749: [The OAuth 2.0 Authorization Framework](https://datatracker.ietf.org/doc/html/rfc6749)
