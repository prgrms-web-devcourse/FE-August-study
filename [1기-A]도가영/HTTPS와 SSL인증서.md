## # HTTPS

### 1. HTTP over SSL

#### SSL(=TLS) 프로토콜 위에서 동작하는 HTTP 프로토콜

![https://images.velog.io/images/young18/post/d03e5ed1-4fe5-44f2-b69f-904ced3cfd39/1.jpg](https://images.velog.io/images/young18/post/d03e5ed1-4fe5-44f2-b69f-904ced3cfd39/1.jpg)

> SSL(Secure Sockets Layer, 보안 소켓 레이어)
TLS(Transport Layer Security, 전송 계층 보안)</span>
네스케이프에 의해서 SSL이 발명되어 통용되다가 IETF의 관리로 변경되면서 표준화된 이름의 TLS로 바뀌었다. TLS 1.0은 SSL 3.0을 계승한다.
그러나 여전히 TLS라는 이름보다 SSL이라는 이름으로 많이 사용되고 있다.

<br/>

### 2. HTTP의 약점

#### **도청의 위험**

- 악의를 가진 사람이 기기를 통해 통신 내용을 도청할 수 있습니다.

#### **통신 상대의 신원 보장이 없음**

- Client를 확신할 수 없습니다.
→ HTTP는 누가 Request를 보내도 Response하는 구조입니다. 신원이 보장된 특정 Client와만 통신할 수 없습니다
또한 대량의 Request를 통한 Dos 공격의 위험이 있습니다.
- Server를 확신할 수 없습니다.
→ Response를 보낸 Server가 내가 의도한 Server인지 확신할 수 없기 때문에 위장 Server라는 위험성이 있습니다.

#### **변조 가능성**

- Client와 Server가 보낸 정보를 중간에 누군가 바꿀 위험성이 있습니다.

<br/>
<br/>

## # SSL (Secure Socket Layer)

### 1. 목적

#### - 도청의 위험 ⇒ **암호화**

#### - 통신 상대의 위장 가능성 ⇒ **인증**

#### -  변조 가능성 ⇒ **무결성 보장**

<br/>

### 2. SSL의 암호화 방식

#### **대칭키 (비밀키)**

- 서버와 클라이언트 모두 개인키(비밀키)를 가지고 있어야 합니다.
- 클라이언트가 키를 가지고 있기에는 위험성이 높습니다.

![](https://images.velog.io/images/young18/post/aef561a5-a189-41e1-9713-86e514a24fd8/Untitled.png)

#### **비대칭키 (공개키 + 개인키)**

- 공개키는 클라이언트(정보를 보내는 쪽)가 데이터를 암호화하는데 쓰입니다.
- 개인키(비밀키)는 서버가 공개키로 암호화된 데이터를 복호화 할 수 있는 키입니다.
- 안전한 방법이지만 속도가 느리다는 단점이 있습니다.

![](https://images.velog.io/images/young18/post/83a1df9e-d1ab-4230-a9db-68468354bf2f/%E1%84%87%E1%85%B5%E1%84%83%E1%85%A2%E1%84%8E%E1%85%B5%E1%86%BC%E1%84%8F%E1%85%B5.png)


#### 💡 결론적으로 HTTPS 통신을 할 때는 **대칭키와 비대칭키 방식을 모두 사용**합니다.

- 실제 전송하는 데이터 암호화 →  대칭키 방식 (session key 값)
- 대칭키 암호화 → 공개키 방식

<br/>

### 3. 인증서와 CA

#### SSL 인증서

#### - 역할

1. 클라이언트가 접속한 서버가 **신뢰 할 수 있는 서버임을 보장**합니다.
2. SSL 통신에 사용할 **공개키를 클라이언트에게 제공**합니다.

#### - 내용

1. **서비스의 정보** (인증서를 발급한 CA, 서비스의 도메인 등등)
2. **서버 측 공개키** (공개키의 내용, 공개키의 암호화 방법)

<br/>

#### CA (Certificate authority)
- **도메인 소유권을 인증하고, 인증서 및 개인키를 생성**해주는 민간기업
- 브라우저는 이러한 CA 리스트와 CA의 공개키를 가지고 인증을 진행합니다.

<br/>

### 4. 동작 방식

```악수: 암호화 방식에 대한 클라이언트-서버 간 협상 단계```

1. **Client Hello**
브라우저 마다 지원하는 암호화 알고리즘과 TLS 버전이 다르므로 해당 정보를 전송하며, 랜덤 값을 생성하여 전송합니다.
2. **Server Hello**
사용할 TSL 버전, 사용할 암호화 알고리즘, 랜덤 값을 전송합니다.

```인증서의 신뢰성 검토 단계```

3. **Certificate**
CA로 부터 발급받은 인증서를 전송합니다.
브라우저는 인증서가 신뢰할 수 있는 것인지 검토합니다.

```키 값(pre master secret) 공유 단계 - 공개키 방식```

4. **pre master secret 암호화 및 전송**
브라우저가 앞서 주고받은 랜덤 값을 이용해 pre master secret 키를 생성한 후 
서버의 공개키를 이용해서 암호화하여 서버로 전달합니다.
5. **master secre**t
서버는 자신의 개인키(비밀키)로 복호화를 하여 master secret 값으로 만듭니다.

```session key 공유 및 데이터 전송 단계 - 대칭키 방식```

6. master secret → **session key 생성 (공유)
→ 데이터 전송 시** session key를 이용하여 **대칭키 방식으로 암호화하는데 사용**됩니다.

```종료: handshake 종료를 서로에게 알린다.```

<br/>
<br/>

## # HTTP가 HTTPS가 되는 과정

#### 2014년부터 [구글의 SEO 정책](https://developers.google.com/search/docs/beginner/seo-starter-guide?hl=ko)에서 https인 사이트에 가산점을 주며 권장하면서부터 대부분의 사이트가 HTTPS를 선호하는 추세입니다.

<br/>
<br/>
