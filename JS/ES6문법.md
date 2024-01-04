2015년에 발표된 JS 버전 중 하나

```jsx
// 2015년도 이전 => var
// (1) es6 => let(변수), const(상수)

// (2) arrow function

// (3) 삼항 연산자
```

- let, const
    - let : 재할당은 가능하고, 재선언은 불가능해요
    - const : 재할당, 재선언이 불가능, 초기값이 없으면 선언 불가능해요.
    - var : 재할당, 재선언이 가능해요.
    
- 화살표 함수
    
    ```jsx
    const func = () => true
    const func = () => {
    	return true
    }
    ```
    
- 삼항 연산자
    
    ```jsx
    condition ? expr1 : expr2
    
    console.log(true ? "참" : "거짓") // 참
    console.log(false ? "참" : "거짓") // 거짓
    ```
    
- 구조분해 할당
    - de + structuring
    - 배열이나 객체의 속성을 분해해서 변수에 담게 해줌
    1. 배열의 경우
    
    ```jsx
    let [value1, value2] = [1, "new"];
    
    let arr = ["value1", "value2", "vlaue3"];
    let [a,b,c,d] = arr;
    //d는 undefined 초기값 설정해주고 싶으면 d=4이런식으로
    ```
    
    2. 객체인 경우
    
    ```jsx
    let user = {
        name: "nbc", 
    		age: 30
    };
    let {name, age} = user;
    
    // 새로운 이름으로 할당
    let {
    		name: newName, 
    		age: newAge
    } = user;
    
    console.log(name, age) // ReferenceError: name is not defined
    console.log(newName, newAge) //nbc 30
    
    let {name, age, birthDay} = user;
    console.log(birthDay) // undefined
    
    let {name, age, birthDay = "today"} = user; //초기값 설정
    console.log(birthDay) // today
    ```
    
- 단축속성명
    
    ```jsx
    // 1. 단축 속성명 : property shorthand 정말 많이 쓰임!!
    const name = "nbc"; 
    const age = "30";
    
    //key-value
    const obj = {
        name : name, //똑같으면 생략할 수 있음 -> name, 이렇게
        age : age
    }
    const obj1 = {name: name, age: age}
    const obj2 = {name, age}; //배열같지만 객체임!
    ```
    
- 전개구문
    
    ```jsx
    // destructuring과 함께 가장 많이 사용되는 es6 문법 중 하나!
    let arr = [1, 2, 3];
    console.log(arr);       //[1, 2, 3]
    console.log(...arr);    // 1, 2, 3
    
    let newArr = [...arr, 4];
    console.log(newArr);    //[1, 2, 3, 4]
    ```
    
    - 객체 적용
    
    ```jsx
    let user = {
        name: 'nbc',
        age: 30,
    };
    let user2 = {...user}
    console.log(user);       //{ name: 'nbc', age: 30 }
    console.log(user2);      //{ name: 'nbc', age: 30 }
    ```
    
- 나머지 매개변수
    - 매개변수의 개수를 정확히 모를 때 ...arguments
    
    ```jsx
    function exampleFunc (a, b, c, **...args**) {
        console.log(a, b, c); // 1 2 3
        console.log(...args); // 4 5 6 7
        console.log(args);    //[4, 5, 6, 7]
    }
    exampleFunc(1, 2, 3, 4, 5, 6, 7);
    ```
    
- 템플릿 리터럴
    - multi line 가능!!!
    
    ```jsx
    console.log(`Hello World ${testValue}`); //``백틱 자바스크립트 코드 넣으려면: ${}
    const testValue = "안녕하세요"
    console.log(`
    Hello
        My name is JavaScript
    
        Nice to meet you! 
        `); //multi line 가능
    ```
    
- named export vs default export
    1. default Export
        
        ```jsx
        // name.js
        const Name = () => {
        }
        
        export default Name
        
        // other file 
        // 아무 이름으로 import 가능
        import newName from "name.js"
        import NameFromOtherModule from "name.js"
        ```
        
    2. named export
        
        ```jsx
        // 하나의 파일에서 여러 변수/클래스 등을 export 하는 것이 가능
        
        export const Name1 = () => {
        }
        
        export const Name2 = () => {
        }
        
        // other file
        import {Name1, Name2} from "name.js"
        import {newName} from "name.js" // x
        
        // 다른 이름으로 바꾸려면 as 사용
        import {Name1 as newName, Name2} from "name.js"
        
        // default export 처럼 가져오려면 * 사용
        import * as NameModule from "name.js"
        
        console.log(NameModule.Name1)
        ```