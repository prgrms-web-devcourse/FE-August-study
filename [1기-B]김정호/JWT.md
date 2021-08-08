[포스팅 페이지 주소](https://velog.io/@flareon/CS-%EC%8A%A4%ED%84%B0%EB%94%94)

# JWT (JSON Web Token)

#### 기본 개념

     1. 토큰 기반 인증 시스템의 구현체
     2. 웹 표준 (RFC 7519)에 등록되어있다.
     3. JSON 객체를 사용하여 정보를 안전성 있게 전달 가능
     4. 수많은 프로그래밍 언어에서 지원이 가능

#### 사용처

##### 1.회원 인증

![](https://images.velog.io/images/flareon/post/4b899202-5ec9-4c26-91d3-d8eddd97c052/image.png)

#### 회원인증 방식 단계

    1. ID와 PWD로 로그인
    2. 서버측에서 해당 계정정보를 검증
    3. 유저에게 토큰 발급
    4. 유저는 전달받은 토큰을 저장해두고, 요청 필요시 해당 토큰을 함께 서버에 전달.
    5. 서버는 토큰을 검증하고, 요청에 응답.

**여기서 사용되는 토큰이 바로 JWT!**

#### 2.정보 교류

- 정보가 sign되어있기 때문에 정보를 보낸이가 바뀌거나 조작 되었는지에 대해검증이 가능하기에 두 개체 사이에 안정성 있게 정보의 교환이 가능.

#### JWT의 구성

> ![](https://images.velog.io/images/flareon/post/dc9acdf2-6a47-4f10-b5c5-54f09d8e14f5/image.png)헤더, 내용, 서명의 세 파트로 이루어져 있고, 각각의 파트는 '.' 로 구분한다.

#### 1. 헤더 (Header)

- ** Header의 속성**
  - typ: 토큰의 타입을 지정. (JWT)
  - alg: 해싱 알고리즘을 지정.
    -HMAC SHA256 혹은 RSA를 사용한다.
    > ![](https://images.velog.io/images/flareon/post/ab5d27a6-9566-4b6d-9b9e-0b6bd164c3a4/image.png)헤더의 내용을 base64로 인코딩하고 JWT의 헤더 부분에 담는다.

#### 2. 내용 (Payload)

- ** Payload의 속성 **
  - 등록된 (registered) 클레임
    - 서비스에서 필요한 정보들이 아닌, 토큰에 대한 정보들을 담기 위하여 **이름이 이미 정해진 클레임**
    1. **iss** : 토큰 발급자 (issuer)
    2. **sub** : 토큰 제목 (subject)
    3. **aud** : 토큰 대상자 (audience)
    4. **exp** : 토큰의 만료시간 (expiraton)
    5. **nbf** : Not Before 를 의미, 토큰의 활성 날짜
    6. **iat** : 토큰이 발급된 시간 (issued at) -토큰의 age 판단기준
    7. **jti** : JWT의 고유 식별자, 중복적인 처리 방지-일회용 토큰에 사용
       **등록된 클레임의 사용은 모두 선택적이다.**
  - 공개 (public) 클레임
    - JWT를 사용하는 사람들에 의해 정의되는 클레임
    - 클레임 충돌을 피하기 위해서 URI형식으로 정의
  - 비공개 (private) 클레임 + 클라이언트와 서버간 협의하에 사용되는 클레임 이름들 + 클레임 이름 중복에 의한 충돌 유의
    > ![](https://images.velog.io/images/flareon/post/66c2a78a-492c-4310-b5c9-9a3e2ae88b92/image.png)내용(Payload)를 base64로 인코딩하여 내용 부분에 담는다.

#### 3. 서명 (Signature)

- 헤더의 인코딩값 + ' . ' + 정보의 인코딩 값 으로 비밀키로 해쉬하여 생성한다.
  > ![](https://images.velog.io/images/flareon/post/7a0177c9-9685-46ad-a499-26ce7b78f4dd/image.png) 합친 문자열을 alg에 입력한 암호화 알고리즘을 사용하여 암호화 하고, base 64로 인코딩한 다음 서명(signautre)부분에 담는다.
