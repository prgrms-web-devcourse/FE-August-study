<p align="center">
<img src="https://images.velog.io/images/rlacksals96/post/6ab8b0c9-85aa-4c81-9b2a-e460ab2f9551/restapi.png"style="margin-bottom:5px">
  REST API Model
<p/>

사진출처: [cchloe2311.log](https://velog.io/@cchloe2311/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%82%B9-REST-API)


## REST & API 정의
REST(Representational State Transfer)
: 아키텍처의 제약 조건을 준수하는 애플리케이션 프로그래밍 인터페이스
> 리소스를 다룰때 인터페이스에서 정의한 방식으로만 다룰 수 있다는 의미이다.


API(Application Programming Interface)
: 애플리케이션 소프트웨어를 구축하고 통합하는 정의 및 프로토콜 세트
> 사용자와 클라이언트가 얻으려 하는 리소스 사이의 조정자라고 할 수 있다. 
>
>클라이언트의 시스템과 상호작용하여 정보를 검색하거나 기능을 수행하고자 할 때, API를 통해 요청사항을 전달 함으로써 시스템이 요청을 이해하고 응답할 수 있다.

## REST API의 탄생 배경
<p align="center">
<img src="https://images.velog.io/images/rlacksals96/post/3331c605-e82b-4418-9139-b3c66cf00135/%E1%84%85%E1%85%A9%E1%84%8B%E1%85%B5%E1%84%91%E1%85%B5%E1%86%AF%E1%84%83%E1%85%B5%E1%86%BC.jpeg" width="300px" height="300px" align="center" style="margin-bottom:5px" >
  
</p>


HTTP의 주요 저자중 한명인 로이 필딩(Roy Fielding)은 많은 사람들이 웹 설계의 우수성을 이해하지 못한채 사용하고 있던게 아쉬워 웹의 장점을 최대한 활용할 수 있는 아키텍처로 REST라는 개념을 제안하였다.

REST는 프로토콜이나 표준이 아닌 **아키텍처 원칙!**


## REST의 특징
* Uniform Interface
	:`URI`로 지정한 리소스를 인터페이스를 통해서만 조작하는 아키텍처 스타일을 사용한다
* Stateless
	: `state(상태)`를 저장하지 않는다. 상태를 저장하지 않으므로 서버에게 주는 부담을 줄일 수 있다.
* Cacheable
	: `HTTP`라는 웹 표준을 쓰기 때문에 웹에서 지원하는 캐싱 기능을 사용할 수 있다.
* Self-descriptiveness(자체표현구조)
	: 표현(REST API 메시지)만 보고도 어떤 요청인지 이해할 수 있다.
* Client-Server 구조
	: 서버와 클라이언트의 역할이 명확하다. 따라서 서로간에 의존성이 줄어든다.
* 계층형 구조
	: 다층으로 구성할 수 있기 때문에 보안, 암호화 계층을 별도로 추가할 수 있고, proxy, gateway 같은 네트워크 기반의 중간매체도 이용할 수 있다.
    
**다음 규칙들을 따르는 `API`를 `RESTful API`라고 한다**


## REST의 구성
1. 자원(Resource): URI
2. 행위(Verb): HTTP Method
3. 표현(Representation): Payload
![](https://images.velog.io/images/rlacksals96/post/61c05c41-264e-4499-9836-a6116be9b313/image.png)


## REST API 설계하기
**: 리소스 '자체'와 리소스에 대한 '행위'를 분리하여 설계해야 한다**

1. `URI`는 정보의 **리소스(자원)를 표현**하는데 사용해야 한다
```
GET /Users/1 (O)
```
-> 리소스이름은 주로 `명사`를 사용한다

```
GET /Users/update/1 (X)
```
-> `URI`는 **리소스 자체 위주로 표기**해야한다. 리소스에 대한 ~~행위~~ 를 표현하면 안된다!

2. 리소스에 대한 **행위**는 **HTTP Method**를 사용한다
_(다음 4개의 메소드를 통해 데이터 CRUD를 할 수 있다)_
	* POST: 데이터를 새로 추가할 때 사용
    * GET: 데이터를 받을 때만 사용
    * PUT: 기존의 데이터를 수정할 때 사용 
    * DELETE: 기존의 데이터를 삭제할 때 사용
   
## REST API 설계시 유의사항
1. `/`는 계층 관계를 나타내는데 만 사용한다
2. `URI`의 마지막에는 `/`를 사용하지 않는다.
3. `리소스명`의 가독성을 높이기 위해 `-`을 사용할 수 있다
(단, `_`는 사용하지 않는다)
4. `URI`에 `대문자` 사용은 지양한다.
-> 웹에서 대소문자를 구분하기 때문이다
-> RFC3986(URI 문법 형식)에서도 URI스키마와 호스트를 제외하고는 대소문자를 구별해서 사용하라고 규정하고 있다)
5. `Collection`, `Document` 개념의 리소스 사용시 단/복수를 구분하여 사용한다
```
ex) 
collection(document의 집합): books
document: history

GET : https://this.is.rest-api.com/library/books/history/1

```

## HTTP 상태 코드
|번호|힌트|내용|
|--|--|--|
|1xx|정보|요청을 받았으며 프로세스를 게속 진행한다|
|2xx|성공|요청을 성공적으로 받았으며 인식했고 수용했다|
|3xx|리다이렉션|요청 완료를 위해 추가 작업 조치가 필요하다|
|4xx|클라이언트 오류|요청의 문법이 잘못되었거나 요청을 처리할 수 없다|
|5xx|서버 오류|서버가 유효한 요청에 대한 충족을 실패했다|
자세한 상태 코드 내용은 [MDN: 상태 코드](https://developer.mozilla.org/ko/docs/Web/HTTP/Status)에서 확인할 수 있다.


## 추가 개념
* URI (Uniform Resource Identifier) : 네트워크 상에 존재하는 자원을 구분하는 식별자(ID)로서 의미가 강하다.
* URL (Uniform Resource Locator) : 네트워크 상에 존재하는 자원의 위치를 말합니다. 즉 자원의 어디에 있는지 나타내는 Where의 개념
* URN (Uniform Resource Name) : 자원의 이름을 나타내는 말이다. 즉 자원이 무엇인지를 말하는 What의 개념으로 이해할 수 있다. URN은 서로 중복되지 않는 유일한 값이어야 한다.


출처: https://nsinc.tistory.com/192 [NakedStrength]

## 참고자료
[레드햇: REST API란?](https://www.redhat.com/ko/topics/api/what-is-a-rest-api)
[NHN Cloud: REST API 제대로 알고 사용하기](https://meetup.toast.com/posts/92)
[MDN: HTTP 요청 메서드](https://developer.mozilla.org/ko/docs/Web/HTTP/Methods)
[HTTP 상태 코드 정리](https://www.whatap.io/ko/blog/40/)