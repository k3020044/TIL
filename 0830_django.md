**framework** : 서비스 개발에 필요한 기능들을 미리 구현해서 모아 놓은 것

**www(world wide web)** : 전 세계에 퍼져 있는 연결망. 인터넷을 이용한다는 것은 전세계의 컴퓨터가 연결돼있는 하나의 인프라를 이용하는 것

client - server 구조 : client는 인터넷에 연결된 장치, 웹 브라우저, 서비스를 요청하는 주체. server는 컴퓨터, 응답하는 주체

web page : static web page - 같은 상황에서 모든 사용자에게 동일한 정보를 표시, dynamic web page - 사용자의 요청에 따라 웹 페이지에 추가적인 수정이 되어 전달됨

- MTV design pattern
  
  소프트웨어 디자인 패턴은 자주 사용되는 구조와 해결책을 구조화 해둔 것. 이로 인해 복잡한 커뮤니케이션이 간단해지는 장점이 있음.
  
  **MVC(model-view-controller)** 디자인 패턴을 기반으로 변형한 것. mvc는 각 부분을 독립적으로 개발할 수 있어 분업이 쉬워지는 장점이 있음.
  
  MVC패턴과 크게 다르지는 않지만 부르는 이름이 다름. model-template-view
  
  **model** : 데이터와 관련된 로직을 관리, 데이터 구조를 정의하고 데이터베이스의 기록 관리
  
  **template** : 레이아웃과 화면을 처리. mvc에서 view의 역할
  
  **view** : model&template과 관련된 로직을 처리해서 응답을 반환. mvc에서 controller의 역할. model에 접근해서 데이터를 가져오고, 가져온 데이터를 template로 보내 화면을 구성하고, 구성된 화면을 응답으로 만들어 client에게 반환. mvc에서 controller의 역할

- django 시작하기
  
  1. **python -m venv** {venv} : venv라는 이름의 가상환경 만들기
  
  2. **source venv/Scripts/activate** : 가상환경으로 들어가기
  
  3. **pip install django==3.2.13** : 장고 설치
  
  4. **pip freeze > requirements.txt** : 현재 디렉토리 텍스트 파일로 저장
     
     **pip install -r requirements.txt** : requirements.txt에 있는 디렉토리 저장
  
  5. **django-admin startproject {firstpjt} .** : firstpjt라는 프로젝트 만듦. 점 뒤에 찍어야 편함
  
  6. **python [manage.py](http://manage.py) runserver** : 서버 실행
  
  7. **python [manage.py](http://manage.py/) startapp articles** : articles라는 어플리케이션 생성
  
  8. firstpjt - [settings.py](http://settings.py) - installed_apps 리스트에 articles 앱 추가. 리스트 내 순서는 local > third party > django apps
  
  9. url > view >template 순으로 코드 작성 = 데이터 흐름의 순서
     
     - firstpjt 폴더 내 urls.py에서 views.index 작성
     - articles 앱폴더 내 views.py에서 index함수 정의
     - articles 앱폴더 내 templates 폴더 만들고 index.html 페이지 만듦

- django template
  
  데이터 표현을 제어하는 도구이자 표현에 관련된 로직
  
  **DTL(django template language)** : 프로그래밍 구조(if, for 등)를 사용할 수 있지만 파이썬 코드로 시행되지는 않음
  
  1. variable
  2. filters : \ 키로 적용함. 여러 필터 동시에 chain도 가능
  3. tags : 출력 텍스트를 만들거나 반복 또는 논리 수행 ex. if, for

- template inheritance
  
  ```python
  {% extends 'base.html' %} > base.html 상속해온다는 뜻
  
  {% block content %} 
  이 사이에 해당 html 내용 입력하면 됨. 나머지는 base.html에서 상속됨
  {% endblock (content) %}
  ```
  
  기본적으로 부모 html은 같은 directory depth에 있어야 하며, 다른 경로를 추가할 때는
  
  settings.py에서 ‘DIRS': [BASE_DIR / 'templates',] 로 따로 지정해야함. manage.py와 같은 depth에 위치해있는 ‘templates’ 폴더에서 검색한다는 뜻.

- form element
  
  데이터를 어디로(action), 어떤 방식으로(method) 보낼지 정의. method는 GET, POST
  
  ```html
  <form action="" method="">
    <label for="서치">search : </label>
    <input type="text" name="search" id="서치"> # label이랑 id 같아야 연결됨
          # type의 기본값은 text
    <input type="submit">
  </form>
  ```
  
  GET 방식에서는 url의 ?key=value&key=value 형태로 데이터 전달함(query string parameters)
  
  value = request.GET.get('search') 로 입력한 데이터 가져올 수 있음

- URL
  
  **trailing slashes** : url 끝에 / 가 없다면 자동으로 붙여주는 것이 기본 설정. / 있는 것과 없는 것은 다르기에 복수의 페이지에서 같은 콘텐츠가 존재하는 것을 방지하기 위함임
  
  variable routing : url 주소를 변수로 사용. url의 일부를 변수로 지정하여 view 함수의 인자로 넘김. ex. 인스타그램 url 주소에서 아이디 부분만 변경하면 다른 유저 계정으로 이동됨
  
  view 함수 내부에서 <변수명> 으로 정의함

- App URL mapping
  
  앱이 여러개일 때 프로젝트의 [urls.py](http://urls.py) 하나로 모든 앱 관리가 힘들기 때문에(각각 지정해서 연결해야 함), 각 앱 내부에 [urls.py](http://urls.py) 만들어서 프로젝트의 urls.py와 연결함.
