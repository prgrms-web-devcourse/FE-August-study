## 실행 컨텍스트 (Execution Context)

실행 가능한 코드를 형상화하고 구분하는 추상적인 개념

-> 실행 가능한 코드가 실행되기 위해 필요한 환경

<br/>

> 실행가능한 코드 : 전역코드, 함수 코드, eval코드
>
> 실행에 필요한 정보 : 변수, 함수 선언, 스코프, this

<br/>
<hr/>
<br/>

## 실행 컨텍스트 스택

자바스크립트 엔진이 실행에 필요한 정보를 형상화하고 구분하기 위해 물리적 객체로 관리.

코드 실행시 실행 컨텍스트 스택이 생성/소멸

현재 실행 중인 컨텍스트에서 관련 없는 코드가 실행되면 새로운 컨텍스트 생성, 제어권 이동.

<br/>

> 전역 실행 컨텍스트 (Global Execution Code)
>
> 코드 실행시 최초로 생성되는 실행 컨텍스트. 애플리케이션 종료시 까지 유지.

<br/>
<hr/>
<br/>

### 실행 컨텍스트의 3가지 객체

> (추상적 개념이지만 물리적으로는 객체의 형태)

![](https://images.velog.io/images/94chl/post/8c331740-4922-42c6-832e-2b0e19e7d68b/image.png)

<br/>

- Varialbe Object(변수객체)
  실행 컨텍스트 생성 시, 실행에 필요한 정보를 담을 객체. JS엔진에 의해 참조되며 코드에서는 접근 불가.

  > 담는 정보: 변수, 매개변수(parameter)/인수정보(arguments), 함수 선언
  >
  > 가리키는 객체가 2종류로 각각 다름

  - 전역 컨텍스트 : 전역객체(Golbal Object)

    VO의 유일하고 최상위에 위치하며, 모든 전역 변수/함수 등을 포함하는 전역 객체를 가리킴. 전역에 선언된 전역 변수와 전역 함수를 프로퍼티로 소유,

  - 함수 컨텍스트 : 활성 객체(Activatipn Object)

    매개변수와 인수들의 정보를 배열의 형태로 가진 객체인 `argument object`가 추가됨

<br/>

- Scope Chain
  일종의 리스트로서 전역 객체와 중첩된 함수의 스코프의 레퍼런스를 차례로 저장

  해당 전역/함수가 참조할 수 있는 변수, 함수 선언 등의 정보를 담고있는 전역 객체/활성 객체의 리스트

  현재의 실행 컨텍스트의 활성 객체(Activation Object)를 선두로, 순차적으로 상위 컨텍스트의 활성객체를 가리킨다. (마지막 대상은 전역 객체)

  -> 부모함수의 스코프가 자식 함수의 스코프 체인에 포함된다.

  -> 역으로 거슬러 올라가면서 검색 가능
  (끝까지 못찾으면 Reference 에러)

  -> 그래서 이름이 스코프 “체인”

  > 식별자 중에서 객체의 식별자(변수)를 검색하는 메커니즘(전역 객체 제외)
  >
  > 함수의 숨겨진 프로퍼티인 `[[scope]]`로 참조가능

<br/>

- thisValue

  `this`의 값을 할당. 할당 값은 함수 호출 패턴에 의해 결정

<br/>
<hr/>
<br/>

## 실행 컨텍스트 생성 과정

```javascript
const x = 5;

function timesTen(a) {
  return a * 10;
}

const y = timesTen(x);

console.log(y);
```

![](https://images.velog.io/images/94chl/post/20621285-d4c8-431d-afe2-3b7f3478c4c5/image.png)

<br/>
<hr/>
<br/>

### 1. 전역 코드 진입

실행 컨텍스트에 컨트롤이 진입하기 이전, 유일한 전역객체가 생성

전역 객체는 단일 사본으로 존재하며 이 객체의 프로퍼티는 코드의 어떠한 곳에서도 접근 가능

초기 상태의 전역 객체에는 빌트인 객체(Math, String, Array 등)와 BOM, DOM이 설정되어 있다.

전역 객체 생성 후, 전역 코드로 컨트롤이 진입하면 전역 실행 컨텍스트가 생성되고 실행 컨텍스트 스택에 쌓임

이후 이 실행 컨텍스트를 바탕으로 이하의 처리가 실행됨

<br/>

#### 1) 스코프 체인의 생성/초기화

스코프 체인은 전역 객체의 레퍼런스를 포함하는 리스트

<br/>

#### 2) 변수 객체화(Variable Instantiation)

변수객체VO(전역 코드는 GO)에 프로퍼티와 값을 추가하는 것을 의미

변수, 매개변수(paramter)와 인수 정보(arguments), 함수 선언을 변수객체VO에 추가하여 객체화

- 변수객체화 순서
  1. 매개변수(parameter)가 VO의 프로퍼티로, 인수(argument)가 값으로 설정
  2. 대상 코드 내의 **함수 선언식**을 대상으로 함수명이 VO의 프로퍼티로, 생성된 함수 객체가 값으로 설정(함수 호이스팅)
  3. 대상 코드 내의 **변수 선언**을 대상으로 변수명이 VO의 프로퍼티로, undefined가 값으로 설정(변수 호이스팅) (\*순서: 선언단계! -> 초기화 단계 -> 할당 단계)

<br/>

#### 3) this value 결정

함수 호출 패턴에 의해 this에 값 할당.

(this value 결정 전에는 this는 전역 객체를 가리킴)

> 전역 컨텍스트(코드)의 경우 VO, 스코프 체인, this value가 항상 전역 객체

![](https://images.velog.io/images/94chl/post/2fe6b754-d8c9-4e49-a0cf-e85a1ae51039/image.png)

<br/>
<hr/>
<br/>

### 2. 전역 코드 실행

![](https://images.velog.io/images/94chl/post/680fc614-1af3-4ddf-9b56-63415c2dade6/image.png)

<br/>
<hr/>
<br/>

### 3. 함수 코드 실행

![](https://images.velog.io/images/94chl/post/03d7467e-d735-43f3-b385-6c3af7070c7f/image.png)

<br/>
<hr/>
<br/>

## 참고자료

https://poiemaweb.com/js-execution-context

https://www.javascripttutorial.net/javascript-execution-context/
