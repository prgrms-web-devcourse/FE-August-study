
## 비동기 처리란?
특정 로직의 실행이 끝날 때까지 기다려주지 않고 나머지 코드를 먼저 실행하는 것.

### 동기 vs 비동기

![](https://images.velog.io/images/yooon26/post/3b0a83b0-8eb1-402d-9d1f-369c84f78788/image.png)

**동기 처리**
앞 순서의 처리가 다 끝난 다음에 다음 순서가 처리된다.
- 실행 순서가 보장된다.
- 앞 순서가 얼마나 걸리든 간에 기다렸다가 실행되어 비효율적.


**비동기 처리**
앞 순서를 기다리지 않고 다음 코드 실행를 실행한다
- 바로 답을 내놓을 수 없는 상황이 아니라면, 계속 기다리고 있지 않는다.
- 실행 순서가 보장 안된다.


</br>

## 자바스크립트의 비동기 처리

자바스크립트는 동기적인 언어이다.
hoisting이후에 순차적으로 코드가 실행된다. 
`hoisting : 변수, 함수 선언이 자동적으로 제일 위로 올라가는 현상`

#### **자바스크립트에서 비동기 처리가 필요한 이유**
web에서는 순차적으로 자료가 오게 된다면 비효율적이다.
화면에서 서버로 데이터를 요청했을 때 서버가 언제 그 요청에 대한 응답을 줄지도 모르는 상황에서 응답이 오기 전까지 다른 코드를 실행하지 않는다면,
사용자가 어떤 동작을 할 때마다 매번 기다려야하기 때문에 사용성이 매우 떨어진다. 


자바스크립트의 비동기 처리는 3가지 방식으로 할 수 있다.


### 1. 콜백 함수 활용
콜백 함수란? 특정 함수의 인자로 넘겨져서, 함수 내부에서 호출되는 함수.


#### 동기적인 콜백
순차적으로 바로 실행됨.

```js
function printImmediately(print) {
  print();
}

console.log(1),
printImmediately(() => console.log(2));
console.log(3)

// 1, 2, 3
```

#### 비동기적인 콜백
순서가 아닌 조건에 따라 실행됨.
단지 함수를 등록해놓기만 하고 어떤 이벤트가 발생했거나 
특정 시점에 도달했을 때 시스템에서 호출된다.

- 서버 통신 API사용 시, 데이터를 받았을 때 호출되는 콜백함수,
- setTimeout API사용 시, 일정 시간 후에 호출되는 콜백함수


</br>

**setTimeout()**을 이용한 예제
```js
function printWithDelay(callback, delayTime) {
  setTimeout(callback, delayTime)
}

console.log(1)
printWithDelay(() => console.log(2), 2000);
console.log(3)
printWithDelay(() => console.log(4), 1000);

// 1, 3, 4, 2
```



 </br>

### 콜백 지옥이란?
콜백함수 안의 함수 실행문의 콜백함수 안의 함수 실행문 안의 콜백함수 안의..

웹 서비스를 개발하다 보면 서버에서 데이터를 받아와 화면에 표시하기까지 인코딩, 사용자 인증 등 여러 가지를 처리해야 하는 경우가 있는데, 이 모든 과정을 비동기로 처리해야 한다고 하면 위와 같이 콜백 안에 콜백을 계속 무는 형식으로 코딩하게 될 수 있다.

- 가독성이 매우 떨어진다.
- 디버깅과 유지보수가 어렵다.

```js
a(
   () => {
          b(1,2,()=>{
            c(4,5,()=>{
            	d()
            })
          })
    },
    6,
    7
)
```



비동기 처리를 위한 콜백 함수 사용 시 발생할 수 있는 콜백 지옥과 같은 문제를 해결하기 위해 Promise가 사용된다.

## Promise
javascrip에 내장되어 있는 비동기 처리 오브젝트로,
주로 서버에서 받아온 데이터를 화면에 표시할 때 사용한다.

API를 사용하여 서버에서 데이터를 요청하고 받아오는 작업을 할 때,
비동기 처리를 위해 데이터를 받아오기도 전에 마치 데이터를 다 받아온 것 처럼 화면에 데이터를 표시하려고 하면 오류가 뜨는데 이를 해결하기 위한 방법 중 하나이다.



### 중요한 포인트 2가지

1. **State의 상태를 이해하는 것**
promise가 비동기  처리를 수행하는 3가지 과정에 따른 상태.
- 수행 중인지 → **pending**
- 성공했는지 → **fulfilled**
- 실패했는지 → **rejected**

2.  **producer와 consumer의 차이점을 아는 것.**
     producer: 필요한 데이터를 제공하는 객체. (=== promise) 
     consumer: 제공된 데이터를 사용하는 객체.
     
</br>

## 1. producer

### promise 생성하기

- new Promise()호출 시 **대기(Pending)**상태
```js
const promise = new Promise()

promise(function(resolve, reject) {
  // ...
});
```
Promise에 excutor라는 콜백함수를 전달에 주어야하는데,
excutor는 또 resolve, reject라는 2가지 콜백함수들를 전달받아야한다.

**resolve**: 기능이 정상적으로 수행됐을 때 호출되는 함수.
**reject**: 기능 수행 중 문제가 생기면 호출되는 함수.




### ajax 통신 API를 사용한 예제
- **resolve()**호출 시 **이행(Fulfilled)**상태
- **reject()**호출 시 **실패(Rejected)** 상태
```js
function getData() {
  
  // new Promise() 추가
  return new Promise((resolve, reject) => {
    
    // 서버에서 데이터를 받아오는 API실행
    $.get('url 주소/products/1', function(response) {
      
      // 데이터(response)를 받으면 resolve() 호출
      if (response) {
      // 받은 데이터(response)를 그대로 전달
        resolve(response);
      }
      
      // 실패 시 reject() 호출 - 오류 메세지 전달
      reject(new Error("Request is failed"));
    });
  });
}

});
```

</br>

## 2. Consumers

### promise 사용하기

promise의 실행이 끝나면 호출되는 함수들

- **then()**
Promise가 정상적으로 수행되어 resolve가 호출되면 이 함수를 실행해라
```js

// resolve(response)의 결과 값 data를 resolvedData로 받음
getData().then((resolvedData) => {
  console.log(resolvedData); // 받은 데이터를 인자로 받아 그대로 출력.
});
```
- **catch**()
오류가 발생해 reject가 호출되면 이 함수를 실행해라
```js
getData()
  .then((resolvedData) => {
  console.log(resolvedData); 
})

// reject()의 결과 값 Error객체를 err에 받아서 오류메세지 출력
  .catch((err) => {
  console.log(err); // Error: Request is failed
});
```
- **finally()**
성공하든 실패하든 마지막에 무조건 실행해라
```js
getData()
  .then((resolvedData) => {
  console.log(resolvedData); 
})
  .catch((err) => {
  console.log(err); 
});
  .finally(()=>{
  console.log("finally"); 
})
```

</br>
</br>

참고 
https://blog.metafor.kr/164
https://deftkang.tistory.com/20
https://www.youtube.com/watch?
[드림코딩 엘리 유튜브] (https://www.youtube.com/watch?v=JB_yU6Oe2eE&list=RDCMUC_4u-bXaba7yrRz_6x6kb_w&start_radio=1&rv=JB_yU6Oe2eE&t=0)