

## **[Symbol.iterator()];**


최근 이터레이터를 학습하게 되면서 이 코드를 굉장히 많이 사용하게 되었는데, 이터레이터/이터러블 프로토콜에 대해서는 강의를 통해 설명을 들었기 때문에 인지하고 있었으나 `Symbol`은 거의 별안간 나타난 키워드였기 때문에 이번 스터디 주제에 `Symbol`을 선정하여 학습하고자 했다.

![](https://images.velog.io/images/gyulhana/post/bb849407-fd11-486c-835e-ab86efc5d669/%EC%8A%AC%EB%9D%BC%EC%9D%B4%EB%93%9C3.PNG)

사실 ES5까지 JavaScript에서 가지고 있던 데이터타입은 `Number`, `String`, `Boolean`, `undefined`, `null`, `object` 정도였고(물론 그 외에도 더 있다고 할 수 있지만) `Symbol`이란 데이터 타입은 존재하지 않았다.

![](https://images.velog.io/images/gyulhana/post/0d7a71c2-b4d8-40b3-bf06-0fe4af191a7e/%EC%8A%AC%EB%9D%BC%EC%9D%B4%EB%93%9C4.PNG)

그러나 ES6+에서 `Symbol`과 `BigInt`라는 놈이 등장하게 되었다. `BigInt`는 간략하게 설명하자면 여태까지 JS에서 우리가 정확하게 구현할 수 있던 수는 2의 53승`(9007199254740992)`-1까지였다. 그 이후의 수는 정확하게 계산할 수 없었고 보통 이렇게 큰 수는 잘 사용되지 않지만 당연히 그래도 사용이 필요한 순간들이 있고 이를 해결하기 위해 생긴 데이터타입이다. `9007199254740992`를 초과하는 수 뒤에 `n`을 붙여 사용한다. 하지만 일반적인 `Number` 데이터타입과는 함께 계산이 불가하가고 `BigInt`타입은 `BigInt`타입끼리만 가능하다.

그래서 이런 건 됐고 도대체 `Symbol`은 뭐냐는거다.


### 📝 Symbol( )
> `심볼(symbol)` 데이터 형은 **원시** 데이터 형의 일종으로 `symbol()` 함수는 `심볼(symbol)` 형식의 값을 반환하는데 반환되는 모든 심볼 값은 **고유**하다.

>심볼은 주로 이름의 `충돌 위험이 없는 유일한 객체의 프로퍼티 키(property key)`를 만들기 위해 사용한다.

👇 How to use Symbol
```
const symbol = Symbol(); 
const hello = Symbol("Hello!");
```
`Symbol`을 사용하기 위해서는 무조건 `Symbol()`함수 호출을 해주어야 한다. 그리고 그 안에 문자열을 파라미터로 전달할 수 있긴 하지만 어떤 영향을 주지는 않는다. 

### 📝 Symbol( )의 특징 


방금 문자열을 파라미터로 받을 순 있지만 그게 어떤 영향을 주진 않는다고 한 것처럼 즉, 이 문자열이 같다고 해서 같은 값으로 평가되지 않는다는 것이다.

👇 Code Example : 같아 보이지만 모두 `false`를 리턴하고 있다. 

```
console.log(Symbol() === symbol); // false
console.log(Symbol() === Symbol()); // false
console.log(Symbol("Hello!") === hello); // false
```

이를 통해 `Symbol`의 **유일무이하다**는 의미에 대해서 조금 느낄 수 있다.

사실, 자바스크립트 데이터 타입의 큰 특징 중 하나는 데이터 타입의 형 변환이 아주 유연하게 이루어진다는 것이다. 예를 들어 개발자가 의도하지 않았다고 하더라도 적절한 형 변환을 자동으로 적용한다.

```
const string = 'a' + 1;
console.log(string); // "a1"
```

이렇듯 문자열과 숫자를 더하고자 하는 상황에서 자동적으로 숫자의 데이터타입을 문자열로 변환해 더해준다. 그러나 `Symbol`은 이런 **데이터 형 변환이 불가능**한 데이터 타입이다.

그렇기 때문에 아까 만들어둔 `hello`라는 `Symbol`타입 변수에 `!`하나를 더 추가해주고 싶다고 한다면, 즉시 심볼 값을 스트링으로 변환할 수 없다는 타입 에러가 출력된다.

👇 Code Example

```
const hello = Symbol("Hello!");
const hello2 = hello + '!'; 
// TypeError: Cannot convert a Symbol value to a string
```

그리고 `Symbol`타입은 **기본적인 열거 대상에서 제외**된다. 만약 어떤 객체를 선언하고 그 객체의 프로퍼티를 `Symbol`로 할당할 수 있다. 그러나 이후, 해당 프로퍼티에 접근한다거나 `for.. in`을 통해 순회하고자 할 때 `Symbol` 타입인 경우는 접근이 불가해 탐색에서 제외된다. 그렇기 때문에 프로퍼티 키를 은닉하고자 할 때 많이 사용한다. 

```
const TYPE = Symbol('타입');
const FLAVOR = Symbol('맛');

const icecream = {
	[TYPE] : "아이스크림",
  	[FLAVOR] : "MINT CHOCOLATE",
  price : 3500,
}

const cupcake = {
	[TYPE] : "컵케이크",
  	[FLAVOR] : "Vanilla Cream",
  price : 5700,
}

console.log(icecream[FLAVOR], cupcake[FLAVOR]); 
// 접근 가능 : "MINT CHOCOLATE", "Vanilla Cream"
```

물론 이렇게 이미 `TYPE`과 `FLAVOR`로 선언해준 값을 바로 접근하여 출력하고자 하면 가능하지만, 아래의 예시를 보면 해당 탐색에 전혀 포함되어 있지 않음을 알 수 있다.

```
for (const i in icecream) {
	console.log(`${i} : ${icecream[i]}`); // "price : 3500"
}

for (const c in cupcake) {
	console.log(`${c} : ${cupcake[c]}`); // "price : 5700"
```

`Object.keys()`, `Object.getOwnPropertyNames()`를 통해서도 위의 예시처럼 `price`에만 접근이 가능하다. 그렇기 때문에 `Symbol`을 통해 할당된 키값에 접근하기 위해서는 `Object.getOwnPropertySymbols()`를 사용해야 하는데

```
Object.getOwnPropertySymbols(icecream).forEach(i => console.log(i, icecream[i]));
// Symbol(타입) "아이스크림" 
// Symbol(맛) "MINT CHOCOLATE"
```
이 경우엔 정말 딱 심볼로 선언된 키값들만 출력이 되기 때문에 그 객체 안에 들어있는 키와 밸류를 심볼이든 아니든 상관 없이 모두 접근해 출력해보고 싶다면 `Reflect.ownKeys()`를 사용해야만 모든 키에 접근이 가능하다. 

```
Reflect.ownKeys(icecream).forEach(i => console.log(i, icecream[i]));
// price 3500 
// Symbol(타입) "아이스크림" 
// Symbol(맛) "MINT CHOCOLATE"
```

## 📝 Symbol.for() / Symbol.keyFor()

앞서 심볼의 특징에서 두 변수에 심볼을 통해 값을 할당할 때 심볼 함수의 파라미터로 같은 문자열을 넣는다고 하더라도 심볼은 유일무이한 값을 생성하기 때문에 두 변수는 다르다고 판단한다는 것을 알 수 있었다. 하지만 심볼에 같은 문자열을 담고 있는 변수를 생성하고 싶으면 어떻게 해야 할까? 이럴 때 `Symbol.for()` 메소드를 사용할 수 있다.

- **Symbol.for( )** : 파라미터로 전달받은 문자열을 키로 사용해서 키와 심벌 값의 쌍들이 저장되어 있는 `전역 심볼 레지스트리`에서 해당 키가 있는지 탐색을 하고 만약 이미 존재한다면 해당 값을 반환해주고, 없다면 새로운 전달받은 키로 심볼 값을 생성해 `전역 심볼 레지스트리`에 저장한 후 값을 반환해준다. 
\* 전역 심볼 레지스트리(gloabl symbol registry) : JS 엔진이 관리하는 심볼 값 저장소

```
const s1 = Symbol.for('hello, stranger');
const s2 = Symbol.for('hello, stranger');

console.log(s1 === s2); // true
```

- **Symbol.keyFor( )** : `Symbol.for()` 메소드를 통해 전역 심볼 레지스트리에 저장된 키 값을 찾아서 추출, 반환해준다.

```
Symbol.keyFor(s1); // "hello, stranger"
Symbol.keyFor(s2); // "hello, stranger"
Symbol.keyFor(hello); // undefined
```
기존에 그냥 `Symbol()`함수를 호출해서 생성한 심볼 값은 전역 심볼 레지스트리에 저장되지 않기 때문에 `undefined`만 출력된다.


## 📝 Well-Known Symbol : 표준 확장 혹은 이터러블 타입 확인
`console.dir(Symbol)`을 콘솔 창에 입력하면 이런 결과를 확인할 수 있다. 이는 자바스크립트에서 기본으로 제공하는 빌트인 심벌 값들인데 `Symbol`함수의 프로퍼티에 할당되어 있으며 ECMA에서는 이를 Well-Known Symbol이라 부른다. 또한, 이는 자바스크립트 엔진의 내부 알고리즘에 사용된다.

![](https://images.velog.io/images/gyulhana/post/1e342b92-1c63-44c8-80f9-696aa917f5c7/image.png)

**+)**
여기서 `Symbol.iterator`는 `for.. of`로 순회 가능한 빌트인 이터러블이 가진 메소드의 키로써 사용되며 `Symbol.iterator` 메소드를 호출하면 이터러블 프로콜을 따라 이터레이터가 반환된다.

```
const range = {
	[Symbol.iterator]() {
    	let start = 1;
        const end = 10;
        
        return {
        	next() {
            	return {value : start++, done: start > end + 1};
            }
        };
    }
  };

for (const r of range) console.log(r); 
```

빌트인 이터러블이 아닌 객체를 이터러블처럼 동작하게 하고 싶다면 `Symbol.iterator`를 키로 가지는 메소드를 객체에 추가하여 이터레이터를 반환하도록 하면서 위와 같이 사용할 수 있다.


### 결론
심볼은 어떤 것과도 중복되지 않는 상수 값을 생성한다. 그렇기 때문에 기존에 작성된 코드에 어떤 영향도 주지 않은 채로 새로운 프로퍼티를 추가할 수 있으며, 하위 호환성을 보장하기 위해 도입된 데이터 타입이다.
<br><br>

### ✨ References.
https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Symbol
https://poiemaweb.com/es6-symbol
http://hacks.mozilla.or.kr/2015/09/es6-in-depth-symbols/
https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Symbol/iterator
https://another-light.tistory.com/105
https://pks2974.medium.com/javascript%EC%99%80-%EC%8B%AC%EB%B3%BC-symbol-bbdf3251aa28
https://geundung.dev/83
https://jongbeom-dev.tistory.com/138
https://javascript.info/symbol#system-symbols
