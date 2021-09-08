# 📖 비동기 HTTP 통신 종류

<br>

# ❓❗ Ajax vs Fetch vs axios

    프로젝트 수행시 클라이언트와 서버 간 데이터를 주고 받는 과정이 필요하다.
    이를 위해 HTTP 통신을 사용하게 된다.
    HTTP 통신을 위해 JS에서 사용되는 Ajax, Axios, fetch를 알아보자.
    여기서는 통신 방법이나 사용법은 다루지 않을 예정이다.
    통신 종류의 차이점에 대해서만 다룰 예정이다.

# ❓ Ajax (Asynchronous JavaScript And XML)

- Ajax는 빠르게 동작하는 동적인 웹 페이지를 만들기 위한 개발 기법이다.
- 웹 페이지 전체를 다시 로딩하지 않고도, 웹 페이지의 일부분만을 갱신할 수 있게 해준다.
  <br>

<p align="center">
<img src="https://i.imgur.com/9MNdPb3.png" width="49%">
<img src="https://i.imgur.com/BB92bZK.png" width="49%" >
</p>

# ❓ XMLHttpRequest 객체

- Ajax의 가장 핵심적인 구성 요소는 바로 XMLHttpRequest 객체이다.
- Ajax에서 XMLHttpRequest 객체는 웹 브라우저가 서버와 데이터를 교환할 때 사용된다.
- <div style="color:red">콜백지옥이 생길 수 도 있다.</div>

```
var xhr = new XMLHttpRequest();
xhr.onreadystatechange = function() { // 요청에 대한 콜백
  if (xhr.readyState === xhr.DONE) { // 요청이 완료되면
    if (xhr.status === 200 || xhr.status === 201) {
      console.log(xhr.responseText);
    } else {
      console.error(xhr.responseText);
    }
  }
};
xhr.open('GET', 'https://~~URL'); // 메소드와 주소 설정
xhr.send(); // 요청 전송
```

<div style="color:red">하지만 익스플로러의 구형 버전인 5와 6 버전에서는 ActiveXObject 객체를 사용해야 한다.</div>

`var 변수이름 = new ActiveXObject("Microsoft.XMLHTTP");`

# ❓ JQuery Ajax

- 제이쿼리와 Ajax를 이용하여 개발을 손쉽게 할 수 있도록 미리 여러 가지 기능을 포함해 놓은 개발 환경을 Ajax 프레임워크라고 한다.
- 그중에서도 현재 가장 널리 사용되고 있는 Ajax 프레임워크는 바로 제이쿼리(jQuery)이다.
- XMLHttpRequest가 사용되다가 불편함과 호환성을 이유로 jQuery 내에서 Ajax를 사용하는 것이 대세가 되었다.
- <div style="color:red"> jQuery를 사용하기 위해서는 cdn을 import해야하거나 API파일을 직접 다운받아서 사용해야 한다. </div>

```
$.ajax({
    url: "/examples/media/request_ajax.php", // 클라이언트가 요청을 보낼 서버의 URL 주소
    data: { name: "홍길동" },                // HTTP 요청과 함께 서버로 보낼 데이터
    type: "GET",                             // HTTP 요청 방식(GET, POST)
    dataType: "json"                         // 서버에서 보내줄 데이터의 타입
})
```

- <p style="color:red"> 콜백지옥이 생길 수 도 있다.</p>

```
$.ajax({
  url: "~~",
  success: function(data) {
    const result = JSON.parse(data);
    $.ajax({
        url: `~~~`
        success: function(data2){
            const result2 = JSON.parse(data2);
            $.ajax({
                url: `~~`
                success: function(data3){
                    ......
                }
            })
        }
    })
  },
})
```

# ❓ fetch API

- 내장 라이브러리이기 때문에 import없이 사용 가능하다.
- XMLHttpRequest 대체자로 나온것이 fetch API이다. -> 비동기 http 요청을 좀 더 쓰기 편하게 해준다.
- fetch API는 Promise기반으로 동작한다. -> 콜백지옥 탈출
- React Native같은 잦은 업데이트에도 잘 적용이 된다.
- <div style="color:red">기본 응답 결과는 Response 객체이다 -> json으로 바꾸거나 text로 바꾸는 처리 과정이 필요하다.</div>
- <div style="color:red">구형 브라우저에서는 지원을 안한다.</div>
- <div style="color:red">네트워크 에러 발생시 계속 기다려야한다.</div>
- <div style="color:red">API 요청을 취소 할 수가 없다. -></div>

```
fetch('주소', 설정객체).then(콜백).catch(콜백);
```

```
fetch(url, {
      headers: {
        "Content-Type": "application/json",
      },
    });
```

XMLHttpRequest와 fetch API둘다 사용해보면 딱히 가독성 차이는 딱히 크게 와닿지 않는다.

하지만 Headers, Request, Response같은 객체를 직접 다룰 수 있다는 장점과

Promise의 강력한 기능 및 async, await 사용으로 생산성 향상이 된다.

# ❓ Axios

- Axios는 브라우저, Node.js를 위한 Promise API를 활용하는 HTTP 비동기 통신 라이브러리이다
- Axios도 Promise 기반이다.
- 크로스 브라우징 기반으로 브라우저 호환성이 뛰어나다.
- 자동으로 JSON데이터 형식으로 변환된다.
- 요청 취소 및 타임아웃 설정이 가능하다.
- fetch에 존재하지 않는 기능이 좀 더 많다.
- React에서 주로 사용된다.
- XSRF 해킹 기법에 비교적 안전하다.
- <div style="color:red">라이브러리 설치가 필요하다.</div>

```
const axios = require('axios')

// ID로 사용자 요청
axios
  .get('url~~')
  // 응답(성공)
  .then(function(response) {
    console.log(response)
  })
  // 응답(실패)
  .catch(function(error) {
    console.log(error)
  })
  // 응답(항상 실행)
  .then(function() {
    // ...
  })
```

# 💡 결론

fetch vs axios이다.
간단한 http요청만 필요로하고 VanillaJS로 통신을 이용할려면 fetch를 이용하고,
좀 더 많은 기능들과 사용자의 편의가 필요하다면 axios를 이용하면 된다.

fetch는 업데이트가 빈번한 react native에서 주로 사용된다고 한다.

axios는 주로 react에서 사용된다. 저 또한 react 개발시 api요청을 axios를 많이 이용했다.

<br>

| axios                                           | fetch                                                          |
| ----------------------------------------------- | -------------------------------------------------------------- |
| 요청 객체에 url이 있다.                         | 요청 객체에 url이 없다.                                        |
| 써드파티 라이브러리로 설치가 필요               | 현대 브라우저에 빌트인이라 설치 필요 없음                      |
| XSRF 보호를 해준다.                             | 별도 보호 없음                                                 |
| data 속성을 사용                                | body 속성을 사용                                               |
| data는 object를 포함한다                        | body는 문자열화 되어있다                                       |
| status가 200이고 statusText가 ‘OK’이면 성공이다 | 응답객체가 ok 속성을 포함하면 성공이다                         |
| 자동으로 JSON데이터 형식으로 변환된다.          | .json()메서드를 사용해야 한다.                                 |
| 요청을 취소할 수 있고 타임아웃을 걸 수 있다.    | 해당 기능 존재 하지않음                                        |
| HTTP 요청을 가로챌수 있음                       | 기본적으로 제공하지 않음                                       |
| download진행에 대해 기본적인 지원을 함          | 지원하지 않음                                                  |
| 좀더 많은 브라우저에 지원됨                     | Chrome 42+, Firefox 39+, Edge 14+, and Safari 10.1+이상에 지원 |

다른 사이트에서 가져온 axios, fetch 비교 표이다.
<br>

📃 참고 사이트
https://www.zerocho.com/category/HTML&DOM/post/595b4bc97cafe885540c0c1c

https://kyun2da.dev/%EB%9D%BC%EC%9D%B4%EB%B8%8C%EB%9F%AC%EB%A6%AC/axios-%EA%B0%9C%EB%85%90-%EC%A0%95%EB%A6%AC/
