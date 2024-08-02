---
title: "Closure (2)"
description: "Closure 고급"
date: 2024-08-01
update: 2024-08-01
tags:
  - javascript
series: "Execution Context"
---

## closure의 사용

### 데이터 은닉(캠슐화)
자바스크립트에서는 제공하지 않는 private method 기능 구현 가능  
private method : 같은 클래스 내부의 특정 메서드에서만 해당 메서드 호출 가능


```js
const makeCounter = function () {
  let privateCounter = 0;
  function changeBy(val) {
    privateCounter += val;
  }
  return {
    increment() {
      changeBy(1);
    },

    decrement() {
      changeBy(-1);
    },

    value() {
      return privateCounter;
    },
  };
};

const counter1 = makeCounter();
const counter2 = makeCounter();

console.log(counter1.value()); // 0.

counter1.increment();
counter1.increment();
console.log(counter1.value()); // 2.

counter1.decrement();
console.log(counter1.value()); // 1.
console.log(counter2.value()); // 0.
```

### 상태 유지
closure의 사전적 의미는 '폐쇠'  
특정 데이터를 스코프 안에 가두어 둔 채로 계속 사용 가능  
-> 메모리 측면에서 손해

```js
function Counter() {
    let count = 0;

    function increment() {
        count += 1;
        console.log(count);
    }

    return increment;
}

const counter = Counter();
counter(); // 1
counter(); // 2
counter(); // 3
```


<br/>

## 참고
- https://developer.mozilla.org/ko/docs/Web/JavaScript/Closures
- https://adjh54.tistory.com/64