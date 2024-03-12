- **(1) Next.js의 버전의 주요 분기점**

  - 일반적으로 `Next.js`를 채택할 때 주요한 의사결정은
    > **1) App router를 쓸 것인가? 2) Pages router를 쓸 것인가?**
  - 왜 `router` 기반으로 **주요한 속성**을 분류하는 것일까?

    1. 웹사이트를 기획/설계 할 때, 어떤 페이지가 존재하게 할지, 라우팅은 어떻게 하게 할지를 항상 먼저 고려
    2. 기존의 `React.js`를 사용하여 웹 애플리케이션을 만들 때는 `react-router-dom`을 이용
    3. 이렇게 중요한 라우팅에 대한 근본적인 시각, 전략이 next.js 13버전을 기점으로 변경되었다.

       1. 변경 전 : `pages` 폴더에 원하는 페이지의 파일 이름을 둔다.

       2. 변경 후 : `app` 폴더 밑에 폴더명을 기반으로 자동 라우팅이 된다. → `app router`

- **(2) app router와 pages router 어떤 것을 사용해야 할까?**

  - (2)-1. app router
    > ?? : “이제 안정화도 되었는데, 자꾸 이전 버전을 사용하는 것은 마치 `리액트`에서 `class`형 컴포넌트를 쓰는 것과 같아!”
    >
    > ?? : “추가기능이 얼마나 많은데, 당연히 `최신 버전` 사용해야 하는 것 아님??”
    >
    > ?? : “공식 홈페이지에서도 이제 웬만하면 `app router` 쓰라고 하는데?”
  - (2)-2. pages router\
    > ?? : “`next.js`가 나온지 얼마나 오래됐는데! 이미 존재하는 프로젝트에 투입되면 어쩌려고 그래?”
    >
    > ?? : “더 직관적이야! `app router`에서 새롭게 제시하는 내용이 정말 효용이 있나?? 난 잘 모르겠는디;;”
  - (2)-3. 하지만 **정말 중요한 것**
      <aside>
      ☝ `Next.js`가 제시하는 본질! 기존의 `React.js`가 가지고 있는 한계를 이해하고 내가 왜 `Next.js`를 사용하는지를 정확히 알고 있어야 한다. 따라서 `app` router든, `pages` router든 주요 핵심 개념**(Routing, Rendering, Data Fetching 등)**을 잘 이해하면 된다.
      
      </aside>

- **(3) MPA부터 SSR까지**
    <aside>
    💡 렌더링 기법을 가볍게 살펴봅니다.
    
    </aside>
    <img width="100%" src="https://teamsparta.notion.site/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F83c75a39-3aba-4ba4-a792-7aefe4b07895%2Fcf7516c9-3e25-4dc3-92c3-67eb8673b458%2FUntitled.png?table=block&id=c27eb597-5039-422e-902d-59dc697ec17b&spaceId=83c75a39-3aba-4ba4-a792-7aefe4b07895&width=1370&userId=&cache=v2"/>

  1. 원시적 방법, `MPA`

     1. 원시적인 서버 사이드 렌더링 방식인 `MPA`로부터 프론트엔드 웹 개발은 시작되었다.

        ```jsx
        /about → about.html
        /profile → profile.html
        ```

     2. 페이지 이동시 및 렌더링 시 깜빡거리는 현상이 있으므로 `UX가 저하`
     3. 이러한 문제 때문에 React, Angular, Vue 등 `SPA`(Single Page Application)이 등장했다.

  2. 획기적 방법, `SPA`
     1. 브라우저에서 `Javascript`를 이용해 동적으로 페이지를 렌더링 하는 방식
     2. ‘`Client`의 사이드에서 렌더링을 한다’라는 개념은 기존 프론트엔드 개발자들에게 획기적 방법으로 소개
     3. 최초 서버로부터는 텅 빈, `root`라는 `id`를 가진 `div`만 다운로드 ⇒ **javascript로 UI가 완성**
     4. 더 이상 새로고침이나 깜빡거림 없이 웹서비스 이용이 가능하여 `UX`가 크게 `향상`
     5. 그러나 다음과 같은 단점이 새롭게 대두됨
        1. 늦는 초기로딩속도
           1. 이를 보완하기 위해 `Code Spilitting`(Lazy-Loading) 방법 제시
           2. 하나로 `번들`된 코드를 여러 코드로 나눠 당장 필요한 코드가 아니면 나중에 불러옴
  3. 주요 렌더링 기법을 다음과 같이 분류

     <aside>
     ☝ `build`라는 용어가 어렵다면, 소스코드를 실행 가능한 상태로 만들어 놓은 과정이라고 생각하기
     `vercel`을 통해서 자동으로 배포가 되게 했다면, `vercel` 내부적으로 `build`를 하는 과정을 포함

     </aside>

     - (1) CSR(Client Side Rendering)
       <img width="100%" src="https://teamsparta.notion.site/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F83c75a39-3aba-4ba4-a792-7aefe4b07895%2Fb210e576-1cac-445f-8f21-8b3626e1789c%2FUntitled.png?table=block&id=0620f8cd-8690-4ba1-a503-292c1f2e99b8&spaceId=83c75a39-3aba-4ba4-a792-7aefe4b07895&width=1370&userId=&cache=v2"/>

       - 특징
         - 순수 리액트 사용했을 때 100%
         - 브라우저에서 JavaScript를 이용해 동적으로 페이지를 렌더링하는 방식
         - 렌더링의 주체 : 클라이언트
       - 장점
         - (최초 한번 로드가 끝나면) 사용자와의 상호작용이 빠르고 부드럽다
         - 서버에게 추가적인 요청을 보낼 필요가 없기 때문에, 사용자 경험이 좋다.
         - 서버 부하가 적음
       - 단점
         - 첫 페이지 로딩 시간(Time To View)이 길 수 있다.
         - JavaScript가 로딩되고 실행될 때까지 페이지가 비어있어 검색 엔진 최적화(SEO)에 불리하다.
       - 코드

         ```jsx
         import React from "react";
         import ReactDOM from "react-dom";

         function App() {
           return <h1>Hello, Client Side Rendering!</h1>;
         }

         // index.js
         ReactDOM.render(<App />, document.getElementById("root"));
         ```

     - (2) SSG(Static Site Generation)
       <img width="100%" src="https://teamsparta.notion.site/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F83c75a39-3aba-4ba4-a792-7aefe4b07895%2F2c7fda9d-2867-4ff3-8555-9f5c25217101%2FUntitled.png?table=block&id=3496fec1-c565-40e7-8c7b-518b603d1f8d&spaceId=83c75a39-3aba-4ba4-a792-7aefe4b07895&width=1370&userId=&cache=v2"/>

       - 특징
         - 서버에서 페이지를 렌더링하여 클라이언트에게 HTML을 전달하는 방식
         - **최초 빌드시에만 생성이 됨**
         - **‘빌드’**
           - vercel을 이용할 때 알아서 build를 해주기 때문에 Local 환경에서 빌드를 할 필요가 없었다
         - 사전에 미리 정적페이지를 여러개 만들어놓음 → 클라이언트가 홈페이지 요청을 하면, 서버에서는 이미 만들어져있는 사이트를 바로 제공! → 클라이언트는 표기만 함
       - 장점
         - 첫 페이지 로딩 시간이 매우 짧아(TTV) 사용자가 빠르게 페이지를 볼 수 있다. 또한, SEO에 유리
         - CDN(Content Delivery Network) 캐싱 가능
       - 단점
         - 정적인 데이터에만 사용할 수 있음
         - 사용자와의 상호작용이 서버와의 통신에 의존하므로, 클라이언트 사이드 렌더링보다 상호작용이 느릴 수 있다. 또한, 서버 부하가 클 수 있다.
         - 마이페이지 처럼 데이터에 의존하여 화면을 그려주는 경우 사용 불가
       - 코드(next.js 12 버전)
           <aside>
           ☝ 렌더링 형태에 집중 🙂
           
           </aside>
           
           ```jsx
           import React from 'react';
           
           function HomePage({ data }) {
             return <div>{data}</div>;
           }
           
           export async function getStaticProps() {
             const res = await fetch('https://...'); // 외부 API 호출
             const data = await res.json();
           
             return { props: { data } };
           }
           
           export default HomePage;
           ```

     - (3) ISR(Incremental Static Regeneration)
       <img width="100%" src="https://teamsparta.notion.site/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F83c75a39-3aba-4ba4-a792-7aefe4b07895%2Fb1c36054-c087-4c86-83b7-ba4bd11b45e1%2FUntitled.png?table=block&id=4af5f64c-55ff-4e13-91c1-62b290a4bf30&spaceId=83c75a39-3aba-4ba4-a792-7aefe4b07895&width=1370&userId=&cache=v2"/>

       - 특징
         - SSG처럼 정적 페이지를 제공
         - 설정한 주기만큼 페이지를 계속 생성해 줌
           - ex : 주기가 10분이라면? → 10분마다 데이터베이스 또는 외부 영향 때문에 변경된 사항을 반영하는 역할
         - 정적 페이지를 먼저 보여주고, 필요에 따라 서버에서 페이지를 재생성하는 방식
       - 장점
         - 정적 페이지를 먼저 제공하므로 사용자 경험이 좋으며, 콘텐츠가 변경되었을 때 서버에서 페이지를 재생성하므로 최신 상태를 (그나마) 유지할 수 있다.
         - CDN 캐싱 가능
       - 단점
         - 동적인 콘텐츠를 다루기에 한계가 있을 수 있다. 실시간 페이지 아님
         - 마이페이지 처럼 데이터에 의존하여 화면을 그려주는 경우 사용 불가
       - 코드

         ```jsx
         import React from "react";

         function HomePage({ data }) {
           return <div>{data}</div>;
         }

         export async function getStaticProps() {
           const res = await fetch("https://..."); // 외부 API 호출
           const data = await res.json();

           return {
             props: { data },
             revalidate: 60, // 1초 후에 페이지 재생성
           };
         }

         export default HomePage;
         ```

     - (4) SSR
       <img width="100%" src="https://teamsparta.notion.site/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F83c75a39-3aba-4ba4-a792-7aefe4b07895%2F562916c0-e8a2-4ea8-9cc3-852e21948210%2FUntitled.png?table=block&id=f5e13c08-dca4-4446-ac8d-cd476c3fcddf&spaceId=83c75a39-3aba-4ba4-a792-7aefe4b07895&width=1370&userId=&cache=v2"/>

       - 특징
         - 빌드 시점에 모든 페이지를 미리 생성하여 서버 부하를 줄이는 방식
         - SSG, ISR처럼 렌더링 주체가 서버!
         - 클라이언트의 요청 시 렌더링
           - C → S : 이 페이지 줘!
           - S → C : (데이터베이스 읽고 등등 한 후) html 파일을 제공
       - 장점
         - 빠른 로딩 속도(TTV)와 높은 보안성을 제공
         - SEO 최적화 좋음
         - 실시간 데이터를 사용
         - 마이페이지 처럼 데이터에 의존한 페이지 구성 가능
         - CDN 캐싱 불가
       - 단점
         - 사이트의 콘텐츠가 변경되면 전체 사이트를 다시 빌드해야 하는데, 이 과정이 시간이 오래 걸릴 수 있다. → **_서버 과부하_**
         - 요청할 때 마다 페이지를 만들어야 함
       - 코드

         ```jsx
         import React from "react";

         function HomePage({ data }) {
           return <div>{data}</div>;
         }

         export async function getServerSideProps() {
           const res = await fetch("https://..."); // 외부 API 호출
           const data = await res.json();

           return { props: { data } };
         }

         export default HomePage;
         ```

     - (종합) 비교
       | | CSR | SSR | SSG | ISR |
       | ---------------------------- | ---- | ---- | ---- | ------------ |
       | 빌드 시간 | 짧다 | 짧다 | 길다 | 길다 |
       | SEO | 나쁨 | 좋음 | 좋음 | 좋음 |
       | 페이지 요청에 따른 응답 시간 | 보통 | 길다 | 짧다 | 짧다 |
       | 최신 정보인가? | 맞음 | 맞음 | 아님 | 아닐 수 있음 |

  4. Hydration

     <aside>
     ☝ TTV, TTI 그리고 Hydration

     Next.js를 이해할 때는 위 언급한 세 개념. TTV, TTI, Hydration의 개념이 상당히 중요! 이 개념을 아는지 모르는지에 따라서 렌더링 패턴을 이해하고 쓸 수 있느냐가 결정하기 때문이다.

     </aside>

     1. CSR
        - React에서 CSR로만 컴포넌트 렌더링을 할 때는 TTV가 오래 걸렸다. 모든 React 소스파일을 다운로드 받아야만 화면을 볼 수 있기 때문
        - 여기에서 Hydration 개념이 들어간다. 최초 서버에서는 index.html 파일만 제공하지만 이후 React 소스파일을 바탕으로 한 자바스크립트 파일이 모두 다운로드 돼야만(즉, Hydration이 돼야만) 최종 소스코드를 볼 수 있는 것
          > 하지만 CSR의 과정에서의 Hydration 과정을 Hydration으로 볼 것이냐 하는 것은 이견이 있을 수 있다.
     2. SSR
        - 서버에서는 사용자의 요청이 있을 때 마다 페이지를 새로 그려서 사용자에게 제공
        - 두 과정으로 나눠서 제공
          1. pre-rendering : 사용자와 상호작용하는 부분을 제외한 껍데기만을 먼저 브라우저에게 제공. TTV가 엄청나게 빠름
          2. hydration : 이 과정이 일어나기 전까지는 껍데기만 있는 html 파일이기 때문에 사용자가 아무리 버튼을 click 해도 아무 동작이 일어나지 않는다. 인터렉션에 필요한 모든 파일을 다운로드 받는 과정 즉, hydration 과정이 끝나야 그제서야 인터렉션이 가능하다. 이 간극! TTI를 줄이는 것이 관건!
     3. SSG, ISR도 SSR과 마찬가지로 hydration 과정이 존재한다.
