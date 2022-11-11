- 개요
  
  TMDB API key로 영화관련 데이터를 AJAX 통신으로 가져오고, Vue router를 활용하여 영화 관련 정보를 제공하는 SPA를 제작하는 활동을 함
  
  ---

- 4L
  
  1. LIKED
     
     지난 프로젝트에 이어서 페어 프로그래밍을 통해 프로젝트를 진행하여서 협업을 통해 결과물을 만들어 내는 활동을 함. 지난 프로젝트의 페어와는 다른 페어와 진행하게 되어서 사람마다 서로 다른 코드 및 업무 스타일을 경험할 수 있었음.
  
  2. LEARNED
     
     - api key 받아오는 법
       
       ```jsx
       // .env.local 만듦
       VUE_APP_TMDB = api key 작성
       
       // MovieView.vue
       methods: {
          getMovie() {
            const TMDBAPI = process.env.VUE_APP_TMDB
            const TopRatedURL = <https://api.themoviedb.org/3/movie/top_rated?api_key=${TMDBAPI}&language=en-US&page=1>
                  axios({
              method: 'get',
              url: TopRatedURL,
            })
       ```
     
     - 문자열 안에 변수 있을 때 문자열 + 변수 형태도 가능(string이기 때문에)
       
       ```jsx
       <div v-for="(movie, index) in this.movieList" :key="index">
        // 이미지 url에 변수 있을때 고정 문구를 따로 posterURL로 data에 저장하고 변수를 + 로 연결함
          <img :src="posterURL + movie.poster_path" alt="">
        <p>{{ movie.title }}</p>
        <p>{{ movie.overview }}</p>
       
       data() {
          return {
            movieList: [],
            posterURL: '<https://image.tmdb.org/t/p/w500/>'
          }
        },
       ```
  
  3. LACKED
     
     프로젝트를 할때마다 기본 개념 숙지에 대한 중요성을 매번 느끼게 됨. 개념에 대한 이해가 확실하면 이전 코드를 보지 않고도 작성할 수 있을텐데, 아직은 이전에 학습한 코드 없이는 자유롭게 코드를 구현할 수 없는 것 같음. 따라서 지속적인 학습과 이해를 통해서 성장해 나가야겠다는 생각이듦.
  
  4. LONGED FOR
     
     사람마다 코드 작성 스타일이 다르고 업무 진행 방식도 다른 만큼 유연한 자세를 가지고 여러 사람들과 무리 없이 소통할 수 있는 사람이 되어야 겠다는 생각을 하게됨.
     
     ---

- 느낀점
  
  협업을 진행하며 의견 충돌이 있을 때 근거를 들어 자유롭게 의견을 교환하는 것이 업무 진행에 있어 중요하다는 것을 느끼게됨. 의견 충돌은 피해야할 대상이 아니라 효율적인 협업을 위해 필수적이며, 의견 충돌을 통해 더 나아가기 위해서는 나의 실력과 지식이 충분해야 한다는 것도 깨닫게됨.
