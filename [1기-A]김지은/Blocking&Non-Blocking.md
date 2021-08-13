## Introduction

- 네트워크(소켓), OS 관련 개념입니다.
- I/O : Input/Output
- 이번에는 컴퓨터에서 word 파일을 읽고, 쓰는 것만 생각해봅시다.
- 커널 레벨에서만 입출력이 가능합니다.
- 커널은 운영체제(OS)를 통제하는 brain입니다.
- 유저 프로세스는 요청을 하고, 작업 완료 후 커널이 반환해주는 결과를 기다리는데 이때 Blocking/Non-Blocking 개념이 등장합니다.

## 이해해야 할 것

- Blocing/Non-Blocking의 작동원리와 차이
- 동기, 비동기의 작동원리와 차이
- 어떻게 사용할 수 있을까?

## 애플리케이션과 커널의 관계

![](https://images.velog.io/images/xjpassion/post/12662334-ca41-4cc7-9baa-bebb3ea14bdc/image.png)

> 이번에는 빨간 네모 부분만 알아봅니다.

![](https://images.velog.io/images/xjpassion/post/9a30aee6-80e3-4975-8171-e2485401cf43/image.png)

## Blocking

- I/O 작업이 진행되는 동안 유저 프로세스가 잠깐 대기하는 방식입니다.
- return이 올 때까지 다른 일을 안하고 기다리므로 applications의 자원을 낭비합니다.
- 스레드 1개를 사용합니다.
- 파일을 저장할 때 사용할 수 있습니다.

![](https://images.velog.io/images/xjpassion/post/a901c5d5-a176-49c0-8ebe-afb7ba21af4e/image.png)

## Non-blocking

- I/O 작업이 진행되지 않으면 바로 결과를 반환합니다.
- 커널은 작업이 끝나고도 결과를 반환합니다.
- 하나의 스레드가 여러 개의 I/O call를 보냅니다.
- 계속 작업을 할 수 있는지 커널에게 지속적으로 물어봐야 하므로 역시 application의 자원을 낭비합니다.

![](https://images.velog.io/images/xjpassion/post/79aafa07-674b-40a9-a0a1-ea9eaa0ff115/image.png)

## 동기(Synchronous)

- 스레드가 첫번째 작업이 끝나고 난 후에 두번째 작업을 시작합니다.
- 작업 요청을 했을 때 **결과값(return)을 스레드가 직접 받습니다.**
- **호출한 함수가 작업 완료를 신경 씁니다.**

## 비동기(Asynchronous)

- 스레드가 작업을 요청하고, 완료를 기다리지 않고, 스레드는 다른 일을 처리할 수 있습니다.
- 작업 요청을 했을 때 요청의 **결과값(return)을 간접적으로 받습니다.**
- 해당 요청 작업은 별도의 스레드에서 실행하게 됩니다. 콜백을 통한 처리가 비동기 처리라고 할 수 있습니다.
- **호출된 함수(callback 함수)가 작업 완료를 신경 씁니다. 해당 스레드가 신경쓰진 않습니다.**

## 4가지 Blocking, 동시성 종류

![](https://images.velog.io/images/xjpassion/post/f1cdb0d3-b04d-4fd2-b81a-3242674ae7ef/image.png)

### blocking + Synchronous

- 결과가 처리되어 나올때까지 기다렸다가 return 값으로 결과를 전달한다.

### blocking + Asynchronous

- 호출되는 함수가 바로 return하지 않고, 호출하는 함수는 작업 완료 여부를 신경쓰지 않는다.
- (이 조합은 사실 이점이 없어서 일부러 이 방식을 사용하진 않는다.)

### non-blocking + Synchronous

- 결과가 없다면 바로 return하고, 결과가 있으면 바로 결과를 return 한다.
- 결과가 생길때까지 계속 완료 되었는지 물어본다.

### non-blocking + Asynchronous

- 작업 요청을 받아서 별도의 프로세서에서 진행하게 하고, 바로 return(작업 끝)한다.
- 결과는 별도의 작업 후 간접적으로 전달(callback)한다.

## 잠깐 정리

- Blocking : 일 다하고 결과 준다!
- Non-Blocking : 일 못하면 일단 알려주고, 작업 끝나고도 알려준다!

- Synchronous(동기) : 작업 완료했는지 관심 있다!
- Asynchronous(비동기) : 작업 완료했는지 관심 없다!

## 참고자료

커널

[https://ko.wikipedia.org/wiki/커널_(컴퓨팅)](https://ko.wikipedia.org/wiki/%EC%BB%A4%EB%84%90_(%EC%BB%B4%ED%93%A8%ED%8C%85))

Blocking/Non-blocking

[https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=93lms&logNo=221412867554](https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=93lms&logNo=221412867554)

[https://velog.io/@wonhee010/동기vs비동기-feat.-blocking-vs-non-blocking](https://velog.io/@wonhee010/%EB%8F%99%EA%B8%B0vs%EB%B9%84%EB%8F%99%EA%B8%B0-feat.-blocking-vs-non-blocking)
