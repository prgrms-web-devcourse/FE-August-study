# TDD
### 테스트하는 코드?
### 테스트가 개발을 이끌어간다고?

> 오늘 발표에서는
테스트라는게 뭔지, TDD라는게 뭔지에 대한 개념을 간략히 정리해보고, 실제로 TDD 방식으로 간단한 코드를 작성해보면서
"TDD라는 개념을 익히는 것"을 목표로 합니다.

<br>

## 🌀 테스트를 위한 코드?

테스트 코드란, 우리가 작성한 코드를 검증하는 코드입니다.

우리가 작성한 코드가 다양한 경우의 수에 대해 우리가 예상하는 대로 작동하는지에 대해 검증하는 코드를 테스트 코드라고 말합니다.

하나의 어플리케이션 코드 베이스는 수십, 수백 개의 파일 혹은 모듈이 다양한 방식으로 엮여 있습니다. 이 중 일부를 수정했을 때 과연 기존의 기능이 여전히 잘 작동되는지 확인하는 것은 생각보다 굉장히 어려운 작업입니다. 테스트 코드를 작성해 놓을 경우, 이런 작업을 조금 더 수월하게 진행할 수 있게 됩니다.

<br>

## 🌀 테스트 종류

**Different purpose, different tests**

### **1. Unit Test**

단위 테스트라고 불리는 테스트입니다. 단위라는 의미는 문맥에 따라 달라질 수 있습니다. 프론트엔드에서는 컴포넌트 하나를 하나의 단위로 설정하고 테스트를 작성할 수도 있고, 백엔드에서는 Endpoint 하나를 단위로 설정하고 테스트를 작성할 수도 있습니다. 그 외에도 함수 하나를 하나의 단위로 설정할 수도 있습니다.

### **2. Integration Test**

통합 테스트라고 불리는 테스트입니다. 각각의 단위들이 모여 우리 어플리케이션의 기능을 만들어주게 되는데, 그 단위들이 모여 성공적으로 기능이 작동되는지에 대한 검증을 하는 테스트입니다. 그 말은 즉, 단위들이 모여 성공적으로 기능이 작동하지 않을 수도 있다는 의미입니다.


![GIF](https://c.tenor.com/v37fPItR6u8AAAAd/integration-test-sink-water.gif)
위의 gif를 보시면, 자동 감지 수도꼭지는 매우 잘 작동하고 있습니다. 그리고 세면대 또한 별 이상이 없습니다. 이것은 각각의 단위는 정상적으로 작동함을 의미합니다.

하지만 두 가지 단위가 만나서 통합되었을때, 우리가 원하는 방향으로 작동하지 않고 있음을 확인할 수 있습니다.

<br>

## 🌀 TDD란?

TDD란 Test Driven Development의 약자로 **'테스트 주도 개발'**입니다.

테스트가 개발을 이끌어 나가는 형태의 개발론입니다.

반복 테스트를 이용한 소프트웨어 방법론으로, 작은 단위의 테스트 케이스를 작성하고 이를 통과하는 코드를 추가하는 단계를 반복하여 구현한다.

<br>

## 🌀 TDD 개발주기

![TDD개발주기](https://blog.kakaocdn.net/dn/mG0Pb/btqBZMj04hL/iFrPHyeudxXYfxkWANylY0/img.png)

TDD란, Red - Green - Refactor의 사이클로 반복하며 개발하는을 뜻합니다.

- **Red**: 테스트를 먼저 작성 후, 테스트가 실패하는 것을 확인합니다. 처음부터 모든 테스트 케이스를 작성하는 것이 아니라, 가장 먼저 구현할 기능을 하나씩 작성합니다.
- **Green**: 테스트를 성공시키기 위한 실제 코드를 작성합니다.
- **Refactor**: 작성한 코드를 개선시키는 방향으로 코드를 수정합니다. 수정 후, 테스트가 실패할 경우 다시 Red - Green - Refactor를 반복합니다.

중요한 것은 실패하는 테스트 코드를 작성할 때까지 실제 코드를 먼저 작성하지 않는 것과, 실패하는 테스트를 통과할 정도의 최소한의 코드를 작성해야 하는 것입니다.
실제 코드에 대해 기대되는 바를 보다 명확하게 정의함으로써 불필요한 설계를 피할 수 있고, 정확한 요구 사항에 집중할 수 있습니다.

<br>

## 🌀 TDD를 같이 연습해봅시다!

초보자가 가장 먼저 시도해 볼 수 있는 테스트는 Utility 함수 단위 테스트 입니다. Utility 함수란, Underscore/Lodash 의 함수들처럼 일반적으로 다양한 상황에 사용될 수 있는 단순한 함수들을 일컫습니다. 

### 배열이 주어졌을 때 최빈값을 구하는 함수 구현하기

- RED: 배열의 형태에 따른 최빈값을 결정합니다.
    1. 주어진 값들 중에서 가장 자주 나타난 값이 결과가 됩니다.
        - `[1,2,2,2,3]` → `2`
    2. 모든 값들의 빈도가 똑같다면 최빈값은 없습니다.
        - `[1,2,3]`, `[1,1,2,2,3,3]` → `null`
    3. 빈도가 똑같은 값이 여러개라면, 결과값도 여러개입니다.
        - `[1,2,2,3,3,4]` → `[2,3]`

- RED: 테스트 코드를 작성합니다.

    ```jsx
    // stats.test.js
    describe('mode', () => {
      it('has one mode', () => {
        expect(stats.mode([1, 2, 2, 2, 3])).toBe(2);
      });
      it('has no mode', () => {
        expect(stats.mode([1, 2, 3])).toBe(null);
      });
      it('has multiple mode', () => {
        expect(stats.mode([1, 2, 2, 3, 3, 4])).toEqual([2, 3]);
      });
    });
    ```

    ✅ [Jasmine](https://velog.io/@pjeeyoung/%ED%85%8C%EC%8A%A4%ED%8A%B8-%ED%94%84%EB%A0%88%EC%9E%84%EC%9B%8C%ED%81%AC-jasmine) : 자바스크립트를 위한 오픈소스 테스트 프레임워크

    `stats.mode` 함수가 존재하지 않으니 `TypeError: stats.mode is not a function` 이라는 오류가 발생할 것입니다.

- GREEN: 첫번째, 두번째, 세번째 테스트 케이스를 통과하는 함수를 작성합니다.



- YELLOW: 리팩토링을 합니다.
- RED: 리팩토링 도중 발생하는 예외사항(버그, 수정사항)들을 테스트 케이스에 추가합니다.
- GREEN: 테스트 케이스를 통과하도록 코드를 수정합니다.

...(반복)

<br>

## 🌀 일반개발 방식과의 차이이자 TDD의 장점

- 반복적인 자동화된 검증 과정을 통해 자연스럽게 버그가 줄어들고, 소스코드는 간결해집니다. 또한 개발자에게 자신의 코드를 신뢰하게 해줍니다.
→ ***"Clean Code that works"***
- 테스트 케이스 작성으로 인해 자연스럽게 설계가 개선됨으로서 재설계 시간이 절감됩니다.
- 요구사항을 제대로 만족시키는지를 시각적으로 확인할 수 있고, 개발을 하게 될 때 각 테스트 케이스에 맞춰서 할 수 있기 때문에 해결하고자 하는 문제에 집중할 수 있게 됩니다.

<br>


## 🌀 그럼에도 TDD를 하기 어려운 이유는?

- 생산성의 저하

    처음부터 2개의 코드를 짜야하고, 중간중간 테스트를 하면서 고쳐나가야하기 때문에 개발 속도가 느려진다고 생각하는 사람이 많습니다. 실제로 TDD 방식의 개발 시간은 일반적인 개발 방식에 비해 대략 10-30% 늘어나고, 결함율(버그)는 약 40-90% 줄어든다고 합니다.
    
<br>

## 🌀 테스트를 작성할 때 중요한 개념 - **Mocking**

[What is Mocking?](https://stackoverflow.com/questions/2665812/what-is-mocking)

우리가 특정 단위의 코드를 테스트한다고 하면, 그 단위는 여러 가지 Dependency를 갖고 있을 가능성이 있습니다. 그럴 경우, 우리가 테스트하고자 하는 단위 이외의 Dependency들은 그와 유사한 가짜 정보를 생성하여 대체하곤 합니다. 이것을 Mocking이라고 합니다.

![mocking image](https://blog.kakaocdn.net/dn/0jefm/btqwO7gyMd4/mkwx6tsab9MzVKfi5B9tVK/img.png)

예를 들어, A라는 단위를 테스트한다고 생각해보세요. 그리고 A라는 단위는 B라는 모듈에 대해 Dependency를 갖고 있습니다. 실제 B 모듈을 이용하여 A 모듈의 단위 테스트가 실행될 경우, A 모듈 자체적인 이유로도 테스트가 실패할 수 있지만, 정작 A 모듈은 정상적으로 작동하는데 B 모듈의 문제로 인해 A 모듈의 테스트가 실패할 수도 있습니다. 그런 경우가 생긴다면 우리는 A 모듈 단위 테스트의 목적을 벗어나게 됩니다.

그렇기에 우리가 테스트하고자 하는 단위를 고립시키고 정말 순수하게 해당 단위만을 테스트하기 위해, 관련 Dependency들은 가짜로 대체하는 행위를 Mocking이라고 합니다.


<br>

## 🌀 기타

> TDD의 자세한 기대효과

[On the Effectiveness of the Test-First Approach to Programming](https://www.researchgate.net/publication/3188484_On_the_effectiveness_of_the_test-first_approach_to_programming)

> Test Library Documents

- [Jest](https://jestjs.io)
- [Mocha](https://mochajs.org/)
- [Jasmine](https://jasmine.github.io/)
- [Enzyme](https://enzymejs.github.io/enzyme/)

> reference

[https://velog.io/@velopert/TDD의-소개](https://velog.io/@velopert/TDD%EC%9D%98-%EC%86%8C%EA%B0%9C)

[https://ahea.wordpress.com/2018/09/10/선택이-아닌-필수-tdd/](https://ahea.wordpress.com/2018/09/10/%EC%84%A0%ED%83%9D%EC%9D%B4-%EC%95%84%EB%8B%8C-%ED%95%84%EC%88%98-tdd/)

[https://hyunalee.tistory.com/33](https://hyunalee.tistory.com/33)
