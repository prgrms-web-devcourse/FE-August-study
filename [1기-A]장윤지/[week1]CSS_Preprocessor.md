# css 전처리기 알아보기

# 전처리기?

Preprocessor

프로그램을 컴파일할 때 컴파일 직전에 실행되는 별도의 프로그램.

CSS의 전처리기는 많은 반복적인 작업으로 길어지는 코드의 유지보수를 용이하게 하는 데 주 목적이 있습니다.

## 1. sass / scss

[Sass: Syntactically Awesome Style Sheets](https://sass-lang.com/)

Syntactically Awesome Style Sheets (문법적으로 짱짱 멋진 스타일시트)의 줄임말.

전처리기 중 가장 먼저 나왔고, Ruby를 기반으로 구동되어 조금 느리다는 단점이 있었습니다.
하지만 npm에 sass가 등록되면서 현재까지도 많이 쓰이고 있습니다.

### sass vs scss

sass가 처음 나왔을 때 주 문법이 css와 많이 달랐다고 합니다. 개발자들이 이러한 새로운 문법에 익숙해지지 않았기에, 버전 3 이상부터 문법을 css와 공유하는 방향으로 업데이트되었습니다. 이 업데이트 버전으로 scss가 새로 생겼으나, 기존의 sass가 사라진 것은 아닙니다.

문법적으로 도드라진 차이는,
'들여쓰기냐, {}(중괄호)와 ;(세미콜론)이냐'라고 할 수 있겠네요.

```sass
.list
  width: 150px
  float: right
  li
    color: blue
    background: url("./img.jpg")
    &:last-child
      margin-right: -20px

=border-radius($radius)
  -webkit-border-radius: $radius
  -moz-border-radius:    $radius
  -ms-border-radius:     $radius
  border-radius:         $radius

.box
  +border-radius(15px)
```

```scss
.list {
  width: 150px;
  float: right;
  li {
    color: blue;
    background: url("./img.jpg");
    &:last-child {
      margin-right: -20px;
    }
  }
}

@mixin border-radius($radius) {
  -webkit-border-radius: $radius;
  -moz-border-radius: $radius;
  -ms-border-radius: $radius;
  border-radius: $radius;
}

.box {
  @include border-radius(15px);
}
```

mixin을 사용하면 그룹단위의 스타일을 변수처럼 적용할 수 있습니다.
scss에서는 @mixin으로 먼저 설정을 해 두고, 사용할 때는 @include를 사용하면 됩니다.
sass에서는 =와 + 기호를 써서 설정합니다.

## 2. Less

[Getting started | Less.js](https://lesscss.org/)

Less는 css에 스크립트의 능력(변수, 함수, 연산 등...)을 덧붙여 확장한 언어입니다.
JavaScript로 만들어져 처음에는 구동이 빨랐기 때문에 sass보다 많이 쓰였지만, sass도 npm으로 확장되면서 별 차이가 없어지게 되었습니다. Twitter에 사용되면서 전파되기 시작했습니다.

다만, 현재 npm 다운로드 수와 github star 기준으로 다른 두 전처리기보다 많은 빈도로 쓰이는 것을 확인할 수 있습니다.

css의 기존 문법을 사용합니다.

```less
@color: #000000;

#header {
  color: @color;
}

h2 {
  color: @color;
}
```

```less
.rounded-corners (@radius: 5px) {
  border-radius: @radius;
  -webkit-border-radius: @radius;
  -moz-border-radius: @radius;
}
#header {
  .rounded-corners;
}
#footer {
  .rounded-corners(10px);
}
```

```less
@the-border: 1px;
@base-color: #111;
@black: #000000;
#header {
  color: @base-color * 3;
  border-left: @the-border;
  border-right: @the-border * 2;
}
#footer {
  color: @base-color + #003300;
  border-color: desaturate(@black, 10%);
}
```

## 3. Stylus

[Expressive, dynamic, robust CSS](https://stylus-lang.com/)

위 두 전처리기에 비해 가장 최근에 나온 전처리기입니다. Node.js 기반으로 구현되어 있습니다.
다른 전처리기보다 많은 기능을 쓸 수 있고, 최근에는 버그도 많이 해결되어 사용자가 늘어나는 추세입니다.

{}(중괄호)와 ;(세미콜론), :(콜론)을 생략할 수 있습니다. 그리고 들여쓰기로 블럭을 판단합니다.

```css
body
   font 14px "Lucida Grande", Helvetica, Arial, sans-serif
   background-color #efeeee

a
  color #00678f
  &:hover
  &:focus
    color #be2a2a

DARKGREY_COLOR = #424242

header
   background-color DARKGREY_COLOR
```

```css
body {
  font: 14px "Lucida Grande", Helvetica, Arial, sans-serif;
  background-color: #efeeee;
}

a {
  color: #00678f;
}
a:hover,
a:focus {
  color: #be2a2a;
}

header {
  background-color: #424242;
}
```

& 문자는 부모의 셀렉터를 참조합니다. 그리고 다른 두 전처리기와 마찬가지로 변수를 선언하여 사용할 수도 있습니다.

또한 함수를 지원하기도 합니다.

```css
vendor(prop, args)
  -webkit-{prop} args
  -moz-{prop} args
  -o-{prop} args
  {prop} args

border-radius()
  vendor('border-radius', arguments)

div
  border-radius 5px
```

```css
div {
  -webkit-border-radius: 5px;
  -moz-border-radius: 5px;
  -o-border-radius: 5px;
  border-radius: 5px;
}
```

## 4. PostCSS

[PostCSS](https://postcss.org/)

~~CSS의 전처리기이는 하지만~~, CSS의 후처리기입니다. 자체적으로 다양한 기능을 제공하는 위 세 전처리기와는 달리 PostCSS는 그 자체로는 기능이 없습니다. 정확히 말하자면, PostCSS가 제공하는 '플러그인'이 기능합니다.

JavaScript로 만들어졌으며, 위의 세 전처리기에 비해 유명세는 좀 덜한 편입니다. 다만 사용자들의 만족도가 제일 높은 편이라고 하네요.

몇 가지 사용하기 편한 플러그인을 소개해 보겠습니다.

### autoprefixer

-webkit-등의 접두어를 따로 입력할 필요가 없이 자동 지정해줍니다.

```css
a {
  display: flex;
}
```

```css
a {
  display: -webkit-box;
  display: -webkit-flex;
  display: -ms-flexbox;
  display: flex;
}
```

### postcss-custom-properties

변수를 사용할 수 있게 해줍니다.

```css
:root {
  --color: red;
}

div {
  color: var(--color);
}
```

```css
div {
  color: red;
}
```

### postcss-import

css의 @import 문법을 사용합니다.

```css
/* foo.css */
.foo {
  width: 100px;
}

/* bar.css */
.bar {
  height: 20px;
}

/* index.css */
@import "foo.css";
@import "bar.css";
```

```css
.foo {
  width: 100px;
}
.bar {
  height: 20px;
}
```

### purgeCSS

html과 CSS를 비교하면서, CSS 스타일에는 정의되어 있지만 html에는 사용되지 않은 스타일을 지워버립니다. CSS가 더욱 최적화되겠죠?

## 정리

### CSS 전처리기의 장점

- 재사용이 용이하다.
- 유지관리/보수로 인한 시간적 비용이 절약된다.
- 러닝커브가 낮은 편이다.

### CSS 전처리기의 단점

- 전처리기로 작성한 문서를 CSS로 컴파일해주는 도구가 필요함
- 퍼블리셔나 디자이너가 배우기에는 어려울 수 있음 (개발적인 요소가 있으므로)
- 결과물을 확인하려면 localhost등의 서버가 필요함

### 그래서 어떤 거 쓰면 돼요?

네 개 중에 마음에 드는 거 쓰세요! 차이는 별로 없습니다!

- Github Star

![[week1]CSS_Preprocessor/untitled.png]([week1]CSS_Preprocessor/untitled.png)

![[week1]CSS_Preprocessor/untitled1.png]([week1]CSS_Preprocessor/untitled1.png)

![[week1]CSS_Preprocessor/untitled2.png]([week1]CSS_Preprocessor/untitled2.png)

- The State of CSS Survey 2020

![[week1]CSS_Preprocessor/untitled3.png]([week1]CSS_Preprocessor/untitled3.png)

[The State of CSS 2020: Pre-/Post-processors](https://2020.stateofcss.com/en-US/technologies/pre-post-processors/#tools-section-overview)

## 참고 자료

- CSS 전처리기 성능 비교 사이트

[Compile](https://csspre.com/compile/)

- CSS 전처리기를 넘어서, 스타일 자체를 코드로 관리하는 css in js도 하나의 트렌드라고 합니다.

[CSS-in-JS 가 좋은 이유](https://velog.io/@dev-mish-mash/CSS-in-JS-%EA%B0%80-%EC%A2%8B%EC%9D%80-%EC%9D%B4%EC%9C%A0)
