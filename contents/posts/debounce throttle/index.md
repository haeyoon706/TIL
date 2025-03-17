---
title: "Debounce / Throttle"
description: "Debounce와 Throttle의 정의와 차이점"
date: 2025-03-17
update: 2025-03-17
tags:
  - javascript
series: "javascript 최적화"
---


## Debounce란
연속적으로 발생한 이벤트를 하나로 처리하는 방식<br>
입력이 계속 들어오면 실행 타이머가 계속 초기화 됨
<br/><br/>
### 예제
- input 이벤트 함수를 타이핑 할 때마다가 아닌 입력을 멈춘 후 일정 시간이 지난 후 실행
- resize 이벤트 함수를 resize 할 때마다가 아닌 resize 완료 후 실행
<br/><br/>

```js
// debounce를 사용한 훅

import { useEffect, useState } from "react";

/**
 * 입력이 멈춘 후 일정 시간이 지나면 값을 업데이트하는 Debounce 훅
 * @param value 변경되는 값 (ex: input 값)
 * @param delay 지연 시간 (기본값: 500ms)
 * @returns delay 후에 업데이트된 값
 */
export const useDebounce = (value: any, delay: number = 500) => {
  const [debouncedValue, setDebouncedValue] = useState(value);

  useEffect(() => {
    // delay 후에 debouncedValue를 업데이트하는 타이머 설정
    const handler = setTimeout(() => {
      setDebouncedValue(value);
    }, delay);

    // 값이 변경될 때마다 기존 타이머를 제거하고 새로운 타이머 시작 (즉, 입력 중이면 실행되지 않음)
    return () => {
      clearTimeout(handler);
    };
  }, [value, delay]);

  return debouncedValue;
};

```

## Throttle란
이벤트를 일정한 주기마다 발생하도록 하는 방식<br>
마지막 입력 여부와 관계없이 주어진 간격마다 실행
<br/><br/>
### 예제
- 일정 시간마다 scroll 감지
- 실시간 UI 업데이트
<br/><br/>

```js
// throttle를 사용한 훅

import { useEffect, useState } from "react";

/**
 * 일정 간격마다 값이 업데이트되는 Throttle 훅
 * @param value 변경되는 값 (ex: 입력값, 스크롤 값)
 * @param limit 실행 간격 시간 (기본값: 500ms)
 * @returns 일정 간격마다 업데이트된 값
 */
export const useThrottle = (value: any, limit: number = 500) => {
  const [throttledValue, setThrottledValue] = useState(value);
  const [lastUpdated, setLastUpdated] = useState(0);

  useEffect(() => {
    const now = Date.now();

    // 마지막 업데이트 이후 limit(ms) 시간이 지나면 실행
    if (now - lastUpdated >= limit) {
      setThrottledValue(value);
      setLastUpdated(now);
    }
  }, [value, limit, lastUpdated]);

  return throttledValue;
};


```
## 비교
|  | **Debounce** | **Throttle** |
|---|---|---|
| 📌 실행 시점 | 입력이 멈춘 후 일정 시간이 지나야 실행됨 | 일정한 시간 간격마다 실행됨 |
| 🛠️ 사용 사례 | 검색어 자동완성, 버튼 중복 클릭 방지 | 스크롤 감지, 실시간 UI 업데이트 |
| ✅ 장점 | 불필요한 API 호출 방지 | 성능 최적화 (이벤트 과부하 방지) |



## 참고

- https://velog.io/@jiynn_12/Debounce-%EC%99%80-throttle-%EC%97%90-%EB%8C%80%ED%95%B4-%EC%95%8C%EC%95%84%EB%B3%B4%EA%B3%A0-%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8%EC%97%90-%EC%A0%81%EC%9A%A9%ED%95%B4%EB%B3%B4%EC%9E%90
