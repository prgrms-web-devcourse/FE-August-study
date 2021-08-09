# 데브코스 1주차 스터디: 웹 스토리지

<br>

## 🍪 웹 스토리지 탄생 배경

- 쿠키의 단점 
  - HTTP요청마다 포함되어 있어 서버에 부담이 가고, 웹이 느려지는 원인이 될 수 있음
  - 4KB라는 작은 용량
  - 만료기간이 존재
- 웹스토리지
  - 서버가 없어도 로컬 스토리지에 저장 가능
  - 더 많은 용량 (5MB)
  
<br>

## 💾 웹 스토리지
웹스토리지는 로컬스토리지와 세션스토리지로 나뉜다.
Chrome 기준, **개발자 도구 ▶ 어플리케이션 ▶ 스토리지** 에서 확인 가능
![](https://images.velog.io/images/te-ing/post/0363b8b9-19cd-4561-90df-27f824771b26/image.png)
### 📚 로컬스토리지
- 브라우저를 닫아도 데이터가 남음
- 반영구적 데이터 저장(사용자가 지우지 않으면 안사라짐)
- 도메인이 다르면 접근 불가
- 아이디 저장, 검색 기록에 주로 사용
### 📖 세션스토리지
- 새 창, 새 탭마다 개별적으로 데이터 저장
- 브라우저를 닫는 순간 데이터 사라짐(브라우저가 열려있는 한 최대한 유지)
- 저장된 데이터를 절대 서버로 보내지 않음
- 도메인이 다르면 접근 불가
- 세션이 다르면 접근 불가
- 입력 폼 정보, 비로그인 장바구니에 주로 사용

<br>

## 📋 웹스토리지 API 사용법
```javascript
localStorage. || sessionStorage +

length // 읽기 전용 현재 Storage의 길이

key(int) // 해당 인덱스 위치에 있는 문자열 키 값을 리턴

getItem(key) // 해당 키 값에 해당하는 정보(문자열)을 리턴

setItem(key, value) // 해당 키 값에 대하여 해당 값을 설정

removeItem(key) // 해당 키 값의 값을 삭제

clear() // Storage 전체 내용을 삭제
```

<br>


## 🔎 IndexedDB
웹스토리지와 비교하여 알아보는 IndexedDB

### 📙 Webstorage
- 5MB 저장용량
- 동기(synchronous) 방식 동작
- String 형태로만 저장 가능
### 📚 IndexedDB
- 저장용량의 제한 없음
- 비동기 방식 (Asynchronous)
- 어떤 형태로든 저장 가능
- Window객체, 웹워커, 서비스워커에서 접근 가능
- low-level API로 사용하기가 어려움

<br>

***

참고사이트: [kjwsx23.tistory](https://kjwsx23.tistory.com/605), [ktko.tistory](https://ktko.tistory.com/entry/HTML-%EC%9B%B9-%EC%8A%A4%ED%86%A0%EB%A6%AC%EC%A7%80-%EA%B8%B0%EB%8A%A5%EA%B3%BC-%EC%98%88%EC%A0%9C-%EA%B7%B8%EB%A6%AC%EA%B3%A0-%ED%99%9C%EC%9A%A9-%EB%B0%A9%EB%B2%95), [unikys.tistory](https://unikys.tistory.com/352), [han41858.tistory](https://han41858.tistory.com/54), [pks2974.medium](https://pks2974.medium.com/indexeddb-%EA%B0%84%EB%8B%A8-%EC%A0%95%EB%A6%AC%ED%95%98%EA%B8%B0-ca9be4add614)