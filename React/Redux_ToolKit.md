## 1. 리덕스툴킷이란?

- **(1) 리덕스툴킷**
  ### **리덕스 툴킷은 우리가 이전에 배운 리덕스를 개량한 것**
  리덕스를 사용하기 위해 작성했던 ducks 패턴의 요소들이 전체적인 코드의 양을 늘린다는 개발자들의 불만이 발생하기 시작했고, 리덕스 팀에서는 이것을 수용하여 **코드는 더 적게, 그리고 리덕스를 더 편하게 쓰기 위한 기능들을 흡수해서 만든 것이 리덕스툴킷** 이다. 줄여서 RTK
- **(2) 새로운 것이 아님!**
  ### **리덕스 툴킷은 리덕스와 구조나 패러다임이 모두 똑같.**
  리덕스의 전체 코드의 양을 줄이기 위해 새로운 API가 추가되었고 우리가 일일히 손으로 만들어 줘야 했던 ducks 패턴의 요소들이 어느정도 자동화 되었음
  **컴포넌트에서 useSelector를 통해서 사용하는 것은 모두 똑같.
  바뀐 부분은 그저 모듈 파일 뿐!**

## 2. 일반 리덕스와 코드 비교

- **(1) 툴킷 설치하기**
  **CRA를 통해 새로운 프로젝트틀 생성**하고 `yarn`을 통해 아래 패키지를 설치합니다.
  ```bash
  yarn add react-redux @reduxjs/toolkit
  ```
- **(2) count 프로그램 코드 비교**
  - 일반 리덕스의 counter 프로그램 모듈

    Action Value, Action Creator를 별도로 생성하고 리듀서에서 값을 어떻게 변화시킬 지 만들어줌

    ```jsx
    // 일반 리덕스 예시 코드

    // Action Value
    const ADD_NUMBER = "ADD_NUMBER";
    const MINUS_NUMBER = "MINUS_NUMBER";

    // Action Creator
    export const addNumber = (payload) => {
      return {
        type: ADD_NUMBER,
        payload,
      };
    };

    export const minusNumber = (payload) => {
      return {
        type: MINUS_NUMBER,
        payload,
      };
    };

    // Initial State
    const initialState = {
      number: 0,
    };

    // Reducer
    const counter = (state = initialState, action) => {
      switch (action.type) {
        case ADD_NUMBER:
          return {
            number: state.number + action.payload,
          };
        // [퀴즈 답]
        case MINUS_NUMBER:
          return {
            number: state.number - action.payload,
          };
        default:
          return state;
      }
    };

    // export default reducer
    export default counter;
    ```

  - 리덕스 툴킷을 사용해서 만든 counter 프로그램 모듈입니다.

    **큰 차이점은 Action Value와 Action Creator를 이제 직접 생성해주지 않고, Action Value, Action Creator, Reducer가 하나로 합쳐졌다는 점**

  - `Slice` 라는 API 사용: 한번에 3개가 모두 만들어진다.
  ```jsx
  // src/redux/modules/counterSlice.js

  import { **createSlice** } from "@reduxjs/toolkit";

  const initialState = {
    number: 0,
  };

  const counterSlice = createSlice({
  // 안에 객체가 들어감
    name: "counter",
    initialState,
    reducers: {
      addNumber: (state, action) => {
        state.number = state.number + action.payload;
      },

      minusNumber: (state, action) => {
        state.number = state.number - action.payload;
      },
    },
  });

  // counterSice안에는 액션크리에이터와 리듀서가 있음
  // 액션크리에이터는 컴포넌트에서 사용하기 위해 export 하고
  export const { addNumber, minusNumber } = counterSlice.actions;
  // reducer 는 configStore에 등록하기 위해 export default 한다.
  export default counterSlice.reducer;
  ```
  슬라이스는 **createSlice** 라는 API를 통해 만들 수 있다. 그리고 그 인자로 설정정보를 **객체**로 받는데, 그 안에 필수로 작성해줘야 하는 값은 `name`, `initialState`, `reducers`가 있습니다.
  ```jsx
  //createSlice API 뼈대

  const counterSlice = createSlice({
    name: "", // 이 모듈의 이름
    initialState: {}, // 이 모듈의 초기상태 값
    reducers: {}, // 이 모듈의 Reducer 로직
  });
  ```
  **신기한 것은 위의 counterSlice 리듀서 객체 안에서 만들어주는 함수가 리듀서의 로직이 되면서도 동시에 Action Creator가 된다는 점**
  **그리고 Action Value 까지 함수의 이름을 따서 자동으로 만들어진다. →** Reducer만 만들어주면 됨!
  ```jsx
  // counterSlice.js의 Slice 구조

  const counterSlice = createSlice({
    name: "counter",
    initialState,
    reducers: {
      // 리듀서 안에서 만든 함수 자체가 리듀서의 로직이자, 액션크리에이터가 된다.
      addNumber: (state, action) => {
        state.number = state.number + action.payload;
      },

      minusNumber: (state, action) => {
        state.number = state.number - action.payload;
      },
    },
  });
  ```
  **일반 리덕스에서 export를 통해서 각각의 Action Creator를 내보내주었던 것을 아래 코드를 작성하면 똑같이 내보낼 수 있다.**
  ```jsx
  // 액션크리에이터는 컴포넌트에서 사용하기 위해 export 하고
  export const { addNumber, minusNumber } = counterSlice.actions;
  // reducer 는 configStore에 등록하기 위해 export default 한다.
  export default counterSlice.reducer;
  ```
- **(3) configStore 비교**
  - 일반 리덕스
  ```jsx
  // 일반 리덕스 combineReducers 예시 코드

  import { createStore } from "redux";
  import { combineReducers } from "redux";
  import counter from "../modules/counter";

  const rootReducer = combineReducers({
    counter,
  });
  const store = createStore(rootReducer);
  export default store;
  ```
  - 리덕스 툴킷
  ```jsx
  // src/redux/modules/config/configStore.js

  import { configureStore } from "@reduxjs/toolkit";
  /**
   * import 해온 것은 slice.reducer 입니다.
   */
  import counter from "../modules/counterSlice";
  import todos from "../modules/todosSlice";

  /**
   * 모듈(Slice)이 여러개인 경우
   * 추가할때마다 reducer 안에 각 모듈의 slice.reducer를 추가해줘야 합니다.
   *
   * 아래 예시는 하나의 프로젝트 안에서 counter 기능과 todos 기능이 모두 있고,
   * 이것을 각각 모듈로 구현한 다음에 아래 코드로 2개의 모듈을 스토어에 연결해준 것 입니다.
   */
  const store = configureStore({
    // 객체가 들어감
    reducer: { counter: counter, todos: todos },
  });

  export default store;
  ```
  - todos 모듈
    - 내보내는게 무엇인지 ? action creator
    - reducer을 export
    - action value 정의
    - → slice로 한꺼번에
    아래 코드를 작성해서 todosSlice.js를 만들고, 위 설명과 같이 `configureStore` 에 todosSlice를 추가한다.
  ```jsx
  // src/redux/modules/todosSlice.js

  import { createSlice } from "@reduxjs/toolkit";

  const initialState = {
    todos: [],
  };

  const todosSlice = createSlice({
    name: "todos",
    initialState,
    reducers: {
  		addTodo : (state, action) => {
  			// return : [...state, aciton.payload] (불변성을 유지해아한다. push는 불변성 유지 못해서 원래 못씀)
  			// redux toolkit 안에 immer라는 기능이 내장되어 있기 때문! -> push 쓸 수 있음
  			state.push(action.payload)
  }
  		removeTodo : () => {}
  		switchTodo : () => {}
  	},
  });

  export const {addTodo, removeTodo, switchTodo} = todosSlice.actions;
  export default todosSlice.reducer;
  ```
  그리고 이렇게 생성한 store를 export default 해서 **최상위의 index.js Provider에 주입해주는 것은 전혀 바뀐게 없다.**
  ```jsx
  // index.js

  import React from "react";
  import ReactDOM from "react-dom/client";
  import App from "./App";
  import { Provider } from "react-redux";
  import store from "./redux/config/configStore";

  const root = ReactDOM.createRoot(document.getElementById("root"));
  root.render(
    <Provider store={store}>
      <App />
    </Provider>
  );
  ```
  `App.jsx`에서는 툴킷을 사용해서 만든 모듈을 조회할 수 있다. 방식은 일반리덕스를 사용했을 때와 동일하다.
  ```jsx
  // src/App.js

  import React from "react";
  import { useSelector } from "react-redux";

  const App = () => {
    // Store에 있는 todos 모듈 state 조회하기
    const todos = useSelector((state) => state.todos);

    // Store에 있는 counter 모듈 state 조회하기
    const counter = useSelector((state) => state.counter);

    return <div>App</div>;
  };

  export default App;
  ```
  파일 구조
  - src
    - redux
      - config
        configStore.js
      - modulse
        todoSlice.js (이름은 임의로)

## 3. Redux Devtools 사용하기

- **(1) devtools 소개**
  **리덕스를 사용하면, 리덕스 devtools를 사용할 수 있다.**
  다른 패키지에서는 찾아볼 수 없는 굉장히 **강력한 개발툴**
  현재 프로젝트의 state 상태라던가, 어떤 액션이 일어났을 때 그 액션이 무엇이고, 그것으로 인해 state가 어떻게 변경되었는지 등 리덕스를 사용하여 개발할 때 아주 편리하게 사용할 수 있다.
  **구글 웹스토어에서 플러그인을 설치하**
  - Redux Devtools설치 : [링크 바로가기](https://chrome.google.com/webstore/detail/redux-devtools/lmhkpmbekcpmknklioeibfkpmmfibljd?hl=ko)
- **(2) 사용하는 법**
  웹 스토어에서 devtools를 설치하고 만약 리액트 프로젝트에서 리덕스를 사용하고 있으면 플러그인에 녹색으로 불이 켜진다.
  그리고 개발자도구 탭에서 Redux 라는 메뉴를 볼 수 있.
    <aside>
    💡 **툴킷이 아닌 일반 리덕스에서 devtools를 사용하고자 한다면, 별도 설정이 필요하다.**
    
    </aside>

## 4. Flux 패턴

- 만화로 보는 Flux
  [Flux로의 카툰 안내서](https://bestalign.github.io/translation/cartoon-guide-to-flux/)
- Flux와 redux의 관계
  [Flux와 Redux](https://taegon.kim/archives/5288)
