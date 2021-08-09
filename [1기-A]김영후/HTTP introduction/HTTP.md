# HTTP 개요 입문

# HTTP란?

- 웹 브라우저와 서버가 통신하기 위한 프로토콜
- 팀 버너스 리 고안
- 1991년 최초 문서화 HTTP/0.9
- 1996년 HTTP/1.0
- 1999년 HTTP/1.1
- 2015년 HTTP/2
- HTTP 버전을 매기는 방식이 존재(학습하진 못함)
- HTTP 버전 형식은 HTTP/<메이저>.<마이너>
- 메이저와 마이너는 정수

# 웹 클라이언트와 서버의 통신
- 주소창에 http://www.oreily.com/index.html 입력시,
  ![그림1](https://github.com/prgrms-web-devcourse/FE-August-study/blob/Week1/mooomeeen%5DStudy/%5B1%EA%B8%B0-A%5D%EA%B9%80%EC%98%81%ED%9B%84/HTTP%20introduction/image/image1.png)


# 리소스
- 웹 리소스(렌더링에 필요한 모든 데이터) 관리 및 제공
- 가장 간단한 웹 리소스로는 웹 서버 파일 시스템에 있는 정적 파일 데이터

# 미디어 타입(MIME, Multipurpose Internet Mail Extension)
- 웹 서버는 모든 HTTP 객체 데이터에 MIME 타입을 붙임
- 웹 브라우저는 MIME 타입을 통해서 다룰 수 있는 객체인지 확인
  ![그림2](https://github.com/prgrms-web-devcourse/FE-August-study/blob/Week1/mooomeeen%5DStudy/%5B1%EA%B8%B0-A%5D%EA%B9%80%EC%98%81%ED%9B%84/HTTP%20introduction/image/image2.png)

# URI(Uniform Resource Identifier)
- 웹 서버의 웹 리소스는 URI를 가짐. = 클라이언트가 지목 가능
- 이후 HTTP에 의해서 URI가 해석
- URI는 URL과 URN으로 구성
  ![](./image/image3.PNG)
- 해석과정
- (1) HTTP 프로토콜을 이용하라
- (2) www.조하드웨어.com으로 이동하라
- (3) specials/saw-blade.gif 리소스를 가져와라

# URL(Uniform Resource Locator)
- 리소스에 대한 구체적인 위치 서술
- 예)
- http://www.oreily.com/index.html
- http://www.yahoo.com/images/logo.gif
- http://www.joes-hardware.com/inventory-check.cgi?item=12731
![그림3](https://github.com/prgrms-web-devcourse/FE-August-study/blob/Week1/mooomeeen%5DStudy/%5B1%EA%B8%B0-A%5D%EA%B9%80%EC%98%81%ED%9B%84/HTTP%20introduction/image/image3.png)
> 1.  scheme으로, 리소스에 접근하기 위한 프로토콜
> 2. 서버의 인터넷 주소(www.joes-hardware.com)
> 3. 웹 서버의 리소스(/specials/saw-blade.gif)

# URN(Uniform Resource Name)
- URN은 리소스의 위치에 영향 받지 않는 유일무이한 이름
> **URL과 URN의 구분 방법**
>
> URN은 위치(주소)나 접근법에 대한 명시 없이, 리소스에 대해 이야기 할 때 사용. 예를들면, ISBN 시스템에서 0-486-27557-4는 셰익스피어의 작품 로미오와 줄리엣의 특정 에디션을 지칭. 이를 URN으로 나타내면, urn:isbn:0-486-27557-4로 표기. 이 표기법은 **리소스에 어떻게 접근할 것인지를 명시하지 않으며, 리소스 자체를 특정하는 것을 목표**

> **ISBN이란**
>
> 전 세계적으로 쏟아져 나오는 방대한 양의 서적을 체계적으로 분류하기 위해 국제적으로 정한 도서표준 고유코드 번호. 세계 어디서나 통용될 수 있으며, 번호만으로 어느 나라 어느 출판사에서 나온 책인지 알 수 있음.

# HTTP 트랜잭션
- 요청 명령과 응답 결과로 구성
- 이 과정에서 정형화된 데이터인 HTTP 메시지 이용

# 메서드
- 서버에게 어떤 동작을 취해야 하는지 알려줌
- 모든 HTTP 요청 메시지는 한 개의 메서드를 가짐
> 1. GET: 서버에서 클라이언트로 지정한 리소스를 보내라
> 2. PUT: 클라이언트에서 서버로 보낸 데이터를 지정한 이름의 리소스로 저장하라
> 3. DELETE: 지정한 리소스를 서버에서 삭제해라
> 4. POST: 클라이언트 데이터를 서버 게이트웨이 애플리케이션으로 보내라
> 5. HEAD: 지정한 리소스에 대한 응답에서 HTTP 헤더 부분만 보내라

# 상태코드
- 모든 HTTP 응답 메시지는 상태 코드와 함께 반환
- 요청이 성공했는지, 조치가 필요한지 등을 알려주는 세 자리 숫자
- 예) 200(문서가 바르게 처리됨), 302(Redirection), 404(리소스를 찾을 수 없음)
- 사유구절(reason phrase)와 함께 반환
- 예) 200 OK, 200 Success, 200 All's cool, dude

# HTTP는 여러 객체로 이루어질 수 있다
- 브라우저는 시각적으로 풍부한 웹페이지를 가져올 때 대량의 HTTP 트랜잭션을 수행
- 페이지 레이아웃을 서술하는 HTML ‘뼈대’를 한 번의 트랜잭션(적색박스)으로 가져온 뒤, 첨부된 이미지, 그래픽 조각, 자바 애플릿 등을 가져오기 위해 추가로 HTTP 트랜잭션 수행. 
- 리소스들은 서로 다른 서버에 위치할 수 있음
![그림4](https://github.com/prgrms-web-devcourse/FE-August-study/blob/Week1/mooomeeen%5DStudy/%5B1%EA%B8%B0-A%5D%EA%B9%80%EC%98%81%ED%9B%84/HTTP%20introduction/image/image4.png)

# HTTP 메시지
- 줄 단위의 문자열로, 일반 텍스트이기 때문에 사람이 읽고 쓰기 쉬움
- HTTP 요청 메시지, HTTP 응답 메시지로 구성
![그림5](https://github.com/prgrms-web-devcourse/FE-August-study/blob/Week1/mooomeeen%5DStudy/%5B1%EA%B8%B0-A%5D%EA%B9%80%EC%98%81%ED%9B%84/HTTP%20introduction/image/image5.png)
> 1. 요청줄: HTTP메서드, 대상(URL), HTTP버전
> 2. 응답줄: HTTP버전, 상태코드
> 3. 헤더: 요청 또는 응답에 대한 부가적인 정보, 헤더나 엔터티 본문이 있든 없든 항상 빈 줄(CRLF)로 끝남!
> 
> 4. 헤더의 종류로는 공통헤더, 요청헤더, 응답헤더, 엔터티헤더 존재 
> 5. 엔터티 본문: 가공되지 않은 데이터로, 엔터티 헤더는 데이터의 의미를 설명함
> 6. 엔터티 헤더로는 Content-type, Content-Length 등이 존재

---

## 참고자료
[HTTP 완벽 가이드](http://www.kyobobook.co.kr/product/detailViewKor.laf?ejkGb=KOR&mallGb=KOR&barcode=9788966261208&orderClick=LAG&Kc=)

[쉽게 배우는 운영체제](http://www.kyobobook.co.kr/product/detailViewKor.laf?ejkGb=KOR&mallGb=KOR&barcode=9791156644071&orderClick=LAG&Kc=)
