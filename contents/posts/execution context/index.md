---
title: "Execution Context(실행 컨텍스트)란"
description: "Execution Context(실행 컨텍스트)의 종류와 구성요소"
date: 2024-07-02
update: 2024-07-03
tags:
  - javascript
series: "Execution Context"
---

## 실행 가능한 자바스크립트의 코드 블록

> 실행 컨텍스트란 실행할 코드에 제공할 환경 정보들을 모아놓은 객체  
> 여기서 환경이란 전역공간 혹은 함수 내부의 환경을 의미  
> 해당 환경에는 변수, 함수, this, arguments 등에 대한 정보를 담고 있음  
> 자바스크립트 엔진에서 코드가 실행 된다는 것은 실행 컨텍스트 내부에서 코드가 실행되고 있다는 의미

<br/>

## 컨텍스트 종류

자바스크립트의 코드는 아래 세 종류가 있음

1. 글로벌 스코프에서 실행하는 글로벌 코드
2. 함수 스코프에서 실행하는 함수 코드
3. eval()로 강제 실행되는 코드 (사용하지 않음)

<br/>

각각의 코드는 자신만의 실행 컨텍스트를 생성  
엔진이 스크립트 파일 실행 전 글로벌 실행 컨텍스트(GEC)가 생성되고,  
함수가 호출될 때마다 함수 실행 컨텍스트(FEC)가 생성되어  
자바스크립트 엔진의 Call Stack이라는 곳에 쌓이게 됨


### 예시

```js
// 1

let hi = "Hi";

function foo() {
  let hello = "Hello";
}

function bar() {
  foo(); // 3
}

bar(); // 2
```

1. 가장 먼저 글로벌 실행 컨텍스트(GEC) 생성  
   스택의 가장 아래에 위치
2. bar() 함수 호출 시 해당 함수의 함수 실행 컨텍스트(FEC) 생성  
   1에서 생성된 GEC 위로 올라감
3. bar() 함수 안에서 foo() 호출 시 또 다른 FEC 생성  
   2에서 생성된 FEC 위로 올라감
4. foo() 함수 리턴 시 3에서 생성된 FEC를 스택에서 제거
5. bar() 함수 리턴 시 2에서 생성된 FEC를 스택에서 제거
6. GEC만 남음

<br/>

## 컨텍스트 구성 요소

### 1. Variable Environment

현재 컨텍스트 내부의 식별자 정보인 'Environment Record'와
외부 환경 정보 'Outer Environment Reference'가 포함되어 있음  
Variable Environment에 먼저 정보를 담고, Lexical Environment에 복사해서 사용

### 2. Lexical Environment

초기에는 Variable Environment와 같지만 변경사항이 실시간으로 적용 되므로 최신 상태를 저장하고 있음

### 예시

```js
let hi = "Hi";

function foo() {
  let hello = "Hello";
  console.log(hi);
}

foo();
```

- foo() 함수에서 hello는 Environment Record
- foo() 함수에서 hi 호출 시 Outer Environment Reference를 통해 상위 컨텍스트에 접근 가능
- 순서: Environment Record -> Outer Environment Reference
- 위 과정을 스코프 체인이라고 함

### 3. This Binding

this는 현재 컨텍스트를 가리킴

<br/>

## 참고

- https://github.com/baeharam/Must-Know-About-Frontend/blob/main/Notes/javascript/execution-context.md
- https://gamguma.dev/post/2022/04/js_execution_context
- https://blog.naver.com/dlaxodud2388/222655214381
