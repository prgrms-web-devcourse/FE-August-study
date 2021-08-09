# 📖 Scope & Closure

# 🚀Scope?

![](https://i.imgur.com/jghpfNR.png)
즉, scpoe란 지역변수와 전역변수 개념이다.

# Scope chain?

scope란 유효범위라고 언급했다. scope chain은
scope가 chain처럼 엮겨있다 해서 생긴 명칭이다.
![](https://i.imgur.com/8OKTcPp.png)

## Quiz

다음의 출력값을 생각해보세요.
![](https://i.imgur.com/SEqURrc.png)

![](https://i.imgur.com/ZXrUL1e.png)

위와 같이 생각을 하셨다면 정확히 알고있는 것이다.
즉, scope chain은 유호범위 즉, 식별자를 찾는 과정이다.

그렇다면 어떻게 식별자를 찾을까요?
간단히 설명하자면 함수가 호출될때마다 실행컨텍스트 생성이 되어 스택처럼 쌓이게 된다. 실행컨텍스트는 물리적으로 생성할 때, 변수 객체, scope chain, this 등의 속성을 가지게 된다.

즉, 실행컨텍스트가 스택에 쌓이고 해당 변수가 사용될때,
스택에 쌓여있는 정보들을 이용하여 제일 가까운 곳에 있는
변수정보를 저장하여 출력하게 된다.

![](https://i.imgur.com/CWO5zbc.png)

위의 그림을 보면 전역변수 x가 있고
outer함수가 생성이 되어 실행하게 된다.
outer에서 break point를 활용해서 함수 정보를 보면
scope항목에서 전역변수 x의 정보를 확인할 수 있게 된다.

![](https://i.imgur.com/lrCfDXH.png)

inner함수도 마찬가지로 출력해서 정보를 보게 되면
상위 함수 outer의 y와 전역변수 x의 정보를 담고 있는 것을 볼수있다.

# 🚀Closure

closure는 함수가 선언된 환경의 scope를 기억하여
함수가 스코프 밖에서 실행될때에도 "기억"한 scpoe에
접근할 수 있게 만드는 문법이다.

## Quiz2

다음의 출력값을 생각해보세요.
![](https://i.imgur.com/Ctndgbc.png)

설명 : outer함수를 시행시키면 return값으로 inner()함수를
반환한다. inner함수는 outer함수안에 있지만 return을 통해
outer함수가 종료되어 실행컨텍스트도 스택에서 없어진다.

![](https://i.imgur.com/2fsddRS.png)

엥? 근데 왜 1이아니라 10이야?

아까 위에서 함수가 종료되면 실행컨텍스트가 반환이 된다고 말했다. 하지만 실행컨텍스트는 렉시컬 환경의 정보를 참조하고 있습니다. 아까 설명한 실행컨텍스트 생성시 각종 정보를 렉시컬 환경에 저장되어있고 실행컨텍스트는 이를 참조하는 구조입니다.

![](https://i.imgur.com/C2DnJbW.png)

즉 outer함수가 종료되면서 inner함수를 return할 때,
outer함수의 실행 컨텍스트가 참조하고 있는 렉시컬 환경 정보를
inner함수가 이어 받아서 해당 정보를 기억하고 있기 때문이다.
그 후, outer함수는 렉시컬 참조를 끊고 실행 컨텍스트를 반납하면서 종료가 된다.

#### Closure 설명시에 함수가 선언된 환경의 scpoe를 기억한다고 했죠? 위의 설명이 이에 해당합니다.

![](https://i.imgur.com/qAbE7DY.png)

위 사진을 보면 outer함수가 종료되었지만 scope를 기억하여
해당 정보가 출력되는 것을 볼 수 있다.
또한 closure가 명시적으로 적혀있는 것도 확인 할 수 있다.
변수 y에 대해서는 inner함수에서 사용하지 않기 때문에
참조할 필요가 없기 때문에 해당 정보가 없다.

## Closure 사용 이유

왜 헷갈리게 closure를 사용할까?

#### 가장 큰 이유는 전역 변수를 줄일 수 있기 때문이다.

예시) 함수가 실행 횟수를 구해라

```
let count = 0;

function handleCilck() 
{
  count++;
  return count;
}
```

위 같은 조건이 있을때 보통 전역변수를 두어 count를 하게 된다.
만약 코드가 길고 기능이 많다면 수 많은 전역 변수가 있어
메모리 측면에서도 비효율적이고 개발자가 실수할 가능성도 높다.

이를 closure를 이용하여 작성하면 다음과 같다.

```
function handleCilck() {
  let count = 0;
  return function () {
    count++;
    return count;
  };
}
```

함수안에 변수가 있기 때문에 count변수가 정확히 무엇을 하는지도 명확히 알 수 있고 다른 함수에서는 접근할 필요가 없기 때문에
메모리 측면에서도 좋고 개발자의 실수도 없어진다.

이외에 사용 이유에는 scope, this등의 상태유지, 정보은닉이 존재한다.
