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
    

