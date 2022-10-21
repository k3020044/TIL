#### 0805 PJT 3주차

---

- **개요**
  
  Bootstrap을 활용해여 영화 소개에 관한 반응형 웹사이트를 구현하는 활동을 함. Component 중에서는 Navbar, modal, carousel, card, list group, pagination을 활용함

- **4L**
  
  1. **Liked**
     
     - 일주일동안 수업시간에 배운 내용을 토대로 직접 html 페이지를 만들어서 html에 대한 이해도를 한층 높일 수 있었다. 또한 모르는 부분들은 bootstrap 공식 홈페이지 검색과 구글링을 통하여 직접 알아보며 해결하면서 좀 더 발전할 수 있는 시간이 되었다
  
  2. **Learned**
     
     - fixed와 sticky의 차이점: navbar나 footer를 fixed position으로 지정하면 스크롤을 내리면서 다른 content가 짤리지만, sticky를 쓰면 짤리지 않음
     
     - 그리드시스템을 적용할 때의 유의사항 
       
       1. 클래스를 아무데나 적용 하면 안된다. 
          
          list group과 table에 그리드시스템을 적용하는 과정에서 ul과 table에 col 클래스를 적용했더니 작동하지 않았다. table에는 table클래스만 적용하고, 상위에 section클래스를 만들고 col을 적용했더니 정상적으로 작동했다.
       
       2. col 적용할때 default 값을 꼭 넣어줘야한다.
          
          default 값을 넣을때 한줄에 다 차지하는 거라면 col-12로 적용하면 된다.
          
          ```html
          <aside class="container">
                <div class="row">
                  <ul class="list-group my-3 col-12 col-lg-2">
                        ......
                  <section class="col-12 col-lg-10">  
                    <table class="table">
          ```
  
  3. **Lacked**
     
     - content 속성을 넣으려고 할때 외부참조로 넣을때 적용이 잘 안될때가 있다. 이런 경우 인라인요소로 직접 넣으면 대부분 해결은 되지만, 외부참조로 넣을 때는 왜 가끔 적용이 안되는지 잘 모르겠다. 직접 더 해보면서 익혀가야겠다.
     
     - 반응형 navbar 구현을 완전히 하지못하였다. md이하 범위에서는 메뉴바 형식의 button을 만들어야 하는데, bootstrap에서 코드를 찾기는 했지만 적용이 잘 안됐다. 또한 modal도 잘 적용이 안돼서 bootstrap 페이지를 더 살펴봐야겠다.
  
  4. **Longed for**
     
     - 코드 구조에 대해 더 학습을 해야할 것 같다. 특히 부모-자식으로 구성되는 코드 내에서 각각의 요소에 대한 정확한 이해가 부족하여 오류가 발생하는 것 같다. bootstrap 예시들을 참고하여 잘 짜여진 코드들을 분석을 하면서 익혀나가야겠다.

- **느낀 점**
  
  코딩을 배우기 시작한지 3주가 되었는데 여전히 배워야할 것들 투성이지만, 그래도 조금씩 발전해나가는 모습에 뿌듯함을 느낀다. 나름대로 고민하고 찾아보면서 80% 이상은 완성할 수 있었다. 그 과정에서 재미를 느끼기도 하여서 앞으로도 차근차근 성장해 나가도록 해야겠다.

  
