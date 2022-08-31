- **Namespace**
  
  1. **url namespace** : 다른 앱에서 url name이 중복될때, app name을 정해서 중복 문제 해결
  
  ```python
  # urls.py 에서 app name 정함
  app_name = 'pages'
  #html 파일에서 url태그 작성할때 app_name 명시, 안하면 norevercematch 에러 발생
  {% url 'pages:index' %}
  ```
  
  1. **template namespace** : 다른 앱 내에서 동일한 이름의 html 파일이 존재할때 장고는 이를 구분할 수 없음. articles/templates/index.html, pages/templates/index.html 파일 존재할때 장고에서는 templates 이후부터 검색하기 때문에 html 파일 이름이 중복돼면 settings.py에서 먼저 등록된 앱 기준으로 인식함.
     
     그래서 templates 뒤에 폴더 하나 더 만들어서 구분함. new repository
     
     articles/templates/**articles/**index.html, pages/templates/**pages/**index.html

- **Model** : 데이터를 구조화하고 조작하기 위한 것
  
  - database : 구조화 작업을 보다 쉽게 하기 위해 데이터를 체계화하여 저장한 것
    - schema 스키마 : structure, 자료의 구조, 표현 방법, 관계 등을 정의
    - table : field(열,column)와 record(행)를 사용해 조직된 데이터 요소들의 집합
    - pk(primary key) : 각 record의 고유한 값, 기술적으로 다른 항목과 중복될 수 없는 unique한 값 ex. 주민번호
    - query : 데이터를 추출하거나 조작하는 명령어
  
  model class 1개 == database table 1개. 데이터베이스의 layout
  
  model class를 만드는 것은 table의 schema를 정의하는 것
  
  ```python
  class Article(models.Model): #models의 Model을 상속받음
      title = models.CharField(max_length=10) # 클래스를 통한 인스턴스 생성. 
  # title은 필드 이름, models.charfield는 타입. 두개가 곧 스키마. max_length는 필수 인자. 최대 255까지. 유효성 검사
      content = models.TextField() # textfield는 길이 제한이 더 김. 유효성 검증 x
  # id는 자동으로 생성됨
  ```

- **migrations**
  
  django가 modle에 생긴 변화(필드 추가 등)를 database(db.sqlite3)에 반영하는 방법
  
  ```python
  python manage.py makemigrations
  >> migrations.py 하위 파일로 0001_initial.py 생성됨= 설계도(blueprint)
  ```
  
  ```python
  python manage.py migrate
  >> makemigrations로 만든 설계도를 실제 데이터베이스에 반영. model과 database의 동기화
  기본 내장앱들이 있어서 출력되는 결과물이 많음 
  ```
  
  model 만들고 migration 끝난 다음에 model에 변경 사항이 생긴다면?
  
  mode 변경 후 migrate 하면 column은 빈 값으로 추가될 수 없기 때문에 default 값 설정하라고 메세지 뜸. 1 누르고 enter 누르면 설계도 생성 완료
  
  변경할때 같은 class에서 변경했다면, 새로 만든 설계도는 첫번째 설계도에 dependent 하게됨. 설계도 파일 dependencies 리스트에서 확인
  
  ```python
  class Article(models.Model): 
      title = models.CharField(max_length=10) 
      content = models.TextField() 
          # auto_now_add = 데이터가 최초로 만들어진 날짜, 시간으로 자동 초기화    
          created_at = models.DateTimeField(auto_now_add=True)
          # auto_now = 최종 수정 날짜, 시간으로 자동 갱신
      updated_at = models.DateTimeField(auto_now=True)
  ```

- **ORM (objext-relational-mapping)**
  
  migration된 설계도는 python으로 작성되어있지만, database는 sql 언어를 쓰기 때문에 중간에 번역을 담당하는 역할을 함. django ORM
  
  장점 : 객체 지향적 접근으로 인한 높은 생산성
  
  단점 : ORM만으로는 세밀한 조작이 불가능한 경우가 있음

- **QuerySet API**
  
  ```python
  Article.objects.all() : 현재 있는 데이터 보여주는 명령어
  # article : model class, objects : manager 자동으로 추가, all() : method. queryset AP
  ```
  
  database 에서 전달받은 객체 목록을 ORM이 QuerySet 이라는 자료 형태로 전달. 순회가 가능함. 반환할 객체가 1개일때는 model클래스의 인스턴스로 반환
  
  QuerySet API = QuerySet과 상호작용하기 위해 사용하는 도구(method, 연산자 등)
  
  - create 데이터 생성
    
    ```python
    article = Article() # 인스턴스 생성
    article.title = 'first' # data 입력
    article.content = 'django!'
    article.save() # database에 저장
    # 여기서 print하면 <Article: Article object (1)> 1 = id(=pk)임
    article = Article(title='second', content='django!') # 두번째 데이터 저장
    article.save()
    Article.objects.create(title='third', content='django!') # save까지 해서 따로 안해도 됨
    ```
  
  - read

```python
all() # 전체 데이터 조회
get() # pk 같은 단일 데이터 조회. 객체 없으면 doesnotexist 에러, 둘 이상이면 multipleobjectsreturned 에러
Article.objects.get(pk=1) >> <Article: Article object (1)>
filter() # 특정 조건의 데이터 조회, 없으면 빈 queryset 반환, 1개여도 단일 객체 아니라 하나의 queryset 반환
Article.objects.filter(content='django!')
Article.objects.filter(content__contains='dj') # content에 'dj' 포함된거 출력 
```

- update 수정
  
  ```python
  article = Article.objects.get(pk=1) # 바꿀 객체 호출
  article.title = 'byebye' # 데이터 입력
  article.save() # 저장
  ```

- delete
  
  ```python
  article = Article.objects.get(pk=1) # 바꿀 객체 호출
  article.delete() # 삭제
  # 삭제 후 데이터 다시 입력하면 1번이 아니라 그 다음 pk에 저장됨
  ```


