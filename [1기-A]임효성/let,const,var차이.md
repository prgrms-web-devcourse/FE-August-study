
JS에서 var로 변수 선언이 가능했는데, 왜 const와 let이 나왔으며 이 둘의 사용을 권장할까?

이를 정확하게 알기 위해서는, **변수의 선언 및 할당 과정, 호이스팅, 스코프**를 알아야한다!

# 📥 변수의 선언 및 할당 과정

### 변수란?

- 하나의 값을 저장하기 위해 확보한 메모리 공간 자체 또는 그 메모리 공간을 식별하기 위해 붙인 이름.
- JS는 managed language 이기 때문에 개발자가 직접 메모리를 제어하지 못함.
- 개발자는 변수를 통해 안전하게 값에 접근이 가능.

### const로 알아보자

```jsx
const myNumber = 23
// 변수명(식별자): myNumber
// 해당 값의 위치(메모리 주소): 0012CCGWH80
// 변수 값(저장된 값): 23
```


- 변수명(식별자)인 myNumber는 변수의 값이 아닌 메모리 주소를 기억.
- 자바스크립트 엔진이 변수명과 매핑된 메모리 주소를 통해 저장된 값(23)을 반환.
- 이처럼 변수에 값을 저장하는 것을 **할당**(assignment), 변수에 저장된 값을 읽어 들이는 것을 **참조**(reference).
- 변수명을 자바스크립트 엔진에 알리는 것을 **선언**(declaration)이라 한다.

### 변수 선언과 할당

- 선언은 `var`, `const`, `let`으로 할 수 있음.
- 자바스크립트에서 변수 선언은 선언 → 초기화 → 할당을 거쳐 수행됨.
    - **선언 단계**: 변수명을 등록하여 자바스크립트 엔진에 변수의 존재를 알린다.
    - **초기화 단계**: 값을 저장하기 위한 메모리 공간을 확보하고 암묵적으로 undefined를 할당해 초기화한다.

```jsx
var myName // 변수 선언
myName = 'hyosung' // 값의 할당
```

---

# 🎈 호이스팅

```jsx
var myName;
console.log(myName) // output: undefined
```

```jsx
console.log(myName) // output: undefined
var myName;
```

- 변수 선언이 런타임에서 되는 것이 아니라, 그 이전 단계에서 먼저 실행되기 때문.
- 자바스크립트 엔진은 소스코드를 한 줄씩 순차적으로 실행하기에 앞서, 변수 선언을 포함한 모든 선언문을 찾아내 먼저 실행한다.
- 즉, 변수 선언이 어디에 있든 상관없이 다른 코드보다 먼저 실행되는 특징을 **호이스팅**(hoisting)이라 한다.
- `var`, `let`, `const`, `function`, `function*`, `class` 키워드를 사용해 선언한 모든 식별자(변수, 함수, 클래스 등)는 호이스팅 됨.

---

# 📦 스코프

- 스코프(scope)는 식별자(ex. 변수명, 함수명, 클래스명 등)의 유효범위를 말함.
- 선언된 위치에 따라 유효 범위가 달라짐.
- 전역에 선언된 변수는 **전역 스코프**, 지역에 선언된 변수는 **지역 스코프**.

### var를 주의하자

- JS에서 모든 코드 블록(if, for, while, try/catch 등)이 지역 스코프를 만듦 → **블록 레벨 스코프**
- 하지만 var는 블록 레벨 스코프를 무시하고 전역 스코프나 함수 스코프로 선언됨 → **함수 레벨 스코프**

```jsx
var a = 1 //전역 변수 선언

if (true) {
  var a = 5 //var의 중복선언
}

console.log(a) // output: 5
```

---

# 🌟 드디어 알아보는 차이점

앞에서 발견한 var의 문제점을 요약하자면

- 변수 중복 선언 가능하여, 예기치 못한 값을 반환할 수 있음.
- 변수 선언문 이전에 변수를 참조하면 언제나 undefined를 반환함.
- 함수 레벨 스코프로 인해 선언한 변수는 모두 전역 변수가 됨.

ES6에서 나온 let과 const는 위의 문제점을 해결함.

### 변수 중복 선언 불가

- let - 변수 중복 선언 불가, 재할당 가능.

```jsx
let fruit = 'apple';
console.log(fruit); // 'apple'
 
let fruit = 'banana';
console.log(fruit); // SyntaxError: Identifier 'fruit' has already been declared
 
fruit = 'coconut';
console.log(fruit); // 'coconut'
```

- const - 변수 중복 선언 불가, 원시값 재할당 불가, 객체는 재할당 가능.

```jsx
//원시값 재할당
const fruit = 'apple';
console.log(fruit); // 'apple'
 
const fruit = 'banana';
console.log(fruit); // SyntaxError: Identifier 'fruit' has already been declared
 
fruit = 'coconut'; // TypeError: Assignment to constant variable.

// 객체 재할당
const name = {
  eng: 'hyosung',
}
name.eng = 'minsu'

console.log(name) // output: { eng: "minsu" }
```

### 블록 레벨 스코프

let, const 키워드로 선언한 변수는 지역 스코프로 인정하는 블록 레벨 스코프를 따름.

```jsx
let a = 1

if (true) {
  let a = 5
}

console.log(a) // output: 1
```

### 변수 호이스팅

- let - 선언 단계와 초기화 단계가 분리되어 진행.

```jsx
console.log(name) // output: Uncaught ReferenceError: name is not defined

let name = 'hyosung'
```

- const - 선언 단계와 초기화 단계가 동시에 진행.

```jsx
console.log(name) // output: Uncaught ReferenceError: Cannot access 'name' before initialization

const name = 'hyosung'
```

- let 키워드로 선언한 경우, 런타임 이전에 선언이 되어 자바스크립트 엔진에 이미 존재하지만 초기화가 되지 않았기 때문에 name is not defined라는 문구가 떴다.
- 하지만 const 키워드로 선언한 경우, 선언과 초기화가 동시에 이루어져야 하지만 런타임 이전에는 실행될 수 없다.
- 따라서 초기화가 진행되지 않은 상태이기 때문에 Cannot access 'name' before initialization 에러 문구가 뜬다.

---

# ‼️ 결론

- 기본적으로 변수의 스코프는 최대한 좁게 만드는 것을 권장함.
- 따라서, var 보다 let과 const를 사용하며, 변경하지 않는 값(상수)이라면 let 보다는 const 키워드를 사용하는 것이 안전함.