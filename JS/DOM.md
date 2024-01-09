## 1. DOM

### (1) DOM의 기본 개념

1. **javascript가 왜 생겼는데?**
    1. 브라우저에서 쓰려고
    2. 웹 페이지를 동적으로 만들기 위해
2. **웹 페이지가 뜨는 과정**
    1. 사용자가 브라우저에 `‘www.naver.com’` 주소를 입력
        -  **사용자 = 브라우저 = 클라이언트**
    2. HTML 문서를 서버로부터 수신 (document)
    3. **브라우저가 HTML 파일을 해석(parsing 파싱)**
        -  브라우저는 HTML을 렌더링하는 엔진이 있음 
    4. javascript가 알아들을 수 있는 방식으로 해석한 내용을 토대로 **DOM Tree를 구성**
    
    -  <img width="50%" src="https://teamsparta.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F72510615-539e-4ea0-aec8-f4af3cf309e5%2FUntitled.png?table=block&id=49837026-d296-4c14-bf8c-674ab36cc9a2&spaceId=83c75a39-3aba-4ba4-a792-7aefe4b07895&width=770&userId=&cache=v2"/>
        
    5. **DOM Tree**랑 CSSOM Tree를 묶어서 Render Tree를 구성
        
        > 렌더 트리(Render Tree)는 HTML, CSS 및 JavaScript 문서를 파싱하여 브라우저에서 **실제로 렌더링되는 최종 문서 모델**을 나타내는 객체의 계층 구조입니다.
        
        결국은, 브라우저 화면에 그리기 위한 최종 버전을 만. 그리고 나서 브라우저에 그림을 그리기 위한 **`레이아웃 계산`** → `페인팅 과정`이 시작
        > 
3. **그래서 DOM이 뭐라고?**
    1. **Document**(HTML 파일)를 Javascript가 알아먹을 수 있는 **Object** 형태로 **Model**ing 한 것(2-c과정의 결과물)
    2. 브라우저에 기본적으로 내장되어 있는 API 중 하나
        -  **API는 다른 시스템에서 제공하는 기능을 사용할 수 있도록 도와주는 중간자 역할**
    3. DOM이 **브라우저에 내장**되어있기 때문에 우리는 HTML의 내용을 **javascript**로
        -  **`접근`**할 수 있음 ***
        -  **`제어`**할 수 있음 ***
    4. 모든 DOM의 node들은 ‘속성’과 ‘메서드'를 갖고있음
        
        <aside>
        💡 DOM의 node?
        
        - **웹 페이지를 구성하는 모든 HTML 태그와 텍스트, 그리고 속성 등을 하나의 블록으로 취급하는 것**
        - 서로 계층 구조로 연결
        - 각 블록은 자식 노드, 부모 노드, 형제 노드와 관계를 가지고 있음 → DOM 트리를 탐색하고 조작할 수 있음
        
        - 아래 DOM 요소 하나 하나를(네모, 동그라미) 노드라고 할 수 있어요 🥸

        - <img width="50%" src="https://teamsparta.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F954ac741-1f2d-4229-87d7-75d060220967%2FUntitled.png?table=block&id=4ebda28d-8b3f-47b0-8a3d-868bfd6556f7&spaceId=83c75a39-3aba-4ba4-a792-7aefe4b07895&width=860&userId=&cache=v2"/>
        </aside>
        
        <aside>
        💡 DOM 요소의 속성과 메서드를 구분해볼까요?
        
        - **Node 객체의 속성은 값**을 가지고 있다
        - **메서드는 동작을 수행**
        
        즉, Node 객체의 속성은 **해당 객체의 특성을 나타내는 값을 가져오거나 설정**하는 데 사용되며, **메서드는 해당 객체가 수행하는 작업을 나타내는 함수**
        
        예를 들어, Node 객체의 **`nodeName`** 속성은 해당 노드의 이름을 나타내는 문자열 값을 반환합니다. 반면에 **`appendChild()`** 메서드는 해당 노드의 자식 노드를 추가하는 메서드이며, DOM 트리에서 해당 노드의 위치를 변경하는 동작을 수행합니다.

        </aside>
        
        ```jsx
        //속성(innerHTML)과 메서드(getElementById 보통 동사)
        document.getElementById("demo").innerHTML = "Hello World!";
        ```
        
    5. 참고(함수와 메서드의 차이)
        
        ```jsx
        // example1(메서드의 예)
        person.getName();
        
        // example2(함수의 예)
        testLogging();
        ```
        
    

### (2) DOM에 접근 및 제어해보기

1. 항상 돔트리의 최상단 노드는 document
    
    ```jsx
    // 무슨 결과가 나올까요?
    document.getRootNode();
    ```
    
2. childNodes, parentNode를 이용해 기어다녀보기 :)
    
    DOM은 모두 노드로 이루어져 있기 때문에 `부모노드 - 자식노드관계`로 이루어져 있습니다. 왔다갔다 해 보는 연습을 통해서 익숙해 질 수 있어요!
    
3. document 관련 api
    1. Finding
        
        ```jsx
        /** 찾아봅시다 */
        
        // 해당 id명을 가진 요소 하나를 반환합니다.
        document.getElementById("id명")
        
        // 해당 선택자를 만족하는 요소 하나를 반환합니다.
        document.querySelector("선택자") //"#id명"
        
        // 해당 class명을 가진 요소들을 배열에 담아 인덱스에 맞는 요소를 반환합니다.
        document.getElementsByClassName("class명")[인덱스]
        
        // 해당 태그명을 가진 요소들을 배열에 담아 인덱스에 맞는 요소를 반환합니다.
        document.getElementsByTagName("태그명")[인덱스]
        li 태그 가지고 있는것 다 가져와봐
        
        // 해당 선택자를 만족하는 모든 요소들을 배열에 인덱스에 맞는 요소를 반환합니다.
        document.querySelectorAll("선택자명")[인덱스]
        
        // 새로운 노드를 생성합니다.(html파일에서 적은적 없는 div태그를 JS코드로 제어한 것)
        const div = document.createElement('div'태그 이름);
        document.body.append(div);
        ```
        
    2. changing
        
        ```jsx
        /** property(=속성)을 바로 바꿔버려잇! */
        
        // 이 둘은 차이가 있어요!
        element.innerHTML = new html content
        element.innerText = new text
        
        // style을 바꿔요.
        element.style.property = new style
        
        //method를 통해 클래스를 추가해봐요.
        element.setAttribute(attribute, value)
        
        // 어랏? 그럼 이런것도 가능??
        element.setAttribute("style", "background-color:red;");
        
        // ....
        element.style.backgroundColor = "red";
        
        // input 필드의 변신
        ```
        
    3. 몇 가지만 더 해볼까요?
        
        ```jsx
        // createElements
        const para = document.createElement("p");
        para.innerText = "This is a paragraph";
        document.body.appendChild(para);
        
        // createTextNode(elements는 아니구여, 그냥 글자...)
        let textNode = document.createTextNode("Hello World");
        document.body.appendChild(textNode);
        
        // write. 조심 또 조심!
        document.write("Hello World!");
        
        document.write("<h2>Hello World!</h2><p>Have a nice day!</p>");
        
        // 골로 가는 코드
        function myFunction() {
          document.write("Hello World!");
        }
        
        // version 01
        element.addEventListener("click", myFunction);
        function myFunction() {
          document.getElementById("demo").innerHTML = "Hello World";
        }
        
        // version 02
        element.addEventListener("click", function() {
          document.getElementById("demo").innerHTML = "Hello World";
        });
        ```