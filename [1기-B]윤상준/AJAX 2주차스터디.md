# Asynchronous JavaScript and XML

ajax에 대해 얘기전에 용어 정리를 먼저 하겠습니다.

> Asynchronous : 비동기적
> Javascript : 웹을 동적으로 만들기 위해 고안된 프로그래밍 언어
> XML : 데이터 통신에 쓰이는 마크업 언어

### 그래서 AJAX가 뭔데?

ajax는 xml,javascript 뿐만 아니라 웹을 이루는 파일들(html,javascript,css 등등) 과 가장 중요한 XMLHttpsRequest 객체에 대한 새로운 접근법입니다.

> AJAX 이전의 방식
> 서버---> (html,javascript,css)-----> 브라우저(초기 렌더링)
> 변경사항 발생
> 서버---> (html,javascript,css)-----> 브라우저(새로고침 랜더링)

> AJAX방식
> 서버---> (html,javascript,css)-----> 브라우저(초기 랜더링)
> 변경사항 발생
> 서버---> (json)-----> 브라우저(바뀐부분만 랜더링)

기존에 페이지를 수정하는 방법은 서버로부터 모든파일을 다시 받아와서 렌더링하는 방식이였다면
AJAX방식은 부분적으로만 필요한 데이터만을 받아서 부분적으로 렌더링을 하는 방식입니다.

#### AJAX의 특징

- 부분적 데이터 통신
- 부분적 렌더링
- 비동기 통신

# JSON

AJAX의 특징중에 부분적인 데이터 통신을 가능하게 해주는 JSON에 대해 알아보겠습니다.

JSON은 http 통신을 위한 데이터 포맷입니다. 이는 자바스크립트에서만 쓰이지않고 여러 프로그래밍언어에서 범용적으로 쓰입니다.

```javascript
{
  "name" : "Leo",
  "age" : 26,
  "addess" : "seoul"
}
```

자바스크립트에서 객체와 비슷하게 생겼습니다. 다른점이라고 하면 자바스크립트에서 Key값에는 따옴표를 쓰지않아도 되지만 json에서는 key값에는 큰따음표로 처리를 해줘야합니다.(작은 따음표로 하면 안됩니다.)

### JSON.stringify( )

자바스크립트에서 객체를 데이터로 보내기 위해서 JSON파일로 변환해주는 메서드입니다.

```javascript
let object = {
  name: "leo",
  age: 26,
};
const jsonData = JSON.stingify(object);
console.log(jsonData); //{"name":"leo","age":26}
console.log(object);
```

<img src="https://media.vlpt.us/images/alajillo/post/99f72fc7-9727-47b7-9534-a9271d5fcccc/JSON.PNG">

출력값을 보면 name에 큰따음표가 생긴것을 알수 있습니다.

### JSON.parse( )

이제 받아온 json 데이터를 자바스크립트에서 활용할수 있도록 객체로 변환해주는 JSON.parse() 메서드를 알아보겠습니다.

```javascript
const objectData = JSON.parse(jsonData);
console.log(objectData); // { name: 'leo', age: 26 }
```

```javascript
JSON.parse("{}"); // {}
JSON.parse("true"); // true
JSON.parse('"foo"'); // "foo"
JSON.parse('[1, 5, "false"]'); // [1, 5, "false"]
JSON.parse("null"); // null
```

출력값을 보면 name에 있던 큰따음표가 사라졌습니다.

이런식으로 서버와의 데이터 통신에는 JSON이 사용되고 있으며 데이터를 주고 받기 위해서 JSON.stringify와 JSON.parse를 이용해서 처리를 해줍니다.

# XMLHttpRequest

XMLHttpsRequest는 Web API로 HTTP 통신에 필요한 다양한 기능을 제공하는 객체입니다.

```javascript
// XMLHttpRequest 객체 생성
const com = new XMLHttpRequest();

const data = JSON.stringify({ name: "leo" });
// HTTP 요청 초기화
com.open("GET", "/users");

//HTTP 요청 헤더 설정
//클라이언트가 서버로 전송할 데이터의 MIME 타입 지정 : json
com.setRequestHeader("content-type", "application/json");

//HTTP 요청 전송
com.send(data);
```

위에 코드를 하나하나 풀어보겠습니다.

### new XMLHttpRequest( )

"new XMLHttpRequest()"를 통해 XMLHttpRequest 객체를 생성합니다. 이는 기본적으로 제공되는 Map,Set 객체와 같습니다.

### XMLHttpRequest.open( )

XMLHttpRequest를 만들었으니 초기화를 해줘야합니다. .open을 통해 초기화를 할수 있고 인자로 들어가는 것은 method,url[, async]이있습니다.

method에는 총 GET,POST,PUT,PATCH,DELTE가 있습니다.

method는 요청을 하는 목적과 종류로 나눌수 있습니다. 이는 html태그에서 h1,p,article과 같이 데이터를 요청하는 동작은 같지만 그 동작의 목적과 의미를 담는 것입니다. 또한 html에서 h1태그를 쓰게될 경우 글씨가 굵어지는데 method의 종류마다 조금씩의 차이점이 있습니다. 그중에서 GET과 POST의 차이점을 알아보겠습니다.

#### GET과 POST의 차이점

GET의 경우 데이터를 보내라고 요청할때 사용하고 요청하는 정보가 노출이 되기 때문에 아이디,비밀번호와 같은 민감한 정보는 GET을 이용하면 안됩니다.

POST의 경우 데이터를 보낼테니 서버에 반영해달라는 목적으로 사용하고 정보가 노출이 안되기때문에 보안에 신경써야할 정보를 보내기에 적합합니다.

#### url[,async]

.open의 두번째 인자로 들어가는 url은 데이터를 보낼 서버의 주소를 적어줍니다. 그리고 async라는 옵션값이 존재하는데 이는 AJAX에서 주로사용하는 비동기통신을 한다는 의미입니다. async의 옵션값이 true이면 비동기적통신을하고 false로 되어있다면 동기적 통신을 합니다. 기본값으로 true가 되있기 때문에 별도의 옵션값을 안적어줘도 됩니다.

### XMLHttpRequest.setRequestHeader( )

<img src="https://media.vlpt.us/images/alajillo/post/2f2ba6f2-0892-4971-ad44-4fed145f7f3f/%EB%A6%AC%ED%80%98%EC%8A%A4%ED%8A%B8%20%ED%97%A4%EB%93%9C.png">
개발자모드에서 Network탭을 들어가면 서버로부터 받은 데이터들을 볼수 있습니다. 위에 사진은 그중에 html파일을 담고 있는 데이터의 정보입니다. Header라는 영역이 있는데 이는 html에서 head태그와 매우 유사합니다.

<img src="https://media.vlpt.us/images/alajillo/post/fedab0bd-ea1f-4570-a9c9-a53801e7f011/%EB%A6%AC%ED%80%98%EC%8A%A4%ED%8A%B8%EB%B0%94%EB%94%94.png">
실제로 사용되는 정보는 위에 사진처럼 바디영역에 따로 있지만 Header에는 부수적인 정보들이 있습니다. 예를 들어서 이 데이터가 어떤 MIME타입인지 그리고 .open에서 나온 요청메서드는 어떤것인지에 대한 메타정보들이 들어있습니다.

### MIME 타입이란?

com.setRequestHeader('content-type','application/json');
첫번째 인자로 content-type이 들어가 있습니다. 이는 리퀘스트헤더안에 MIME타입을 정의한다는 뜻입니다. 두번째 인자로 application/json이 나오는데 이는 MIME타입 application 중에서 json이라는 뜻입니다. 파일은 .txt .mp3 , mp4 등등 여러가지 파일 확장명을 가지고 있습니다.
MIME타입은 어떤 종류의 파일인지를 의미하고 application은 text,audio,video등등 여러가지 MIME타입중에서 어느곳에도 명확하게 정의되기 힘든 잡다한 것을 모아둔 파일종류 입니다.

### XMLHttpRequest.send( )

이제 드디어 서버에 보낼 데이터를 잘 포장한 후 서버로 보낼 일만 남았습니다.
send의 인자로는 보낼 데이터가 리퀘스트 몸체로 들어간뒤 서버에 요청을 하게됩니다.

#### GET방식의 경우

XMLHttpRequest.open( )안에 들어간 data는 URL의 일부분인 쿼리문자열로 서버에 전송됩니다.

쿠팡에서 키보드를 검색한후 url의 끝부분에 q=키보드라는 쿼리문자열이 생성된것을 알수있습니다. 이렇게 전달된 정보가 표시가 되기때문에 비밀번호같은 민감한 정보를 GET방식으로 전송하면 안됩니다.

#### POST방식의 경우

<img src="https://media.vlpt.us/images/alajillo/post/829c7e23-6cbd-4603-b2f5-28a462d1d42f/%ED%80%84;.png">
POST의 경우에는 url 쿼리문자열로 들어가는게 아니라 Request body에 넣어서 전송됩니다. 이때 Http,https등을 이용하면서 암호화되기때문에 정보전달에 있어서 안전합니다.

## TMI

페이로드는 전송될 데이터를 부르는 다른 이름입니다.여기서 페이로드는 알맹이로 메타정보가 들어간 헤더를 제외한 몸체에 들어있는 알맹이 데이터를 말합니다.

참고자료
https://youteam.io/blog/angular-vs-react-comparison/ //ajax
https://noahlogs.tistory.com/35 // get과 post의 차이점
https://developer.mozilla.org/ko/docs/Web/HTTP/Basics_of_HTTP/MIME_types// MIME타입
https://ko.wikipedia.org/wiki/%ED%8E%98%EC%9D%B4%EB%A1%9C%EB%93%9C_(%EC%BB%B4%ED%93%A8%ED%8C%85)#:~:text=%ED%8E%98%EC%9D%B4%EB%A1%9C%EB%93%9C(%EC%98%81%EC%96%B4%3A%20payload),%EC%9D%98%20%EC%9D%BC%EB%B6%80%EB%A5%BC%20%EB%9C%BB%ED%95%9C%EB%8B%A4. //페이로드

참고도서
자바스크립트 딥다이브(p.816 ~ p.829)
