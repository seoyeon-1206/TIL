## 1. 인증&인가

- (1) 인증(Authentication) vs. 인가(Authorization)
  - 인증(Authentication): 서비스를 이용하려는 유저가 `등록된 회원`인지 확인하는 절차 (ex.로그인할 때)
  - 인가(Authorization): `특정 리소스에 접근할 수 있는 권한`이 있는지 확인하는 절차
  - Quiz: 유저가 마이페이지에서 서버에 API 요청으로 개인정보를 받아오는 요청을 할 때, 서버는 클라이언트에게 `인증`을 해주나요 아니면 `인가`를 해줄까? A : 인가
- (2) http 프로토콜 통신의 특징 2가지

  1. 무상태 (Stateless) : 서버는 클라이언트의 상태를 기억하지 않는다. 따라서 각 요청마다 서버에서 요구하는 모든 상태 정보를 담아서 요청해야 한다.

     - 상태값은 매 요청마다 클라이언트가 가지고 오기 때문에, 서버는 클라이언트의 상태를 별도로 기억할 필요없이 주문받은 대로 응답해준다.
     - 무상태라는 특성 덕분에 동일한 서버를 여러개로 확장시킬 수 있다. (Scale-Out)

  2. 비연결성 (Connectionless) : 서버와 클라이언트는 연결되어 있지 않다. 서버 입장에서는 매번 새로운 요청이다.

     <img width="{해상도 비율}" src="https://teamsparta.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fdf1b2407-550c-4b34-8b92-9c2b074f829d%2FUntitled.png?table=block&id=663820fa-ac97-4b45-99ad-144ca503433a&spaceId=83c75a39-3aba-4ba4-a792-7aefe4b07895&width=1370&userId=&cache=v2"/>

     - 비연결성으로 인해 최소한의 서버 자원으로 서버를 유지할 수 있게 해준다.
     - 그러나, 각 사용자별 요청이 잦은 서비스의 경우 이러한 비연결성은 오히려 비효율적일 수 있습니다.

## 2. 쿠키, 세션, 토큰

- (1) 쿠키 (cookie) 란?

  - 브라우저 주머니에 가지고 있는 과자
  - 무상태와 비연결성이라는 http 통신의 특징에도 불구하고 마치 서버가 클라이언트의 인증 상태를 기억하는 것처럼 구현할 수 있는 수단으로 사용할 수 있다.
  - 쿠키란 `브라우저에 저장`되는 텍스트 파일이며, key-value 형태로 저장된다.
  - 쿠키는 별도로 삭제처리하거나 유효기간이 만료되지 않는 이상 서버와 통신할 때 **자동으로 주고 받게 된다**.
  - 서버에 특정 API 요청을 했을 때 서버가 응답 시 header 안에 set-cookie 속성으로 쿠키 정보를 담아주면 응답을 받은 브라우저는 쿠키를 브라우저에 `자동으로` 저장한다. (저장된 쿠키정보는 `개발자도구 → 애플리케이션 → 저장용량 → 쿠키` 에서 확인 가능)
  - 서버에서 클라이언트로 보낸다.
  - 서버에 http 요청 할 때 마다 브라우저에 저장되어 있는 쿠키는 `**자동으로` 서버에 보내진다\*\*. (단, 동일한 Origin 또는 CORS를 허용하는 Origin에만 쿠키를 보낸다. ex- 유튜브 서버에서 받은 쿠키는 유튜브 이용 시에만 주고 받을 수 있다.)
  - Origin 이란?
    출처(origin) 이란 protocol + host + port 를 의미합니다.
    <img width="100%" src="https://teamsparta.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fccc90ddf-4a82-494f-a153-ad4037dc7925%2FUntitled.png?table=block&id=b4f47ee6-1e2d-474d-a2ef-09f3e32b95f7&spaceId=83c75a39-3aba-4ba4-a792-7aefe4b07895&width=1370&userId=&cache=v2"/>

  - CORS 란?
    - Cross Origin Resource Sharing(CORS)는 다른 출처에 리소스 요청하는 것을 허용하는 정책이다.
    - 브라우저는 보안상의 이유로 기본적으로 Same Origin Policy(SOP)를 원칙으로 하고 있지만, 서버와 클라이언트 각각 CORS 설정을 통해 상호합의된 웹사이트는 예외적으로 서로 다른 출처(Cross-Origin)임에도 API 요청이 가능하다.
  - 쿠키는 클라이언트에서 직접 추가/수정/삭제 할 수 있다.

- (2) 세션 (session) 이란?
  - [세션이 종료 되었습니다. 다시 로그인 하십시오.]
  - 세션이란 사용자와 서버 간의 `연결이 활성화된 상태`를 의미하는 개념이다. (또는 인증이 유지되고 있는 상태)
  - 로그인 성공 → 서버에서 세션 생성 및 저장(key-value 형식) → key(sessionId)를 브라우저에 응답(by 쿠키)
- (3) 쿠키-세션 인증 방식

  - 로그인/회원가입 시 세션 인증
    <img width = "100%" src="https://teamsparta.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Ffc876fd8-84a3-469b-9637-86067d28b068%2FUntitled.png?table=block&id=4b85f2c9-96ff-4578-bcbf-089a41e26c3f&spaceId=83c75a39-3aba-4ba4-a792-7aefe4b07895&width=1500&userId=&cache=v2">

  - 로그인/회원가입 성공 시 서버에서 쿠키에 sessionId를 담아서 보내준다.
  - 세션 유지 상태 : 서버에서 관리하는 세션 저장소에 회원 데이터가 있다.
  - 세션 만료 상태 : 서버에서 관리하는 세션 저장소에 회원 데이터가 없다.
  - 인가(Authorization) 필요한 API 요청/응답
    <img width="100%" src="https://teamsparta.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fe970e3ca-7f30-4c53-8227-af10c264915c%2FUntitled.png?table=block&id=7e665bed-b578-48cf-97ba-bc3f77bec142&spaceId=83c75a39-3aba-4ba4-a792-7aefe4b07895&width=2000&userId=&cache=v2">
  - 서버는 인가가 필요한 API 요청을 받으면 클라이언트 쿠키에 들어 있는 sessionId를 세션 저장소에 조회하여 있으면 DB에 데이터를 조회하여 응답한다.

- (4) 토큰 (token) 이란?

  - **클라이언트**에서 보관하는 `암호화된 인증 정보`를 의미한다.
  - 세션처럼 서버에서 사용자의 인증 정보를 보관할 필요가 없기 때문에 서버 부담을 줄여주는 인증 수단
  - 웹에서 인증 수단으로 사용되는 토큰은 주로 `JWT (Json Web Token)` 을 이용한다.
  - JWT의 특징
    1. `header.payload.signature` 형식으로 3가지 데이터로 구성되어 있다.
    2. 국제 인터넷 표준 인증 규격 중 하나이다.
    3. 암호화된 토큰을 누구나 복호화하여 payload를 볼 수 있다. → 토큰의 용도는 인증정보(payload)에 대한 보호가 아니라 `위조 방지`
    4. 정보(payload)를 토큰화할 때 signature에 secret key가 필요하고, secret key는 복호화가 아니라 토큰이 유효한 지를 검증하는 데 사용된다.

## 3. JWT 토큰 인증 방식

- (1) JWT 토큰 인증 방식 (중요!)

  - 로그인/회원가입 시 토큰 인증
    <img width="100%" src ="https://teamsparta.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F24ec8fc5-9db0-4f75-af25-54c2b00f3780%2FUntitled.png?table=block&id=ab39929c-4a0a-4562-b589-1da3e13633fc&spaceId=83c75a39-3aba-4ba4-a792-7aefe4b07895&width=1950&userId=&cache=v2">

  - 인가(Authorization) 필요한 API 요청/응답
    <img width="100%" src ="https://teamsparta.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F566c9bc7-fd4d-46e8-92ef-c7ac95b7c2bb%2FUntitled.png?table=block&id=e8b7cf58-a010-40a4-8275-1702ed0a9a5e&spaceId=83c75a39-3aba-4ba4-a792-7aefe4b07895&width=1950&userId=&cache=v2">

  - `Refresh Token` 으로 보안 강화
    리소스 접근 인가를 받기 위해 사용되는 토큰을 Access Token 이라고 부른다.
    Access Token의 만료기간을 길게 잡고 인증상태를 오래 가져갈 경우 서버 부담은 줄어드나 보안성(탈취 당할 경우)에 허점이 있다.
    인증 보안이 중요한 서비스의 경우 인증 시 2개의 토큰 (Access Token, Refresh Token)을 발급하고 Access Token의 기간은 30분 정도로 짧게 가져 가고, Refresh Token은 1~2주 정도로 길게 잡는 경우가 많다.
    이 경우 서버에서는 Access Token이 만료되었을 때 Refresh Token 이 유효한 상태면 새로운 Access Token을 클라이언트에 새로 발급해주고 인증상태를 유지할 수 있도록 하고, Refresh Token 만료 시 다시 로그인하라는 메시지를 응답한다.

- (2) 세션 인증 vs. 토큰 인증
  | | 세션 인증 | 토큰 인증 |
  | ------------------ | --------- | ---------- |
  | 인증정보 저장 위치 | 서버 | 클라이언트 |
  | 확장성(Scale-out) | Bad | Good |
  | 보안성 (상대적) | Good | Bad |
  | 비용 (상대적) | Expensive | Cheap |
