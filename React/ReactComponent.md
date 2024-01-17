## React Component란 무엇일까요?

### **컴포넌트 개념 이해하기**

> 컴포넌트를 통해 UI를 재사용이 가능한 개별적인 여러 조각으로 나누고, 각 조각을 개별적으로 살펴볼 수 있다.
개념적으로 컴포넌트는 JavaScript 함수와 유사
“props”라고 하는 임의의 입력을 받은 후, 화면에 어떻게 표시되는지를 기술하는 React 엘리먼트를 반환한다.
> 

### **리액트 컴포넌트를 표현하는 두 가지 방법**

1. 함수형 컴포넌트
    
    ```tsx
    // props라는 입력을 받음
    // 화면에 어떻게 표현되는지를 기술하는 React 엘리먼츠를 반환(return)
    
    function Welcome(props) {
      return <h1>Hello, {props.name}</h1>;
    }
    // 쉬운 표현
    function App () {
    	return <div>hello</div>
    }
    ```
    
2. 클래스형 컴포넌트 → 사용X
    
    ```tsx
    class Welcome extends React.Component {
      render() {
        return <h1>Hello, {this.props.name}</h1>;
      }
    }
    ```
    

<aside>
👉 결론적으로 리액트 세계에서 말하는 컴포넌트(블럭)는 즉 `함수` `html`을 `return` 하는 함수를 만들면 된다.

</aside>

## 우리가 만든 CRA 프로젝트 살펴보기

### **코드 살펴보기**

```tsx
import logo from './logo.svg';
import './App.css';

function App() {
  return (
    <div className="App">
      <header className="App-header">
        <img src={logo} className="App-logo" alt="logo" />
        <p>
          Edit <code>src/App.js</code> and save to reload.
        </p>
        <a
          className="App-link"
          href="https://reactjs.org"
          target="_blank"
          rel="noopener noreferrer"
        >
          Learn React
        </a>
      </header>
    </div>
  );
}

export default App;
```

### **Component 보는 방법**

- 영역을 나누어서 보기
- import, export default 코드
- 컴포넌트 안에서는 자바스크립트를 쓸 수 있는 부분이 있음, 어떤 자바스크립트 코드를 작성하고 싶다면 여기에다가 작성
- `return` 을 기준으로 아랫부분에서는JSX을 작성

### **주의**

- 컴포넌트를 만들 때 반드시 가장 첫 글자는 **대문자**
- 폴더는 소문자로 시작하는 카멜케이스
- 컴포넌트를 만드는 파일은 대문자로 시작하는 카멜케이스

## 부모-자식 컴포넌트

### **컴포넌트 안에 컴포넌트 넣기**

다른 컴포넌트를 품는 컴포넌트를 `부모 컴포넌트`

다른 컴포넌트 안에서 품어지는 컴포넌트를 `자식 컴포넌트`

```tsx
// src/App.js
import React from "react";

function Child() { //자식 컴포넌트
  return <div>나는 자식입니다.</div>;
}

function App() { //부모 컴포넌트
  return <Child />;
}

export default App;
```