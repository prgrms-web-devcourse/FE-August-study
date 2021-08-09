# 📌 스택(Stack)

   선형 자료 구조 중 하나로 대표적인 예로 배열(Array)가 있다.
   
   상자에 짐을 하나씩 쌓듯이 요소를 쌓고 가장 위의 요소만 컨트롤 하는 **자료구조**를 스택(stack)이라고 한다.
   
>    **Last in First out (후입 선출)** 

<center><img src = "https://images.velog.io/images/chloe41297/post/962942ba-e28e-4f03-9d6f-0144c588a679/image.png" width="40%"></center>

스택은 **배열(Array)** 과 **연결 리스트(linked list)**로 구현 할 수 있다.

+ 중간요소를 추가하거나 삭제가 불가능 하다는 점이 배열과 비슷하여 배열로 구현하는 것이 효율적이다.
+ 연결 리스트의 경우 head가 top이 된다.

## 응용
실제로 가장 와닿는 스택의 사용 예는 다음과 같다.

+ redo/undo 등의 작업
+ reverse (문자열 뒤집기)
+ 함수호출(call stack) 
+ Back tracking



**요소 편집 방식**
> 
push( ) : 맨 위에 요소 추가
pop( ) : 맨 위 요소 삭제

스택은 통에 데이터를 하나씩 쌓는 구조라 결국에는 크기에 한계가 있다. 

참고로, 요소가 스택의 Maximum값을 넘으면 **"stack overflow"**라 한다고 한다.

## 예시 
 💻 문자열 뒤집기
> Js 에서 문자열을 뒤집는 가장 쉬운 방식은 함수형 프로그래밍으로 다음과 같다.

```
const char = "abcdef";
console.log(char. split("").reverse().join("")); //"fedcba"
```
> Stack으로 구현해 보면 조금 길긴하다.

```
const char = "abcdef";
let charArr = [];
for(const i of char){
	charArr.push(i);
    }
console.log(charArr);
let result = "";
for(const i of char){
	result += charArr.pop();
    }
    console.log(result); // "fedcba"
```

### 참조

+ 참고한  [article](https://levelup.gitconnected.com/the-stack-data-structure-what-is-it-and-how-is-it-used-in-javascript-23562fb8a590)
+ 스택으로 풀 수 있는 코테 문제 [프로그래머스 문제](https://programmers.co.kr/learn/courses/30/lessons/12909)

# 📌 큐(Queue)
큐(Queue)는 다음 그림과 같이 생겼다.
<center><img src="https://images.velog.io/images/chloe41297/post/4577e27f-faba-4c1b-a6f7-b4ffbfb7f046/image.png" width="70%"></center>
쉽게 생가가하면 대기열 이라고 생각하면 된다. 
가장 먼저 들어온 사람이 가장 먼저 나간다.

맨 뒤에 새로운 요소를 넣는 것을 **EnQueue**
맨 앞에 요소를 삭제하는 것을 **DeQueue**

> **First in First out (선입 선출)**

맨 앞의 요소를 **front**, 맨 뒤의 요소를 **rear** 라고 한다.
## 응용

큐도 스택처럼 배열(Array) 과 연결 리스트(linked list)로 구현 할 수 있다.

큐의 특징상 즉시 사용하지 않을 때 유리하다. 주요 사용 예는 다음과 같다.
+ 요소들을 다양한 사용자가 사용할 때 (cpu/disk 스케줄링에 사용)
+ data가 비동기적으로 변환 될때 (data가 실시간으로 주고 받지 않아도 될때) 
      예를들어, IO Buffer, pipe, file IO 등에 쓰인다.
+ 운영체제(**O**peration **S**ystem) 등에서 쓰인다.
    + 세마포어
    + 프린터 스풀러
    + 키보드 등에서 쓰이는 버퍼(buffer)
+ 네트워크
    + 라우터/ 스위치에서의 큐
    + 메일 큐
    

### 참조

+ 참고한 [article](https://www.geeksforgeeks.org/applications-of-queue-data-structure/)
+ 스택과 큐의 차이점을 잘 정리해준 [article](https://www.educba.com/stack-vs-queue/)
