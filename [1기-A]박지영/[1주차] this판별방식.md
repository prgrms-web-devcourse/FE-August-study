# ✔ `this` 키워드란?

자바스크립트의 모든 함수(화살표 함수 제외) 내부에서는 별도의 변수 선언없이 `this`라는 키워드를 사용할 수 있다. (비슷한 예, `arguments` 키워드)

</br>

# ✔ `this`의 용도

## 1) "나"의 용도

- [박지영]: 나는 집에 가는 중입니다.      ⇒  "나"는 "박지영"
- [전지현]: 나는 집에 가는 중입니다.      ⇒  "나"는 "전지현"

⇒ 같은 문장이지만, 상황에 따라서 **유연하게 적용**될 수 있으며 **재사용성** 또한 훌륭하다.

</br>

## 2) 함수의 유연성&재사용성 UP

`this` 키워드 또한 유사한 용도로 **함수를 더욱 유연하고 재사용성이 뛰어나게 만들 수 있다.**

</br>

# ✔ `this`는 함수 실행 시점에 결정

- 자바스크립트의 `this` 키워드는 함수 내부에서 사용됩니다.
- this 값은 this를 포함하고 있는 함수가 "어떻게 실행되느냐"에 따라 결정됩니다. 즉, 함수 선언 시점이 아닌, **함수 실행 시점에 결정**되는 값입니다.

⇒ this 값을 판별하기 위해서는 함수의 실행문을 찾아야 한다.

⇒ 맨 아래의 두 줄은 같은 함수를 실행하지만, 실행방식이 다르기 때문에 출력되는 값 또한 다르다.

</br>

# ✔ `this`의 4가지 판별 방식
### 1) Regular Function Call

- strict mode : `this` refers to `undefined`
- non strict mode :  `this` refers to **Global Object(`window` 객체)**

### 2) Dot Notation (Object Method Call)

- `this` refers to **object with the dot**

### 3) Call, Apply, Bind

- `this` refers to **explicit value**

### 4) "new" Keyword

- `this` refers to **empty object**

</br>

# ⭐1. Regular function call & strict mode
일반 함수 실행(호출) 방식일 경우, 해당 함수의 this 값은 Global Object이다.(브라우저에서는 window 객체)</br>
단, Strict Mode에서는 undefined이다.

### 예제 1 - 함수명 호출

```javascript
var age = 30;

function log () {
  console.log(this.age);
}

log();      // 출력 : 30
```

### 예제 2 - 객체의 키값인 함수 실행

```javascript
var age = 30;

const person = {
  age: 20,
  printAge: function () {
    bar();       // bar()가 일반함수 실행 방식인 것만 보면 됨
  }
};

function bar () {
  console.log(this.age);
}

person.printAge();    // 출력 : 30
//`this`를 담고 있는 `bar()` 함수가 어떻게 실행되었는지만 보면 됨
```

</br>

# ⭐2. Dot Notation
Dot Notation을 이용하여 함수를 실행할 경우, 해당 함수의 this 값은 Dot 앞에 있는 객체이다.

### 예제 1 - regular function call과 비교

```javascript
var age = 100;

const coco = {
  age: 3,
  foo: function () {
    console.log(this.age);
  }
};

const func = coco.foo;

// Dot Notation
coco.foo();    // 출력결과 : 3

// Regular Function Call
func();       // 출력결과 : 100
```

</br>

# ⭐3. Call, Apply, Bind & Explicit Binding
.call, .apply, .bind 메소드를 사용하는 Explicit Binding 방식은 특정 this 값을 지정한다.

### call 예제 : 함수 실행 + `this`값 지정 + 매개변수 전달

```javascript
function foo (a, b, c) {
    console.log(this.age);
    console.log(a + b + c);
}

const coco = {
    age: 3
};

foo.call(coco, 1, 2, 3);  // 출력결과 : 3, 6
```

- 첫 번째 인자는 해당 함수의 `this` 값으로 사용한다.
- 두 번째부터 나머지 인자들은 해당 함수의 인자로 전달된다.

- `.call` 메소드는 받을 수 있는 **인자의 개수의 제한이 없다.**


### apply예제 : 함수 실행 + `this`값 지정 + 매개변수 전달

- 차이점은?`.apply` 는 **인자의 개수가 2개**이다.
첫 번째 인자는 `this`값이고, 두 번째 인자는 **배열**이며, 그 배열의 요소를 함수의 인자로 전달한다.

```javascript
function foo (a, b, c) {
    console.log(this.age);
    console.log(a + b + c);
}

const coco = {
    age: 3
};

foo.apply(coco, [ 1, 2, 3 ]);   // 출력결과 : 3, 6
```

### bind예제 - `.call`이나 `.apply`가 함수를 바로 실행시키는 것과 달리, `.bind`는 함수를 실행하지 않고 새로운 함수를 반환한다.

```javascript
function foo () {
    console.log("hello!");
}

const bar = foo.bind();
// bind 메소드가 새로운 함수를 반환하고, 그 함수를 bar라는 변수에 할당함

bar();
// 함수가 담긴 bar를 실행할 때, bind메소드가 사용된 foo()함수가 실행됨
```

-  `.bind` 메소드가 반환한 함수(즉, `bar()`함수)를 실행할 때, 메소드가 사용된 함수(즉, `foo()` 함수)가 실행된다.


</br>

# ⭐4. "new" keyword & 생성자함수
"new" keyword를 이용한 함수 실행 방식의 경우, 해당 함수의 `this` 값은 빈 객체가 된다.

### 예제 - 빈객체를 반환하고 속성을 추가할 수 있다.

```javascript
function person (name) {
    this.name = name;
    console.log(this);
}

new person("coco");  // 출력결과 : {name: "coco"}
```

</br>

# 🔥 화살표 함수에서의 this
화살표 함수는 `this`와 `arguments` 값을 갖고 있지 않다. 따라서 선언되지 않은 변수로 받아들여서, 기본 스코프 체인 규칙에 따라 상위 스코프를 탐색하게 된다.
⇒ 즉, this와 arguments가 없지만, 작동은 할 수 있다.

```javascript
function foo () {
  return {
		log: () => {
			console.log(arguments);
		}
	};     //foo()함수는 객체를 return하는데,
}        //그 객체에는 log라는 메서드가 있음

const obj = foo(1, 2, 3);  // obj = { log: () => consle.log(arguments) }
const obj2 = foo(4, 5, 6); // obj2 = {log: f }

obj.log();  // [object Arguments] { 0: 1, 1: 2, 2: 3}
obj2.log();  // [object Arguments] { 0: 4, 1: 5, 2: 6}
```
