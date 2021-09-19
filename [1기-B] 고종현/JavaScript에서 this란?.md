# this란?

현재 실행되는 함수가 참조하고 있는 객체
(The object that is executing the current function)

## 1. method의 this

method가 포함된 객체를 가리킨다.

## 2. regular function의 this

browser/global object를 가리킨다.

- 위에서 regular function: 어떠한 객체에도 포함되어 있지 않고 화살표함수가 아닌 함수

# JavaScript의 this는 동작방식이 달라질 수 있다

## 1. 함수를 호출할 때 this 결정

this의 값은 함수를 호출하는 방법에 의해 결정

```javascript
const someone = {
  name: '종현',
  whoAmI: function () {
    console.log(this);
  },
};

const myWhoAmI = someone.whoAmI;

// 1. 호출할 때 this는 someone을 가리키고 있음
someone.whoAmI(); // { name: '종현', whoAmI: [λ: whoAmI] }

// 2. 호출할 때 this는 Window을 가리키고 있음
myWhoAmI(); // Window
```

```html
<body>
  <button id="btn">버튼</button>
</body>
```

```javascript
// 위 코드과 연결되는 코드

const btn = document.getElementById('btn');

btn.addEventListener('click', someone.whoAmI); // button 태그
btn.addEventListener('click', myWhoAmI); // button 태그
```

myWhoAmI와 some.whoAmI는 같다
버튼 클릭시 this는 button태그 자체가 된다

### JavaScript에서의 this의 핵심은 누가 실행했느냐 이다

## 2. 함수를 선언할 때 bind로 this를 묶어놓기

### bind로 함수를 정의할 때 함수가 누가 어디서 호출되었는지 상관없이 미리 설정할 수 있다

```javascript
const bindedWhoAmI = myWhoAmI.bind(someone);
btn.addEventListener('click', bindedWhoAmI); // someone
```

### bind를 쓰지 않은 예시

```javascript
const someone = {
  name: '종현',
  tags: ['코딩', '디자인', '창업'],
  whoAmI: function () {
    this.tags.forEach(function (tag) {
      console.log(this, tag); // this는 Window 객체를 가리킨다.
    });
  },
};

someone.whoAmI();
/*
Window, '코딩'
Window, '디자인'
Window, '창업'
*/
```

### bind의 예시

```javascript
const someone = {
  name: '종현',
  tags: ['코딩', '디자인', '창업'],
  whoAmI: function () {
    this.tags.forEach(
      function (tag) {
        console.log(this, tag); // this는 someone.whoAmI function의 this를 가리킨다.
      }.bind(this)
    );
  },
};

someone.whoAmI();
/*
{ name: '종현', tags: [ '코딩', '디자인', '창업' ], whoAmI: [λ: whoAmI] } '코딩'
{ name: '종현', tags: [ '코딩', '디자인', '창업' ], whoAmI: [λ: whoAmI] } '디자인'
{ name: '종현', tags: [ '코딩', '디자인', '창업' ], whoAmI: [λ: whoAmI] } '창업'
*/
```

bind가 없었다면 console.log()의 인자로 쓰인 this는 regular function이 this가 가리키는 곳은 함수가 실행된 장소인 global이 된다.

## 3. this가 없는 Arrow Function 을 사용하기

Arrow Function(() => {})에는 this, function name, arguments가 없다.

### bind없애고 arrow function으로 바꾸기

```javascript
const someone = {
  name: '종현',
  tags: ['개발', '디자인', '창업'],
  whoAmI: function () {
    this.tags.forEach((tag) => {
      console.log(this, tag);
    });
  },
};

someone.whoAmI();

// 여기서 다시 질문!
const whoAmI = someone.whoAmI;
whoAmI(); // 어떻게 실행될까?
// 답: Cannot read property 'forEach' of undefined
```

#### whoAmI는 어떻게 실행될까?

위에서 설명했던 내용과 같은 내용이다. 함수가 실행되는 곳인 global의 property에 tags가 없기 때문에 배열 객체의 메소드인 forEach를 실행할 수 없다.

## Arrow Function는 this가 없기 때문에 new를 통해 객체를 생성하는 constructor로 사용할 수 없다.

```javascript
const RegularConstructor = function (name, tags) {
  this.name = name;
  this.tags = tags;
};

const ArrowConstructor = (name, tags) => {
  this.name = name;
  this.tags = tags;
};

const regularInstance = new RegularConstructor('종현', ['개발', '디자인']);
// RegularConstructor { name: '종현', tags: [ '개발', '디자인' ] }

const arrowInstance = new ArrowConstructor('종현', ['개발', '디자인']);
// Error: ArrowConstructor is not a constructor
```

Arrow Function에는 this가 없기 때문에 constructor로 사용될 수 없다.
