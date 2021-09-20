# This

call,apply,bind를 알아보기전에 우선 This에 대해 알아야합니다.

```javascript
const someone = {
  whoAmI: function () {
    console.log(this);
  },
  arrow: () => {
    console.log(this);
  },
};

someone.whoAmI(); // someone Object
someone.arrow(); //  window
```

this는 어떻게 호출되는가에 따라서 달라지는 자바스크립트의 중요한 개념중 하나입니다. someone이라는 객체가 있고 그 안에는 name과 this를 출력하는 메서드가 두개 들어있습니다. 하나는 일반적인 함수 표현식이고 나머지 하나는 화살표함수를 이용해서 출력한것입니다.

결과값을 보면 whoAmI의 경우 호출한 주체는 someone입니다. 그렇기 때문에 whoAmI에서의 this는 someone이 됩니다.
arrow 메서드의 경우 조금 다르게 동작합니다. 왜 저런 결과값이 나왔는지 살펴보겠습니다. 일반적인 함수표현식의 경우 고유의 this를 가집니다. 하지만 화살표 함수의 경우 고유의 this를 가지지 않고 상위 컨텍스트의 this를 물려받습니다. 그렇기 때문에 arrow메서드의 경우 someone의 this를 물려받게됩니다.
이떄 someone을 부른것은 코드를 실행하고 있는 window 즉 전역객체입니다. 그렇기 때문에 이러한 결과값이 나오게 됩니다.

이렇게 호출하는 방법에 따라 변하는 this를 인위적으로 고정시키거나 지정을 할수 있게 하는 것이 call,apply,bind입니다.

# call

```javascript
function callName(name,age){
  console.log(`${this}가 호출 했고 이름은 ${name}, 나이는${age}`);
}
callName.("leo",26); // window가 호출 했고 이름은 leo, 나이는 26
const thisName = "알렉스"
callName.call(thisName,"leo",26); //  알렉스가 호출 했고 이름은 leo, 나이는 26
```

일반적으로 this는 호출방식에 따라 정해져있지만 call메서드를 이용하게되면 호출방식에 상관없이 this를 인위적으로 지정할수 있다.
call의 첫번째 인자로는 this로 지정할 값 과 그 다음으로는 함수에 인자로 전달될 값들을 넣어주게된다.

# apply

```javascript
callName.apply(thisName, ["tom", 27]); // 알렉스가 호출 했고 이름은 tom, 나이는 27
```

apply는 call과 같은 동작을 하지만 가장 큰 차이점은 this로 지정될 값이 처음으로 오고 두번째로 오는 값은 배열이다. 이 배열의 요소들을 하나씩 꺼내어 함수의 인자로 전달하는 역할을 한다.

# bind

```javascript
callName.bind(thisName, "cook", 25); // 아무일도 일어나지 않음
const newBind = callName.bind(thisName, "cook", 25);
newBind(); // '알렉스가 호출 했고 이름은 cook, 나이는25'
```

bind의 경우에는 조금 다른 방식으로 동작한다. call과 apply의 경우 바로 함수를 실행시키는 반면 bind의 경우 this의 값과 인자의 값을 하나로 묶어서 바로 실행하지 않고 새로운 함수를 반환한다.

# 참고자료

https://ibrahimovic.tistory.com/29 // call,apply,bind 관련 블로그

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/call // mdn

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/bind // mdn

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/apply // mdn
