# DOM (Document Object Model)
## 정의
> * HTML 또는 XML 문서의 계층구조와 정보를 표현하며, 이를 제어할 수 있는 프로퍼티와 메서드를 제공하는 트리 자료구조다. 

## 과정
* 브라우저는 웹 문서(HTML, XML 등)를 로드한 후, 파싱하여 DOM을 생성한다.	
![](https://images.velog.io/images/kimin3004/post/e746ee2a-70a5-45c3-a59e-608aa02242ab/image.png)
* html 문서를 구성하는 각 요소와 요소의 어트리뷰트, 텍스트를 각각의 객체로 만들고 이들 객체를 부자 관계를 표현할 수 있는 트리 구조로 구성한다.
![](https://images.velog.io/images/kimin3004/post/3f0404a4-f300-4137-ac8d-6e75f17646c6/image.png)
* 노드 트리의 각 노드는 html의 요소를 나타낸다.

* DOM은 자바스크립트를 통해 동적으로 변경될 수 있고,  변경된 DOM은 렌더링에 반영된다. (html을 직접 변경X) 



<br>

---
# DOM Manipulation(조작)
## 1-1. 노드 취득 (단일요소)
![](https://images.velog.io/images/kimin3004/post/7f11140f-f090-464a-b160-367810624af2/image.png)
##### image from https://poiemaweb.com/js-dom

> #### 01.  getElementById(id)

* id 값으로 요소 하나 취득
<br>


> #### 02. querySelector(cssSelector)

* cssSelector 값으로 요소 하나 취득
<br>

### 공통점
* 조건에 해당하는 요소가 여러개면 첫번째 요소만 선택한다.
* HTML Element(요소)를 상속받은 객체를 반환한다.

> #### 태그? 요소?
##### 태그(tag) : 단순히 < 와 >로 묶인 명령어들을 의미
##### ex) `<p>`
##### 요소(element) : 태그들을 이용해서 만들어낸 웹페이지의 구성요소, 내용을 포함해 시작태그와 종료태그까지를 엘리먼트
##### ex) `<p>이것은 문단입니다.</p>`


<br>

---
## 1-2. 노드 취득 (여러요소)
![](https://images.velog.io/images/kimin3004/post/40bb713f-d32d-4aa2-a2dc-e63e5bbe8a0d/image.png)
##### image from https://poiemaweb.com/js-dom
> ### HTML Collection

![](https://images.velog.io/images/kimin3004/post/b6d36637-4f43-4791-b639-099481200b59/image.png)
> #### getElementsByClassName(class),
#### getElementsByTagName(tagName) 메서드가 반환하는 객체

* 노드 객체의 상태 변화가 실시간 반영되는 live객체이다.
<br>

예시 )
``` 
<body>
    <div>
      <ul>
        <li id="one" class="red">1</li>
        <li id="two" class="red">2</li>
        <li id="three" class="red">3</li>
      </ul>
    </div>
  </body>
  
  
  <script>
  const elems = document.getElementsByClassName('red');
  for (let i = 0; i < elems.length; i++) {
    // 클래스 어트리뷰트의 값을 변경한다.
    elems[i].className = 'blue';
  }
  </script>
``` 
![](https://images.velog.io/images/kimin3004/post/b0b04a1e-d3af-44c5-96f1-dd1d4e69e255/image.png)
3번의 loop )

* i = 0, elems의 첫 요소(li#one.red)의 class 값이 blue로 변경.
elems는 실시간으로 Node의 상태 변경을 반영.
elems의 첫 요소는 li#one.blue로 변경되었으므로 선택 조건(‘red’)에 해당되지않아 실시간으로 제외된다.

* i= 1, 첫째 요소는 제거되었으므로 elems[1]은 3번째 요소(li#three.red)가 된다. li#three.blue로 변경되고 마찬가지로 HTMLCollection에서 제외된다.

* i= 2, 1,3번째 요소가 제거되었으므로 2번째 요소(li#two.red)만 남았으나, 현재 elems.length는 1이므로 반복을 종료한다. 따라서 elems에 남아 있는 2번째 li 요소(li#two.red)의 class 값은 변경되지 않는다.


<br>
<br>

> ### NodeList

![](https://images.velog.io/images/kimin3004/post/7ca90f5f-202a-4f84-95a1-b0723260ffcc/image.png)
> #### querySelectorAll 메서드가 반환하는 객체

* forEach() 사용가능
* 대부분 노드 객체의 상태 변화가 실시간 반영 되지 않는 Non-live 객체이다.
<br>

### 공통점
* **유사 배열 객체** (배열에서 제공하는 map(), reduce(), filter() 등의 메소드 사용불가) 
* ** 배열로 변환 후 사용하는 것이 효율적 -> (...)스프레드 연산자 사용**

<br>
<br><br>
<br>

---
## 2. 노드 추가
> ### DOM을 변경할 경우

* 리플로우와 리페인트가 실행된다.
* 가급적 횟수를 줄이는 편이 성능에 유리하다. (반복적으로 추가하는 것은 비효율적이므로 지양)

> #### 리플로우? 리페인트?
##### JS 코드에 DOM이나 CSSOM가 API를 통해 변경된 경우, 변경된 DOM,  CSSOM은 다시 렌더 트리로 결합되고 다시 레이아웃과 페인트 과정을 거쳐 브라우저 화면에 렌더링 된다. 

### 성능테스트
```javascript
const start = Date.now();
console.log("Page load took " + (Date.now() - start) + "milliseconds");
```


<br>

> ### Element.innerHTML (프로퍼티)

요소 노드의 시작태그와 종료태그 사이 내 포함된 모든 HTML 마크업을 문자열로 반환한다. 

<br>

#### 장점 
* 쉽게 새로운 요소를 추가할 수 있다.
#### 단점
* 유지하고 싶은 기존 자식노드까지 제거된다. 
* 새로운 요소를 삽입할 때 삽입 위치를 지정할 수 없다. (형제 요소 사이 등)
* innerHTML에 작성한 마크업 문자열은 DOM으로 반영되기 때문에 크로스 사이트 스크립팅 공격에 취약하다.

```javascript
const app = document.querySelector('#app');

for (let i = 0; i < 1000; i++) {
	app.innerHTML += '<div>Hello<div>';
}
```
* 위는 "+="를 이용하여 작성해도, 요소노드의 모든 자식노드가 제거되고 재생성되기 때문에 비효율적이다. 
* 루프를 돌고 난 후 보여지는 div 태그의 갯수는 1,000개이지만 실제로 브라우저가 파싱해서 만들거나 지우게 된 div태그는 500,500개이다.
```javascript
const app = document.querySelector('#app');

let template = ''
for (let i = 0; i < 1000; i++) {
	template += '<div>Hello<div>';
}
app.innerHTML = template;
```
* 이처럼 변수를 할당하여 DOMString을 축적 후 반복문 밖에서 한 번만 사용하는 것이 바람직하다!

<br>
<br>

> ### Element.insertAdjacentHTML(position, DOMString)

* 기존요소를 제거하지 않으면서 위치를 지정해 새로운 요소를 삽입한다. 
* HTML 마크업 문자열(DOMString)을 파싱 후 생성된 노드를 명령 위치(position)에 삽입하여 DOM에 반영한다.

---


<position의 종류>
![](https://images.velog.io/images/kimin3004/post/0f10acab-d9c4-430e-98ad-09c694b16c71/image.png)

 #### 장점 
* 기존요소를 유지하며 위치를 지정해 새로운 요소 삽입이 가능하다. (innerHTML보다 효율적이다.)	
#### 단점
* innerHTML과 마찬가지로 작성한 마크업 문자열은 DOM으로 반영되기 때문에 크로스 사이트 스크립팅 공격에 취약하다.
<br>

```javascript
const app = document.querySelector('#app');

for (let i = 0; i < 1000; i++) {
	app.insertAdjacentHTML('beforeend', '<div>Hello<div>');
}
// Page load took 13ms

```

```javascript
const app = document.querySelector('#app');

let template = '';
for (let i = 0; i <1000; i++) {
	template += '<div>Hello<div>';
}
app.insertAdjacentHTML('beforeend', template);
// Page load took 30ms
```
* innerHTML와 달리 기존 노드를 제거하지 않고 추가한다.
* innerHTML 케이스와 달리 반복문 안과 밖에서 사용 시 성능에 큰 차이는 없다. 
<br>
<br>

> ### Document.createElement(tagName)

* 요소노드를 생성하여 반환한다. (자식노드 X)
* 생성한 요소 노드는 기존 DOM에 추가되지 않는다. 
<br>

> ### Node.appendChild(childNode)

* 매개변수 childNode에게 인수로 전달한 노드를 appendChild()를 호출한 노드의 마지막 자식 노드로 추가한다.
<br><br>

```javascript
const app = document.querySelector('#app');
  
for (let i = 0; i < 1000; i++) {
	const li = document.createElement('li');
  	app.appendChild(li);
}
```
* 위는 1000개의 요소 노드를 생성하여 DOM에 1000번 추가하므로 DOM이 1000번 변경된다.



```javascript
const app = document.querySelector('#app');
// 컨테이너 요소 노드 생성
const container = document.createElement('div');
  
for (let i = 0; i < 1000; i++) {
	const li = document.createElement('li');
  	container.appendChild(li);
}
app.appendChild(container);
```
* 위는 DOM을 한 번만 번경하지만, 불필요한 container 요소(div)가 추가된다.

> ### DocumentFragment

* 노드객체의 일종으로, 부모노드가 없어 기존 DOM과는 별도로 존재한다.
* DocumentFragment 객체에 변경이 일어나더라도 기존 DOM에는 영향이 없다.
* DocumentFragment 객체는 DOM에 추가 될 때 제거되며, 자신의 자식노드만 DOM에 추가된다. 
<br>

```javascript
function addElements() {
	const app = document.querySelector('#app');
  //DocumentFragment 노드 생성
    const docFrag = document.createDocumentFragment();
 
    for (let i = 0; i < 1000; i++) {
		const li = document.createElement('li');
        docFrag.appendChild(li);
    }
    app.appendChild(docFrag);
}
```
* 마지막에 DOM조작이 한번만 일어나며, 불필요한 요소가 남지 않는다. 
* 최신 브라우저의 경우에는 리플로우(reflow)가 발생하지 않도록 엔진을 최적화하기 때문에 DocumentFragment 객체를 사용할 때 발생하는 성능 향상을 체감할 수 없는 경우도 있다.



----
참조 <br>
https://poiemaweb.com/js-dom<br>
https://www.youtube.com/watch?v=q1fQnGG1bgU



