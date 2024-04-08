## Database 짜는법
오늘은 최종 프로젝트의 데이터베이스를 짰다. 데이터가 복잡해서 구조를 짜는게 쉽지 않았다.
<br>튜터님의 도움을 받았는데 알게된 점이 몇가지 있다.

### 관계형 데이터베이스
```
const type Room = {
room_id: string;
room_title: string;
room_contents: string
room_leader: string;
members: string[];
locacion: string;
status: boolean;
}
```
members를 보면 이 방에 참여하고 있는 사람들의 id를 배열 형식으로 넣으려고 하는걸 볼 수 있다.
<br>하지만 이는 noSQL 방법으로 내가 있는 방을 확인하고 싶을 때 방 테이블을 다 돌아야하기 때문에 적합하지 않다.

<img width="637" alt="image" src="https://github.com/seoyeon-1206/TIL/assets/128902050/411ce995-65d4-4a24-a8b5-6a62e0b8bfec">

따라서 이렇게 참가자 테이블을 따로 빼서 만들었다. 룸 아이디만을 가지고 내가 참여한 방을 알 수 있게 된 것이다. 

### PK, FK
- PK(primary key: 기본키) : 테이블 행의 여러 정보들 중 행을 식별하는 역할, 비어있으면 안되고(NOT NULL) 중복되어서도 안된다(Unique).
  <br> 식별을 할 때 테이블의 정보를 최대한 빠르게 검색해야 하므로 간단한 정보일 수록 좋다.

- FK(foreign key:외래키)는 참조하는 테이블과 참조되는 테이블의 관계를 나타낸다.
  <br>다른 테이블의 정보를 참조하기 위한 룸 아이디는 참가자 테이블 내에서 FK(외래키)가 된다.

### 삭제되었을 때를 고려하기
<img width="1126" alt="image" src="https://github.com/seoyeon-1206/TIL/assets/128902050/09d7eb7f-53fb-410d-98ef-f9e6d4c4c901">

방이 삭제되었을 때 그 방에 접근을 하면 유저에게 어떻게 보여줄것인가?
<br>
'이 페이지는 삭제된 페이지 입니다.'를 많이 보았을거다.
<br>
이런 경우에는 방 테이블의 column을 아예 삭제하는 것이 아니라 삭제 상태를 boolean으로 두고 유저에게 보여줘야 더 좋은 UX가 될것이다.

