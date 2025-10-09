---
title: "Next.js의 ISR"
description: "ISR을 통한 캐싱 전략"
date: 2025-10-08
update: 2025-10-08
tags:
  - Next.js
series: "Next.js의 렌더링 기법"
---

## 들어가며

React와 Next.js를 사용한 여러 프로젝트를 진행하면서 CSR, SSR, SSG 렌더링 기법은 익숙하지만
ISR 기법은 사용해보지 않아서 여러 아티클과 예제를 보며 포스팅을 써보겄읍니다.

## ISR(Incremental Static Regeneration)이란

> **“정적 페이지를 부분적으로 다시 생성하는 렌더링 방식”**

ISR은 SSG(정적 생성)와 SSR(서버 렌더링)의 중간 형태로  
특정 주기마다 정적 페이지를 **부분적으로 갱신(revalidate)** 하여  
최신 데이터를 유지하면서도 빠른 응답 속도를 제공

Next.js 서버는 사용자가 접근한 페이지의 **캐시된 HTML을 우선 반환**하고,  
`revalidate`로 지정된 시간이 지나면 **백그라운드에서 새 페이지를 생성**  
이렇게 새로 만들어진 페이지는 이후 요청 시 곧바로 제공되어,  
SSR처럼 최신 데이터를 보여주면서도 SSG처럼 빠르게 동작하게 됨

## ISR 사용 방법

```js
import Link from "next/link";
import { getAllPosts } from "@/lib/posts";

export default function PostList({ posts }: { posts: any[] }) {
  return (
    <main>
      <h1>게시글 목록</h1>
      <ul>
        {posts.map((post) => (
          <li key={post.id}>
            <Link href={`/posts/${post.id}`}>{post.title}</Link>
          </li>
        ))}
      </ul>
    </main>
  );
}

export async function getStaticProps() {
  const posts = await getAllPosts();
  return {
    props: { posts },
    revalidate: 120, // 2분마다 최신 목록으로 업데이트
  };
}
```

## ISR의 한계

> ISR은 자주 읽히지만 가끔만 업데이트되는 페이지(블로그, 뉴스, 상품 상세)에 적합

### 1.실시간 데이터 반영 어려움

- `revalidate` 주기 동안 페이지는 캐시 상태 유지
- 댓글, 재고, 채팅 등 실시간성이 필요한 서비스에는 부적합

### 2. 첫 요청 지연

- `fallback: "blocking"` 사용 시 처음 요청은 서버에서 렌더링 후 반환
- 초기 요청 속도는 SSG보다 느릴 수 있음

### 3. 외부 데이터 변경 감지 한계

- `revalidate`는 시간 기반 갱신
- 데이터가 바뀌어도 즉시 반영되지 않음 → 필요 시 **on-demand revalidate** 사용

## 참고

- https://velog.io/@codns1223/Nextjs-ISR-Incremental-Static-Regeneration%EC%9D%B4%EB%9E%80
- https://tech.kakao.com/posts/743
