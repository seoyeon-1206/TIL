## 01. 컴파일러, 그리고 tsc!

<aside>
💡 컴파일러는 특정 프로그래밍 언어가 *정적 언어로서의 정체성을 유지할 수 있게 하는 도구에요!

</aside>

- 1. 우리는 왜 컴파일러를 알아야 하는가?
  ### ☑️ 컴파일러의 근본적인 역할
  - **타입 검사**를 해주는 녀석
    - TypeScript 컴파일러는 소스 코드의 정적 타입 검사를 수행
    - 이를 통해 개발자는 코드에서 타입 관련 오류를 미리 발견하고 수정
  - **코드 변환**
    - 타입스크립트 컴파일러인 tsc는 TypeScript → JavaScript 코드 변환
    - C언어 컴파일러는 C언어 → 기계어 코드 변환을 해준다.
    - 컴파일러를 이해하면 변환된 코드가 어떻게 실행되는지 예측할 수 있다!
    - 이는 디버깅 과정에서 큰 도움이 된다.
  ### ☑️ 컴파일러의 매력
  - 에러 메시지 해석할 때
    - 컴파일러는 소스 코드에서 문제가 발견되면 에러 메시지를 출력
    - 컴파일러를 이해하면 에러 메시지를 보다 정확하게 해석하고 문제를 신속하게 해결할 수 있다
  - 컴파일러의 끝은 **최적화!**
    - 코드가 최적화되면 **전반적인 어플리케이션 실행 시간이 더 빨라진다!**
    - 컴파일러는 이런 것을 자동으로 도와주는 매우 유익한 친구
- 2. 컴파일러란?
  - 컴파일러는 **프로그래밍 언어로 작성된 소스 코드 → 다른 프로그래밍 언어로 변환**하는 도구
  - 이러한 변환 과정에서 컴파일러는 소스 코드의 구문과 구조를 검사하여 문제가 없는지 확인한다.
  - 이를 통해 개발자가 작성한 코드에 오류가 있는 경우 미리 알려주어 문제를 해결할 수 있다.
- 3. 컴파일러는 왜 등장했을까?
        <aside>
        💡 컴퓨터는 기본적으로 기계어로 작성된 프로그램만 이해할 수 있다!
    </aside>
    
    - 그런데, 기계어는 사람이 이해하기 어렵다.
    - 따라서, C언어와 같은 고수준 프로그래밍 언어가 등장
    - C언어로 작성된 코드를 컴퓨터가 이해하려면 기계어로 변환을 해야되었고 컴파일러가 필요하게 되었다.
- 4. tsc = TypeScript 컴파일러
  ### ☑️ 여기서 갑자기 궁금한 점 **🤔🤔🤔**
    <aside>
    ⁉️ 위에서는 기계어로 변환되어야 한다고 했는데 JavaScript는 기계어로 변환될 필요가 없나?
    
    </aside>
    
    - yes
    - 왜냐하면, JavaScript는 동적 언어(=인터프리터 언어)이기 때문이다!
        - Node.js나 Chrome → JavaScript를 실행할 때는 V8 엔진이 코드 해석 및 실행
        - Firefox → JavaScript를 실행할 때는 SpiderMonkey가 코드 해석 및 실행
    - 정리하자면요.
        - 정적 언어(=컴파일 언어) → 기계어로 변환이 되어야 한다!
        - 동적 언어(=인터프리터 언어) → 엔진이 코드를 한 줄씩 실행하면서 동적으로 해석
    
    ### ☑️ tsc 명령어 사용해보기
    
    - 자세한 명령어 옵션 확인
        - https://www.typescriptlang.org/docs/handbook/compiler-options.html
    - 주요 명령어
        - `tsc —-init`
            - tsconfig.json 생성하기
        - `tsc index.ts`
            - index.ts를 컴파일 한다.
            - **.ts**는 **TypeScript 파일의 확장자**
        - `tsc src/*.ts`
            - src 디렉토리 안에 있는 모든 TypeScript 파일을 컴파일
        - `tsc index.js --declaration --emitDeclarationOnly`
            - **@types** 패키지를 위한 .d.ts 파일 생성을 하는 명령어
            - TypeScript로 작성된 모듈이 아니라 JavaScript로 작성된 모듈에 타입 선언을 제공할 때 유용하게 쓰인

---

## **02.** tsconfig.json 해부하기

<aside>
💡 `compilerOptions - strict`옵션은 **true로 설정하는** 것을 권장

</aside>

<aside>
💡 `compilerOptions - sourceMap`옵션은 **개발 환경에서 true로 설정하는** 것을 권장

</aside>

- 1. tsconfig.json이란?
  - `tsc --init` 명령을 실행하면 생성되는 파일
  - tsconfig.json은 **TypeScript 프로젝트의 설정 파일**
  - 주로 프로젝트의 **컴파일 옵션** 및 **입력 파일**들을 정의하는데 사용
- 2. tsconfig.json 주요 옵션
  ### ☑️ 옵션 매뉴얼
  - https://www.typescriptlang.org/ko/tsconfig
  ### ☑️ compilerOptions - target 옵션
  - 해당 TypeScript 프로젝트 내 코드들이 **어떤 JavaScript 버전으로 변환을 할 지 정하는 옵션**
  - `es5` 로 설정하면 CommonJS 버전으로 컴파일
  - `es2016(=es7)` 로 설정하면 ES2016 버전으로 컴파일
    - **최신 브라우저는 보통 ES2016을 지원하니 이렇게 설정하는 것을 추천**
  - 단, 이것을 정할 때는 TypeScript가 어느 환경에서 실행이 되어야하는지를 고려해야 한다!
    - 만약, 내가 만든 프로젝트가 생각보다 레거시한 환경에서 동작해야 된다면? → es5
    - 그렇지 않다면 → es2016
  ### ☑️ compilerOptions - module 옵션
  - TypeScript 파일을 컴파일한 후 생성되는 JavaScript 모듈의 형식을 지정
  - 모듈을 가져오고 내보내는 방식을 결정하는 옵션
  - **target 옵션과는 서로 독립적인 관계**니 프로젝트의 요구사항에 따라 옵션을 설정
  ### ☑️ compilerOptions - outDir 옵션
  - 컴파일된 JavaScript 파일이 저장될 출력 디렉터리를 지정
  - 예를 들어, `"outDir": "dist"`로 설정하면 컴파일된 파일들이 `dist` 폴더에 저장된다.
  - 5. `compilerOptions - strict` 옵션
    - 엄격한 타입 검사 옵션을 모두 활성화하는 옵션
    - TypeScript 컴파일러가 보다 엄격한 타입 검사를 수행해 코드의 실수를 미리 찾아낼 수 있.
    - 해당 옵션을 true로 설정하면 아래의 옵션들이 자동으로 true로 설정
      - `strictNullChecks`
        - 잠재적으로 null(undefined)이 될 수 있는 값들에 대해서 엄격하게 확인하는 옵션
      - `strictFunctionTypes`
      - `strictBindCallApply`
      - `strictPropertyInitialization`
      - `noImplicitAny`
        - 함수의 인자 또는 변수의 타입이 명시적으로 선언되지 않은 경우에 컴파일러가 자동으로 `any`타입을 부여하지 않도록 한다.
        - 이 옵션을 활성화하면 개발자가 누락된 타입 선언을 확인하고 명시적으로 타입을 선언할 수 있다.
      - `noImplicitThis`
      - `alwaysStrict`
    - 당연하게도, **해당 옵션은 꼭 true로 설정하는 것을 권장**
  ### ☑️ compilerOptions - sourceMap 옵션
  - 컴파일된 JavaScript 파일에 대한 소스 맵을 생성하는 옵션 (소스의 단서)
  - 소스 맵을 사용하면 실행 중에 에러가 발생했을 때 원래 TypeScript 소스 코드의 위치를 확인할 수 있다.
  - 코드 디버깅에 매우 큰 도움이 되기 때문에 **개발 환경**에서는 **꼭 true로 설정하는 것을 권장**
    - 프로덕션 환경에서는 용량이나 성능상의 이유로 sourceMap을 사용하지 않는 것이 나을 수 있다
  ### ☑️ include , exclude 옵션
  - tsc가 컴파일을 할 때 포함하거나 제외할 파일이나 디렉터리를 지정하는 옵션
  - “include": ["src/**/*"]
    - src 디렉토리 밑의 친구들을 컴파일 하겠다는 의미
  - "exclude": ["node_modules", "dist"]
    - node_modules, dist 디렉토리 밑의 친구들은 컴파일 대상에서 제외하겠다는 의미

---

## 03. .d.ts 파일 알아보기

<aside>
💡 **JavaScript 라이브러리도 TypeScript 코드에서 사용할 수 있게 하는 보물**

</aside>

- 1. 생각해 볼 것이 있다! **🤔🤔🤔**
  - TypeScript가 나오기 전에는 당연히도 JavaScript로만 코드를 작성했겠지?
  - 아직까지 이 세계에는 JavaScript 코드의 양 >>> TypeScript 코드의 양
  - 그렇다면, **이미 작성된 다양한 JavaScript 라이브러리와 호환성을 유지**할 수 있어야 한다 (하위 호완성)
- 2. @types 라이브러리의 등장
  - TypeScript는 `@types` 라이브러리를 통해 외부 라이브러리에 대한 타입 정보를 제공
  - 그런데, 해당 디렉토리를 들어가보니 `.d.ts` 파일들이 엄청 많이 있는 것을 확인할 수 있다.

  - 그렇다면 대체 이 .d.ts 파일은 무엇?
- 3. .d.ts 파일의 정체
  - `.d.ts` 파일은 **TypeScript 타입 정의 파일이다.** 즉, JavaScript 라이브러리에 대한 타입 정보를 제공한다!
  - `.d.ts` 파일로 TypeScript 컴파일러는 다음을 알 수 있다! **[💪💪💪](https://emojipedia.org/flexed-biceps/)**
    - 외부 라이브러리의 함수 타입 정보
    - 외부 라이브러리 클래스 타입 정보
    - 외부 라이브러리 객체 타입 정보
  - 뿐만 아니라, `.d.ts` 파일로 외부 라이브러리의 **타입 추론**도 할 수 있다.
    - 타입 추론이란 **타입이 명시가 되지 않았을 때 컴파일러가 알아서 해당 타입에 대해 추론을 하는 것이다**.
- 4. 금쪽같은 내 JavaScript 라이브러리를 TypeScript에서!
    <aside>
    ⁉️ TypeScript 좋은 것 다 알겠는데 이거로 옮기면 내가 아끼던 **JavaScript 라이브러리는 버려야 되나?** 🤔
    
    </aside>
    
    - JavaScript 라이브러리를 TypeScript에서 쓰려면 해당 **라이브러리에 대한 .d.ts 파일만 제공**을 해주면 된다!
    - TypeScript 프로젝트에서도 **JavaScript 라이브러리를 한 줄도 수정하지 않고 그대로 쓸 수 있다!**
- 5. 실습 - JavaScript 라이브러리를 TypeScript 프로젝트에서 사용해보기!
