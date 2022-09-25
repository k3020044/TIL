### django authentication system

인증(authentication)과 권한 부여(authorization)를 함께 처리함

django.contrib.auth 기본 내장앱에 기능 있음

accounts 라는 이름으로 앱을 생성함. django 내부적으로 인증 관련된 것들은 accounts라는 이름으로 사용하기 때문에

기본 user model을 custom user model로 대체해야함 >> django에서 제공하는 model의 기본 이증 요구사항이 내가 만드려는 것과 일치하지 않을 수 있기 때문에

ex. 회원가입시 username이 아닌 email을 사용하려는 경우

user model 설정한 후에 migrations 해야함 >> 프로젝트 중간에 변경하면 모델 관계에 영향을 미치기 때문에 작업이 어려워짐

```python
# user model : settings.py의 부모인 global.settings.py에서 정해져 있음
AUTH_USER_MODEL = auth.User # 기본값. auth앱(내장앱)의 User 모델
```

- coustom user model 대체하기
  
  1. Abstract를 상속받는 coustom user class 작성
     
     ```python
     #models.py
     from django.db import models
     from django.contrib.auth.models import AbstractUser
     
     class User(AbstractUser):
        pass
     ```
  
  2. [settings.py](http://settings.py) 변경
     
     ```python
     AUTH_USER_MODEL = 'accounts.User'
     ```
  
  3. admin.py에 custom user model 등록
     
     ```python
     #admin.py
     from django.contrib import admin
     from django.contrib.auth.admin import UserAdmin
     from .models import User
     
     admin.site.register(User, UserAdmin)
     ```

- 데이터베이스 초기화 (프로젝트 중간에 custom user model 대체하는 경우)
  
  1. migrations 파일 삭제. 번호가 붙은 파일만 삭제
  2. dbsqlite3 삭제
  3. migrations 진행(makemigrations, migrate)

### cookie & session

- http 특징
  
  1. 비연결지향(connectionless) : 서버는 요청에 대한 응답을 보낸 후 연결 끊음
  2. 무상태(stateless) : 연결을 끊는 순간 상태 정보가 유지되지 않음

- **cookie**
  
  로그인 페이지에서 로그인하고 다른 페이지로 이동하면 상태 정보는 유지되지 않기에 로그아웃 되는데, 이를 방지하기 위해 쿠키와 세션이 존재함
  
  개념 : 서버가 사용자의 웹 브라우저에 전송하는 작은 데이터 조각
  
  원리 : 브라우저는 쿠키(서버에서 전달받음)를 로컬에 데이터 형식으로 저장해 놓았다가 동일한 서버에 재요청 시 저장된 쿠키를 함께 전송(로그인 정보를 전송)
  
  목적 : 세션 관리(**session management**, 로그인, 아이디 자동완성, 장바구니 저장), 개인화(**personalization**, 사용자 테마), 트래킹(**tracking**, 사용자 행동 분석)
  
  **session** : 사이트와 특정 브라우저 사이의 상태를 유지시키는 것
  
  클라이언트가 서버에 접속하면 서버가 특정 session id를 발급하고, 클라이언트는 session id를 쿠키에 저장했다가, 재접속할 때 이 session id가 포함된 쿠키를 전달함
  
  수명 : session cookie는 현재 session이 종료되면 삭제됨, persistent cookies는 지정된 기간이 지나면 삭제됨
  
  django는 database-backed sessions 저장 방식을 사용하고, django_session 테이블에 관련 데이터 저장함(session_key(사용자에게 전하는 값), session_data, expire_date)

### log in

로그인은 session을 create하는 과정

1. 로그인 페이지 작성

```python
# accounts/urls.py
urlpatterns = [
    path('login/', views.login, name='login'),
]
```

2. views.login 함수 작성

```python
#accounts/views.py
from django.shortcuts import render, redirect
from django.contrib.auth.forms import AuthenticationForm
from django.contrib.auth import login as auth_login

def login(request):
    if request.method == 'POST': # 인증과정
        form = AuthenticationForm(request, data=request.POST) # 첫번째 인자는 request, 두번째가 data임
        if form.is_valid(): #유효한 id,pw이면
            auth_login(request, form.get_user()) # 함수 이름 중복되므로 바꿔줌
            # get_user : 유효성 검사를 통과한 사용자 객체를 반환
            return redirect('articles:index')

    else: # 로그인 페이지 보여주는 과정
        form = AuthenticationForm()
    context = {
        'form' : form,
    }
    return render(request, 'accounts/login.html', context)
```

3. html 페이지 작성

```python
#accounts/login.html
{% extends 'base.html' %}

{% block content %}
<h1>login</h1>
<form action="{% url 'accounts:login' %}" method="POST">
    {% csrf_token %}
    {{ form.as_p }}
    <input type="submit">
</form>
{% endblock content %}
```

4. base.html 수정

```python
<div class="container">
    <h3>{{ user }}</h3> # context에 user변수 없지만, settings.py context processors에서 자동으로 설정돼있음
    <a href="{% url 'accounts:login' %}">login</a>
    {% block content %}
    {% endblock content %}
  </div>
```

### log out

로그아웃은 session을 delete하는 과정

반환 값이 없으며, 로그인하지 않은 경우 오류를 발생시키지 않음

session data를 database에서 삭제하고, 클리이언트의 cookie에서도 session id를 삭제함. session data만 삭제해도 매칭되는 것이 없기때문에 로그인은 안되지만, 다른 사람이 이전 사용자의 session data에 access하는 것을 방지하기 위해서임

1. url 작성

```python
urlpatterns = [
    path('login/', views.login, name='login'),
    path('logout/', views.logout, name='logout'),
]
```

2. views.logout 작성

```python
from django.contrib.auth import logout as auth_logout
def logout(request):
        auth_logout(request)
        return redirect('articles:index')
```

3. base.html 수정

```python
<form action="{% url 'accounts:logout' %}" method="POST">
    {% csrf_token %}
    <input type="submit" value="logout">
    </form>
```

### 유저 생성 및 관리

- 회원 가입

user를 create하는 것, UserCreationForm 사용. admin과 달리 권한이 없는 user가 생성되며, username, password1, password2(비밀번호 확인용)의 3가지 필드 가짐

```python
#1. urls.py 작성
urlpatterns = [
    path('login/', views.login, name='login'),
    path('logout/', views.logout, name='logout'),
    path('signup/', views.signin, name='signup'),
]

#2. forms.py
from django.contrib.auth.forms import UserCreationForm
from django.contrib.auth import get_user_model
#from .models import User 직접 참조 안함 

class CustomUserCreationForm(UserCreationForm):

    class Meta(UserCreationForm.Meta):
        #model = User
        model = get_user_model()
#장고에 내장된 usercreationform(modelform)은 user model을 따르기때문에, forms.py에서
#usercreationform을 상속받은 customcreationform을 새로 만듦. 
#만약에 usercreationform 그대로 사용하면 attributeerror 발생하게됨
                fields = UserCreationForm.Meta.fields + ('email',)
      # username만 필드로 받기에 추가하려면 필드에 따로 추가해줘야함(get_user_model에 있는것만 가능) 
      # 비밀번호는 인증만을 위한 것

#3. views.signup 작성
from .forms import CustomUserCreationForm

def signup(request):
    if request.method == 'POST':
        form = CustomUserCreationForm(request.POST)
        if form.is_valid():
            form.save()
                        #회원가입 후 로그인 자동으로 하려면
                        user = form.save()
            auth_login(request, user)
            return redirect('articles:index')
    else:
        form = CustomUserCreationForm()
    context = {
        'form' : form,
    }
    return render(request, 'accounts/signup.html', context)
```

Custom user 사용하게 되면, UserCreationForm & UserChangeForm은 반드시 변경해야함 (상속 후 확장)

- 회원 탈퇴
  
  ```python
  #1.urls.py
  path('delete/', views.delete, name='delete')
  
  #2.views.py
  def delete(request):
      request.user.delete()
          auth_logout(request) #탈퇴 후 session 정보까지 지우고 싶은 경우
      return redirect('articles:index')
  
  #3.base.html
  <form action="{% url 'accounts:delete' %}" method="POST">
     {% csrf_token %}
     <input type="submit" value="회원탈퇴">
  <form>
  ```

- 회원정보 수정
  
  user을 update하는 것이며 UserChangeForm을 사용함. 사용자의 정보 및 권한을 변경하기 위해 admin 인터페이스에서 사용되는 modelform
  
  ```python
  #1.urls.py
  path('update/', views.update, name='update'),
  
  #2.views.py
  from .forms import CustomUserCreationForm, CustomUserChangeForm
  
  def update(request):
      if request.method == 'POST':
          form = CustomUserChangeForm(request.POST, instance=request.user)
          if form.is_valid():
              form.save()
                  return redirect('articles:index')
      else:
          form = CustomUserChangeForm(instance=request.user)
      context = {
          'form' : form,
      }
      return render(request, 'accounts/update.html', context)
  
  #3.update.html
  {% block content %}
  <h1>회원정보수정</h1> 
  <form action="{% url 'accounts:update' %}" method="POST">
      {% csrf_token %}
      {{ form.as_p }}
      <input type="submit"> 
  </form>
  {% endblock content %}
  
  #4.forms.py 
  class CustomUserChangeForm(UserChangeForm):
  
      class Meta(UserChangeForm.Meta):
          model = get_user_model()
          fields = ('email', 'first_name', 'last_name',) 
                  # 불필요한 필드 사용자에게 보이지 않도록
  ```

- 비밀번호 변경
  
  ```python
  #1.urls.py
  path('password/', views.change_password, name='change_password'),
  
  #2.views.py
  from django.contrib.auth import update_session_auth_hash
  
  def change_password(request):
      if request.method == 'POST':
          form = PasswordChangeForm(request.user, request.POST) #user를 첫번째로 받음
          if form.is_valid():
              user = form.save()
              update_session_auth_hash(request, user)
                          form.save()
              update_session_auth_hash(request, form.user)
                          #비밀번호 변경하면 기존 session 정보와 일치하지 않아서 로그아웃 자동으로 되는데, 이를 방지하고 로그인 유지
              return redirect('articles:index')
      else:
          form = PasswordChangeForm(request.user) #request.user 필수 인자 존재. setpasswordform을 상속 받는데, user를 인자로 받음
      context = {
          'form' : form,
      }
      return render(request, 'accounts/change_password.html', context)
  ```

### **is_authenticated & login_required decorator**

로그인한 상태에서는 로그인 버튼이 보일 필요가 없음. 화면 정리가 필요.

1. **is_authenticated**
   
   로그인한 사용자만 TRUE임. request.user에서 이 속성에 접근 가능함. 권한과는 관련이 없고, 사용자가 활성화 상태인지 유효한 세션인지는 확인하지 못함. 로그인/비로그인 상태만 구분 가능

```python
#1.base.html
{% if  request.user.is_authenticated %}
    <h3>{{ user }}</h3>
    <form action="{% url 'accounts:logout' %}" method="POST">
      {% csrf_token %}
      <input type="submit" value="logout">
    </form>
    <a href="{% url 'accounts:update' %}">회원정보 수정</a>
    <form action="{% url 'accounts:delete' %}" method="POST">
      {% csrf_token %}
      <input type="submit" value="회원탈퇴">
    </form>

{% else %}
    <a href="{% url 'accounts:login' %}">login</a>
    <a href="{% url 'accounts:signup' %}">signup</a>
{% endif %}

#2.articles/index.html
인증된 사용자만 게시글 작성 링크 볼 수 있도록
    {% if request.user.is_authenticated %}
    <a href="{% url 'articles:create' %}">새글작성</a>
  {% else %}
  <a href="{% url 'articles:login' %}">로그인하세요</a>
  {% endif %}

#3.articles/views.py
from django.contrib.auth.decorators import login_required

def login(request):
    if request.user.is_authenticated: #로그인한 사용자면 로직 수행하지 않도록
        return redirect('articles:index')
```

2. **login_required decorator**

views.py의 함수에 login_required 적용하면 로그인 하지 않은 사용자의 경우 accounts/login 페이지로 이동함

url에서 articles/create/로 강제 접속하면 로그인 페이지로 리다이렉트 후, next 파라미터와 함께 articles/create/ 저장됨. 이는 로그인이 다시 정상적으로 됐을때 articles/create/로 이동시키기 위함임

그런데 login.html에서 form action에 특정 url 정해져있으면 그쪽으로 이동해서 작동 안함. form action=””여야지만 url상 주소로 이동함

delete의 경우 비로그인 상태에서 다시 로그인 하더라도 @require_post로 인해서 405 error 발생함. next 파라미터는 GET method이기 때문. 그래서 이 때는 is_authenticated 속성값으로 작성해야함. @login_required 는 GET method에만 적용

```python
#1.accouts/views.py
@require_http_methods(['GET', 'POST'])
def login(request):
   if request.user.is_authenticated: #로그인한 사용자면 로직 수행하지 않도록
       return redirect('articles:index')
   if request.method == 'POST': # 인증과정
       form = AuthenticationForm(request, data=request.POST)
       if form.is_valid(): #유효한 id,pw이면
           auth_login(request, form.get_user()) # 함수 이름 중복되므로 바꿔줌
           # get_user : 유효성 검사를 통과한 사용자 객체를 반환
           return redirect(request.GET.get('next') or 'articles:index')
           # or이기 때문에 next 파라미터 있으면 그쪽으로 이동하고, 아니면 index로 이동
```
































