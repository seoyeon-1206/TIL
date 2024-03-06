## 1. 함수에서 사용법

```jsx
function sum(a: number, b: number): number {
  return a + b;
}

function objSum({ a, b }: { a: number, b: number }): string {
  return `${a + b}`;
}
```

---

## 2. 비동기 함수에서 사용법

1. Promise 제네릭으로 감싸주기

   ```jsx
   type Person = { id: number, age: number, height: number };

   async function getPerson(): Promise<Person[]> {
     const res = await fetch(`http://localhost:5008/people`);
     if (!res.ok) {
       throw new Error();
     }
     return res.json();
   }

   getPerson().then((res) => console.log(res[0]));
   ```

2. as any as Person[]

---

## 3. TS 꿀팁

```jsx
npx create-react-app nameofyourapp --template typescript
```

---

## 4. Generic `<T>`에 대한 아주 간단한 이해

타입을 변수화한다.

```jsx
// 제네릭(generic)이란 데이터의 타입(data type)을 일반화한다(generalize)는 것을 의미
type Generic<T> = {
  someValue: T,
};

type Test = Generic<string>; //someValue는 type이 string

function someFunc<T>(value: T) {}

someFunc < string > "hello";
```

---

## 5. useState 에서 사용하기

```jsx
import { useState } from "react";

function App() {
  const [counter, setCounter] = useState < number > 1;
  const increment = () => {
    setCounter((prev) => prev++);
  };
  return <div onClick={increment}>{counter}</div>;
}

export default App;
```

---

## 6. Passing Props

- 예제1
  ```jsx
  import { useState } from "react";

  function Parent() {
    const [count, setCount] = useState(""); //초기값 채우기
    return <Child count={count}></Child>;
  }

  function Child({ count }: **{count:string}**) {
    return <div>{count}</div>;
  }

  export default Parent;
  ```
  1. 타입 분리
  ```jsx
  import { useState } from "react";

  function Parent() {
    const [count, setCount] = useState("");
    return <Child count={count}></Child>;
  }

  **type Props = {
    count: string;
  };**

  function Child({ count }: **Props**) {
    return <div>{count}</div>;
  }

  export default Parent;
  ```
- setTodos
  - 복붙
    ```jsx
    import { useState } from "react";

    type Todo = {
      id: string;
      isDone: boolean;
    };

    function App() {
      const [todos, setTodos] = useState<Todo[]>([]);
      return (
        <>
          {todos.map(({ id }) => (
            <Todo key={id} id={id} setTodos={setTodos} />
          ))}
        </>
      );
    }

    function Todo({
      id,
      setTodos,
    }: {
      id: string;
      **setTodos: React.Dispatch<React.SetStateAction<Todo[]>>;** //복붙
    }) {
      const deleteTodo = () => {
        setTodos((prev) => prev.filter((todo) => todo.id !== id));
      };

      return <div onClick={deleteTodo}></div>;
    }

    export default App;
    ```
  - 두 번 째 방법의 과정(1)
    ```jsx
    import { useState } from "react";

    type Todo = {
      id: string;
      isDone: boolean;
    };

    function App() {
      const [todos, setTodos] = useState<Todo[]>([]);
      return (
        <>
          {todos.map(({ id }) => (
            <Todo key={id} id={id} setTodos={setTodos} />
          ))}
        </>
      );
    }

    function Todo({ id, setTodos }: { id: string; **setTodos: () => void }**) {
      const deleteTodo = () => {
        setTodos((prev) => prev.filter((todo) => todo.id !== id)); //인자인 콜백함수를 setTodos 옆 괄호에 넣어주기
      };

      return <div onClick={deleteTodo}></div>;
    }

    export default App;
    ```
    두 번째 방법의 과정(2)
    ```jsx
    import { useState } from "react";

    type Todo = {
      id: string;
      isDone: boolean;
    };

    function App() {
      const [todos, setTodos] = useState<Todo[]>([]);
      return (
        <>
          {todos.map(({ id }) => (
            <Todo key={id} id={id} setTodos={setTodos} />
          ))}
        </>
      );
    }

    function Todo({ id, setTodos }: { id: string; setTodos: **(cb: (todo: Todo[])=> Todo[])** => void;
    }) {
      const deleteTodo = () => {
        setTodos((prev) => prev.filter((todo) => todo.id !== id)); //인자인 콜백함수를 setTodos 옆 괄호에 넣어주기
      };

      return <div onClick={deleteTodo}></div>;
    }

    export default App;
    ```
    좀 더 간단하게
    ```jsx
    import { useState } from "react";

    type Todo = {
      id: string;
      isDone: boolean;
    };

    function App() {
      const [todos, setTodos] = useState<Todo[]>([]);
      return (
        <>
          {todos.map(({ id }) => (
            <Todo key={id} id={id} todos={todos} setTodos={setTodos} />
          ))}
        </>
      );
    }

    function Todo({
      id,
      todos,
      **setTodos,**
    }: {
      id: string;
      todos: Todo[];
      setTodos: **(todoList: Todo[]**) => void;
    }) {
      const deleteTodo = () => {
        **const newTodos = todos.filter((todo) => todo.id === id);
        setTodos(newTodos);**
      };

      return <div onClick={deleteTodo}></div>;
    }

    export default App;
    ```
- 부모 컴포넌트에서 만든 함수를 자식에 전달
  ```jsx
  import { useState } from "react";

  type Todo = {
    id: string;
    isDone: boolean;
  };

  function App() {
    const [todos, setTodos] = useState<Todo[]>([]);

    **const deleteTodo = (id: string) => {
      const newTodos = todos.filter((todo) => todo.id === id);
      setTodos(newTodos);
    };**
    return (
      <>
        {todos.map(({ id }) => (
          <Todo key={id} id={id} **deleteTodo={deleteTodo}** />
        ))}
      </>
    );
  }

  function Todo({
    id,
    **deleteTodo,**
  }: {
    id: string;
    **deleteTodo: (id: string) => void;**
  }) {
    const handleOnClick = () => {
      deleteTodo(id);
    };
    return <div onClick={handleOnClick}></div>;
  }

  export default App;
  ```

---

## 7. Children Props

1. 리액트 FC 사용한 방법 (암시적인 문제)

   ```jsx
   type BaseType = {
     id: string;
   };

   //React 18버전 이전
   const Child: React.**FC<BaseType>** = ({ id }) => {
     return <div>{id}</div>;
   };

   export function Parent() {
     return (
       <Child id="">
         <div>has children</div>
       </Child>
     );
   }
   ```

2. PropsWithChildren (옵셔널로 가지는 문제)

   ```tsx
   import { PropsWithChildren } from "react";

   type BaseType = {
     id: string;
   };

   function Child({ children }: **PropsWithChildren<BaseType>)** {
     return <div>{children}</div>;
   }

   export function Parent() {
     return <Child id=""></Child>;
   }
   ```

3. ReactNode

   ```tsx
   import { ReactNode } from "react";

   type BaseType = {
     id: string;
   };

   type StrictChildren<T> = **T & { children: ReactNode };**

   function Child({ children }: StrictChildren<BaseType>) {
     return <div>{children}</div>;
   }

   export function Parent() {
     return (
       <Child id="">
         <div>chlidren</div>
       </Child>
     );
   }
   ```

---

## 8. Generic, Utility Type 통해서 Props용 Type 만들기

```jsx
import {
  AddressComponent,
  PersonChildComponent,
  ProfileComponent,
} from "./UtilityTypeChildren";

export type PersonProps = {
  id: string,
  description: string,
  address: string,
  age: number,
  profile: string,
};

export const PersonComponent = ({
  id,
  description,
  address,
  age,
  profile,
}: PersonProps) => {
  return (
    <>
      <PersonChildComponent>
        <div>{id}</div>
      </PersonChildComponent>
      <ProfileComponent
        description={description}
        address={address}
        age={age}
        profile={profile}
      />
      <AddressComponent address={address} />
    </>
  );
};
```

```tsx
// PersonChildComponent children을 옵셔널하게 받기

import { PropsWithChildren, ReactNode } from "react";
import { PersonProps } from "./UtilityType";

export const PersonChildComponent = ({ children }: **PropsWithChildren**) => {
  return <>{children}</>;
};

type OmitType = **Omit<PersonProps, "id">;**

export const ProfileComponent = ({ // id가 빠짐
  description,
  address,
  age,
  profile,
}: OmitType) => {
  return <></>;
};

type PickType = **Pick<PersonProps, "address">;**

export const AddressComponent = ({ address }: PickType) => {
  return <></>;
};
```

---

## 9. Event Handler 사용하기

```jsx
import { useState } from "react";

function App() {
  const [counter, setCounter] = useState<number>(1);

  const eventHandler = **(e: React.MouseEvent<HTMLDivElement>)** => {};

  return <div onClick={eventHandler}>{counter}</div>;
}

export default App;
```

```jsx
import { FormEvent, useState } from "react";

function App() {
  const [counter, setCounter] = useState<number>(1);

  const increment = **(e: FormEvent<HTMLDivElement>)** => {
		e.target;
};

  return <div onChange={increment}>{counter}</div>;
}

export default App;
```
