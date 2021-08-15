프론트엔드는 자바스크립트를 주로 사용하기에 자바스크립트의 동작원리에 대해 이해하고 있어야한다고 생각 했습니다. 강의에서도 배우기도 했지만, 이해가 어려웠기에 제대로 정리하고 넘어가고싶어서 주제로 선정했다.
## 자바스크립트 엔진의 구조
- 자바스크립트 엔진은 크게 Call Stack과 Heap으로 나뉘며, 엔진 외부의 다른 요소들과 상호작용하여 자바스크립트 코드를 실행시킨다.
![자바스크립트 엔진 구조](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/7de94fed-dcc0-42a9-926b-284487e72cb4/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20210815%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20210815T051440Z&X-Amz-Expires=86400&X-Amz-Signature=64c981b98a44e3f7a6bd8eb6193f89f2198739150c642fb930024ac923dc2c54&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)
### Memory Heap 
- 메모리 할당이 일어나는 곳 구조화되지 않은 넓은 메모리 영역으로 객체, 변수, 함수들이 담긴다.
- 가비지 컬렉션을 통해 메모리 정리가 이루지는 영역
### Call Stack
- Call Stack은 코드가 실행되면서 스택 프레임이 쌓이는 장소 영역
- 코드내에 동기적인 코드만 있다면 다른거 필요없이 콜스택 만으로도 동작이 된다.
```javascript
function multiply(x, y) {
    return x * y;
}
function printSquare(x) {
    var s = multiply(x, x);
    console.log(s);
}
printSquare(5);
```
위 예제 코드가 call stack에서 어떤 단계로 실행되는지 아래 그림으로 살펴보자.
![call stack 실행 단계](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/086dda1b-006a-4dd3-a5ea-1a10e6d413f9/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20210815%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20210815T051844Z&X-Amz-Expires=86400&X-Amz-Signature=b803a9e4365dcd8455e7e5ed27aa9d3a87a2e21a12789885fc80ebe9cfd29289&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)
명령문이 하나씩 push되어 쌓이고 할일을 마친 함수는 stack에서 pop되며 진행된는걸 볼 수 있다. 이처럼 자바스크립트는 단일스레드로 하나의 콜스택에서 하나의 명령문만 처리 할 수 있다. 그렇다면 비동기 요청은 어떻게 이루어지는걸까?
## 브라우저 환경에서 자바스크립트 동작
실제 자바스크립트가 구동되는 환경(브라우저,node.js)에서는 여러 개의 스레드가 사용되기 때문이다. 그리고 이러한 구동환경이 자바스크립트 엔진과 상호 연동하기 위해 사용하는 장치가 바로 이벤트 루프이다.
![브라우저 환경](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/ec3974b8-3d5e-48d7-85d9-2680994af607/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20210815%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20210815T052821Z&X-Amz-Expires=86400&X-Amz-Signature=00d5559be150423dd4b08a6b78192450c30abee4b272983d6b20a757de5d49e8&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)
위 그림은 브라우저환경을 그림으로 표현한 것이다. 저 환경을 구성하고 있는 Web API, Task Queue, Event Loop에 대해서 간략하게 알아보자.
### Web API
- 웹 브라우저에 내장된 API로 브라우저 및 주변 컴퓨터 환경에서 데이터를 제공하거나 복잡한 작업을 수행 할 수 있다.
    - 웹 콘텐츠 조작을 위한 DOM
    - 서버에서 데이터를 가져오기 위한 XMLHttpRequest 및 Fetch
    - 그래픽 그리기 및 조작을 위한 Canvas 및 WebGL
    - 오디오 및 비디오 조작을 위한 Web Audio 및 WebRTC
    - 웹 브라우저에 데이터를 저장하기 위한 Web Storage
    - 이외에도 다양한 API가 존재한다.
### Task Queue
- 비동기 처리가 끝난 후 실행되어야 할 콜백 함수들이 차례로 할당 되어 대기하는 큐
- Task Queue의 종류로는 (macro)task queue, microtask queue, animation frames가 있다.
- **(Macro)Task Queue**: 이 큐는 setTimeout(), setInterval(), setImmediate()와 같은 task를 넘겨받는다.
- **Microtask Queue**: Promise나 async/await, process.nextTick 과 같은 비동기 호출을 넘겨받는다.
    - 그리고 Microtask의 우선순위는 일반 task(또는 macrotask)보다 더 높다.
- **Animation Frames**: requestAnimationFrame과 같이 브라우저 렌더링과 관련된 task를 넘겨받는 Queue이다.
    - 우선순위는 Microtask보다는 낮고, (Macro)Task보다는 높다.
- Task 우선순위 **microtask queue > animation frames > task queue**
### Event Loop
- Call Stack을 체크하며 비어 있다면 콜백 큐에 할당된 함수를 순서에 맞춰 Call Stack에 할당해 준다.
- 자바스크립트 엔진에서 제공하는 것이 아니고 브라우저나 node.js에서 지원 한다.
## 동작원리
아래 그림을 보며 위에서 살펴본 구성원들이 각각 어떻게 동작하는지 간략하게 살펴보자.
![](https://images.velog.io/images/goum/post/6bae2bba-0401-4f17-a5b9-f84616f79b96/img.jpg)
1. Call Stack에 Web API 실행 명령문 예를들어 promise, setTimeout, requestAnimationFrame가 들어오면 해단 함수는 web API로 전달되 비동기 처리를 대기한다.
2. 처리가 끝난 함수들을 콜백함수를 각 API와 연관된 Queue로 넘어가 대기한다.
3. 이벤트루프는 Call Stack을 체크하며 Stack이 비어있다면 queue에 있는 함수들을 우선순위에 맞춰 차례로 Stack에 전달한다. *이때 주의해야 할 것이 stack이 비어 있을때 이벤트 루프는 task를 전달한다는 것이다.
그렇다면 아래 예제 코드는 어떻게 순으로 콘솔값을 보여주게 될까?
```javascript
console.log("시작");

setTimeout(function () {
	console.log("중간");
}, 0);

Promise.resolve()
	.then(function () {
		console.log("프로미스");
	}
);

console.log("끝");
```
답은 시작, 끝, 프로미스, 중간 순으로 실행된다.
자세한 과정은 아래 링크에 들어가서 슬라이드를 하나씩 넘겨보면 알 수 있다.
[예제코드 실행 슬라이드](https://docs.google.com/presentation/d/176-3l1qwCW6Czt1Eidkog6sXnXrCklMyxCsl_mQ05mI/edit#slide=id.gb86527c4ba_0_5)
## 마치며
이번 스터디는 깊게 알기보다는 이해하지 못했던 동작원리를 이해하는 것에 중점을 맞췄다. 이렇게 자료를 정리하여 남에게 전달하는 능력이 부족하다 보니 디테일한 부분을 많이 놓친거 같아 아쉬운점이 많았다.
### 참고
[자바스크립트와 이벤트루프 - toast 블로그](https://meetup.toast.com/posts/89)
[JavaScript Visualized: Promises & Async/Await](https://dev.to/lydiahallie/javascript-visualized-promises-async-await-5gke#syntax)
[Tasks, microtasks, queues and schedules](https://jakearchibald.com/2015/tasks-microtasks-queues-and-schedules/)
[이벤트루프시리즈 - 우리밋](https://www.youtube.com/watch?v=QFHyPInNhbo)
[자바스크립트의 동작원리: 엔진, 런타임, 호출 스택 - CAPTINPANGYO](https://joshua1988.github.io/web-development/translation/javascript/how-js-works-inside-engine/)