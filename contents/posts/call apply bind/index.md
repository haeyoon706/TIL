---
title: "call / bind / apply"
description: "call / bind / apply 어쩌구"
date: 2024-07-26
update: 2024-07-26
tags:
  - javascript
series: "this 도장깨기"
---


## call

```js
var obj = { b: "발" };

var example = function (a) {
  return a + this.b;
};

example('닭'); // 닭undefined
example.call(obj, '닭'); // 닭발
```

첫번째 인자인 obj가 this를 대체함
this는 기본적으로 window를 가리킴
하지만 call, apply, bind의 첫번째 인자에서 this를 바꿀 수 있음

## apply

```js
function example() {
  var arr = Array.prototype.slice.apply(arguments);
}
example(1, 2, 3); // [1, 2, 3]
```

arguments는 기본적으로 유사배열이므로 배열의 프로토타입인 slice 메소드를 사용할 수 없음
하지만 apply()를 사용하여 arguments를 바인딩 하면 배열처럼 사용 가능

## bind

bind는 call, apply와 마찬가지로 this를 바꾸지만 호출은 하지 않음
bind()()로 하면 호출까지 가능 !


## 참고

- https://inpa.tistory.com/entry/JS-%F0%9F%93%9A-Call-Bind-Apply