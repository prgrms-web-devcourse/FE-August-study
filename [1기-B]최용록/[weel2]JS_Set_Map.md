![](https://images.velog.io/images/94chl/post/a6825398-996b-4736-a93f-202d72ffd8ed/%EC%8D%B8%EB%84%A4%EC%9D%BC.png)

## 0. Set? Map?

Array, Object 두 가지 자료구조만으로는 현실 세계를 반영하기 어려운 탓에 ES6부터 추가된 자료구조

key-value의 형태로 데이터를 저장

<br/>
<br/>

### 필요성

Object는 iterable하지 못해 내부 데이터를 순회할 수 없음

이름 충돌의 위험 X

<br/>
<hr/>
<br/>

## 1. Set

중복되지 않는 값을 모아놓은 컬렉션

자료형에 관계 없이 원시값과 객체 참조 모두 유일한 값으로 저장가능

<br/>
<br/>

### 메서드 & 프로퍼티

<br/>
- 객체 생성

`new Set( [iterable] )`

iterable한 컬렉션이 파라미터로 전달된 경우, 요소 전부가 Set의 값으로 추가된다.

파라미터가 없거나, null이 전달되면 빈 Set이 생성된다.

```javascript
let it = [1, 2, { name: "최용록" }, [4]];
let set = new Set(it);
let arr = it;
it.pop();
//set : Set(4) {1, 2, {…}, Array(1)}
//array : (3) [1, 2, {…}]
```

<br/>
- 값 추가

`set.add( value )`

중복된 값을 허용하지 않기 때문에, 중복된 값을 add해도 아무런 반응이 일어나지 않는다.
<br/>

- 값 삭제

  `set.delete( value )`

  파라미터에 해당하는 값이 Set 내부에 있다면 해당 값을 삭제하고 True를 반환, 없을시 False를 반환

<br/>

- 값 확인

  `set.has( value )`

  Set 내에 값이 존재하면 True를 반환, 없을시 False를 반환

  해당 값의 위치(index)는 반환하지 않음

<br/>

- 값 전체 삭제

  `set.clear()`

<br/>

- 값 개수 확인

  `set.size`

<br/>

- 순회 메서드

  - `set.keys()`

    set 내의 모든 값을 담은 iterator 반환

  - `set.values()`

    set.keys()와 동일. map과 호환성을 위해 만들어짊

  - `set.entries()`

    `[값, 값]`의 형태로 반환. map과 호환성을 위해 만들어짊

```javascript
console.log(set.keys());
// SetIterator {1, 2, {…}, Array(1)}
console.log(set.values());
// SetIterator {1, 2, {…}, Array(1)}
console.log(set.entries());
// SetIterator {1 => 1, 2 => 2, {…} => {…}, Array(1) => Array(1)}
```

<br/>

### vs Array

- set은 중복된 값을 다루거나, 값의 유무를 체크할 때 유리

  ![](https://images.velog.io/images/94chl/post/3e685e0d-4646-4e24-ae92-743fc2adddfc/image.png)

- set은 index를 통한 접근이 안 됨

<br/>
<hr/>
<br/>

## 2. Map

키와 값이 쌍으로 이루어진 컬렉션

<br/>
<br/>

### vs 객체

<br/>
객체: 키 = 문자열, 심벌 / 이터러블x / length / 순서 x

<br/>
Map: 키 = 모든 값 / 이터러블o / size / 순서o

<br/>

### 메서드 & 프로퍼티

- 맵 생성

  `new Map()`
  <br/>

- 요소 추가

  `map.set(key, value)`

```javascript
map.set(2, "num2");
map.set(1, "num1"); // 숫자형 키
map.set("1", "str1"); // 문자형 키
map.set({ todo: "posting" }, "not yet"); // 객체형 키
```

![](https://images.velog.io/images/94chl/post/89c6cd0e-463a-4521-8cd1-e5d861d0cd1f/image.png)
<br/>

> 객체처럼 요소추가도 할 수는 있다. 하지만 이렇게 추가된 요소는 객체취급되어 map의 기능이 적용되지 않는다.

```javascript
map[5] = "5";
console.log(map.get(5)); // undefined
```

![](https://images.velog.io/images/94chl/post/9f818fb4-e459-4b5e-87d1-c748d46ff8f4/image.png)

> 체이닝
> map.set()은 호출할 때마다 자신을 반환.
> 동시에 여러개의 요소를 추가 가능
> `map.set().set().set()`

<br/>

- 요소 값 반환

  `map.get(key)`

  key를 파라미터로 입력하고 해당하는 값을 반환, 없으면 undefined

<br/>

- 요소 체크

  `map.has(key)`

  key를 파라미터로 입력하고 일치하는 key가 있으면 true 반환, 없으면 false

<br/>

- 요소 삭제

  `map.delete(key)`

  key를 파라미터로 입력하고 해당하는 값을 삭제

<br/>

- 요소 전체 삭제

  `map.clear()`

<br/>

- 요소 개수

  `map.size`

<br/>

- 순회 가능한 요소 반환

  `map[Symbol.iterator]()`

  `map.entries()`

  map 내부의 entries 중 순회할 수 있는 iterator를 반환

  > ![](https://images.velog.io/images/94chl/post/9cd4858f-d892-4ef1-acf2-9c05d5bf065e/image.png)

<br/>

- 순회 가능한 key 반환

  `map.keys()`

<br/>

- 순회 가능한 values 반환

  `map.values()`

<br/>
<hr/>
<br/>

## Q & A
