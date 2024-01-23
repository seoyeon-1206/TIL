## ****컴포넌트 스타일링****

### **컴포넌트 파일에서 CSS 코드 분리하기**

JSX는 사실 HTML과 굉장히 닮아 있기 때문에 방법이 크게 다르지 않다. 

단, 한가지 차이점이 있다면 `class` → `className` 을 사용한다는 점 

```jsx
import React from "react";

function App() {
  const style = {
    padding: "100px",
    display: "flex",
    gap: "12px",
  };

  const users = [
    { id: 1, age: 30, name: "송중기" },
    { id: 2, age: 24, name: "송강" },
    { id: 3, age: 21, name: "김유정" },
    { id: 4, age: 29, name: "구교환" },
  ];

  return (
    <div style={style}>
      {users.map((user) => {
        return <Square user={user} key={user.id} />;
      })}
    </div>
  );
}

export default App;
```

먼저, 컴포넌트 파일에서 `className` 을 넣어준다. 그리고 이 컴포넌트에서 적용할 CSS 파일을 import 해주자

```jsx
// src/App.js
import React from "react";
import Square from "./components/Square.js";
import "./App.css"; // 🔥 반드시 App.css 파일을 import 해줘야 한다.

function App() {
  const users = [
    { id: 1, age: 30, name: "송중기" },
    { id: 2, age: 24, name: "송강" },
    { id: 3, age: 21, name: "김유정" },
    { id: 4, age: 29, name: "구교환" },
  ];

  return (
    <div className="app-style">
      {users.map((user) => {
        return <Square user={user} key={user.id} />;
      })}
    </div>
  );
}

export default App;
```

❗️기존에는 style을 자바스크립트 객체로 작성했었기 때문에 CSS 프로퍼티의 값을 따옴표(””)로 감싸주었지만 CSS 문법에서는 따옴표를 다 지워주기

```jsx
<!--src/App.css-->
.app-style {
  padding: 100px;
  display: flex;
  gap: 12px;
}
```