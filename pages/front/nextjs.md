---
layout: page
related_posts:
  - /be/
title: nextjs
description: >
  nextjs 수업
---

## React vs Next.js의 차이

| 특징           | 리액트         | 넥스트.js                |
| -------------- | -------------- | ------------------------ |
| 유형           | 라이브러리     | 프레임워크               |
| 렌더링         | CSR 기본       | SSR,SSG,CSR 지원         |
| 라우팅         | 별도 설정 필요 | 파일기반 라우팅 내장     |
| 설정 난이도    | 수동 설정 필요 | 제로 컨피그 기본 제공    |
| 주요 사용 사례 | SPA,동적 UI    | SEO 친화적 사이트,풀스택 |

🏀 결론적으로, 리액트는 자유도가 높고 UI 중심의 프로젝트에 적합하며, 넥스트.js는 리액트를 기반으로 생산성과 편의성을 높여주는 프레임워크로, 더 구조화된 솔루션이 필요한 경우에 유용합니다. 프로젝트 요구사항에 따라 선택하면 됨.

## 프로젝트 생성하기

**❗ 해당 폴더에 들어가 cmd창에 입력**

```
npx create-next-app@latest . (현재폴더)
```

**❗ 체크하기**
![Image](/assets/img/pageimg/nextjs1.png)

**❗ react-dom 설치**

```
 npm install next@latest react@latest react-dom@latest
 npm run dev
```

**❗ 실행 성공화면**
![Image](/assets/img/pageimg/nextjs2.png)

<br>

#### 👉 [공식페이지 참고](https://nextjs.org/docs/app/getting-started/installation)
