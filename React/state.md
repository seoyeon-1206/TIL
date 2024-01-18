## State

### **State란 무엇인가?**

<aside>
💡 State란 컴포넌트 내부에서 바뀔 수 있는 값


목적: UI(엘리먼트)로의 반영

</aside>

### **State 만들기**

<aside>
💡 State를 만들 때는 useState()를 사용한다.

</aside>

GrandFather 컴포넌트에서 기존에 있었던 `const name = “김할아”` 라는 코드가 `const [name, setName] = useState("김할아");` 라는 코드로 바뀜

```jsx
import React, { useState } from 'react';

function GrandFather() {
  const [name, setName] = useState("김할아"); // 이것이 state!
  return <Mother grandFatherName={name} />;
}

// .. 중략 
```

`useState` 라는 함수를 이용해서 state를 만든다. 

`useState` 는 state를 만들어주는 리액트에서 제공하는 기능 → 리액트에만 존재하는 개념이자 기능

`“기능”` 이라고 하지 않고 `“훅”` 이라고 표현하겠음

### **useState 훅을 사용하는 방식**

```jsx
const [ value, setValue ] = useState( 초기값 )
```

먼저 `const` 로 선언을 하고 `[ ] 빈 배열` 을 생성하고, 배열의 첫번째 자리에는 이 state의 이름, 그리고 두번째 자리에는 `set` 을 붙이고 state의 이름을 붙인다. 그리고 `useState( )` 의 인자에는 이 state의 `원하는 처음값` 을 넣어준다. → initial state (언제든지 변할 수 있는 값이기 때문에 `처음값` 이라는 개념이 존재)

### **State 변경하기**

<aside>
💡 state를 변경할때는 `setValue(바꾸고 싶은 값)` 를 사용한다.

</aside>

`setName(”박할아”)` 를 하면 이름이 바뀐다.  

버튼을 눌렀을 때 `setName("박할아")` 를 실행

```jsx
// src/App.js

import React, { useState } from "react";

function Child(props) {
  return (
    <div>
      <button
        onClick={() => {
          **props.setName("박할아");**
        }}
      >
        할아버지 이름 바꾸기
      </button>
      <div>{props.grandFatherName}</div>
    </div>
  );
}

function Mother(props) {
  return (
    <Child grandFatherName={props.grandFatherName} setName={props.setName} />
  );
}

function GrandFather() {
  const [name, setName] = useState("김할아");
  return <Mother grandFatherName={name} setName={setName} />;
}

function App() {
  return <GrandFather />;
}

export default App;
```

일단 Child 컴포넌트에 `할아버지 이름 바꾸기` 라는 라벨을 가진 버튼을 만들어줌

```jsx
// src/App.js

//.. 중략

function Child(props) {
  return (
    <div>
      <button>할아버지 이름 바꾸기</button>
      <div>{props.grandFatherName}</div>
    </div>
  );
}

//.. 중략
```

그리고 이 버튼을 눌렀을 때 `setName` 을 실행 할 수 있도록 준비

```jsx
// src/App.js

//.. 중략

function Child(props) {
  return (
    <div>
      <button
        onClick={() => {
					// 여기에서 setName()을 실행
        }}
      >
        할아버지 이름 바꾸기
      </button>
      <div>{props.grandFatherName}</div>
    </div>
  );
}

//.. 중략
```

 GrandFather에 있는 setName을 Child에게 전달을 해줘야 함. 주고 받고, 주고 받고해서 전달해준다.

```jsx
// src/App.js

import React, { useState } from "react";

function Child(props) {
  return (
    <div>
      <button
        onClick={() => {
					props.setName("박할아"); // 드디어 받은 setName을 실행
        }}
      >
        할아버지 이름 바꾸기
      </button>
      <div>{props.grandFatherName}</div>
    </div>
  );
}

function Mother(props) {
  return (
    <Child grandFatherName={props.grandFatherName} setName={props.setName} /> // 받아서 다시 주고
  );
}

function GrandFather() {
  const [name, setName] = useState("김할아");
  return <Mother grandFatherName={name} setName={setName} />; // 주고
}

function App() {
  return <GrandFather />;
}

export default App;
```

하지만 이렇게 바뀐 값은 브라우저를 새로고침하면 다시 초기값으로 바뀜. `setName`을 통해서 바꾼 값은 어디에 저장되는 것이 아니기 때문에 단순히 화면에서만 바뀐 값으로 다시 렌더링이 된다!

## (응용1) useState + onClick Event

- **(1) Button과 이벤트 핸들러 구현하기**
    
    `onClickHandler` 라는 함수를 만들고 `onClick={}` 에 넣어줌 → **함수와 컴포넌트(button 태그)를 연결**시킴
    
    이 함수를 `이벤트 핸들러` 라고 표현한다. 
    
    ```jsx
    import React from "react";
    
    function App() {
      // 버튼을 눌렀을 때 하고 싶은 행동 
      function onClickHandler() {
        console.log("hello button");
      }
      return (
        <div>
          <button onClick={onClickHandler}>버튼</button>
        </div>
      );
    }
    
    export default App;
    ```
    
- **(2) state 구현하고 이벤트 핸들러와 연결하기**
    
    우선 `state`를 하나 만들고 이 버튼을 클릭을 했을 때 `state` 값을 바꿔보자. 이벤트 핸들러를 만들어주고 그 안에 `setName` 을 넣어준다.
    
    버튼을 누르면  `setName()`안에 있는 값이 `“누렁이”`니까, `state`가 `“길동이"`에서 `“누렁이”`로 바뀜
    
    ```jsx
    import React, { useState } from "react";
    
    function App() {
      const [name, setName] = useState("길동이");
    
      function onClickHandler() {
        setName("누렁이");
      }
    
      return (
        <div>
          {name}
          <button onClick={onClickHandler}>버튼</button>
        </div>
      );
    }
    
    export default App;
    ```
    

## (응용2) useState + onChange Event

- **(1) Input과 state 구현하기**
    
     버튼과 자주 사용하는 input!
    
    input에서는 보통 사용자가 입력한 값을 state로 관리하는 패턴을 많이 사용한다. 
    
    ```jsx
    import React, { useState } from "react";
    
    const App = () => {
      const [value, setValue] = useState("");
    
      return (
        <div>
          <input type="text" />
        </div>
      );
    };
    
    export default App; 
    ```
    
    input과 useState를 사용해서 input의 값을 넣을 value라는 state를 생성했다. 
    
    <aside>
    💡 화살표 함수, function 키워드 모두 똑같이 함수 컴포넌트를 만들 수 있음
    
    </aside>
    
- **(2) 이벤트핸들러를 구현하고 state와 연결하기**
    
    input과 생성한 state(value)를 연결해보자
    
    먼저 input에 onChange라는 이벤트를 불러내고, 생성한 이벤트 핸들러 함수를 넣는다. 
    
    ```jsx
    import React, { useState } from "react";
    
    const App = () => {
      const [value, setValue] = useState("");
    
      const onChangeHandler = (event) => {
        const inputValue = event.target.value;
        setValue(inputValue);
      };
    
    	console.log(value) // value가 변함
    
      return (
        <div>
          <input type="text" onChange={onChangeHandler} value={value} />
        </div>
      );
    };
    
    export default App;
    ```
    
    그리고 **우리는 이벤트 핸들러 안에서 자바스크립트의 event 객체를 꺼내 사용!!**
    
    사용자가 입력한 **input의 값은 event.target.value** 로 꺼내 사용할 수 있.
    
    마지막으로 state인 **value를 input의 attribute인 value에 넣어주**면 input과 state 연결하기, 끝 
    
- **(3) 정리**
    
    **사용자가 input에 어떤 값을 입력**하면, **그 값을 입력할 때마다, 같은 말로 onChange될 때마다 value라는 state에 setValue해서 넣어주는 것** 
    
    **이러한 과정을 통해 우리는 사용자의 input값을 state로 관리할 수 있는 것**.  참고로 위에서 사용한 이벤트 객체는 리액트의 개념이 아님. HTML DOM event 의 개념! 
    

## 실습

```jsx
import React, { useState } from 'react'

function App() {
  const [id, setId] = useState('');
  const [password, setPassword] = useState('');

  console.log(id);
  console.log(password);

  // id 필드가 변경될 경우
  const onIdChangeHandler = (event) => {
    setId(event.target.value);
  }
  // 비밀번호 필드가 변경될 경우
  const onPwChangeHandler = (event) => {
    setPassword(event.target.value);
  }

  return (
    <div>
      <div>
        아이디: <input type='text' value={id} onChange={onIdChangeHandler}/>
      </div>
      <div>
        비밀번호: <input type='password' value={password} onChange={onPwChangeHandler}/>
      </div>
      <button
        onClick={()=> {
          alert(`아이디${id}이고 비번은 ${password}`);
          //초기화
          setId('');
          setPassword('');
        }}>
          로그인</button>
    </div>
  )
}

export default App;
```

## 불변성
 (1) 불변성이란?
    
    불변성이란 메모리에 있는 값을 변경할 수 없는 것
    
    자바스크립트의 데이터 형태중에 원시 데이터는 불변성이 있고, 원시 데이터가 아닌 객체, 배열, 함수는 불변성 없다.
    
    원시 데이터 : 새로운 공간 / 원시데이터 아닌 : 기존 저장된 값 바꿈
    
(2) 변수를 저장하면, 메모리에 어떻게 저장이 될까?
    
    만약 우리가 `let number = 1`  이라고 선언을 하면, 메모리에는 1 이라는 값이 저장된다. 그리고 `number` 라는 변수는 메모리에 있는 1을 참조한다. 그리고 이어서 우리가 `let secondNumber = 1` 이라고 다른 변수를 선언을 하면 이때도 자바스크립트는 이미 메모리에 생성되어 있는 1이라는 값을 참조한다.
    
    즉, number와 secondNumber는 변수의 이름은 다르지만, 같은 메모리의 값을 바라보고 있는 것. 그래서 우리가 콘솔에 `number === secondNumber` 를 하면 `true`가 보임
    
    하지만 원시데이터가 아닌 값(객체, 배열, 함수)는 이렇지 않음.
    
    `let obj_1 = {name: ‘kim’}` 이라는 값을 선언하면 메모리에 obj_1이 저장이 된다. 그리고 이어서 `let obj_2 = {name: ‘kim’}` 이라고 같은 값을 선언하면 **obj_2라는 메모리 공간에 새롭게 저장이 된다다. 그래서 `obj_1 === obj2` 는 `false`  
    
(3) 데이터를 수정하면 어떻게 될까?
    
    다시 원시데이터로 돌아와서 만약에 기존에 1이던 number를 `number = 2` 라고 새로운 값을 할당하면 메모리에서는 어떻게 될까? **원시 데이터는 불변성이 있다. 즉, 기존 메모리에 저장이 되어 있는 1이라는 값이 변하지 않고, 새로운 메모리 저장공간에 2가 생기고 number라는 값을 새로운 메모리 공간에 저장된 2를 참조하게 된다.  그래서 secondNumber를 콘솔에 찍으면 여전히 1이라고 콘솔에 보인다. number와 secondNumber는 각각 다른 메모리 저장공간을 참조하고 있기 때문.
    
    obj_1를 수정→ `obj_1.name = ‘park’` 이라고 새로운 값을 할당? 
    
    객체는 불변성이 없다. 그래서 기존 메모리 저장공간에 있는 `{name: ‘kim’}` 이라는 값이 `{name : ‘park’}` 으로 바뀌어 버린다.
    
    - 원시데이터는 수정을 했을 때 메모리에 저장된 값 자체는 바꿀 수 없고, 새로운 메모리 저장공간에 새로운 값을 저장한다.
    - 원시데이터가 아닌 데이터는 수정했을 때 기존에 저장되어 있던 메모리 저장공간의 값 자체를 바꿔버린다.

(4) 왜 리액트에서는 원시데이터가 아닌 데이터의 불변성을 지켜주는 것을 중요시할까?
    
    리액트는 화면을 렌더링할지를 state의 변화에 따라 결정한다!
    
    단순 변수는 무시한다!
    
    state가 변했는지 변하지 않았는지 확인하는 방법:  state의 변화 전, 후의 `메모리 주소`를 비교
    
    만약 리액트에서 원시데이터가 아닌 데이터를 수정할 때 불변성을 지켜주지 않고, 직접 수정을 가하면 값은 바뀌지만 메모리주소는 변함이 없게 된다.
    
    개발자가 값은 바꿨지만 리액트는 state가 변했다고 인지하지 못하게 된다. →리렌더링 X
    
(5) 리액트 불변성 지키기 예시
    배열을  setState 할 때 불변성을 지켜주기 위해, 직접 수정을 가하지 않고 전개 연산자를 사용해서 기존의 값을 복사하고, 그 이후에 값을 수정하는 식으로 구현한다.

- spread 문법, map, filter

```jsx
import React, { useState } from "react";

function App() {
  const [dogs, setDogs] = useState(["말티즈"]);

  function onClickHandler() {
		// spread operator(전개 연산자)를 이용해서 dogs를 복사합니다. 
	  // 그리고 나서 항목을 추가합니다.
    setDogs([...dogs, "시고르자브르종"]);
  }

  console.log(dogs);
  return (
    <div>
      <button onClick={onClickHandler}>버튼</button>
    </div>
  );
}

export default App;
```