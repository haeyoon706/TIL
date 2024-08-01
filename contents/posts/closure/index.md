---
title: "Closure (1)"
description: "Closure의 기본"
date: 2024-08-01
update: 2024-08-01
tags:
  - javascript
series: "Execution Context"
---

## closure란
클로저는 부모 함수 안에서 자식 함수를 선언하면  
자식함수를 어디에서 호출하더라도 자식함수 안에서 부모함수의 변수에 접근할 수 있음
<br/>
<br/>  

```js
let l0 = 'l0';

function fn2() {
    let l2 = 'l2';
    console.log(l0, l1, l2); // error
}

function fn1() {
    let l1 = 'l1';
}

fn1();
```
fn2의 스코프에서 fn1의 값을 가지고 올 수 없음  
<br/>

```js
let l0 = 'l0';


function fn1() {
    function fn2() {
        // fn2()는 내부 함수이자 클로저
        let l2 = 'l2';
        console.log(l0, l1, l2); // l0 l1 l2
    }

    let l1 = 'l1';
    fn2();
}

fn1();
```
local -> closure -> script  
closure에서 찾은 l1값 !  
-> 함수를 다른 함수 내에 정의 하면 부모 함수의 스코프에 접근 가능  

## dynamic scope (동적 스코프)
어디서 호출 하느냐에 따라 접근할 수 있는 유효범위가 달라지는 것   
but javascript는 정적 스코프  

## static scope, lexical scope (정적 스코프)
어디서 호출 했느냐가 아니라  
어디서 정의 되었느냐에 따라 유효 범위가 달라짐  

<br/>

## 참고

- https://www.youtube.com/watch?v=bwwaSwf7vkE
- https://developer.mozilla.org/ko/docs/Web/JavaScript/Closures