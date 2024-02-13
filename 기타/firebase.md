## 1. 환경설정

1. firebase.js

   파일 만들고 코드 복붙

   ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/f3b9e5d5-5b07-43a2-973a-52431632687a/0b946abd-f218-4364-b76a-059efcb6af5c/Untitled.png)

2. export 붙이기
3. app.js

   ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/f3b9e5d5-5b07-43a2-973a-52431632687a/cf8677a7-85e3-4b53-8750-d630b9eb0d11/Untitled.png)

   useEffect로 app 찍어보기 (이런게 나온다..)

   ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/f3b9e5d5-5b07-43a2-973a-52431632687a/f6161c09-e0ad-4e14-99be-b712a2dc405c/Untitled.png)

## 2. Authentication : 회원가입, 로그인

1. 여기에서 웹 인증 들어가기

   [Add Firebase to your JavaScript project](https://firebase.google.com/docs/web/setup#available-libraries)

2. getAuth

   ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/f3b9e5d5-5b07-43a2-973a-52431632687a/e0c0d342-2835-4cd3-b0a1-ea163769afec/Untitled.png)

3. 신규 사용자 가입

   1. 그 전에 firebase → authentication → 시작하기 눌러주기 → 이메일,비밀번호 사용설정 → 저장
   2. 짠

   ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/f3b9e5d5-5b07-43a2-973a-52431632687a/6c29d90f-214a-4cfe-895c-4abe7142c196/Untitled.png)

### 실습

### 1. createUserWithEmailAndPassword

1. auth, createUserWithEmailAndPassword 세팅

   ```jsx
   const signUp = (event) => {
     event.preventDefault();
     createUserWithEmailAndPassword(auth, email, password)
       .then((userCredential) => {
         // Signed in
         const user = userCredential.user;
         console.log("user with signup", user);
         // ...
       })
       .catch((error) => {
         const errorCode = error.code;
         const errorMessage = error.message;
         // ..
       });
   };
   ```

2. 이렇게 뜬다 user가

   ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/f3b9e5d5-5b07-43a2-973a-52431632687a/ede5652d-4e66-460b-8c42-8ccb862e7ea8/Untitled.png)

3. 에러나면 이렇게 에러 메세지도 잘 써줌

   ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/f3b9e5d5-5b07-43a2-973a-52431632687a/847d136e-dd8f-42f0-b3dc-de81f16e9b84/Untitled.png)

4. 비동기로 리팩토링(이건 잘 모르겟음)

   ```jsx
   const signUp = async (event) => {
     event.preventDefault();

     try {
       const userCredential = await createUserWithEmailAndPassword(
         auth,
         email,
         password
       );
       console.log("user", userCredential.user);
     } catch (error) {
       const errorCode = error.code;
       const errorMessage = error.message;
       console.log("error with signUp", errorCode, errorMessage);
     }
   };
   ```

---

### 2. **인증 상태 관찰자 설정 및 사용자 데이터 가져오기**

```jsx
useEffect(() => {
  onAuthStateChanged(auth, (user) => {
    console.log("user", user);
    // auth.currentUser ---> 현재 로그인한 유저 정보를 보여줌
  });
}, []);
```

---

### 3. 로그인

```jsx
const signIn = async (event) => {
  event.preventDefault();
  try {
    const userCredential = await signInWithEmailAndPassword(
      auth,
      email,
      password
    );
    console.log("user with signIn", userCredential.user);
  } catch (error) {
    const errorCode = error.code;
    const errorMessage = error.message;
    console.log("error with signIn", errorCode, errorMessage);
  }
};
```

---

### 4. 로그아웃

로그아웃을 누르면 콘솔에 user null이 찍힌다

```jsx
const logOut = async (event) => {
  event.preventDefault();
  await signOut(auth);
};
```

## 3. Cloud Firestore를 이용한 데이터 다루기

- Firebase의 데이터 모델

  ### Document

  Document는 Firestore의 기본 데이터 단위입니다. Firestore는 데이터를 객체의 형태로 저장하는데, 이 때, 저장되는 이 객체 형태의 데이터를 문서(Document)라고 합니다. 일반적으로 JSON 형식으로 구조화된 데이터입니다.

  - 어떤 식으로 생겼나요?
    ```jsx
    {
      first: "Ada";
      last: "Lovelace";
      born: 1815;
    }
    ```

  각 Document는 고유한 ID를 가지며, 컬렉션 안에 저장됩니다. Document의 ID는 자동으로 생성되거나, 혹은 직접 지정할 수도 있습니다.

  ### Collection

  <img src="https://teamsparta.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F865c3d9f-3b77-45ad-878e-9d7784b06d21%2FUntitled.png?table=block&id=4d02285d-3539-447f-b409-20dcf5d6f1ca&spaceId=83c75a39-3aba-4ba4-a792-7aefe4b07895&width=860&userId=&cache=v2" width="90%"></img>

  Document는 Collection에 **저장**됩니다. 여러 문서들이 모여있는 폴더나 컨테이너 같은 개념으로 생각하시면 됩니다.

- Firestore 콘솔 설정
  cloudStore → 데이터베이스 만들기 → asia-northeast3 (Seoul) 위치 설정 → 테스트 모드
- 데이터 추가
  Firestore Database → [+ 컬렉션 시작] 클릭후 컬렉션 ID를 지정 → 문서 ID를 지정하고 필드 값 넣어주기
  수정, 삭제
- 코드를 통해 데이터 읽어오기
  ### Firestore SDK 추가
  `firebase.js` 에 Firestore 관련 코드를 추가
  ```jsx
  import { initializeApp } from "firebase/app";
  ...
  import { getFirestore } from "firebase/firestore";

  const firebaseConfig = {
  	...
  };

  const app = initializeApp(firebaseConfig);
  ...
  export const db = getFirestore(app);
  ```
  이제 db를 이용하면 firestore에 있는 데이터를 자유롭게 가져올 수 있습니다.
  먼저, Todo.js 컴포넌트에 todos collection의 데이터를 가져오는 함수를 작성합니다.
  firestore에 있는 데이터를 가져오기 위해서는 어떤 컬렉션으로부터 데이터를 가져올 것인지, 가져올 때 어떤 식으로 정렬할 것인지, 어떤 조건으로 가져올 것인지에 대한 **query**를 먼저 만들게 됩니다.
  - 다양한 query 조건문
    ```jsx
    collection(): 특정 컬렉션 고르기
    where(): 지정된 필드에 대한 조건 지정
    orderBy(): 지정된 필드를 기준으로 정렬
    limit(): 가져올 문서의 최대 개수 지정
    ```
  그리고 **getDocs()**이라는 함수의 인자로 위에서 만든 query를 넘겨주면, getDocs()는 비동기적으로 작동하며, querySnapshot을 반환합니다. 여기서 querySnapshot이란 가져온 문서들에 대한 정보와 해당 문서들에 접근할 수 있는 메서드를 포함합니다.
  ```jsx
  // components/Todo.js
  import { collection, getDocs, query } from "firebase/firestore";
  import { db } from "../firebase";

  ...
  useEffect(() => {
      const fetchData = async () => {
        // collection 이름이 todos인 collection의 모든 document를 가져옵니다.
        const q = query(collection(db, "todos"));
        const querySnapshot = await getDocs(q);

        const initialTodos = [];

        // document의 id와 데이터를 initialTodos에 저장합니다.
        // doc.id의 경우 따로 지정하지 않는 한 자동으로 생성되는 id입니다.
        // doc.data()를 q실행하면 해당 document의 데이터를 가져올 수 있습니다.
        querySnapshot.forEach((doc) => {
          initialTodos.push({ id: doc.id, ...doc.data() });
        });

  			// firestore에서 가져온 데이터를 state에 전달
        setTodos(initialTodos);
      };

      fetchData();
    }, []);
  ```
  실제로 코드를 동작해본 결과, firestore console에 있는 데이터가 화면에 보여지는 것을 확인할 수 있습니다.
- 데이터 추가하기
  데이터를 추가하는 것도 간단합니다. Todo.js의 addTodo 함수를 다음과 같이 수정해줍니다.
  이 때, `addDoc`이라는 함수를 활용해서 새로운 Document를 todos Collection에 추가해줍니다.
  간단하게도, `addDoc`은 첫번 째 인자로 컬렉션에 대한 참조를, 두번째 인자로는 추가할 Document 값을 받습니다.
  ```jsx
  import { **addDoc**, collection, getDocs, query } from "firebase/firestore";

  ...

  const addTodo = async (event) => {
      event.preventDefault();
      const newTodo = { text: text, isDone: false };
      setTodos((prev) => {
        return [...todos, newTodo];
      });
      setText("");

  		// Firestore에서 'todos' 컬렉션에 대한 참조 생성하기
      // collection(어디서 가져올거, 추가할 것)
      const collectionRef = collection(db, "todos");
  		// 'todos' 컬렉션에 newTodo 문서를 추가합니다.
      await addDoc(collectionRef, newTodo);
    };
  ```
  실제로 코드를 추가한 후 콘솔을 확인해보면, 제대로 추가 된 것을 확인할 수 있습니다.
  따로 Document의 ID를 지정하지 않았기 때문에, Document의 ID가 자동으로 생성된 것을 확인할 수 있습니다. 통일성을 위해 기존의 존재하던 Documents들은 모두 제거하고 다시 만들었습니다.
- 데이터 수정하기
  데이터를 수정하는 방법도 다르지 않습니다. addDoc을 사용했던 것 처럼, 이번에는 updateDoc을 사용합니다. 먼저, doc 함수를 이용해서 todos collection의 document 중에서 id가 todo.id인 document의 참조값을 찾습니다. 그리고 updateDoc을 이용해서 해당 document의 isDone을 바꿔줍니다.
  ```jsx
  // components/Todoitem.js
  import { doc, **updateDoc** } from "firebase/firestore";
  import { db } from "../firebase";

  const TodoItem = ({ todos, todo, setTodos }) => {
  ...

  const updateTodo = async (event) => {
      const todoRef = doc(db, "todos", todo.id);
      await **updateDoc**(todoRef, { ...todo, isDone: !todo.isDone (어떤 값으로 바꿀것인가?)});

      setTodos((prev) => {
        return prev.map((element) => {
          if (element.id === todo.id) {
            return { ...element, isDone: !element.isDone };
          } else {
            return element;
          }
        });
      });
    };
  ...
  }
  ```
- 데이터 삭제하기
  데이터를 삭제하는 방법도 데이터를 수정하는 방법과 마찬가지 입니다. 이번에는 `deleteDoc` 을 사용해줍니다.
  ```jsx
  // components/Todoitem.js
  import { **deleteDoc**, doc, updateDoc } from "firebase/firestore";

  const deleteTodo = async (event) => {
      const todoRef = doc(db, "todos", todo.id);
      await deleteDoc(todoRef);

      setTodos((prev) => {
        return prev.filter((element) => element.id !== todo.id);
      });
    };
  ```
