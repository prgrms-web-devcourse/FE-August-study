# 자바스크립트 프로토타입
<br>

# prototype

자바스크립트의 **객체지향**을 지탱하고 있는 핵심 개념

자바스크립트를 일반적인 객체지향 언어와 구분하는 개념

프로토타입을 통해 **상속**이라는 개념을 제공
<br>

### 예제 1)
![](https://images.velog.io/images/serim22/post/f02d6735-ffa1-46ae-9585-6f163c9fa0de/image.png)
<br>
`user` 객체를 하나 만들고
`user.name`을 하면 이름이 나옵니다.

`hasOwnProperty`는 자신이 가진 프로퍼티를 확인하는 메서드
user 객체에 해당 프로퍼티가 있으면 true, 없으면 false

<span style="color:red">그러면 `hasOwnProperty`는 생성한 적이 없는데 어디서 나온 녀석인가??</span>

![](https://images.velog.io/images/serim22/post/a9cc8444-faaf-4b47-9f13-90a4a1a91493/image.png)

`[[Prototype]]`이라는 객체가 바로 프로토타입

객체에서 프로퍼티를 읽으려 하는데 없으면 이곳에서 찾게된다.

만약 `hasOwnProperty`가 객체 안에 있으면
방금 생성한 메서드로 동작하게된다.
<br>
![](https://images.velog.io/images/serim22/post/bdd1c43f-b8e3-4050-8ce1-0e453b2d1433/image.png)
<br>
위의 코드처럼 객체에 프로퍼티가 있으면 탐색을 멈추게 된다.

결과적으로 **객체 내에 프로퍼티가 없을 때**만 **프로토타입**에서 프로퍼티를 찾는다.
<br>

---
<br>

## 프로토타입의 동작원리 - 상속
### 예제 2)

- 공통 : 바퀴 수, 드라이브 메서드
- BMW : 네비게이션
<br>

![](https://images.velog.io/images/serim22/post/2d47bf45-c831-4b73-a24d-85fcf44c98ac/image.png)


<span style="color:red">지금은 차가 3대 그러나 차들이 늘어날 때마다 새로운 변수를 선언할 것인가??</span>


- `car` 라는 상위 객체를 생성하여 공통된 부분을 만들어주고
bmw, benz, audi객체의 `wheels`와 `drive()`를 삭제
- car가 bmw, benz, audi의 프로토타입
- bmw, benz, audi들은 car의 상속을 받음

![](https://images.velog.io/images/serim22/post/3bae70d6-e99c-401c-9a55-9ed04dc4cdf7/image.png)

모든 객체는 `__proto__`를 통해 자신의 프로토타입(`[[Prototype]]` 내부 슬롯)에 접근할 수 있다.


![](https://images.velog.io/images/serim22/post/5170d313-f1ed-4442-85a8-03a546a2905e/image.png)


bmw에서 `wheels`를 찾으면 먼저 bmw객체 내부에서 찾고
없으면 프로토타입에서 확인
프로토타입을 열어보면
`drive()`와 `wheels` 프로퍼티가 있다.

![](https://images.velog.io/images/serim22/post/87c5eda7-67ba-40de-b37c-fb22dcb451bd/image.png)


<br>


---
<br>

## 프로토타입 체이닝
### 예제 3)

상속은 계속 이어진다!!!

이번에는 bmw를 상속받는 x5객체를 생성

<br>

![](https://images.velog.io/images/serim22/post/9114c769-dd90-4f2d-88d0-04823c1239e3/image.png)
<br>

여기서 x5의 color와 name은 객체 내에 있어서 값을 주고 탐색을 멈춘다.
<br>
![](https://images.velog.io/images/serim22/post/2e5deff6-cf7e-4423-8d4e-c909c28712eb/image.png)
<br>
그리고 navigation을 해도 값이 잘 나온다.
<br>

![](https://images.velog.io/images/serim22/post/6daa9c6d-70bb-4243-97fa-be91ce82e515/KakaoTalk_20210806_012606544.jpg)

- x5에서 navigation을 찾는데 없으면 프로토타입인 bmw에서 탐색하여 찾고 멈춘다.

- 만약 x5에서 drive()를 찾을려면, x5에 없어서 프로토타입인 bmw로 올라간다.
그런데도 없어서 bmw의 프로토타입인 car까지 올라가서 찾고 멈춘다.

이를 <span style="color:red">프로토타입 체인</span>이라고 한다!!

<br>
<br>
<br>
<br>

# 감사합니다 🤯






유투버 '코딩앙마' 님의 영상을 참고하여 공부하기 위해 포스팅하였습니다.