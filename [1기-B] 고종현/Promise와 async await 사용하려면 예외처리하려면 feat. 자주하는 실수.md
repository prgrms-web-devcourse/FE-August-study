### 선학습되어야 하는 키워드들

1. Callback, Callback
2. 기본적인 Promise의 사용법

# async가 붙은 함수의 역할

Promise객체를 반환하는 함수와 같다.

```javascript
function delayP(sec) {
  return new Promise((resolve, _) => {
    setTimeout(() => {
      resolve(new Date().toISOString());
    }, sec * 1000);
  });
}

function myFunc() {
  return 'wow';
}

// 1. 키워드 await는 async 함수 안에서 사용할 수 있다.
async function myAsync() {
  // 2. 키워드 await은 Promise가 resolve될 때까지 기다려준다.
  const result = await delayP(3).then((_) => 'x'); // 3. then을 사용하여서 resolve된 값을 변환할 수도 있다.
  console.log(result);

  // 4. 키워드 await은 일반 함수 앞에 붙어도 아무런 영향을 주지 않는다.
  const wow = await myFunc();
  console.log(wow);

  return 'async'; // async 키워드가 붙은 함수가 반환하는 값은 Promise이고 'async'는 resolve값이다.
}

console.log(myFunc()); // "wow"
// 5. 키워드 async가 붙은 함수는 Promise객체를 반환한다.
console.log(myAsync()); // Promise {<pending>}
console.log(delayP(1)); // Promise {<pending>}

// 6. async가 붙은 함수는 Promise객체를 반환하기 때문에 then도 사용할 수 있다.
myAsync().then((result) => {
  console.log(result);
});
```

# 비동기에서 중요한 점은 예외처리이다.

```javascript
// 아래코드에서는 예외처리를 보여주기 위해 reject만 했다.
function rejectedWait(sec) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      reject('setTimeout error in rejectedWait()!');
    }, sec * 1000);
  });
}
```

## 자주 하는 실수

```javascript
try {
  rejectedWait(1);
} catch (e) {
  console.error('try catch', e);
}
```

에러가 잡히지 않는다. 왜냐하면, 위코드는 동기적인 코드 실행이 아니기 때문에 예외를 발생하는 타이밍이 try가 싸고 있는 블록이 실행되는 타이밍이 아니라 나중에 새로운 콜스택으로 콜스택이 비었을 때, 타이머가 큐에 넣고 큐의 내용이 새로운 콜 스택으로 쌓인 것이기 때문에 에러로 잡히지 않는다.

### Promise 해결법

#### 1. Promise 객체의 catch라는 메소드를 이용하면 된다.

```javascript
rejectedWait(1)
  .catch((e) => {
    // rejectedWait에서 반환한 Promise가 예외가 있기 때문에
    console.error('1st catch', e);
  }) // Promise를 반환
  .catch((e) => {
    // 1번째 catch블럭안에 예외가 없기 때문에 실행되지 않는다. 따라서 then이었다면 실행된다.
    console.error('2nd catch', e);
  });
```

#### 2. then의 두번째 인자인 onReject를 활용한다.

```javascript
rejectedWait(1).then(
  () => {},
  (e) => {
    // then의 두번째 인자는 onReject이다. (첫번째 인자는 onFulfilled)
    console.log('1nd catch in Then', e);
  }
);
```

#### 3. await 키워드를 사용해 동기적인 코드로 바꿔준다. (단 script를 부를 때 모듈이어야 하고, 모듈 최상단에서 await가 불가능한 브라우저들이 있다.)

```javascript
try {
  /* 2021년 8월 22일 현재 Safari on iOS, Samsung Internet, IE에서 async 키워드 없이 모듈 최상단에서 사용하는 await키워드를 지원하지 않는데 체킹해봐야 함
  문제의 브라우저들에서는 이 코드가 모듈 최상단에 해당되어 에러가 발생한다. 링크(Can I use | await: Use at module top level) https://caniuse.com/?search=await%20module%20top%20level */
  await rejectedWait(1); // await를 사용해 동기적 코드처럼 바꿔준다.
} catch (e) {
  console.error('try catch', e);
}
```

# Promise와 async 예외 처리 비교

### 1. Promise는 reject로 예외 처리 할 수 있다.

```javascript
function myPromiseErrorFun() {
  return new Promise((_, reject) => {
    reject('myPromiseError!');
  });
}
const resultPromise = myPromiseErrorFun().catch((e) => {
  console.error(e);
});
```

### 2. async는 throw로 예외 처리 할 수 있다.

```javascript
async function myAsyncErrorFun() {
  throw 'myAsyncError';
}
const resultAsync = myAsyncErrorFun().catch((e) => {
  console.error(e);
});
```

# async await 예외 처리하기

### 1. async함수안에서 await하는 Promise의 예외가 발생하면 throw를 반환한다.

```javascript
async function myAsyncFunCheckErrorNextCode() {
  console.log(new Date());
  await rejectedWait(1); // throw를 반환하고, 아래 구문들은 실행되지 않는다.
  console.log(new Date());
}
const resultAsyncFunCheckErrorNextCode = myAsyncFunCheckErrorNextCode();
```

### 2-1. async함수안에서

```javascript
async function myAsyncFunTryCatch1() {
  console.log(new Date());
  try {
    await rejectedWait(1); // throw 되었다.
  } catch (e) {
    console.error('myAsyncFunTryCatch1', e);
  }
  // 그 다음일들을 쭉쭉할 수 있다.
  console.log(new Date());
}
const resultAsyncFunTryCatch1 = myAsyncFunTryCatch1();
```

### 2-2. try catch구문을 Promise.catch로 바꾸기 (주의점 있음)

```javascript
async function myAsyncFunTryCatch2() {
  console.log(new Date());

  const cautionResult = await rejectedWait(1).catch((e) => {
    console.error('myAsyncFunTryCatch2', e);
  }); // catch로 잡아도 상관없다. 하지만! catch가 반환한 Promise를 await하므로
  console.log('cautionResult', cautionResult); // cautionResult에는 catch가 반환하는 Promise의 fulfilled인 undefined가 반환된다.

  console.log(new Date());
}
const resultAsyncFunTryCatch2 = myAsyncFunTryCatch2();
```

### 3. async함수 내부에서 예외가 있을 경우

```javascript
async function myAsyncFunSomethingWrong() {
  coSomethingWrong.log(new Date());
}
```

#### 자주 하는 실수

```javascript
try {
  myAsyncFunSomethingWrong();
} catch (e) {
  // myAsyncFunSomethingWrong에서는 Promise를 반환할 뿐 에러는 없기 때문에 catch에서 에러를 잡아 내지 못한다.
  console.log('try catch, myAsyncFunSomethingWrong!!!!', e);
}
```

#### 해결법

```javascript
myAsyncFunSomethingWrong().catch((e) =>
  console.error('Promise.catch, myAsyncFunSomethingWrong!!!!', e)
);
```
