## children이란?

### **컴포넌트에서 말하는 children은 무엇을 의미할까?**

자식 컴포넌트로 정보를 전달하는 또 다른 방법이다. 얘도 props, 맞음!

### **코드 예시**

App 컴포넌트가 User 컴포넌트를 자식으로 품고 있다. 그리고 `User` 컴포넌트 사이에 **‘안녕하세요'** 라는 문장이 있다. 근데 `yarn start` 를 해서 화면을 보면 아무것도 화면에 표시되지 않는다.

```tsx
// src/App.js

import React from "react";

function User() {
  return <div></div>;
}

function App() {
  return <User>안녕하세요</User>;
}
export default App;
```

Why? **`App` 컴포넌트에서 ‘안녕하세요' 라는 정보를 보냈지만, `User` 컴포넌트에서는 그 정보를 받지 않았기 때문이다.**

위 예시에서는  `<User>안녕하세요</User>` 이렇게 정보를 보내고 있다. `<User hello='안녕하세요'>` 이렇게 props를 보내던 방식과는 조금 다르다. → **이것이 children props를 보내는 방식!!**.  

자식 컴포넌트에서 정보를 받는 방식 : 기존과 동일하지만 이름이 `children` 으로 정해져 있다. 

```tsx
function User(props) {
	console.log(props.children)
  return <div></div>;
}
```

## children 값 받아서 렌더링하기

코드 수정 : `User` 컴포넌트에서 `props.children`을 받아 그대로 렌더링 해준 모습이다.

이제 화면에 정상적으로 ‘안녕하세요’ 라는 문장이 보임

```tsx
import React from "react";

function User(props) {
  return <div>{props.children}</div>;
}

function App() {
  return <User>안녕하세요</User>;
}
export default App;
```

## children의 용도
### **Layout 컴포넌트를 만들 때 자주 사용**

`Layout` 컴포넌트 안에는 `header` 라는 컴포넌트가 있고, `header` 아래에 `{props.children}` 를 통해서  props를 받아 렌더링 하고 있다. 즉,  `Layout` 컴포넌트가 쓰여지는 모든 곳에서 `<Layout>…</Layout>` 안에 있는 정보를 받아서 가져올 수 있는 것!

```bash
// src/About.js

import React from "react";
import Layout from "./components/Layout";

function App() {
  return (

    <Layout> 
      <div>여긴 App의 컨텐츠가 들어갑니다.</div>
    </Layout>
  );
}
export default App;
```

이 코드를 통해, `Layout`에 있는 `header`가 보여지게 되고, **“여긴 App의 컨텐츠가 들어갑니다.”** 라는 문장이 `Layout`의 `props`로 전달되는 것이다. 결과적으로 우리는 `header` 컴포넌트를 `Layout` 컴포넌트에서 한번만 작성하면 여러 페이지에서 모두 보여지게 할 수 있는 것이다.

`Layout` 컴포넌트를 `About` 컴포넌트에 또 사용한 예시

```bash
// src/About.js

import React from "react";
import Layout from "./components/Layout";

function About() {
  return (
    <Layout> 
      <div>여긴 About의 컨텐츠가 들어갑니다.</div>
    </Layout>
  );
}
export default About;
```