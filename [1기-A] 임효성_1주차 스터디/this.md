# ❓ this.. 어디서 많이 봤는데..

*그냥 object일뿐! 근데 실행 콘텍스트를 곁들인..*

- HTML 요소에 이벤트 추가할 때
- bind 함수를 사용할 때
- 화살표 함수를 사용할 때

# ⚡ 왜 문제일까?

다음의 사례를 살펴보자!

- 새로운 객체를 만들고 그 객체에 this를 바인딩하는 **new 바인딩.**

```jsx
<script>
    function hello(a) {
        this.a = a;
    }
    var bye = new hello(2);
    console.log(bye.a)
</script>
```

- 가장 평범한 함수 호출인 '단독 함수 실행'에 관한 규칙.
- 나머지 규칙에 해당하지 않을 경우 적용되는 **기본 바인딩.**
- 전역 객체를 참조함. 단, 엄격 모드에선 전역 객체가 기본 바인딩 대상에서 제외됨(이때 this는 undefined).

```jsx
<script>
    function hello() {
        console.log(this);
    }
</script>
```

```jsx
<script>
    "use strict";
    function hello() {
        console.log(this);
    }  
    hello();   
</script>
```

- 호출부에 콘텍스트 객체가 있는지 → 누가 실행했는지.
- **암시적 바인딩.**

```jsx
<body>
		<h1 class="myTitle">JS에서 This가 뭐지?</h1>
    <script>
        const myTitle = document.querySelector(".myTitle");

        function hello() {
            console.log(this);
        }

        myTitle.addEventListener("click", hello)
    </script>
  </body>
```

### this 바인딩이 소실되는 경우가 있다?

함수의 참조 값을 전달하는 과정에서 **암시적 소설**이 일어난다. 진짜 참조 값만 전달한다고 한다..

```jsx
<script>
    function hello() {
        console.log(this.a);
    }

    const obj = {
        a : 2,
        hello : hello
    }

    const bye = obj.hello;
		
    var a = 1000;

    bye();
</script> //1000
```

# ✅ 강제하면 되지 않을까?

- 가능하다! 어떤 객체를 this 바인딩에 이용하겠다는 의지를 코드에 명확히 밝힐 방법! 이를 **명시적 바인딩**이라 한다.
- call과 apply는 함수를 호출함. bind는 새로운 함수를 반환한다.
    - call()
    - apply()
    - bind()

apply()와 거의 동일하지만, call()은 인수 목록을, 반면에 apply()는 인수 배열 하나를 받는다는 점이 중요한 차이점입니다.

```jsx
<body>
	<h1 class="myTitle">JS에서 This가 뭐지?</h1>
    <script>
        const myTitle = document.querySelector(".myTitle");

        function hello() {
            console.log(this)
        }

        myTitle.addEventListener("click", hello.bind({
            name : 'hyosung',
            age : 24
        }));
    </script>
  </body>
```

# 👮 화살표 함수의 두둥장

- this의 자유로움에 대항(?)하기 위해 등장.
- 상위 실행 콘텍스트를 기억하고 싶을 때 사용 → 실행 콘텍스트를 무시함.

```jsx
<body>
    <h1 class="myTitle">JS에서 This가 뭐지?</h1>
    <script>
        const myTitle = document.querySelector(".myTitle");

        function hello() {
            console.log(this)
        }

        myTitle.addEventListener("click", () => {
            console.log(this);
        });
    </script>
  </body>
```

# 🌟 this 꼭 써야해?

- 의견이 분분하다!
- 찬성파 : JS에서 객체지향 프로그래밍(ex 자바에선 객체 자신을 가리킴)을 적용하고 싶다면 OK.
- 반대파 : this는 너무 자유로워 예측이 어렵다. 모두가 잘 알고 써야 함(risky).

# ⌛ 그래서 this가 뭔데?

- this란 작성 시점이 아닌 런타임 시점(실행 시점)에 바인딩 되며 함수 호출 당시 상황에 따라 콘텍스트(흐름)가 결정되는 객체이다.
- 어떤 함수를 호출하면 실행 콘텍스트가 만들어진다. 여기에 콜스텍과 호출 방법, 전달된 인자 등의 정보가 담겨있다. this 레퍼런스 그중 하나로, 함수가 실행되는 동안 이용할 수 있다.