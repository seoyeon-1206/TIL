## 구조분해 할당과 Props

### **구조 분해 할당 통해 props 내부값 추출하기**

지금까지 자식 컴포넌트에서 props를 받을 때 이렇게 함

```jsx
function Todo(props){
	return <div>{props.todo}</div>
}
```

문제는 없지만 **todo 라는 props를 사용하는 모든 곳에서 `props.` 를 붙여줘야만 했다.** 

→ 자바스크립트의 [구조 분해 할당](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)을 이용하면 짧아짐 

**props는 object literal 형태의 데이터라서** [구조 분해 할당](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)을 이용할 수 있다.

```jsx
function Todo({ title }){
	return <div>{title}</div>
}
```

만약 여러개의 props를 받는다면, `{ }` 안에 여러개의 props를 그대로 써주면 된다.

```jsx
function Todo({ title, body, isDone, id }){
	return <div>{title}</div>
}
```

## defaultProps

### **defaultProps란 무엇인가?**

<aside>
💡 defaultProps란, 부모 컴포넌트에서 props를 보내주지 않아도 설정될 초기 값

</aside>

**자주 받거나 또는 무조건 받아야 하는 props들**이 있다. 

ex) 나이를 props로 받아 화면에 렌더링 하는 컴포넌트

```jsx
// components/Child.js

import React from 'react';

function Child({ name }){
	return <div>내 이름은 {name} 입니다. </div>
}

export default Child
```

Child 컴포넌트 입장에서는 부모 컴포넌트에서 `name`을 `props` 정보를 받기 전까지는 `name` 이 없는 상태

→ **자식 컴포넌트 입장에서는 name이 무엇인지 알 수 없다.** → 자식컴포넌트는 화면에 아무것도 표시해주지 못함. ( `내 이름은 "" 입니다.` )

그래서 부모 컴포넌트에서 `props`를 받기전까지 임시로 사용 할 수 있는 `props`를  `Child` 컴포넌트에서 직접 설정 할 수 있다.

이후에 **부모 컴포넌트에서 name props가 오게되면 설정된 defaultProps는 사라지고 내려 받은 props로 값이 바뀌**게 된다.

### **default props 지정하기(자주 쓰진 않는다)**

```jsx
// components/Child.js

import React from 'react';

function Child({ name }){
	return <div>내 이름은 {name} 입니다. </div>
}

// 이렇게 설정한다.
Child.defaultProps={
	name: '기본 이름'
}

export default Child
```

아래 코드처럼도 할 수 있는데, 방법만 다를 뿐 모두 `defaultProps`를 설정하는 방법 이다. 마치 함수의 `[default argument](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Functions/Default_parameters)`를 설정하는 것과 비슷?

## Default Argument

매개변수가 지정되지 않았으면 자동으로 지정해줄 값을 정하라는 의미

```jsx
function multiply(a, b = 1) {
  return a * b;
}

console.log(multiply(5, 2));
// Expected output: 10

console.log(multiply(5));
// Expected output: 5
```