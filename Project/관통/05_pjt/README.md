#### 1007 PJT 5주차

---

- **개요**
  
  django를 활용하여 영화 관련 데이터를 생성, 저장할 수 있는 웹페이지를 구현하는 활동을 함
  
  다양한 model field를 활용하여 웹페이지를 구현할 데이터베이스를 생성하고, 데이터를 CRUD(create, read, update, delete) 하는 함수를 통하여 사용자가 이용할 수 있는 html 페이지를 만듦
  
  지난번 프로젝트에서 modelform을 추가로 활용해서 코드의 간결성을 추구함

- **4L**
  
  1. **Liked**
     
     지난달에 django를 처음 배우고 한동안 알고리즘 수업을 해서 학습한 내용을 조금 잊어버렸는데, 이번 활동을 통해 처음부터 지금까지 배운 내용들을 되짚어볼 수 있었다.
  
  2. **Learned**
     
     modelform 구현 과정에 대해 익혔다
     
     ```python
     # movies/models.py
     from django.db import models
     
     class Movie(models.Model):
         title = models.CharField(max_length=20)
         audience = models.IntegerField()
         release_date = models.DateTimeField(auto_now=False, auto_now_add=False)
         genre_choices = (('코미디', '코미디'), ('공포', '공포'), ('로맨스', '로맨스'))
         genre = models.CharField(max_length=30, choices=genre_choices)
         score = models.FloatField()
         poster_url = models.URLField(max_length=200)
         description = models.TextField()
     
         # 튜플의 앞에 값은 db에 저장되는 값, 뒤에 값은 admin 페이지나 폼에서 표시하는 값
     ```

     # movies/forms.py
     from django import forms
     from .models import Movie
    
     class MovieForm(forms.ModelForm):
    
         # input element type : date로
         release_date = forms.DateTimeField(
             widget=forms.DateInput
         )
    
         # select element 출력
         genre = forms.CharField(
             widget=forms.Select(choices=Movie.genre_choices)
         )
    
         # input element type : number, step, min, max 설정
         score = forms.FloatField(
             widget=forms.NumberInput(
                 attrs={
                     'step' : 0.5,
                     'min' : 0,
                     'max' : 5,
                 }
             )
         )
    
         description = forms.CharField(
             label='내용',
             widget=forms.Textarea(
                 attrs={
                     'class': 'my-content',
                     'placeholder': 'Enter the content',
                     'rows': 5,
                     'cols': 50,
                 }
             ),
             error_messages={
                 'required': '내용을 입력하세요.',
             }
         )
    
         class Meta:
             model = Movie
             fields = '__all__'
    
    
     # create, update view 함수
     def create(request):
         if request.method == 'POST':
             form = MovieForm(request.POST)
             if form.is_valid():
                 movie = form.save()
                 return redirect('movies:detail', movie.pk) 
         else:
             form = MovieForm()
         context = {
             'form' : form,
         }
         return render(request, 'movies/create.html', context)
    
    
     def update(request, pk):
         movie = Movie.objects.get(pk=pk)
         if request.method == 'POST':
             form = MovieForm(request.POST, instance=movie)
             if form.is_valid():
                 form.save()
                 return redirect('movies:detail', movie.pk)
         else:
             form = MovieForm(instance=movie)
         context = {
             'form' : form,
             'movie' : movie,
         }
         return render(request, 'movies/update.html', context)
     ```

3. **Lacked**
   
   처음 서버를 실행했을때 404 NOT FOUND 에러가 발생했는데, 이는 처음의 기본 연결 과정에 문제가 있다는 것임을 알게되고 해결방법을 찾았다. 내가 놓친 부분은 다음의 2가지 였다.
   
   - settings.py templates 부분 수정 - templates 폴더 이후, 앱과 똑같은 이름의 폴더를 생성한 후에 이 안에 html 파일을 넣기 때문에 'DIRS': [BASE_DIR / 'templates',], 로 수정해야한다. installed apps에 적는 것은 잊지 않았는데 이 부분을 놓쳐서 나중에 발견하고 수정했다.
   
   - 프로젝트 url과 앱 url 연결시키기 - 프로젝트 폴더의 urls.py에서 include를 통해 앱의 url을 연결시켜줘야 404 에러가 발생하지 않는다. 이부분 또한 초반에 놓쳐서 나중에 발견하고 수정했다.

4. **Longed for**
   
   수업시간에 배운 내용을 토대로 다른 상황들에 자유자재로 적용할 수 있도록 개념에 대한 확실한 이해가 필요할 것 같다. 공부를 많이 해야할 것 같다. 
- **느낀점**
  
  서버를 실행시키며 발생한 오류들을 모두 스스로 해결해내서 뿌듯하다
  
  예전에 했던 파일이나 노트 필기를 보면 이해가 가는데, 아무것도 참고하지 않고 하기에는 아직 역부족인 것 같다. 그동안 django 연습을 많이 하지 않아서 연습을 많이 해야할 것 같다.
