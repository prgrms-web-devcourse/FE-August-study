# GET

`GET`은 특정한 리소스를 요청하기 위해 사용됩니다. `GET` 요청은 데이터를 가져올 때만 사용해야 합니다.
`GET`은 요청할 때 필요한 데이터가 있는 경우 URL 끝에 key=value 형태의 query string으로 전달합니다.

> `GET`요청 예시

```
GET www.example.com/resource?name1=value1&name2=value2
```

<br>

# POST

`POST`는 서버로 데이터를 전송합니다. 전송할 데이터는 body에 담깁니다. body의 type을 `Content-Type` 헤더를 지정하여 나타내야 합니다. 데이터 타입을 지정하지 않을 경우 `application/octet-stream`로 요청을 처리합니다.

- `Content-Type`
  - `application/x-www-form-urlencoeded`
    &으로 분리되고, "="기호로 값과 키를 연결하는 key-value tuple로 인코딩되는 값입니다.
    바이너리 데이터에 사용하기에는 적절치 않습니다.
  - `multipart/form-data`
    바이너리 데이터(파일, 이미지)에 사용합니다.

> POST 요청 예시

- `application/x-www-form-urlencoded`

```
POST / HTTP/1.1
Host: foo.com
Content-Type: application/x-www-form-urlencoded
Content-Length: 13
>
say=Hi&to=Mom
```

- `multipart/form-data`

```
POST /test.html HTTP/1.1
Host: example.org
Content-Type: multipart/form-data;boundary="boundary"
>
--boundary
Content-Disposition: form-data; name="field1"
>
value1
--boundary
Content-Disposition: form-data; name="field2"; filename="example.txt"
>
value2
--boundary--
```

<br>

# GET과 POST 비교

|                               |       GET        |          POST          |
| :---------------------------: | :--------------: | :--------------------: |
|     Request에 body가 존재     |        X         |           O            |
| 성공한 Response에 body가 존재 |        O         |           O            |
|            안전함             |        O         |           X            |
|            멱등성             |        O         |           X            |
|           캐시 가능           |        O         |      조건부 가능       |
|     <form>에서 사용 가능      |        O         |           O            |
|       데이터 길이 제한        | 일반적으로 255자 |          없음          |
|          데이터 타입          |  string만 가능   | string, binary 등 가능 |

## 안전함

HTTP method가 서버의 상태를 바꾸지 않으면 그 메서드가 안전하다고 할 수 있습니다. 모든 안전한 메서드는 멱등성 또한 가집니다.

> ❗ 멱등성을 가진 모든 메서드가 안전한 것은 아닙니다. `DELETE`, `PUT`은 멱등성을 가지지만 안전한 메서드는 아닙니다.

- `GET`
  안전합니다. 서버의 상태를 건드리지 않습니다.
- `POST`
  서버에 리소스가 생성되는 등 서버의 상태를 건드리므로 안전하지 않습니다.

## 멱등성

동일한 request를 **한번 보내는 것과 여러 번 연속으로 보내는 것이 같은 효과를 지니고, 서버의 상태도 동일하게 남을 때** 멱등성을 가졌다고 합니다. 멱등성을 따질 때는 실제 서버의 백엔드 상태만 보면 됩니다. 응답 코드가 달라지는 것은 상관 없습니다.

> ❗ `DELETE`, `PUT`은 안전하지 않지만 멱등성을 가집니다.
> `DELETE` 요청의 경우 서버에 `id`를 통해 삭제 요청을 여러 번 보낸다고 하더라도 하나의 자원이 여러 번 삭제 될 수 없습니다. 따라서 한 번 보내는 것과 여러번 보내는 것이 같은 효과를 지니고 있고, 서버의 상태도 같기 때문에 멱등성을 가집니다.

- `GET`
  서버에 여러 번 요청을 보낸다고 해도, 리소스를 받아오기만 할 뿐 서버의 상태를 건드리지 않습니다. 따라서 멱등성을 가집니다.

- `POST`
  서버에 한 번 요청을 보내는 것과 여러 번 연속으로 보내는 것에, 서버에 생성되는 리소스 수의 차이가 있습니다. 따라서 멱등성을 가지지 않습니다.

## 캐시 가능

캐시할 수 있는 HTTP response로, 나중에 검색하여 사용할 수 있도록 서버에 응답을 저장합니다. 1.`GET`과 `HEAD` 메서드는 자체적으로 캐시 가능합니다. `POST`와 `PATCH`는 신선함 정보를 포함하고, `Content-Location` 헤더가 설정되어 있는 경우 캐시 가능합니다. 하지만 거의 사용하지 않습니다. `PUT`과 `DELETE`는 캐시 가능하지 않습니다.

> `PUT`과 `DELETE`는 캐시된 응답의 리소스를 건드릴 경우, 리소스를 변경시키게 되므로 캐시된 `GET` 또는 `HEAD`의 응답을 무효로 만들 수 있습니다.

2. 다음의 상태 코드들이 캐시 가능합니다. `200`, `203`, `204`, `206`, `300`, `301`, `404`, `405`, `410`, `414`, and `501`.

3. response가 캐시되는 것을 막도록 [`Cache-Control`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Cache-Control)헤더를 설정할 수 있습니다.

# GET과 POST에 대한 오해

1. 보안상 `GET`은 URL 파라미터로 전달하기 때문에 데이터가 노출되어 있어 보안적인 면에서 안전하지 않고, 보안이 필요한 데이터는 보통 `POST`로 전달하는 것이 좋다고 알려져 있습니다.

   > POST는 데이터가 Body로 전송되고 내용이 눈에 보이지 않아 `GET`보다 보안적인 면에서 안전하다고 생각할 수 있지만, `POST` 요청도 크롬 개발자 도구, Fiddler와 같은 툴로 요청 내용을 확인할 수 있기 때문에 민감한 데이터의 경우에는 반드시 암호화해 전송해야 합니다.  
   > 출처 : [GET과 POST의 차이](https://hongsii.github.io/2017/08/02/what-is-the-difference-get-and-post/)

2. HTTPS라면 안전할까요?
   안전하지 않습니다. HTTPS는 URL 경로, 요청된 실제 페이지, query 파라미터를 서버로 가는 동안 암호화시킵니다. `POST` 요청 바디 내의 데이터는 암호화하지 않습니다. 웹 서버는 요청(접근)에 대한 로그를 평문으로 유지하기 때문에, 민감한 데이터를 `GET` 메소드로 보내는 것은 지양해야 합니다.

<br>

#### 출처

MDN [HTTP 요청 메서드](https://developer.mozilla.org/ko/docs/Web/HTTP/Methods)  
[GET과 POST의 차이](https://hongsii.github.io/2017/08/02/what-is-the-difference-get-and-post/)  
[Difference between a GET and POST](https://www.guru99.com/difference-get-post-http.html#7)  
[Development: Difference Between GET and POST](https://lazaroibanez.com/difference-between-the-http-requests-post-and-get-3b4ed40164c1)
