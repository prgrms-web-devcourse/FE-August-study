# HTTP란? (Hypertext Transfer Protocol)
HTTP는 Hyper Text Transfer Protocol의 두문자어,
인터넷에서 데이터를 주고받을 수 있는 프로토콜 (프로토콜: 규칙)
모든 프로그램이 이 규칙에 맞춰 개발해서 서로 정보를 교환

에러를 해결하는데 HTTP 지식이 중요
데이터를 주고 받을 때 흔히 발생하는
CORS, CORB 같은 에러들은 HTTP만 잘 알아도 쉽게 해결


## HTTP Request 구조
HTTP request 메세지는 크게 3부분으로 구성된다

### 1. start line (HTTP request의 첫 라인)
- **HTTP Method**
HTTP Methods에는 GET, POST, PUT, DELETE, OPTIONS 등등이 있다. 주로 GET 과 POST과 쓰임.

- **Request target**
해당 request가 전송되는 목표 uri.

- **HTTP Version**
말 그대로 사용되는 HTTP 버젼. 버젼에는 1.0, 1.1, 2.0 등이 있다.

### 2. headers (해당 request에 대한 추가 정보(addtional information)를 담고 있는 부분)
request 메세지 body의 총 길이(Content-Length) 등 해당 request에 대한 추가 정보(addtional information)를 담고 있는 부분
    Key:Value 값으로 되어있다. ex) HOST:google.com
- **Host**
요청이 전송되는 target의 host url: 예를 들어, google.com
- **User-Agent**
요청을 보내는 클라이언트의 대한 정보: 예를 들어, 웹브라우저에 대한 정보.
- **Accept**
해당 요청이 받을 수 있는 응답(response) 타입.
- **Connection**
해당 요청이 끝난후에 클라이언트와 서버가 계속해서 네트워크 컨넥션을 유지 할것인지 아니면 끊을것인지에 대해 지시하는 부분.
- **Content-Type**
해당 요청이 보내는 메세지 body의 타입. 예를 들어, JSON을 보내면 application/json.
- **Content-Length**
메세지 body의 길이
    
### 3. body (해당 reqeust의 실제 메세지/내용)
Body가 없는 request도 많다. 예를 들어, GET request들은 대부분 body가 없는 경우가 많음.
    
```json
    Accept: application/json
    Accept-Encoding: gzip, deflate
    Connection: keep-alive
    Content-Length: 83
    Content-Type: application/json
    Host: intropython.com
    User-Agent: HTTPie/0.9.3

    {
        "imp_uid": "imp_1234567890",
        "merchant_uid": "order_id_8237352",
        "status": "paid"
    }
```

## HTTP Response 구조
Response도 request와 마찬가지로 크게 3부분으로 구성되어 있다.

### 1. Status Line (Response의 상태를 간략하게 나타내주는 부분. 3부분으로 구성되어 있다.)
- HTTP 버젼
- Status code: 응답 상태를 나타내는 코드. 숫자로 되어 있는 코드. 예를 들어, 200
- Status text: 응답 상태를 간략하게 설명해주는 부분. 예를 들어, "Not Found"

    ```
    HTTP/1.1 404 Not Found
    ```

    #### 자주 쓰이는 HTTP Response Status Code

    - 200 OK
        가장 자주 보게 되는 status code.
        문제없이 다 잘 실행 되었을때 보내는 코드.

    - 301 Moved Permanently
        해당 URI가 다른 주소로 바뀌었을때 보내는 코드.
        HTTP/1.1 301 Moved Permanently
        Location: http://www.example.org/index.asp

    - 400 Bad Request
        해당 요청이 잘못된 요청일때 보내는 코드.
        주로 요청에 포함된 input 값들이 잘못된 값들이 보내졌을때 사용되는 코드.
        예를 들어, 전화번호를 보내야 되는데 text가 보내졌을때 등등.
    - 401 Unauthorized
        유저가 해당 요청을 진행 할려면 먼저 로그인을 하거나 회원 가입을 하거나 등등이 필요하다는것을 나타내려 할때 쓰이는 코드.

    - 403 Forbidden
        유저가 해당 요청에 대한 권한이 없다는 뜻.
        예를 들어, 오직 과금을 한 유저만 볼 수 있는 데이터를 요청 했을때 등.
        
    - 404 Not Found
        요청된 uri가 존재 하지 않는다는 뜻.
       
    - 500 Internal Server Error
        서버에서 에러가 났을때 사용되는 코드.
        API 개발을 하는 백앤드 개발자들이 싫어하는 코드.

### 2. Headers
Request의 headers와 동일하다.
    다만 response에서만 사용되는 header 값들이 있다.
    예를 들어, User-Agent 대신에 Server 헤더가 사용된다.

### 3. Body
Response의 body와 일반적으로 동일하다. Request와 마찬가지로 모든 response가 body가 있지는 않다. 데이터를 전송할 필요가 없을경우 body가 비어있게 된다.

- Response Example

    ```
    HTTP/1.1 404 Not Found

    Connection: close
    Content-Length: 1573
    Content-Type: text/html; charset=UTF-8
    Date: Mon, 20 Aug 2018 07:59:05 GMT

    <!DOCTYPE html>
    <html lang=en>
    <meta charset=utf-8>
    <meta name=viewport content="initial-scale=1, minimum-scale=1, width=device-width">
    <title>Error 404 (Not Found)!!1</title>
    <style>
        *{margin:0;padding:0}html,code{font:15px/22px arial,sans-serif}html{background:#fff;color:#222;padding:15px}body{margin:7% auto 0;max-width:390px;min-height:180px;padding:30px 0 15px}* > body{background:url(//www.google.com/images/errors/robot.png) 100% 5px no-repeat;padding-right:205px}p{margin:11px 0 22px;overflow:hidden}ins{color:#777;text-decoration:none}a img{border:0}@media screen and (max-width:772px){body{background:none;margin-top:0;max-width:none;padding-right:0}}#logo{background:url(//www.google.com/images/branding/googlelogo/1x/googlelogo_color_150x54dp.png) no-repeat;margin-left:-5px}@media only screen and (min-resolution:192dpi){#logo{background:url(//www.google.com/images/branding/googlelogo/2x/googlelogo_color_150x54dp.png) no-repeat 0% 0%/100% 100%;-moz-border-image:url(//www.google.com/images/branding/googlelogo/2x/googlelogo_color_150x54dp.png) 0}}@media only screen and (-webkit-min-device-pixel-ratio:2){#logo{background:url(//www.google.com/images/branding/googlelogo/2x/googlelogo_color_150x54dp.png) no-repeat;-webkit-background-size:100% 100%}}#logo{display:inline-block;height:54px;width:150px}
    </style>
    <a href=//www.google.com/><span id=logo aria-label=Google></span></a>
    <p><b>404.</b> <ins>That’s an error.</ins>
    <p>The requested URL <code>/payment-sync</code> was not found on this server.  <ins>That’s all we know.</ins>
    ```



# HTTPS란? (Hypertext Transfer Protocol Secure)
    
HTTPS = HTTP + SSL

- HTTP = Hyper Text Transfer Protocol. Hypertext인 html을 전송하기 위한 통신 규약
- HTTPS = HTTP + S(over Secure socket layers). 보안이 강화된 HTTP
    
SSL(Secure Socket Layer) = TLS(Transport Layer Security)
인터넷에서 정보를 암호화해서 송수신하는 프로토콜로 넷스케이프가 개발.
    국제 인터넷 표준화 기구에서 표준으로 인정받은 프로토콜임.
    표준에 명시된 정식 명칭은 TLS이지만 아직도 SSL이라는 용어가 많이 사용됨.
    고로 HTTP에 SSL이 첨가된 방식으로 주고 받는 정보가 암호화되어 보안성이 강화된 방식이다.



## HTTP의 문제

1. **암호화 기능 없음**
    단순 text형식으로 주고받기 때문에, 중간에서 누군가가 신호를 가로챈다면 내용이 그대로 노출된다.

2. **신뢰할 수 있는 사이트인지 확인 불가**
    통신하려는 사이트를 따로 확인하는 작업이 없어 다른 사이트가 통신하려는 사이트로 위장 가능

3. **통신 내용 변경 가능**
    요청을 보낸 곳과 받은 곳의 리소스가 정확히 일치하는지 확인할 수 없다.
    누군가가 중간에 데이터를 악의적으로 변조한다면 정확한 데이터를 주고받을 수 없게된다.

## HTTPS 문제 해결 방식

### 1. 기밀성: 암호화 기능
HTTP 메세지를 암호화해서 보내준다. 데이터를 암호화해서 요청을 보내고, 받는 곳에서 복호화하여 메세지를 받는다.
    따라서, 복호화를 하지 못하면 중간에 누군가 가로채어도 내용을 알아볼 수 없다.
    암호화/복호화 방식에는 대칭키 방식과, 비대칭키 방식이 있다.

[HTTP에서 HTTPS로 전환하기 위한 완벽 가이드 링크보기](https://webactually.com/2018/11/16/http%EC%97%90%EC%84%9C-https%EB%A1%9C-%EC%A0%84%ED%99%98%ED%95%98%EA%B8%B0-%EC%9C%84%ED%95%9C-%EC%99%84%EB%B2%BD-%EA%B0%80%EC%9D%B4%EB%93%9C/)

#### 암호화 두가지 주요 방법

- **대칭 암호화**
양쪽 당사자가 공통 비밀 키를 공유한다.
            대칭형 방식은 양쪽 당사자가 공유한 시크릿에 의존하는데, 전송자는 정보를 암호화하는 데 사용하고
            수신자는 동일한 방식과 키를 사용해 복호화한다. 이 방법의 문제는 양쪽 당사자가 서로 물리적인 만남 없이 시크릿을 협상(교환)하는 방법이라서 일종의 보안 통신 채널이 필요하다.

- **비대칭 암호화**
당사자 중 한쪽이 비밀 키와 공개 키의 쌍, 공개 키 인프라(PKI) 기반을 갖는다.
            공개 키와 개인 키의 개념을 기반으로 하는 비대칭 방식은 대칭 방식의 문제를 해결한다.
            두 가지 키 중 하나로 평문을 암호화하면 다른 보완 키를 사용해야만 복호화할 수 있다.
            이를테면 서로 안전하게 통신하고 싶은 두 당사자 앨리스와 밥이 있다고 가정하자.
            (앨리스와 밥은 모든 튜토리얼과 보안 매뉴얼에 항상 등장하는 허구의 인물이므로 여기서는 그러한 전통을 따랐다)

  1. 앨리스와 밥은 공개 키와 개인 키의 쌍을 가졌다. 개인 키는 각 소유자만 알고 있으며 공개 키는 누구든 사용할 수 있다.
  2. 앨리스가 밥에게 메시지를 보내고 싶다면, 앨리스는 밥의 공개 키를 얻어 평문을 암호화하고 암호문을 밥에게 보낸다.
  3. 밥은 자신의 개인 키를 사용해 암호문을 복호화한다.
  4. 밥이 앨리스에게 회신하고 싶다면, 앨리스의 공개 키를 얻어서 평문을 암호화해 암호문을 보낸다.
  5. 엘리스는 자신의 개인 키를 사용해 그 암호문을 복호화한다.
            

#### 언제 대칭 암호화를 사용하고, 언제 비대칭 암호화를 사용할까?

1. 비대칭 암호화는 클라이언트와 서버 간 시크릿을 교환할 때 사용한다.
실생활에서는 양방향 비대칭 통신이 필요하지 않다. 당사자 중 한쪽이(서버라고 하자) 일련의 키를 가지고 있으므로 암호화된 메시지를 받을 수 있다는 것만으로 충분하다. 이는 공개 키로 암호화한 정보는 개인키를 사용해야만 복호화되기 때문에 클라이언트에서 서버로 향하는 단방향으로만 정보를 보호한다. 그러므로 서버에서만 그 정보를 복호화할 수 있다. 반대 방향은 보호되지 않는다. 서버의 개인 키로 암호화된 정보는 공개 키를 가진 누구든지 복호화할 수 있다. 상대편(클라이언트라고 하자)은 서버의 공개 키를 사용해 무작위로 생성된 세션 시크릿을 암호화해 통신을 시작한다. 그 다음 암호문을 다시 서버로 보내고, 서버는 다시 자신의 개인 키로 복호화하면 그 시크릿을 갖게 된다.

2. 대칭 암호화는 비대칭 암호화보다 훨씬 빠르기 때문에 전송 중인 실제 데이터를 보호하는 데 사용된다.
        앞서 교환한 시크릿으로 정보를 가진 두 당사자(클라이언트와 서버)만 해당 정보를 암호화하고 복호화할 수 있다.
        이 때문에 핸드셰이크handshake의 첫 비대칭 부분이 키 교환이라고 불리며, 실제 암호화된 통신은 사이퍼 메서드cipher methods라는 알고리즘을 사용한다.

### 2. 인증(Authentication): 웹사이트의 진위 증명
SSL(Secure Socket Layer)은 클라이언트와 서버간의 통신을 제 3자가 보증해주는 인증서를 제공한다.
    클라이언트가 서버에 접속하면 서버는 클라이언트에게 인증서 정보를 제공한다.
    클라이언트는 이 인증서 정보를 검증한 후에 다음 절차를 수행하기 때문에 신뢰성을 확인할 수 있다.
    또한, SSL은 요청과 응답의 변조를 막기 위해 MD5, SHA-1과 같은 해시값을 확인하는 방법과 파일의 디지털 서명을 확인하는 방법을 사용한다.
    따라서, 데이터의 완전성을 증명할 수 있다.

SSL은 보안과 성능상의 이유로 대칭키와 비대칭키 암호화 방식을 사용하고 있다.
    대칭키는 보안이 취약한 단점이 있지만 처리 속도가 빠르다는 장점이 있다.
    반면, 비대칭키는 처리속도가 느리지만 보안이 강한 장점이 있다.
    통신할 때마다 비대칭키를 이용하면 보안 문제가 해결될 수 있으나, 컴퓨터에 많은 부담을 준다.
    따라서, 처음 SSL 인증서를 제공하여 검증하는 과정은 보안이 강한 비대칭키 방식을 사용하고,
    이후에는 대칭키를 사용하여 컴퓨터에 부담을 덜어준다.
    
### 3. 무결성: 데이터 변조 불가능
변조되지 않은 정보로 목적지에 도달하게 한다. 
    예를 들어, 와이파이가 웹사이트에 광고를 추가하거나, 대역폭을 절약하고자 이미지 품질을 저하시키거나
    읽는 기사의 내용을 변조할 수 있지만 HTTPS는 웹사이트를 변조할 수 없도록 한다.
