## 개념

1. query: 데이터 요청
2. Mutation: 데이터 변경
3. query invalidation: 무효화, 기존 쿼리를 무효화 시킨 후 최신화

## 실습 - todoList

```bash
yarn add axios
yarn add react-query
yarn add json-server
json-server --watch db.json --port 4000
```

### 2. App.jsx에서 queryClient setting

```jsx
import React from "react";
import Router from "./shared/Router";
import { QueryClient, QueryClientProvider } from "react-query";

const queryClient = new QueryClient();

const App = () => {
  return (
    <QueryClientProvider client={queryClient}>
      <Router />
    </QueryClientProvider>
  );
};

export default App;
```

### 3. .env

```jsx
REACT_APP_SEVER_URL = http://localhost:4000
```

### 4. api > todos.js

```jsx
// axios 요청이 들어가는 모든 모듈
import axios from "axios";

// 조회
const getTodos = async () => {
  const response = await axios.get(`${process.env.REACT_APP_SEVER_URL}/todos`);
  console.log(response.data);
  return response.data;
};
```

### 5. TodoList.jsx

데이터 조회 로직

- useQuery()
  - Query이름
  - 비동기함수 (api), 반드시 비동기 함수 부분에서 return 해줘야 데이터를 가져올 수 있다.

```jsx

  // 데이터 조회 로직
  const { isLoading, isError, data } = useQuery("todos", getTodos);

  if (isLoading) {
    return <h1>로딩중입니다.</h1>
  }

  if (isError) {
    return <h1>에러입니다.</h1>
  }

  ...
   {data
          .filter((item) => item.isDone === !isActive)
          .map((item) => {
            return <Todo key={item.id} todo={item} isActive={isActive} />;
          })}
```

### 6. Mutation

1. 먼저 todo 추가

   todo.js → post, newTodo

   ```jsx
   // 추가
   // 어떤걸 추가해야 하는지 알아야해서 newTodo를 인자로 받는다.
   // post
   const addTodo = async (newTodo) => {
     await axios.post(`${process.env.REACT_APP_SEVER_URL}/todos`, newTodo);
   };
   ```

1. input.jsx

   - handleSubmitButtonClick 함수
     - useQueryClient
       - new QueryClient 상위에서 해줬으니
     - mutation
       - useMutation이라는 hook 이용
         - 비동기 함수 (api)
         - 객체: 성공/실패 key, value
       - 사용법
         - mutation.mutate(api에서 받는 인자 newTodo)
       - onSuccess
         - todoList에 있는 query key(”todos”)
         - onSuccess가 일어날 때 변경이 필요한지 판단! 해당 query key를 invalidate 해주는 습관 필요

   ```jsx
   // 리액트 쿼리 관련 코드
     const queryClient = useQueryClient();
     const mutation = useMutation(addTodo, {
       onSuccess: () => {
         // 어떤걸 invalidate할거냐
         queryClient.invalidateQueries("todos")
       }
     })

     ...
     const handleSubmitButtonClick = (event) => {
       // submit의 고유 기능인, 새로고침(refresh)을 막아주는 역함
       event.preventDefault();

       ...

       // 추가하려는 todo를 newTodo라는 객체로 세로 만듦
       const newTodo = {
         title,
         contents,
         isDone: false,
         id: uuidv4(),
       };

       mutation.mutate(newTodo);
     };
   ```
