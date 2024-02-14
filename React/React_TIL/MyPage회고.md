1. MyPage.jsx

   ```jsx
   useEffect(() => {
     if (!user.userId) {
       alert("잘못된 접속입니다.");
       navigate("/");
     }
   }, [navigate, user]);
   ```

   ```jsx
   const user = useSelector((state) => state.user); //reducer에서 user 가져오기
   const [nickname, setNickname] = useState("");
   const [image, setImage] = useState("");

   const dispatch = useDispatch();
   const navigate = useNavigate();
   const storage = getStorage();

   const handleUpdate = async () => {
     if (nickname.length > 3 && user.nickname !== nickname) {
       alert("닉네임이 변경되었습니다.");
       setNickname("");
       await updateUserInfoApi(user.userId, nickname);
       dispatch(updateUserData({ nickname }));
     }

     if (image !== "") {
       alert("사진이 변경되었습니다.");
       setImage("");
       const image = await changeUserImage();
       dispatch(updateUserData({ image }));
     }
   };
   ```

   ```jsx
   // input한 이미지 관리 함수, 이미지를 선택하면 호출된다.
   const handleImageChange = (e) => {
     const blob = new Blob(e.target.files);
     e.target.value = "";
     setImage(blob);
   };
   ```

   - `e.target.files` : 사용자가 선택한 파일들의 목록을 나타내는 FileList 객체
   - `Blob` : 선택한 파일을 blob으로 변환
   - Blob은 바이너리 데이터를 나타내는 객체로, 이미지와 같은 미디어 파일을 다루는 데 사용된다.

   ```jsx
   // 사용자의 이미지를 Cloud Storage에 업로드하고, 업로드된 이미지의 다운로드 URL을 반환
   const changeUserImage = async () => {
     if (image === "") {
       alert("이미지를 입력하세요");
       return;
     }

     const imageRef = ref(storage, `users/${user.userId}`);
     await uploadBytes(imageRef, image);

     const downloadURL = await getDownloadURL(imageRef);

     return downloadURL;
   };
   ```

   - `ref`: storage 객체와 사용자의 고유 식별자인 users/${user.userId}를 조합하여 참조를 만듬
   - `uploadBytes`: 이미지를 해당 참조에 업로드, Blob 데이터를 받아서 해당 데이터를 Cloud Storage에 업로드하는 함수
   - `getDownloadURL`: 해당 이미지의 다운로드 URL을 가져옵니다. 이 URL은 나중에 이미지를 표시하거나 다운로드할 때 사용
   - `console.log(image)`:
     _Blob {size: 32171, type: ''}_
     size: 32171
     type: ""
     [[Prototype]]: Blob
   - `console.log(imageRef)` : 업로드할 이미지의 저장 위치를 나타내는 Cloud Storage의 경로가 담겨 있다!!
     Reference {\_service: FirebaseStorageImpl, \_location: Location}
   - **`users/${user.userId}`**: Firebase의 Cloud Storage에서 이미지를 저장할 경로
     - `${user.userId}`: 사용자의 고유 식별자
     - 해당 경로는 `users`라는 폴더(또는 디렉토리) 안에 `user.userId`에 해당하는 이름의 하위 폴더를 생성

   ```jsx
   <StAvatar
                   src={user.image === '기본이미지' ? userDefaultImg : user.image}
                   alt='기본이미지'
                 />
                 <input
                   type='file'
                   accept='image/*'
                   onChange={handleImageChange}
                   style={{ width: '80%' }}
                 />
   ```

   - **`accept`** : HTML **`<input>`** 엘리먼트에서 사용, 허용된 파일 유형을 지정 (제한)
   - **`accept='image/*'`** : 사용자가 이미지 파일만 선택할 수 있도록 허용!
   - '\*' : 모든 이미지 파일 형식을 포함한다
   - 만약 JPEG 이미지만을 허용하고 싶다면 `accept='image/jpeg'`와 같이 사용

2. userReducer

   ```jsx
   const userReducer = (state = initialState, action) => {
     switch (action.type) {
       case POST_DATA:
         return {
           ...state,
           ...action.user,
         };
       case UPDATE_USER_DATA: //액션 타입
         return {
           ...state, //현재 상태
           ...action.updateUser, //action 매개변수는 수행된 액션
         };
       default:
         return state;
     }
   };

   // UPDATE_USER_DATA 타입의 액션을 생성, 업데이트할 사용자 데이터를 전달
   export const updateUserData = (updateUser) => ({
     type: UPDATE_USER_DATA,
     updateUser,
   });
   ```

3. apis/users.js

   ```jsx
   export const updateUserInfoApi = async (userId, nickname) => {
     try {
       await updateDoc(doc(db, "users", userId), { nickname });
       // userId에 해당하는 문서를 찾고, 해당 문서의 'nickname' 필드를 전달된 값으로 업데이트
     } catch (error) {
       console.error(error);
     }
   };
   ```

시간이 촉박해서 마이페이지 기능 구현을 다 못한 점이 아쉽다.

1. 닉네임 변경은 doc import를 잘못 해와서 계속 오류가 났고 reducer에 오류가 있었다.
2. 사진 변경은 storage를 쓰는건데 복잡했다…
