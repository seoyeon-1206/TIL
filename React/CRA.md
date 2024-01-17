### Create React App

### **SUPER EASY!**

한 줄의 명령어 입력으로 React 프로젝트 개발에 필수요소를 자동으로 구성하는 방법

- React 프로젝트를 구성하기 위해 필요한 것들은 상당히 많음! WebPack, babel, eslint 등.
- 이러한 것들을 신경쓰지 않아도 알아서 촥촥 → ***보일러플레이트*** 생성

```bash
ls #현재 내가 위치하고 있는 곳이 어디인지 확인해보세요.

cd 폴더이름 #리액트 프로젝트를 생성하고 싶은 폴더로 들어갑니다.

yarn create react-app week-1 #프로젝트 생성
npx create-react-app my-app #이렇게 작성하기
```

### 프로젝트 실행

```bash
cd week-1 # week-1 폴더로 이동

yarn start # 프로젝트 시작
```

### 상대경로 import → 절대경로 지정하기

1. jsconfig.json 파일을 만들기(반드시 root 경로에)

```bash
{
	"compilerOptions": {
		"baseUrl": "src"
	},
	"include": ["src"]
}
```