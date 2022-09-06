### django form

사용자가 입력한 데이터 유효한지 검증이 필요한데, 반복 코드를 줄여줌으로써 유효성 검증을 쉽게함. form 안에 유효성 검사를 단순화하고 자동화할 수 있는 기능이 내장됨

앱 폴더 안에 [forms.py](http://forms.py) 만들고 forms library의 Form class를 상속하여 선언

꼭 [forms.py](http://forms.py)에 작성해야하는 것은 아니지만, 유지 보수 관점에서 관행적으로 [forms.py](http://forms.py) 파일 안에 작성함

```python
#forms.py
from django import forms

class ArticleForm(forms.Form):
    title = forms.CharField(max_length=10) #max_length 필수값 아님
    content = forms.CharField(widget=forms.Textarea) # Form에는 textfield 없음.widget활용

#views.py
def new(request):
    form = ArticleForm()
    context = {
        'form' : form,
    }
    return render(request, 'articles/new.html', context)

#.new.html
<form action="{% url 'articles:create' %}" method="POST">
    {% csrf_token %}
    {{ form.as_p }} 
# title, content 한줄에 붙어있어서 as_p method 쓰면 한줄 띄워짐
# form 인스턴스 만든것으로 input, label html 태그들 대체
# 개발자도구 페이지 보면, form 쓰면 장고가 알아서 html 태그 속성값 설정함
# 속성을 보면 자동으로 required id라고 생성되는데, 그래서 아무 값도 입력을
  안하면 값을 입력하라는 메세지가 나옴. 만약 required를 지우게 되면 input 값이
    없어도 django로 전달이 됨 
```

- widget : input 요소의 단순한 출력 부분을 담당. 유효성 검증과는 관련이 없으며 html 렌더링을 처리하는 역할을 함. 반드시 forms fields에 할당하여 표현

### django modelform

form class와 model 중복되는 코드 요소들(ex. 필드에 대한 정보들)이 있어서, modelform을 사용하여 효율화

form은 사용자로부터 받는 데이터가 database와 연관되지 않는 경우(ex. 로그인)에 사용하고, modelform은 db에 연관된 경우에 사용함

forms.py에서 forms 라이브러리의 ModelForm 클래스를 상속하여 선언

```python
#forms.py
from django import forms
from .models import Article # models에서 정의한 Article가져옴

class ArticleForm(forms.ModelForm):
    title = forms.CharField( #widgets 활용해서 출력 형태 지정함
        label='제목',   #title이 제목으로 변경돼서 페이지에 출력됨
        widget=forms.TextInput(
            attrs={ #attributes 딕셔너리 형태로 넣음
                'class' : 'my-title',     #개발자 도구에서 class="my-title"로 저장됨
                'placeholder' : 'enter the title', # 제목 입력란에 출력됨
                'maxlength' : 10, # 입력이 10자로 제한됨. 장고랑은 관련없음
            }
        )
    )
    content = forms.CharField(
        label='내용',
        widget=forms.Textarea(
            attrs= {
                'class' : 'my-content',
                'placeholder' : 'enter the content',
                'rows' : 5,
                'colds' : 50,
            }
        ),
        error_messages={
            'required': 'please enter your content' #공백으로 제출했을때 please~ 출력됨
        }
    )
    class Meta:# Meta 라는 inner class 생성. 어떤 modle을 기반으로 form을 작성할 것인지에 대한 정보
        model = Article # ()없음. instance가 아니라 참조값이 반영. 반환값x
        # 함수(클래스)를 호출하지 않고 함수 자체를 그대로 전달하여, 다른 함수에서 필요한 시점에 호출하여 사용할 수 있음
                fields = '__all__' # Article model에서 사용자로부터 입력받는 모든 필드 의미
                # exclude = ('제외할 필드',) 로 입력하면 제외할 필드 지정 가능
```

- Create
  
  ```python
  #views.py
  def create(request):
      form = ArticleForm((data=)request.POST) # 데이터를 넣고 instance 생성
      if form.is_valid(): # t/f 반환
          article = form.save() # 저장. save는 self.instance를 return하도록 설계돼있음
          return redirect('articles:index', article.pk)
      context = {               # f인 경우 html 태그로 오류 원인 터미널에 출력되므로 이를 사용자에게 출력되도록         
                      'form' : form,}
          return render(request, 'articles/new.html', context) # 데이터와 함께 렌더링될 필요 없으니까 redirect대신 render
  ```

- Update
  
  ```python
  form = ArticleForm(request.POST, instance=article)
  form.save() # instance가 제공되지 않으면 create하고, 제공되면 update함
  # request.POST는 사용자가 입력한 값으로 새로운 데이터고, instance는 수정 대상임
  
  #views.py
  def edit(request, pk):
      article = Article.objects.get(pk=pk)
      form = ArticleForm(instance=article)
      context = {
          'article': article,
          'form' : form,
      }
      return render(request, 'articles/edit.html', context)
  
  def update(request, pk):
      article = Article.objects.get(pk=pk)
      form = ArticleForm(request.POST, instance=article)
      if form.is_valid():
          form.save()
          return redirect('articles:detail', article.pk)
      context = {
          'form' : form,
      }
      return render('request', 'articles/edit.html', context)
  ```

### handling http requests

new, create는 create 로직을 구현하기 위한 것이고, edit, update는 update 로직을 구현하기 위한 것.

new, edit은 페이지를 렌더링하기 위해 GET에 대한 처리만 하고, create와 update는 POST에 대한 처리를 해서 DB를 조작함.

> > 공통점과 차이점 기반으로 코드를 간결화할 수 있음

```python
def create(request):
    if request.method == 'POST': 
#GET이 아니라 POST로 분기 처리하는 이유는 POST일때만 DB에 저장하고, 나머지 경우는 X
        #create
        form = ArticleForm(request.POST) # 데이터를 넣고 instance 생성
        if form.is_valid(): # t/f 반환
            article = form.save()
            return redirect('articles:detail', article.pk)  
    else:
        #new
        form = ArticleForm()
    context = { # if form.is_valid() f인 결과값들을 해결함
        'form' : form,
    }
    return render(request, 'articles/create.html', context)

def update(request, pk):
    article = Article.objects.get(pk=pk) # 공통요소이므로 밖으로 뺌
    if request.method == 'POST':
        #update
        form = ArticleForm(request.POST, instance=article)
        if form.is_valid():
            form.save()
            return redirect('articles:detail', article.pk)
    else:
        #edit
        form = ArticleForm(instance=article)
    context = {
        'article': article,
        'form' : form,
    }
    return render(request, 'articles/update.html', context)

def delete(request, pk):
    if request.method == 'POST': #POST일때만 삭제하도록. 그냥두면 GET도 삭제되므로 URL로 삭제가능
        article = Article.objects.get(pk=pk)
        article.delete()
                return redirect('articles:index')
    return redirect('articles:detail', article.pk)
```

### view decorators

view 함수에 기능을 추가

1. require_safe()
   
   ```python
   from django.views.decorators.http import require_safe
   @require_safe # GET method만 허용, 아니면 405 method not allowed error
   def index(request):
      pass
   ```

2. require_http_methods()
   
   ```python
   from django.views.decorators.http import require_http_methods
   @require_http_methods(['GET', 'POST']) # GET, POST만 허용
   def create(request):
      if request.method == 'POST':
          else: #decorator 작성했으므로 'GET' 만 의미함
   ```

3. require_POST()
   
   ```python
   from django.views.decorators.http import require_POST
   @require_POST # POST만 허용
   def delete(request, pk):
   	pass
   ```
