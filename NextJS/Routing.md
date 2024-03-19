## 01. **첫 installation : react와는 어떤 것이 다른지 특징 살펴보기**

- **(1) 설치하기**
  ```bash
  npx create-next-app@latest
  ```
- **(2) 특징살펴보기**

  1. src > app 이라는 directory
  2. 만들어진 파일들이 typescript 기반으로 만들어졌다
  3. 기본 파일로 다음 두 가지

     <aside>
     ☝ 앞으로 **`layout`**, **`page`** 이 두 컴포넌트를 주목!

     </aside>

     1. layout.tsx
        <img width="{해상도 비율}" src="https://teamsparta.notion.site/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F83c75a39-3aba-4ba4-a792-7aefe4b07895%2Fcc098381-1677-41e4-b479-2fdc4ec0fc8b%2FUntitled.png?table=block&id=fe56b305-540d-45f7-a444-c540b9eabeac&spaceId=83c75a39-3aba-4ba4-a792-7aefe4b07895&width=960&userId=&cache=v2)"/>

        - 리액트 컴포넌트
        - children을 가지고 있다

     2. page.tsx
        <img width="{해상도 비율}" src="https://teamsparta.notion.site/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F83c75a39-3aba-4ba4-a792-7aefe4b07895%2F72ec14b7-f827-4d4b-8865-0e08e6852303%2FUntitled.png?table=block&id=148f690e-9c92-4698-ae97-b4e95a7c4a9b&spaceId=83c75a39-3aba-4ba4-a792-7aefe4b07895&width=1270&userId=&cache=v2"/>

        - 리액트 컴포넌트

  4. tailwind.config.ts 파일
  5. package.json 파일
     - scripts 부분을 보니, dev / build / start
     - dev(npm run dev) : 개발자가 개발하는 중 사용하게 될 방법
     - build(npm run build) : production 레벨로 배포하기 전 필요한 빌드 작업 과정을 실행하기 위한 방법
     - start(npm run start) : 만들어진 build 파일을 이용하여 실행시키는 방법
     - 따라서, build하지 않고 start를 하는 경우 실행될 수 없다

## 02. 라우팅을 이해하기 위한 주요 용어

- **(1) Tree 관련**
  <img width="{해상도 비율}" src="https://nextjs.org/_next/image?url=%2Fdocs%2Flight%2Fterminology-component-tree.png&w=3840&q=75&dpl=dpl_8JWihzhXtpkYanrPN4yhGPoiM2MX"/>

  - **Tree**
    - 계층 구조를 시각적으로 잘 보기 위한 규칙(위 → 아래). DOM tree와 비슷
  - **Subtree**
    - tree의 한 부분
    - root부터 시작해서 leaf들에 이르기까지의 범위
  - **Root**
    - Tree 또는 Subtree의 첫 번째 노드
    - root layout 같은 것
      <img width="{해상도 비율}" src="https://teamsparta.notion.site/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F83c75a39-3aba-4ba4-a792-7aefe4b07895%2F97494fd7-3540-4065-baaa-c068d8344d6b%2FUntitled.png?table=block&id=43f2abc6-a933-42af-9afd-27f024835df1&spaceId=83c75a39-3aba-4ba4-a792-7aefe4b07895&width=1690&userId=&cache=v2"/>
  - **Leaf** - children이 더이상 없는 node
    <img width="{해상도 비율}" src="https://nextjs.org/_next/image?url=%2Fdocs%2Flight%2Fterminology-url-anatomy.png&w=3840&q=75&dpl=dpl_8JWihzhXtpkYanrPN4yhGPoiM2MX"/>

- **(2) URL 관련**
  - **URL Segment**
    - 슬래시(/)로 분류된 URL path의 한 부분
  - **URL Path**
    - 도메인(www.sample-web.com) 이후 따라오는 전체 URL 부분

## 03. 파일(폴더) 기반 라우팅

- **(1) page.tsx**
  이미 다뤄보았던 대로, **`page.tsx`**는 main ui가 표시될 곳, 우리는 보통 page.tsx 파일을 만들면서 코딩
  폴더(nesting folders)를 만들어가면서 우리는 routing이 자동 구현된다. 이 과정에서 해당 폴더의 **`page.tsx`** 파일이 읽혀지게 된다는 것을 기억하자
- **(2) static routing**
  src > app 폴더 밑에 test 폴더를 새로 만들고 그 안에 page.tsx 파일을 만들기

  ```tsx
  // src>app>test>page.tsx
  import React from "react";

  const TestPage = () => {
    return (
      <div>
        <h1>Test Page</h1>
        <p>안녕하세요! 테스트 페이지입니다</p>
      </div>
    );
  };

  export default TestPage;
  ```

  이제 브라우저에서 접근해본다면 (localhost:3000/test)
  폴더 기반의 라우팅이 완료

- **(3) dynamic routing**

  - (3)-3-1. React에서
    우리가 react-router-dom에서 특정 경로가 dynamic한 parameter에 의해 계속 변할 때는 어떻게 설정?

    ```jsx
    import React from "react";
    import { BrowserRouter, Route, Routes } from "react-router-dom";
    import Home from "../pages/Home";
    import About from "../pages/About";
    import Contact from "../pages/Contact";
    import Works from "../pages/Works";
    import Product from "../pages/Product";

    const Router = () => {
      return (
        <BrowserRouter>
          <Routes>
            <Route path="/" element={<Home />} />
            <Route path="/about" element={<About />} />
            <Route path="/contact" element={<Contact />} />
            <Route path="/works" element={<Works />} />
            **
            <Route path="/products/:id" element={<Product />} />
            **
          </Routes>
        </BrowserRouter>
      );
    };

    export default Router;
    ```

    **<Route path="/products/:id" element={<Product />} />**

  - (3)-3-2. Next에서는?

      <aside>
      ☝ **폴더 이름을 대괄호로 감싸기**
      
      </aside>
      
      이렇게 하게 되면, 패턴에 일치하는 모든 경로를 페이지로 연결하게 된다.
      
      > app/posts/[id]/page.tsx
      > 
      
      파일의 경우 **`/posts/1`** **`/posts/2`** 등 경로에 대해 동적으로 페이지를 생성하게 된다.

      <img width="{해상도 비율}" src="https://teamsparta.notion.site/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F83c75a39-3aba-4ba4-a792-7aefe4b07895%2Fe96ab7c3-b1d9-4abb-8c9c-be42e1f6f6fc%2FUntitled.png?table=block&id=c4bf9d6d-6bd5-489c-98cb-2cd36da49411&spaceId=83c75a39-3aba-4ba4-a792-7aefe4b07895&width=710&userId=&cache=v2"/>  
      
      
      Next.js 공식 문서에서는 이렇게 표현하고 있다.
      
      <aside>
      ☝ Each folder represents a **[route** segment](https://nextjs.org/docs/app/building-your-application/routing#route-segments) that maps to a **URL** segment. To create a [nested route](https://nextjs.org/docs/app/building-your-application/routing#nested-routes), you can nest folders inside each other.
      
      각 폴더는 URL segment에 대응되는 route입니다. 중첩 라우팅을 위해서는 폴더로 감싸주면 돼요!
      <img width="{해상도 비율}" src="https://teamsparta.notion.site/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F83c75a39-3aba-4ba4-a792-7aefe4b07895%2Fb47c63ce-8064-4269-b894-9ef16a82caa0%2FUntitled.png?table=block&id=7ceb4d62-d6e4-42e3-9293-547f21f34c3a&spaceId=83c75a39-3aba-4ba4-a792-7aefe4b07895&width=2000&userId=&cache=v2"/>  
      
      
      
      </aside>

- **(4) route groups**
  - (3)-4-1. 폴더 === 라우팅?
    <aside>
    ☝ **?? : 폴더만 만들면 routing에 포함된다니.. 그러고 싶지 않은데!**

    </aside>
    <img width="{해상도 비율}" src="https://teamsparta.notion.site/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F83c75a39-3aba-4ba4-a792-7aefe4b07895%2F9ccc4fd9-1b41-4bfb-af9d-5b52d6247843%2FUntitled.png?table=block&id=d7cb1a6a-d705-4810-9b02-5da23a4a63ea&spaceId=83c75a39-3aba-4ba4-a792-7aefe4b07895&width=2000&userId=&cache=v2"/>

## 04. 특별한 파일들

<aside>
✔️ Next.js에서는 Routing을 폴더 기반, 즉 Nested Folders로 구현을 하고 있다. 이 과정에서 어떤 라우트에 대한 특정 처리를 위해 생성되는 여러 **`speciall files`**가 있다.

특정 처리라는 것은 어떤 것이 있을까?

- 특정 경로 하위에 있는 routing은 모두 공통 layout을 적용하고 싶다.
- 특정 컴포넌트가 로딩중일 때, 오류가 발생했을 때 보여주고 싶은 UI가 있다.
</aside>

- **(1) layout**
  `layout`파일은 어떤 `segment`와 그의 자식 `node`에 있는 요소들이 공통적으로 적용받게 할 UI를 정의한다. 자식 `node`에 있는 요소들이 공통 적용을 받아야 하기 때문에 반드시 `children` prop이 존재해야 함을 기억
  동일 `layout` 안에서 다른 경로를 계속해서 왔다갔다 할 때 **re-rendering**이 일어나지 않는다. 따라서 `headers`, `footers`, `sidebars` 처럼 유저가 경로를 마음껏 탐색하고 다녀도 굳이 바뀔 필요가 없는 경우 유용! ← 이후 배울 `template`과의 주요 차이니 기억하기
  ☑️ React.js를 사용했을 때는 react-router-dom으로 이렇게 구현했다.
  - 코드
    ```jsx
    createBrowserRouter(
      createRoutesFromElements(
        <Route path="/" element={<Root />}>
          <Route path="contact" element={<Contact />} />
          <Route
            path="dashboard"
            element={<Dashboard />}
            loader={({ request }) =>
              fetch("/api/dashboard.json", {
                signal: request.signal,
              })
            }
          />
          <Route element={<AuthLayout />}>
            <Route path="login" element={<Login />} loader={redirectIfUser} />
            <Route path="logout" action={logoutUser} />
          </Route>
        </Route>
      )
    );
    ```
  ☑️ Next.js를 사용
  - 개념확인
    1. 특정 segment 이하의 route에서 적용받을 layout UI를 해당 폴더 안에 만듭니다.
       <img width="{해상도 비율}" src="https://teamsparta.notion.site/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F83c75a39-3aba-4ba4-a792-7aefe4b07895%2F1a21349f-a83d-4980-b146-1197534d0358%2FUntitled.png?table=block&id=cd0e86ba-ac39-4c6b-a168-bfef1b054457&spaceId=83c75a39-3aba-4ba4-a792-7aefe4b07895&width=2000&userId=&cache=v2"/>
    2. children을 포함시켜서 공통 UI를 만든다.

       ```jsx
       export default function DashboardLayout({
         children, // will be a page or nested layout
       }: {
         children: React.ReactNode,
       }) {
         return (
           <section>
             {/* Include shared UI here e.g. a header or sidebar */}
             <nav></nav>

             {children}
           </section>
         );
       }
       ```

    3. 우리는 만든 적이 없지만, Next.js 프로젝트를 생성하고 나면 이미 root 경로에 layout.tsx 파일이 존재한다.
       <img width="{해상도 비율}" src="https://teamsparta.notion.site/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F83c75a39-3aba-4ba4-a792-7aefe4b07895%2F3a4c8ad4-1e27-4aaa-9d31-33cd15a66888%2FUntitled.png?table=block&id=923905d9-c84b-4f5b-bfc8-5fccd35b399b&spaceId=83c75a39-3aba-4ba4-a792-7aefe4b07895&width=2000&userId=&cache=v2"/>

       공통 레이아웃이 변경되는지 확인

       ```jsx
       // src>app>layout.tsx
       import type { Metadata } from "next";
       import { Inter } from "next/font/google";
       import "./globals.css";

       const inter = Inter({ subsets: ["latin"] });

       export const metadata: Metadata = {
         title: "Create Next App",
         description: "Generated by create next app",
       };

       export default function RootLayout({
         children,
       }: Readonly<{
         children: React.ReactNode,
       }>) {
         return (
           <html lang="en">
             <body className={inter.className}>
               <nav>
                 <a href="/">Home</a>
                 <a href="/about">About</a>
                 <a href="/contact">Contact</a>
                 <a href="/blog">Blog</a>
               </nav>
               {children}
             </body>
           </html>
         );
       }
       ```
- **(2) template**
  template 파일은 방금 학습한 layout과 상당히 유사한 컴포넌트다.
  > 상태를 유지하는가? 리-렌더링이 일어나는가?
  경로 전반에 걸쳐서 상태가 유지되는 레이아웃과 달리, 템플릿은 라우팅을 탐색할 때 각 하위 항목에 대해 새 인스턴스를 만든다. 즉, User 입장에서 동일한 Template을 공유하는 경로 사이를 왔다갔다 할 때 DOM 요소가 다시 생성된다는 것을 의미한다.
  그래서 이런 use-case가 있다.
  1. 템플릿을 통한 페이지 open animation
     - 페이지 간 전환 시 애니메이션을 계속해서 주고 싶을 때
     - layout으로 만들어놓으면, 최초 렌더링시에만 animation이 적용되고 끝나버린다.
  2. useEffect, useState에 의존하는 기능
  - ☑️ layout vs template
    1. 다음 예시코드를 통해 layout과 template의 차이를 살펴보자
    2. 똑같은 코드르
    3. 코드

       ```jsx
       // src>app>text>layout.tsx
       // src>app>text>template.tsx
       "use client";

       import Link from "next/link";
       import React from "react";

       const TestTemplate = ({ children }: { children: React.ReactNode }) => {
         useEffect(() => {
           console.log("최초 렌더링 한 번만 호출합니다.");
         }, []);

         return (
           <div className="m-8 p-8 bg-white">
             <h1>테스트 페이지</h1>
             <p>테스트 경로 하위에서의 이동을 확인해봅니다.</p>
             <nav>
               <ul>
                 <li>
                   <Link href="/test">테스트 페이지</Link>
                 </li>
                 <li>
                   <Link href="/test/1">테스트 페이지 1</Link>
                 </li>
                 <li>
                   <Link href="/test/2">테스트 페이지 2</Link>
                 </li>
               </ul>
             </nav>
             {children}
           </div>
         );
       };

       export default TestTemplate;
       ```

       테스트페이지 → 테스트 페이지 1 → 테스트 페이지 2 를 계속 왔다갔다 해보기
       (브라우저에서 확인해봅시다)

       - layout을 사용하는 경우 : console.log가 최초 한 번만 호출
       - template을 사용하는 경우 : 페이지 이동 시 마다 계속해서 호출

       [결론] 우리는 특정한 이유가 있지 않는 한, layout.tsx를 사용하자
- **(3) not-found**
  react-router-dom에서는
  ```jsx
  <Router>
    <Routes>
      <Route path={"/home"} element={<Home />} />
      <Route path={"/*"} element={<h1>404: Not Found</h1>} />
    </Routes>
  </Router>
  ```
  next.js에서는 우리가 별도 설정을 하지 않아도 기본 스타일이 된 not found 페이지를 제공
  만일 이 스타일이 마음에 들지 않으면 직접 만들 수 있음
  ```jsx
  // src>app>not-found.tsx
  import React from "react";

  const NotFound = () => {
    return <div>존재하지 않는 페이지입니다.</div>;
  };

  export default NotFound;
  ```
- **(4) metadata와 SEO**

  Next.js는 기본적으로 Metadata를 가지고 있다.

    <img width="{해상도 비율}" src="https://teamsparta.notion.site/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F83c75a39-3aba-4ba4-a792-7aefe4b07895%2Ff2cdb360-f646-45b4-bc59-9bd4341836d4%2FUntitled.png?table=block&id=38e13bd4-4868-4e20-b860-d4898ac99acd&spaceId=83c75a39-3aba-4ba4-a792-7aefe4b07895&width=2000&userId=&cache=v2"/>

  Next.js는 향상된 SEO를 제공하기 위해서 기존 우리가 html head에 삽입했었던 많은 정보를 metadata는 객체 형태로 지원

  리액트에서는 vite를 쓰던 일반 CRA를 쓰던 index.html 파일이 존재했고, 이 파일의 head 태그에 여러분이 SEO 향상을 위해 여러 `meta`, `link` 태그 등 메타데이터를 작성했다.

    <img width="{해상도 비율}" src="https://teamsparta.notion.site/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F83c75a39-3aba-4ba4-a792-7aefe4b07895%2Fb0e44b40-0913-43e2-b77d-a48c39df841f%2FUntitled.png?table=block&id=135fed1f-4fd8-46b3-9866-d5f830040d4d&spaceId=83c75a39-3aba-4ba4-a792-7aefe4b07895&width=1560&userId=&cache=v2"/>
    
    
    
    
    
    - SEO
        
        <aside>
        ☝ SEO란?
        
        SEO(Search Engine Optimization, 검색 엔진 최적화)는 웹사이트나 웹페이지를 검색 엔진에서 더 높은 순위에 노출시키기 위한 방법이다. 간단하게 말해서, SEO는 검색 결과에서 웹사이트가 더 위에 나타나도록 만드는 여러 기술과 전략
        
        예를 들어, <img> 태그 내의 ‘alt’ 속성은 SEO를 위해 상당히 중요하다.
        
        ```html
        <img src="../assets/sample.jpg" />
        <img src="../assets/sample.jpg" alt="sample image" />
        ```
        
        1. **검색 엔진에 이미지 내용 설명하기**: **`alt`** 텍스트는 검색 엔진에 이미지의 내용과 맥락을 설명해 준다. 검색 엔진은 이미지 자체를 "보지" 못하므로, **`alt`** 텍스트를 통해 이미지가 무엇에 관한 것인지 이해하고, 관련 검색 쿼리에 대한 결과로 해당 이미지를 더 정확하게 랭킹할 수 있다.
        2. **접근성 향상**: **`alt`** 텍스트는 시각 장애가 있는 사용자가 웹사이트의 이미지 콘텐츠를 이해하는 데 도움을 준다. 이러한 접근성 개선은 검색 엔진에 의해 긍정적인 신호로 해석되어, 전반적인 사이트의 SEO 점수를 향상시킬 수 있다.
        </aside>
        
    
    그러나, Next.js에서는 Config-based Metadata를 활용할 수 있다. 두 가지 버전
    
    1. static
        - 원하는 `page.tsx` 또는 `layout.tsx` 어디든지
            
            ```tsx
            export const metadata: Metadata = {
            	title: "Sparta Next App",
            	description: "This is awesome Website",
            }
            ```
            
            를 삽입해놓기만 하면 적용이 된다.
            
        - 여기서 기억해야 할 것
            - metadata in page.tsx : 해당 page.tsx 컴포넌트에만 적용
            - metadata in layout.tsx : 해당 layout의 하위 요소에 모두 적용
        - 코드확인
            - page.tsx 케이스
                
                > 해당 페이지만 metadata의 적용을 받는다.
                > 
                
                ```tsx
                // src>app>page.tsx
                import { Metadata } from "next";
                
                export const metadata: Metadata = {
                  title: "Sparta Next App",
                  description: "This is awesome Website",
                };
                
                export default function Home() {
                  return <div>안녕하세요! 내배캠 리액트.. 아니아니 넥스트입니다!</div>;
                }
                ```
                
            - layout.tsx 케이스(기존 page.tsx의 metadata는 삭제)
                
                > 해당 layout의 하위 요소에 metadata 공통 적용
                > 
                
                ```tsx
                import type { Metadata } from "next";
                import { Inter } from "next/font/google";
                import "./globals.css";
                
                const inter = Inter({ subsets: ["latin"] });
                
                export const metadata: Metadata = {
                  title: "Sparta Next App",
                  description: "This is awesome Website",
                };
                
                export default function RootLayout({
                
                ...기존코드
                ```
                
    2. dynamic
        - dynamic route를 갖고있는 route에서 동적으로 변경되는 params를 기반으로 metadata를 변경하고 싶을 수도 있다. 이럴 땐, **`generateMetadata function`**을 사용하면 된다.
        - 코드확인
            
            ```tsx
            import React from "react";
            
            type Props = {
              params: {
                id: string;
              };
            };
            
            **export function generateMetadata({ params }: Props) {
              return {
                title: `Detail 페이지 : ${params.id}`,
                description: `Detail 페이지 : ${params.id}`,
              };
            }**
            
            const TestDetailPage = ({ params }: Props) => {
              return <div>Detail 페이지 : {params.id}</div>;
            };
            
            export default TestDetailPage;
            ```
            
            이처럼, 동적으로(dynamic) 들어온 params에 대해서도 metadata를 이용할 수 있다

