## 📌 HTTP(HyperText Transfer Protocol)
**HTTP** : HTML 문서와 같은 리소스들을 가져올 수 있도록 해주는 프로토콜이다.

클라이언트와 서버들은 개별적인 메시지 교환에 의해 통신하는데, 보통 브라우저인 클라이언트에 의해 전송되는 메시지를 **요청(requests)**이라고 부르며, 그에 대해 서버에서 응답으로 전송되는 메시지를 **응답(responses)**이라고 부른다.

결국 HTTP는 클라이언트(대체로 브라우저) - 서버 프로토콜이라고 할 수 있다. 

![](https://images.velog.io/images/apparatus1/post/84ce62bb-cce0-4cae-9f31-feeb7dc83d16/image.png)

> 이미지 출처 : https://rendaritfactory.tistory.com/20
Browser → Server에 Request 하게되고, Server → Browser에 Response 한다.

HTTP 프로토콜은 클라이언트쪽에서 Request를 보내고 서버쪽에서 Response를 받으면 이어졌던 연결이 끊기게 되어있다. 그래서 화면의 내용을 갱신하기 위해서는 다시 request를 하고 response를 하며 페이지 전체를 갱신하게 된다. 이렇게 할 경우, 엄청난 자원낭비와 시간낭비를 초래하고 말 것이다.

이럴 때 사용하는 것이 **AJAX** 이다.

<br>
<hr>

## 📌 AJAX(Asynchronous JavaScript and XML)

**AJAX** : 동적인 웹 페이지를 만들기 위한 개발 기법의 하나로 웹 페이지 전체를 다시 로딩하지 않고도, 웹 페이지의 일부분만을 갱신할 수 있다. 백그라운드 영역에서 서버와 통신하여, 그 결과를 웹 페이지의 일부분에만 표시할 수 있으며 다음과 같은 데이터들을 서버와 주고받을 수 있다.

 - JSON
 - XML
 - HTML
 - 텍스트 파일

![](https://images.velog.io/images/apparatus1/post/d4c2bf03-7aed-4b0a-9d4f-0f5c63c72a72/image.png)

> 이미지 출처 : http://tcpschool.com/ajax/ajax_intro_works

AJAX는 HTML 페이지 전체가 아닌 일부분만 갱신할 수 있도록 XMLHttpRequest객체를 통해 서버에 request한다. 이 경우, XML이나 JSON형태로 필요한 데이터만 받아 갱신하기 때문에 그만큼의 자원과 시간을 아낄 수 있다.

<br>
<hr>


## 📌 XML(EXtensible Markup Language)

**XML**: HTML과 매우 비슷한 문자 기반의 마크업 언어이며, HTML과 달리 웹상에서 구조화된 문서를 전송가능하도록 설계되었다.

### XML의 특징
 - 소프트웨어나 하드웨어에 독립적으로 데이터를 저장하고 전달할 수 있다.
 - 새로운 운영체제나 프로그램, 브라우저 등에 상관없이 데이터를 안전하고 손쉽게 전달할 수 있다.
 - 수많은 종류의 데이터를 유연하고 자유롭게 기술하는 데 적용할 수 있어서 다양한 용도로 응용할 수 있으며, 인터넷으로 연결된 시스템끼리 쉽게 식별 가능한 데이터를 주고받을 수 있게 된다. 

![](https://images.velog.io/images/apparatus1/post/b8e26daf-871f-41e9-8639-a4081de43d9c/image.png)

> XML 태그 예시
* 이미지 출처 : https://dimestorerocket.com/read-a-xml-file-fast-with-csharp/
>
위 이미지에서 보듯 HTML과는 달리 어디서부터 어디까지가 Part_Name이고 어디서부터 어디까지가 Price인지 알 수 있다.

### XML의 문제점
 - 실제 데이터 양에 비해 많은 메모리를 차지한다.
 - 불필요한 태그들로 인해 가독성이 떨어진다.

이러한 문제점에  비해 간결하고 통일된 양식으로 각광을 받고 있는 것이 **JSON**이다.

<br>
<hr>

## 📌 JSON(Javascript Object Notation)

**JSON** : 자바스크립트 객체를 문자열로 변환하거나 반대로 변환하는 작업을 진행하는데 매우 경량화된 방법이다. XML데이터 형식에 비해 구조 정의의 용이성과 가독성이 뛰어나서 AJAX의 표준으로 사용된다.

![](https://images.velog.io/images/apparatus1/post/d95c37e7-5b99-4f38-ab8e-de66913100cd/image.png)

> 이미지 출처 : https://stackoverflow.com/questions/53694329/printing-json-data-into-innerhtml-getting-undefined-object

```javascript
// XML 방식
<dog>
    <name>식빵</name>
    <family>웰시코기<family>
    <age>1</age>
    <weight>2.14</weight>
</dog>

// JSON 방식
{
    "name": "식빵",
    "family": "웰시코기",
    "age": 1,
    "weight": 2.14
}
```

### JSON의 특징
 - Key: Value로 이루어진 File Format이다.
 - 종료 태그를 사용하지 않는다.
 - XML의 구문보다 더 짧다.
 - XML은 배열을 사용할 수 없지만, JSON은 배열을 사용할 수 있습니다.
 - 대부분의 언어나 플랫폼에 상관 없이 사용할 수 있다.
 (대부분의 언어들이 JSON으로 직렬화된 객체를 그 언어의 특징에 맞게 변환하는 것을 지원하거나 라이브러리를 통해 사용이 가능하다.)

<br>
<hr>

## 📌 참고한 사이트

https://developer.mozilla.org/ko/docs/Web/HTTP/Overview
http://tcpschool.com/ajax/ajax_intro_basic
https://velog.io/@surim014/AJAX%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80
https://namu.wiki/w/JSON