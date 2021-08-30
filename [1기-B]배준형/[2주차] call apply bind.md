## 📌 call, apply, bind

✅ JavaScript에서 this 키워드는 그것이 속한 객체를 참조한다. 

```javascript
const JH = {
  name: "준형"
};

console.log(this.name); // ""
console.log(this); // window Object
```

이 코드에서 나타난 this는 window 객체를 가리킨다. 그래서 JH를 선언하고 key = value 형태로 name에 "준형"이라는 문자열을 넣었지만 this.name에서는 빈 문자열만 나타나게 된다.

※ 여기서 this는 함수 등에서 호출할 때 마다 this가 참조하는 객체가 달라지게 되는데, 이를 **Call, Apply, Bind 메소드를 이용하여 this가 참조하는 객체를 변경할 수 있다.**

<br>
<hr>

## 📌 prototype.call()

✅ **call 메소드의 매개 변수** : call(this로 사용할 값, 호출할 함수의 매개변수들)

🔸 call()은 이미 할당되어있는 다른 객체의 함수나 메소드를 호출하는 해당 객체에 재할당할때 사용된다.

```javascript
const name = {
  fullName: function() {
    return this.lastName + "" + this.firstName;
  }
}
const family1 = {
  firstName:"준형",
  lastName: "배"
}
const family2 = {
  firstName:"콩이",
  lastName: "달"
}
console.log(name.fullName.call(family1)); // 배준형
console.log(name.fullName.call(family2)); // 달콩이
```

성, 이름을 this로 반환하는 name.fullName 함수가 있다고 가정했을 때, call() 메소드를 통해서 참조 객체를 변경하여 원하는 결과를 얻을 수 있다.

🔸 call은 첫 번째 매개변수만 this로 사용할 값을 넣으면 되고, 두 번째 매개변수부터 함수의 매개변수로 사용할 수 있다.

```javascript
const max1 = Math.max.call("", 1, 5, 2, 4, 3);

const max2 = Math.max.call(null, 1, 5, 2, 4, 3);

const min1 = Math.min.call(undefined, 1, 5, 2, 4, 3);

console.log(max1); // 5
console.log(max2); // 5
console.log(min1); // 1
```

Math.max, Math.min 함수는 숫자를 매개변수로 받아 전달받은 변수 중 가장 큰 값, 가장 작은 값을 반환하는데, call 메소드를 통해서도 사용할 수 있다.

여기서 call의 첫 번째 매개변수는 this로 사용할 값이므로 Math.max(min)에서는 this가 사용되지 않기 때문에 빈 배열, null, undefined 등 아무 값이나 넣어도 작동하게 된다.

<br>
<hr>

## 📌 prototype.apply()

✅ **apply 메소드의 매개 변수** : apply(this로 사용할 값, 호출할 함수의 매개변수가 담긴 배열)

🔸 apply 메소드는 call 메소드와 거의 동일하다. apply 메소드도 this로 사용할 값을 첫 번째 매개변수로 받지만, 두 번째 매개변수는 배열로 전달하여 사용해야 한다.

```javascript
const numbers = [1, 5, 2, 4, 3];

const max = Math.max.apply(null, numbers);
const min = Math.min.apply(null, numbers);

console.log(max); // 5
console.log(min); // 1
```

call과 마찬가지로 첫 번째 인자로 아무 값이나 넣어도 동작하게 되며, 동일한 코드를 call로 사용한다면 전개 연산자를 활용하면 같은 동작을 하게된다.

```javascript
const numbers = [1, 5, 2, 4, 3];

const max1 = Math.max.apply(null, numbers);
const max2 = Math.max.call(null, ...numbers);

console.log(max1 === max2); // true
```

만약, 호출할 함수의 매개변수가 필요 없다면 call, apply 어느 것을 사용해도 동일한 결과를 얻을 수 있다.

```javascript
const name = {
  fullName: function() {
    return this.lastName + "" + this.firstName;
  }
}
const family1 = {
  firstName:"준형",
  lastName: "배"
}
console.log(name.fullName.call(family1)); // 배준형
console.log(name.fullName.apply(family1)); // 배준형
```

<br>
<hr>

## 📌 prototype.bind()

✅ **bind 메소드의 매개 변수** : bind(this로 사용할 값, 호출할 함수의 매개변수들)

🔸 bind 메소드는 call과 동일하게 매개변수를 받는다. call, apply와 차이점은 bind 메소드를 통해 새로 바인딩한 함수를 만든다.(this가 참조할 객체를 영구히 바꾼다.)

```javascript
const JH = {
  name: "준형"
}

function exercise(firstWorkout, lastWorkout) {
  this.firstWorkout = firstWorkout;
  this.lastWorkout = lastWorkout;
}

const monday = exercise.bind(JH);

monday("chest", "shoulder");

console.log(JH); // {name: "준형", firstWorkout: "chest", lastWorkout: "shoulder"}
```

- 여기서 monday는 bind 메소드를 통해 새로 바인딩한 함수로 monday 내의 매개변수는 영구히 JH를 가리키게 된다. 
- monday로 바인딩 할 때 exercise는 바뀌지 않는다.

<br>
<hr>

## 📌 참고한 사이트

https://developer.mozilla.org/
https://www.w3schools.com/
