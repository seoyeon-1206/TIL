## props란 무엇일까요?

### 컴포넌트 끼리의 정보교환 방식!

- 부모 컴포넌트가 자식 컴포넌트에게 물려준 데이터
- **컴포넌트 간의 정보 교류 방법**
1. props는 반드시 위에서 아래 방향으로 흐른다. 즉, **[부모] → [자식] 방향**으로만 흐른다(단방향).
2. props는 **반드시 읽기 전용으로 취급하며, 변경하지 않는다.**

### 코드를 통해 살펴보기

```tsx
// src/App.js

import React from "react";

function App() {
  return <GrandFather />;
}

function GrandFather() {
  return <Mother />;
}

function Mother() {
	const name = '홍부인';
  return <Child />;
}

function Child() {
  return <div>연결 성공</div>;
}

export default App;

```

## **props로 값 전달하기**

### (1) 전달하기 - [주체 : 부모]

- **컴포넌트 간의 정보를 교류할 때 `Props`를 사용**
- `Mother` 컴포넌트가 가지고 있는 정보(값)를 `Child`에게 주고 싶을 때는 아래 코드와 같이 한다.
- `motherName`이라는 이름으로 `name` 값을 `Child` 컴포넌트에게 전달해준 것
- **이 과정을 “Props 로 정보를 전달했다” 라고 표현한다.**

```jsx
// src/App.js

import React from "react";

function App() {
  return <GrandFather />;
}

function GrandFather() {
  return <Mother />;
}

function Mother() {
	const name = '홍부인';
  return <Child motherName={name} />; // 💡"props로 name을 전달했다."
}

function Child() {
  return <div>연결 성공</div>;
}

export default App;

```

### (2) 받기 - [주체 : 자식]

그러면, `Mother`가 전달해준  `motherName`은 `Child`가 어떻게 받을 수 있을까?

```jsx
function Child(props){
	console.log(props) // 이게 바로 props다.
	return <div>연결 성공</div>
}
```

- {motherName: “홍부인”} 객체 형태로
- `props`란 결국 부모 컴포넌트가 자식에게 넘겨준 **데이터들의 묶음**

## props로 받은 값을 화면에 렌더링 하기

### 코드로 살펴보기

JSX에서도 `{ }` 중괄호를 사용하면 자바스크립트 코드를 사용할 수 있다

```jsx
import React from "react";

// div안에서 { } 를 쓰고 props.motherName을 넣는다.
function Child(props) {
  return <div>{props.motherName}</div>;
}

function Mother() {
  const name = "홍부인";
  return <Child motherName={name} />;
}

function GrandFather() {
  return <Mother />;
}

function App() {
  return <GrandFather />;
}

export default App;
```

`props`는 `object literal`  형태이기 때문에 `{props.motherName}` 로 꺼내서 사용할 수 있음

`object literal`의 `key`가 `motherName` 인 이유는 Child로 보내줄 때 `motherName={name}` 으로 보내주었기 때문

<aside>
❓ `object literal` 란 `{key: “value”}` 데이터 형태를 의미한다. [더 자세한 설명](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Object_initializer)

</aside>

<aside>
💡 혹시 자식 컴포넌트에서는 부모 컴포넌트로 props를 전달 할 수 없나요?
[답변] : 자식 컴포넌트에서는 부모 컴포넌트로 props를 전달할 수 없습니다. 오직 부모 컴포넌트에서 자식 컴포넌트로만 props를 전달할 수 있습니다!!!

</aside>

## prop drilling

<aside>
💡 [부모] → [자식] → [그 자식] → [그 자식의 자식] 이 데이터를 받기 위해선 무려 3번이나 데이터를 내려줘야  함 → prop drilling, props가 아래로 뚫고 내려간다.

</aside>

아래 예시와 같은 패턴 → Redux와 같은 데이터 상태관리 툴을 배워야함

```bash
export default function App() {
  return (
    <div className="App">
      <FirstComponent content="Who needs me?" />
    </div>
  );
}

function FirstComponent({ content }) {
  return (
    <div>
      <h3>I am the first component</h3>;
      <SecondComponent content={content} />|
    </div>
  );
}

function SecondComponent({ content }) {
  return (
    <div>
      <h3>I am the second component</h3>;
      <ThirdComponent content={content} />
    </div>
  );
}

function ThirdComponent({ content }) {
  return (
    <div>
      <h3>I am the third component</h3>;
      <ComponentNeedingProps content={content} />
    </div>
  );
}

function ComponentNeedingProps({ content }) {
  return <h3>{content}</h3>;
}
```