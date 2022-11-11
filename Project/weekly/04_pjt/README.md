### 0902 PJT 4주차

---

- **개요**
  
  django를 활용하여 영화 관련 데이터를 생성하고 이를 웹페이지로 구현하는 활동을 함
  
  다양한 model field를 활용하여 웹페이지를 구현할 데이터베이스를 생성하고, 데이터를 CRUD(create, read, update, delete) 하는 함수를 통하여 사용자가 이용할 수 있는 html 페이지를 만듦

- **4L**
  
  1. **Liked**
     
     - 아무 것도 없는 '무'의 상태에서 '유'를 창조하는 과정을 통해, django를 활용하여 웹페이지를 구성하는 원리에 대한 이해를 높일 수 있었다
     - url > views > templates 으로 진행되는 데이터의 흐름을 체득할 수 있었다
  
  2. **Learned**
     
     - model field
       
       ```html
       class Movie(models.Model):
           title = models.CharField(max_length=20)
           audience = models.IntegerField()
           release_date =models.DateTimeField(auto_now=False, auto_now_add=False)
           genre = models.CharField(max_length=30)
           score = models.FloatField()
           poster_url = models.URLField(max_length=200)
           description = models.TextField()
       ```
     
     - form의 다양한 태그들
       
       - 여러 항목 중 하나 선택하는 태그
         
         ```html
         <select name="genre" id="genre">
               <option value="" disables selected></option>
               <option value="스릴러">스릴러</option>
               <option value="스릴러">애니메이션</option>
         </select>
         ```
       
       - 긴  text 작성할 때 쓰는 태그
         
         ```html
         <textarea name="description" id="description"></textarea>
         ```
       
       - 초기화 하는 태그
         
         ```html
         <input type="reset">
         ```
  
  3. **Lacked**
     
     - 전체적인 흐름은 익혔지만, 아직 구체적으로 코드 작성하는 것은 조금 어색해서 찾아 보면서 해야지만 할 수 있다. 또 오류가 발생했을 때 오류를 발견하기 까지 시간이 걸린다
  
  4. **Longed for**
     
     - 이번주에 처음 django를 접하게 되어 지금은 연습이 많이 부족한 상태인 것 같다. 과목 평가도 대비하고, 실력도 향상시킬겸 꾸준히 연습하며 학습해야겠다

- **느낀점**
  
  4번째 pjt 활동이었는데, 지금까지의 pjt 활동을 통틀어 가장 완성도가 높았다. 또 스스로 모든 부분을 구현해내서 그동안 공부하면서 실력이 조금 향상된듯 하여 뿌듯한 점도 있다. 하는 과정에 있어서 오류도 많이 발생했지만 코드를 다시 찬찬히 보고, 검색 등을 통하여 해결할 수 있었다. 특히 단순한 스펠링 오타나 '.' 혹은 ',' 등의 기호 오류 기재를 보다 주의해야할 것 같다.  
