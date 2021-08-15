# setTimeout&setInterval

## 호출 스케줄링(schduling a call)

### 정의

- 일정 시간이 지난 후에 원하는 함수를 호출할 수 있도록 함
- javascript에서의 호출 스케줄링에는 setTimeout과 setInterval이 있음
- javascript 명세서엔 setTimeout과 setInterval가 명시되어있지 않지만 시중에 나와있는 모든 브라우저, javascript 호스트 환경 대부분이 이와 유사한 메서드와 내부 스캐줄러를 지원

## setTimeout

```jsx
let timeoutID = setTimeout(func|code, [delay], [arg1], [], ...))
```

### 정의

타이머가 만료된 뒤 함수나 지정된 코드를 실행하는 타이머를 설정함

### 매개변수

- **func | code :** 실행하고자 하는 코드, 함수 또는 문자열 형태. 대개 함수 사용
- **delay :** 실행 전 대기 시간, 단위는 밀리초(millisecond)이며 기본값은 0
- **arg1, arg2 ... :** 함수에 전달할 인수들, 타이머가 만료되고 func에 전달되는 추가적인 매개변수들, IE9이하에선 지원하지 않음. 사용하고 싶다면 polyfill사용

### 반환 값

반환되는 timeoutID는 숫자. 

setTimeout()을 호출하여 만들어진 타이머를 식별할 수 있는 0이 아닌 값. 이 값은 타이머를 취소시키기 위해 WindowTimers.clearTimeout()에 전달할 수도 있음

### 예제

```jsx
function sayHi(){
	alert('안녕하세요.');
}

setTimeout(sayHi, 500);
```

0.5초 뒤에 sayHi()가 호출되어 '안녕하세요' 알림이 뜸

```jsx
function sayHi(who, phrase){
	alert(who + '님' + pharse);
}

setTimeout(sayHi, 500, "신효정", "안녕하세요")
```

0.5초 뒤에 sayHi()가 호출되어 '신효정 님, 안녕하세요.' 알림이 뜸

```jsx
setTimeout("alert('안녕하세요.')", 1000);
```

setTimeout의 첫번째 인수가 문자열이면 javascript는 이 문자열을 이용해 함수를 만들기 때문에 예시가 정상적으로 작동됨

```jsx
setTimeout(() => alert('안녕하세요.'), 1000);
```

되도록이면 화살표 함수를 사용하기

### 활용

- setTimeout의 시간을 0으로 지정하면 스크립트 실행이 끝난 후 호출
- 접속 후 일정 시간이 지난 후 팝업 또는 베너창 띄우기
- 방문자의 스크롤이 브라우저의 일정 위치에 도달할 경우 일정 시간 후 에니메이션 실행
- 검색창 또는 일부 섹션이 일정 시간 뒤 사라지도록 함

### 주의사항

```jsx
setTimeout(sayHi(), 500);
```

setTimeout은 함수의 참조값을 받음. sayHi()를 인수로 전달하면 함수 실행 결과가 전달됨. 따라서 호출 결과는 undefined. 오류발생

### clearTimeout

setTimeout을 호출하면 '타이머 식별자(timer identifier)'가 반환. 스케줄링을 취소하고 싶을 땐 이 식별자를 사용. 실행중인 작업을 중지시키는 것이 아닌 지정된 작업을이 모두 실행되고 다음 작업 스케줄을 중지함

```jsx
let timeoutID = setTimeout(...);
clearTimeout(timeoutID);
```

스케줄링 취소하기

```jsx
let timerId = setTimeout(() => alert("Hi"), 1000);
alert(timerId); 

clearTimeout(timerId);
alert(timerId);
```

 alert 창을 통해 타이머 식별자가 숫자라는 것을 알 수 있음. 다른 호스트 환경에선 타이머 식별자가 숫자형이 아닌 자료형 일 수 있음. 

 Node.js에서는 타이머 객체가 반환됨

타이머를 취소한 후에도 식별자의 값은 null이 아님

### setInterval

```jsx
let timeoutID = setInterval(func|code, [delay], [arg1], [], ...))
```

타이머가 만료된 뒤 함수나 지정된 코드를 반복적으로 실행하는 함수

### 매개변수

- **func | code :** 실행하고자 하는 코드로, 함수 또는 문자열 형태. 대개는 함수 사용
- **delay :** 실행 전 대기시간, 단위는 밀리초(millisecond, 1000초 = 1초)이며 기본값은 0
- **arg1, arg2 ... :** 함수에 전달할 인수들, 타이머가 만료되고 func에 전달되는 추가적인 매개변수들, IE9이하에선 지원하지 않음 사용하고 싶다면polyfill사용

### 주의사항

- 일정한 시간 간격으로 실행되는 작업이 정해진 간격의 시간보다 오래걸릴 경우 문제가 발생할 수 있음
- alert 창이 떠 있더라도 내부 타이머는 멈추지 않음

### clearInterval

setInterval을 호출하면 '타이머 식별자(timer identifier)'가 반환. 스케줄링을 취소하고 싶을 땐 이 식별자를 사용. 실행중인 작업을 중지시키는 것이 아닌 지정된 작업을이 모두 실행되고 다음 작업 스케줄을 중지함

```jsx
let timeoutID = setInterval(...);
clearInterval(timeoutID);
```

### 중첩 setTimeout

```jsx
let timeoutId = setTimeout(function sayHi() {
  alert('Hi');
  timeoutId = setTimeout(sayHi, 2000); // (*)
}, 2000);
```

- (*)표시가 된 줄의 실행이 종료되면 다음 호출을 스케줄링함
- setTimeout을 중첩으로 이용하는 것은 setInterval을 사용하는 방법보다 유연- 호출 결과에 따라 다음 호출을 원하는 방식으로 조정해 스케줄링 할 수 있기 때문

```jsx
let delay = 5000;

let timeoutId = setTimeout(function request() {
  ...요청 보내기...

  if (서버 과부하로 인한 요청 실패) {
    delay *= 2; //요청 간격을 늘림
  }

  timeoutId = setTimeout(request, delay);

}, delay);
```

- 서버 과부하 상태라면 요청 간격을 증가시켜주는 방식으로 구현
- CPU 소모가 많은 작업을 주기적으로 실행하는 경우에 setTimeout을 재귀 실행하는 방법이 유용 - 작업에 걸리는 시간에 따라 다음 작업을 유동적으로 계획할 수 있기 때문

### 중첩 setTimeout과 setInterval의 비교

- 중첩 setTimeout은 지연 간격을 보장하지만 setInterval은 이를 보장하지 않음

setInterval

```jsx
let i = 1;
setInterval(function() {
  func(i++);
}, 100);
```

![dddd](https://user-images.githubusercontent.com/70738281/129437595-ef663fd0-35d1-4311-8586-b1e936e2f6ab.PNG)



[이미지 출처](https://ko.javascript.info/settimeout-setinterval)

중첩 setTimeout

```jsx
let i = 1;
setTimeout(function run() {
  func(i++);
  setTimeout(run, 100);
}, 100);
```
![sssdf](https://user-images.githubusercontent.com/70738281/129437612-62879576-3b1e-4dc7-86fb-5422e1ba6faf.PNG)


[이미지 출처](https://ko.javascript.info/settimeout-setinterval)

- setInterval은 func을 실행하는데 소모되는 시간도 지연 간격에 포함시키기 때문에 func호출 사이의 지연 간격이 실제 명시한 간격보다 짧아짐
- 따라서 func을 실행하는데 걸리는 시간이 명시한 지연 간격보다 길 때 엔진이 func의 실행이 종료될 때 까지 기다리다 func의 실행이 종료되면 엔진은 스케줄러를 확인하지 않고, 다음 호출을 바로 시작
- 중첩 setTImeout은 이전 함수의 실행이 종료된 이후에 다음 함수 호출에 대한 계획이 세워지기 때문에 명시한 지연 시간 보장

### setTimeout과 setInterval의 실제 대기시간

부라우저에선 [HTML5 표준](https://html.spec.whatwg.org/multipage/timers-and-user-prompts.html#timers)에서 정한 제약에 의해 다섯번 째 중첩 타이머 이후엔 대기시간을 최소 4ms 이상으로 강제햠

```jsx
let start = Date.now();
let times = [];

setTimeout(function run() {
  times.push(Date.now() - start); // 이전 호출이 끝난 시점과 현재 호출이 시작된 시점의 시차를 기록

  if (start + 100 < Date.now()) alert(times); // 지연 간격이 100ms를 넘어가면, array를 얼럿창에 띄워줌
  else setTimeout(run); // 지연 간격이 100ms를 넘어가지 않으면 재스케줄링함
});

// 출력창 예시:
// 1,1,1,1,9,15,20,24,30,35,40,45,50,55,59,64,70,75,80,85,90,95,100
```

- 출력창 처럼 다섯번 째 중첩 타이머 이후엔 지연 간격이 4ms 이상이 됨 - setInterval에도 적용
- 오래전부터 있던 제약, 구직 스크립트 중 일부는 아직 이 제약에 의존하는 경우가 있음
- 서버측엔 이런 제약이 없어 Node.js를 이용하면 비동기 작업을 지연 없이 실행할 수 있음

### Garbage Collection과 setInterval, setTimeout

- setInterval이나 setTimeout에 함수를 넘기면, 함수에 대한 내부 참조가 새롭게 만들어지고 이 참조 정보는 스케줄러에 저장됨. 따라서 해당 함수는 참조하는 것이 없어도 가비지 컬렉션의 대상에서 제외됨 - 스케줄러가 함수를 호출할 때까지 함수는 메모리에 유지됨
- setInterval은 clearInterval이 호출되기 전까지 함수에 대한 참조가 메모리에 유지
- 주의해야할 사항으로 외부 렉시컬 환경을 참조하는 함수가 있다고 가정했을 때 이 함수가 메모리에 남아있는 동안 외부 변수 역시 메모리에 남아있어야 하고 이로 인해 함수가 차지해야 하는 공간보다 더 많은 메모리 공간이 사용되는 부작용 발생
- 부작용 방지를 위해 스케줄링할 필요가 없어진 함수는 취소

[참조](https://ko.javascript.info/settimeout-setinterval)
