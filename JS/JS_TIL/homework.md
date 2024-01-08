## 개인과제 해석 정리

<img width="50%" src="https://teamsparta.notion.site/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F83c75a39-3aba-4ba4-a792-7aefe4b07895%2F6c1cce2d-1ca5-4e07-9127-24839a6c1a6a%2FUntitled.png?table=block&id=e47a3099-5ae9-4c76-8e0f-43663f25859a&spaceId=83c75a39-3aba-4ba4-a792-7aefe4b07895&width=1510&userId=&cache=v2"/>

### html

```html
<link rel="stylesheet" href="./style/style.css"> <!-- css불러오기 -->
<link rel="stylesheet" href="reset.css">
<script type="module" src="/src/main.js"></script><!-- js불러오기 -->
<!-- defer: JS실행을 지연하겠다! 파싱할때까지! 근데 타입 모듈쓰면 자동으로 defer내포-->
<!-- module: 특정 관심사에 해당하는 기능을 수행하는 변수와 함수의 모음 + 독립된 scope-> import 쓰려면-->

<ul id="card-list" onclick="hadleClick(event)"></ul> <!-- event.currentTarget : 이벤트 핸들러가 등록되어 있는 요소 -->
```

### js 구조화
```jsx
import { generateMovieCards } from "./movie.js"; //import로 가져오기
import {handleSearch} from "./search.js"

generateMovieCards();

const searchInput = document.querySelector("#search-input") //id가져오기
searchInput.focus() //검색창에 input창 커서가 깜박 거리는 메소드

const form = document.querySelector("#search-form");
form.addEventListener("submit", (event) => { //form태그는 submit시 새로고침
    event.preventDefault(); //새로고침 막아줘
    handleSearch(searchInput.value);
});
```

- js 파일 분리했을 때 import로 가져오기

```jsx
//영화 데이터 가져와서 화면에 나타나기
export const generateMovieCards = async () => {
    const movies = await fetchMovieData();

    const cardList = document.querySelector("#card-list");
    cardList.innerHTML = movies //ul태그 안쪽에 movies라는 영화담긴 배열
    .map( //밑에 이런 형태의 객체를 변환시키기 위해 map을 씀 map은 배열
        (movie) => `
        <li class="movie-card" id=${movie.id}>
              <img src="https://image.tmdb.org/t/p/w500${movie.poster_path}" alt="${movie.title}">
              <h3 class="movie-title">${movie.title}</h3>
              <p>${movie.overview}</p>
              <p>Rating: ${movie.vote_average}</p>
          </li>`
    ).join(""); //베열을 문자열(innerHTML)로 하려고

    cardList.addEventListener("click", handleClickCard); //ul태그에 addEventListener로 "click"할때 함수 실행해

    //이벤트 위임 : 하위요소에서 발생한 이벤트를 상위요소에서 처리하도록
    function handleClickCard(event) {
        if(event.target === cardList) return; //카드 밖 클릭

        if(event.target.matches(".movie-card")){ //li태그 클릭
            alert(`영화 id: ${event.target.id}`); //event.target: 클릭이 일어난 장소
        }else {
            //카드의 자식 태그(img, h3, p)클릭 시 부모의 id로 접근 -> 이벤트 위임 child눌러도 parent(ul태그) 이벤트도 작동 -> event 하나만 작성하면 됌
            alert(`영화 id: ${event.target.parentNode.id}`);
        }
    }
}
//ul태그 안에 있는 html을 문자열로 변환한게 innerHTML
```

```jsx
//영화 검색 기능 구현
export const handleSearch = (searchKeyword) => {
    const movieCards = document.querySelectorAll(".movie-card"); //모든 무비카드 (li태그 하나하나)선택

    movieCards.forEach((card) => {
        const title = card.querySelector(".movie-title").textContent.toLowerCase; //card는 li태그 안
        const searchedValue = searchKeyword.toLowerCase();

        if(title.includes(searchedValue)) {
            card.style.display = "block"; //card 즉 li태그 style에 접근해서 보여줄지 말지 결정
        } else {
            card.style.display = "none";
        }
    });
};
```