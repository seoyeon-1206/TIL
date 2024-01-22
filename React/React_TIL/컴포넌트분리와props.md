### 1. working 과 done에 중복되는 코드들을 컴포넌트로 분리했다.
1. 기존코드
    
    ```jsx
    <div>
          <h3>Working</h3>
          <div className='working-list'>
          {todo.filter((item)=> !item.isDone).map((item) => {
            return(
              <div key={item.id} className='todo-style'>
                <h1>{item.title}</h1>
                <p>{item.body}</p>
                <button onClick={() => removeButtonHandler(item.id)}>삭제</button>
                <button onClick={() => todoUpdateButtonHandler(item.id)}>완료</button>
              </div>
            ) 
        })}
          </div>
        </div>
    
        <div>
          <h3>Done</h3>
          {todo.filter((item)=> item.isDone).map((item) => {
            return(
              <div key={item.id} className='todo-style'>
                <h1>{item.title}</h1>
                <p>{item.body}</p>
                <button onClick={() => removeButtonHandler(item.id)}>삭제</button>
                <button onClick={() => todoUpdateButtonHandler(item.id)}>{item.isDone ? "취소" : "완료"}</button>
              </div>
            ) 
        })}
          </div>
    ```
    

### 2. TodoItem
1. return 안의 부분 분리
    
    ```jsx
    <div key={item.id} className='todo-style'>
                <h1>{item.title}</h1>
                <p>{item.body}</p>
                <button onClick={() => removeButtonHandler(item.id)}>삭제</button>
                <button onClick={() => todoUpdateButtonHandler(item.id)}>완료</button>
              </div>
    ```
    
    ```jsx
    const TodoItem({item, onRemove, onUpdate}) => {
      return (
        <div key={item.id} className='todo-style'>
                <h1>{item.title}</h1>
                <p>{item.body}</p>
                <button onClick={() => onRemove(item.id)}>삭제</button>
                <button onClick={() => onUpdate(item.id)}>
                {item.isDone ? "취소" : "완료"}
                </button>
              </div>
      )
    }
    ```
    
    ```jsx
    //컴포넌트 적용
    <div className='working-list'>
              {todo.filter((item) => !item.isDone).map((item) => (
                <TodoItem
                  key={item.id}
                  item={item}
                  onRemove={removeButtonHandler}
                  onUpdate={todoUpdateButtonHandler}
                />
              ))}
            </div>
          </div>
    ```
    

### 3. TodoList
1. working, done 부분 분리
    
    ```jsx
    import React from "react";
    import TodoItem from "./TodoItem";
    
    const TodoList = ({items, onRemove, onUpdate, updateTitle}) => {
        return (
            <div>
            <h3>{updateTitle}</h3>
            <div className='working-list'>
              {items.map((item) => ( //TodoItem에 item 전달
                <TodoItem
                  key={item.id}
                  item={item}
                  onRemove={onRemove}
                  onUpdate={onUpdate}
                />
              ))}
            </div>
          </div>
        );
    };
    export default TodoList;
    ```
    
    ```jsx
    //최종 컴포넌트 적용
    			<TodoList
            items={todo.filter((item => !item.isDone))}
            updateTitle="Working"
            onRemove={removeButtonHandler}
            onUpdate={todoUpdateButtonHandler}
          />
    
          <TodoList
            items={todo.filter((item => item.isDone))}
            updateTitle="Done"
            onRemove={removeButtonHandler}
            onUpdate={todoUpdateButtonHandler}
          />
    ```
    
    **`{ items, onRemove, onUpdate, updateTitle }`** 
    
    - TodoList 컴포넌트의 props
    - TodoList 컴포넌트 내부에서 사용되고 있음
    - TodoItem 컴포넌트에 직접적으로 전달 X
    
    **`{ item, onRemove, onUpdate }`**
    
    - TodoItem 컴포넌트의 props → TodoList에서 전달받은게 아니라 TodoItem 컴포넌트 자체에서 직접적으로 받아옴
    - TodoList에서 전달받은 items 배열의 각 요소를 item이라는 이름의 props로 받아옴
    - TodoList 컴포넌트에서 전달받은 items 요소와 함께 사용됨
    
    ---
    
    ### TIL
    
    - 처음에는 TodoList와 TodoItem을 합쳐서 컴포넌트화 하려고 했다. 하지만 key의 문제가 오류가 나기도 했고 유지 보수 측면에서 별로인 것 같아 분리를 시켰다.
    - props에 대한 이해
        - props는 부모 컴포넌트에서 자식 컴포넌트로 데이터를 전달하는 데 사용되는 메커니즘
        - 데이터 전달시, 자식 컴포넌트에서 해당 데이터를 props라는 객체로 받아올 수 있음 **`{item}`**
        - 컴포넌트 내에서 직접적으로 사용되는 데이터가 있다면 그 컴포넌트의 로컬 상태로 관리될 수 있음
            
            **`{ items, onRemove, onUpdate, updateTitle }`** 
            
        - 로컬 상태는 컴포넌트 내에서만 사용되며, 부모 컴포넌트에서 전달되는 props와는 다름
        - 즉, 컴포넌트가 자체적으로 필요한 데이터를 가지고 있을 때, 이 데이터를 로컬 상태로 관리하고 부모 컴포넌트에서는 필요 없을 때는 props로 전달하지 않고 사용할 수 있음