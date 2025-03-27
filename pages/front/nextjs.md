---
layout: page
related_posts:
  - /be/
title: nextjs
date: 2025-03-27
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

## 실습하기

[👊코드 참고하기](https://github.com/ju2hong/2025htboot/tree/main/nextjs/weather-app)

- 보통 src 파일을 사용함 ! -> .next 라는 폴더가 생기는데, 이는 지워도 됨.
- page.tsx 보이는 화면, 화면 좌측 하단에 보이는 것은 global.css파일에서 아래 코드를 추가해주면 됨.

  ```
  nextjs-portal {  display: none;  }
  ```

- layout 파일은 페이지 default 값이나 화면간 정보를 나타냄.
- 서브페이지 생성시 src/app/폴더/파일 경로를 확인 후에 만들어야 함
- 예를들어, /detail/page.tsx 하면 localhost:3000/detail 하면 해당 페이지가 보임. -> react 에 비해 간편한 라우팅
- 동적 라우팅을 위해 대괄호폴더 [locaion]을 만들기

- 페이지간 이동

  - 임포트를 통해 간단하게 페이지간 이동 가능

  ```
  import Link from 'next/link'
  <Link href="/">홈으로</Link>
  ```

  - 라우터 사용

  ```
  import { useRouter } from 'next/navigation'
  ```

#### 날씨 api 가져오기

- 로그인 후 개인 api key 받아오기
  [날씨api 사이트](https://www.weatherapi.com/)
- tip. api key는 git에 올리면 공개되기 때문에 주로 .env 파일에 넣어서 아래 코드처럼 사용함.

  ```
  // .env
  NEXT_PUBLIC_WEATHER_API_KEY=(api키를 적으세요)
  ```

- page.tsx에 함수를 넣는 방법 (1)

  ```
  //page.tsx
  const apiKey = process.env.NEXT_PUBLIC_WEATHER_API_KEY
  const getCurrentWeather = async () => {
  const res = await fetch(
    `http://api.weatherapi.com/v1/current.json?key=${API_KEY}&q=${location}&aqi=no`
  )
  return res.json()
  }
  ```

- utils/getCurrentWeather.ts에 함수파일을 생성하는 방법 (2)

  ```
  const API_KEY = process.env.NEXT_PUBLIC_API_KEY;

  export const getCurrentWeather = async () => {
    const res = await fetch(
      `http://api.weatherapi.com/v1/current.json?key=${API_KEY}&q=${location}&aqi=no`
    );

    return res.json();
  };

  ```

#### 에러 페이지 만들기

/src/app/error.tsx 만들기

```
"use client";

export default function Error() {
  return <h1>에러페이지</h1>;
}
```

#### 로딩 페이지 만들기

/src/app/loading.tsx 만들기

```
export default function Loading() {
  return <h1>로딩 중</h1>
}
```

**-> 라우터 없이도 오류가 뜨면 해당 error.tsx와 loading.tsx 화면이 뜨는걸 알 수 있다.**

### 나라별 3일치 날씨 띄우기

1. /util/getForecast.ts 함수 파일 만들기

   ```
   //타입 지정 후
   export const getCurrentWeather = async (
     location: string
   ): Promise<Response> => {
     const res = await fetch(
       `http://api.weatherapi.com/v1/current.json?key=${API_KEY}&q=${location}&aqi=no`
     )

     return res.json()
   }
   ```

2. /[location]/page.tsx 코드 수정

   ```
   import { getForecast } from '../utils/getForecast'
   export default async function Detail({ params }: Props) {
     const { location } = await params
     const json = await getForecast(location)
     return (
       <>
         <h1>{name}의 3일치 날씨 예보</h1>
         <ul>
           {json.forecast.forecastday.map((day) => (
             <li key={day.date}>
               {day.date} / {day.day.avgtemp_c} / {day.day.condition.text}
               <br />
               <img
                 src={`http:${day.day.condition.icon}`}
                 alt={day.day.condition.text}
               />
               <span>{day.day.condition.text}</span>
             </li>
           ))}
         </ul>
         <br />
         <HomeButton />
       </>
     )
   }
   ```

![Image](/assets/img/pageimg/nextjs4.png)

#### 메타데이터 다루기

- 헤더파일의 메타데이터 변경시에 layout.tsx 를 수정

  ```
  export const metadata: Metadata = {
    title: "날씨 앱",
    description: "날씨를 알려주는 앱",
  };
  ```

- 상세 페이지 메타데이터는 해당 페이지(/[locaation]/page.tsx)를 수정

  ```
  export function generateMetadata() {
    return {
      title: '상세 날씨 데이터',
      description: '상세 날씨 데이터입니다.',
    }
  }
  ```

#### fetch 함수의 캐싱 전략

- 기본 캐싱 동작
  - fetch는 기본적으로 캐싱 활성화(디스크 기반 데이터 캐시)
  - 동일한 URL + 옵션 요청시 캐시된 데이터 재사용
  - 환경별 동작 :
    - SSG(정적 생성) -> 빌드 타임에 데이터 캐싱
    - SSR(서버 사이드 랜더링) -> 요청마다 캐시, 동일 요청 시 재사용
    - 클라이언트 사이드 -> 별도 설정이 없으면 기본적으로 캐싱됨
- 캐시 옵션
  - `cache:'force-cache'` (기본값 -> 캐시 강제 사용)
  - `cache: 'no-store'` -> 항상 새요청, 실시간 데이터에 적합
- 캐시 재검증 (ISR)
  - `next: {revalidate : N}` -> `N`초마다 백그라운드에서 데이터 갱신
    ```
    fetch('https://api.example.com/data',{next:{revalidate:60}})
    // 60초마다 새 데이터 가져와 캐시 업데이트
    ```
- 동적 데이터 처리
  - URL에 동적 파라미터 포함 시 각 요청 별도 캐싱됨
  - 실시간 데이터 -> `cache:no-store` 사용
- 클라이언트 사이드 캐싱(SWR 활용)

  - useSWR을 사용하면 자동 캐싱 & 주기적 갱신 가능

  ```
   import useSWR from 'swr';
   const fetcher = (url) => fetch(url).then((res) => res.json());
   const { data } = useSWR('/api/data', fetcher, { refreshInterval: 5000 });
  ```

- 주의
  - GET 요청만 캐싱, POST/PUT은 캐싱 X
  - 쿼리/헤더가 다르면 다른 요청으로 간주
  - 개발 모드(next dev)에서는 캐싱 X, 프로덕션에서만 적용

✨결론

- 정적 데이터 -> `fore-cache` 활용
- 동적 데이터 -> `no-store` 또는 `revalidate` 사용
- 클라이언트 캐싱 -> SWR 등 활용
