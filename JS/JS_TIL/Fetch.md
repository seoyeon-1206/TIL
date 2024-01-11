```jsx
const showMovieDetail = async () => {
    try {
        const details = await fetchMovieDetail();

        const movieDetail = document.querySelector("#detail-list");
        movieDetail.innerHTML = `
        <li class="movie-detail" id=${details.id}>
            <img src="https://image.tmdb.org/t/p/w500${details.poster_path}" alt="${details.title}">
            <h3>${details.title}</h3>
            <p>개봉날짜: ${details.release_date}</p>
            <p>장르: ${details.genres.map(genre => genre.name).join(', ')}</p>
            <p>평점: ${details.vote_average}</p>
            <p>상영시간: ${details.runtime}</p>
            <p>overview: ${details.overview}</p>
        </li>
        `;
    } catch (error) {
        console.error("영화 상세 정보를 가져오는 중 오류 발생:", error);
    }
};

async function fetchMovieDetail() {
    const movieId = 123; // 임시 ID

    const options = {
        method: "GET",
        headers: {
            accept: "application/json",
            Authorization: 'Bearer eyJhbGciOiJIUzI1NiJ9.eyJhdWQiOiJiNTA0YmVlZjQxZjYxNGQxMWZlZDQxZTAwYWFmZjg0YSIsInN1YiI6IjY1OTc5NmJmNWNjMTFkNzc2ZTdkODQyNCIsInNjb3BlcyI6WyJhcGlfcmVhZCJdLCJ2ZXJzaW9uIjoxfQ.IZ78_IkpDpSstACHGP0dddrf52ji2H0-nnKLnae_aQM'

        },
    };

    const response = await fetch(
        `https://api.themoviedb.org/3/movie/${movieId}?language=ko-KR`,
        options
    );

    if (!response.ok) {
        throw new Error(`영화 상세 정보를 가져오지 못했습니다: ${response.status}`);
    }

    const data = await response.json();
    return data;
}

showMovieDetail()
```

오늘은 api를 가져와서 영화의 이미지, 제목, 개봉날짜, 장르 등을 가져오는 기능을 구현했다.

- api 문제
    - [https://api.themoviedb.org/3/movie/?language=ko-KR](https://api.themoviedb.org/3/movie/${movieId}?language=ko-KR) 이 api를 사용했는데 계속 함수 호출이 안됐다.
    - 알고보니 detail api는 특정 movie의 id를 지정해줘야 했다.
    - movieId를 지정하고 api에 ${movieId} 넣어서 데이터를 가져올 수 있었다.
- fetchMovieDetail함수에서는 이미 API 호출에 필요한 헤더에 인증 정보가 담겨있기 때문에 별도의 API 키가 필요하지 않다는 걸 알게 되었다.
- 데이터 확인하는 방법
    - console.log(data)→ data가 response
    - 검사에서 확인하기