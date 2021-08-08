# Generic

## Generic을 발표하는 이유

- API 문서를 읽는 능력을 기르기 위해
  ![api문서예시](https://user-images.githubusercontent.com/84858773/128577079-cba5c6f5-5d08-4c53-aaf5-f147e1ceaeae.png)

<br>

## Generic이란?

- 정적 타입 언어에서, 재사용 가능한 컴포넌트를 생성하는 주요 도구
- 제네릭은 어떤 클래스 혹은 함수에서 사용할 타입을 선언 시점이 아니라 생성 시점에 타입을 결정해서 하나의 타입만이 아닌 다양한 타입을 사용할 수 있도록 하는 프로그래밍 기법
- 제네릭은 타입을 마치 함수의 파라미터처럼 사용한다

  <br>

### 정적 타입 언어 vs 동적 타입 언어

- Java나 C# 같은 정적 타입 언어의 경우, 함수 또는 클래스를 정의하는 시점에 매개변수나 반환값의 타입을 선언하여야 한다. 그래서 기본적으로는 특정 타입을 위해 만들어진 클래스나 함수를 다른 타입을 위해 재사용할 수가 없다.
  ![자바예시](https://user-images.githubusercontent.com/84858773/128578540-05429bc8-810d-4a95-9ca4-792b99406489.JPG)
- JavaScript는 동적 타입 언어. 변수를 정의할 때 타입을 선언하지 않는다. 값을 할당할 때, 할당하는 값의 타입에 의해 자동으로 반환값의 타입이 결정되기 때문이다. 그래서 실수로 의도와 다른 타입의 값을 할당했을 때, 런타임 에러가 나기 전에는 타입 에러를 잡을 수 없다는 문제가 있다.

  ```js
  let text = "hello";
  console.log(typeof text); // string
  console.log(text.charAt(0)); // h

  text = "5" - "3"; // 의도하지 않은 실수 ❗❗❗
  console.log(typeof text); // number
  console.log(text.charAt(0)); // TypeError: text.charAt is not a function
  ```

- 타입 에러를 런타임이 아닌 컴파일 단계에서 잡을 수 있도록 동적 타입 언어의 문제점을 보완한 정적 타입 언어인 Typescript가 등장한다.

  ```ts
  function identity(arg: number): number {
    return arg;
  }
  // identity 함수는 number 타입의 인자 값을 받고 number 타입의 반환 값을 반환하는 함수
  ```

  <br>

## Generic을 사용하는 이유

다양한 타입에서 작동하는 컴포넌트를 만들어 봅시다.

  <br>

### 제네릭이 없다면

- 일일이 타입에 대응하는 함수를 만들어줘야 한다.

  ```ts
  function identityNum(arg: number): number {
    return arg;
  }

  function identityStr(arg: string): string {
    return arg;
  }

  function identityBool(arg: boolean): boolean {
    return arg;
  }
  ```

- 어떤 타입이든 받을 수 있는 `any`를 사용할 수도 있다. 하지만 함수의 인자로 어떤 타입이 들어갔고 어떤 값이 반환되는지 알 수 없다. `any`라는 타입은 타입 검사를 하지 않기 때문이다. (타입이 보장되지 않기 때문에 타입스크립트에서 `any`를 사용하는 것을 권장하지 않는다.)

  ```ts
  function identityAny(arg: any): any {
    return arg;
  }

  let imAny = identityAny(123);
  imAny = identityAny("나는 문자다");
  ```

  ![image](https://user-images.githubusercontent.com/84858773/128582148-c7e0f13e-4e5b-491d-8d75-55417bd88165.png)

### 제네릭을 사용하면

- 함수를 호출하거나 인스턴스를 생성할 때 인자 값의 타입에 따라 해당 함수에서 사용할 수 있는 타입을 컴파일러가 자동으로 정해준다. 다른 타입의 값을 인자로 할당하려고 하면 타입 에러를 띄운다.

  ```ts
  function identity<GENERIC>(arg: GENERIC): GENERIC {
    return arg;
  }

  let imNumber = identity(123);
  imNumber = identity("나는 문자다");
  ```

  ![image](https://user-images.githubusercontent.com/84858773/128582206-eae533e0-58d1-4d45-9719-728e5d13769e.png)
  ![image](https://user-images.githubusercontent.com/84858773/128583868-5816b02b-e17d-4065-86eb-7dd1ceedca70.png)

<br>

## Generic 사용 예시

```ts
function identity<T>(arg: T): T {
  return arg;
}

// 함수를 호출하는 2가지 방법

let howToCall1 = identity<string>("myString");
let howToCall2 = identity("myString"); // type argument inference
```

- 함수명 옆에 제네릭을 사용하겠다는 의미로 `<>` 꺽쇠를 넣고 `T`와 같이 그 안에 타입으로 사용되는 식별자를 넣는다.
- `T`는 제네릭을 선언할 때 관용적으로 사용되는 식별자로 '타입 파라미터(Type parameter)'라 한다. T는 Type의 약자로 반드시 T를 사용하여야 하는 것은 아니다.
- 함수를 호출할 때 일반적으로 2번째 방법을 사용한다. 코드도 더 짧고 가독성이 좋기 때문이다. 하지만 코드가 복잡해져서 명시적으로 타입을 표기해주고 싶다면 1번째 방법을 사용하면 된다.

<br>

## Generic 문법

- 클래스 예시

  ```ts
  class GenericNumber<T> {
    zeroValue: T;
    add: (x: T, y: T) => T;
  }

  let myGenericNumber = new GenericNumber<number>();
  myGenericNumber.zeroValue = 0;
  myGenericNumber.add = function (x, y) {
    return x + y;
  };

  console.log(myGenericNumber.zeroValue); // 0
  console.log(myGenericNumber.add(1, 2)); // 3
  ```

* 함수에서 배열 인자를 사용하고 싶을 때

  ```ts
  // 틀린 예
  function logText<T>(text: T): T {
    console.log(text.length); // Error: Property 'length' does not exist on type 'T'
    return text;
  }

  // 올바른 예
  function logText<T>(text: T[]): T[] {
    console.log(text.length);
    return text;
  }

  console.log(logText<string>(["success", "fail", "success"])); // 3
  ```

* 두 개 이상의 타입 변수

  ```ts
  function toPair<T, U>(a: T, b: U): [T, U] {
    return [a, b];
  }

  console.log(toPair<string, number>("1", 1)); // [ '1', 1 ]
  console.log(toPair<number, number>(1, 1)); // [ 1, 1 ]
  ```

<br>

---

<br>

## 참고 문서

- [[Typescript Docs] Generics](https://www.typescriptlang.org/ko/docs/handbook/2/generics.html)
- [[캡틴판교님의 타입스크립트 핸드북] 제네릭](https://joshua1988.github.io/ts/guide/generics.html#%EC%A0%9C%EB%84%A4%EB%A6%AD-generics-%EC%9D%98-%EC%82%AC%EC%A0%84%EC%A0%81-%EC%A0%95%EC%9D%98)
- [[현섭님의 블로그] TypeScript: 제네릭(Generic)](https://hyunseob.github.io/2017/01/14/typescript-generic/)
- [[poiemaweb] 12.7 TypeScript - Generic](https://poiemaweb.com/typescript-generic)
- [[TCPschool] 제네릭의 개념](http://tcpschool.com/java/java_generic_concept)
- [[93jm님의 velog] JS - 동적타이핑(Dynamic typing)](https://velog.io/@93jm/JS-%EB%8F%99%EC%A0%81%ED%83%80%EC%9D%B4%ED%95%91Dynamic-typing)
