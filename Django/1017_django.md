### URI (uniform resource identifier)

HTTP 요청의 대상을 resource라 하고, resource는 문서, 사진 등 어느 것이든 될 수 있음. 각 리소스 식별을 위해 URI를 사용함

**URN** : uniform resource name, 특정 이름공간에서 **이름**으로 리소스를 식별하는 URI ex. ISBN

**URL** : uniform resource locator, **위치**로 리소스를 식별하는 URI

- URL의 구조
  - **scheme(protocol)** : 브라우저가 리소스를 요청하는데 사용해야 하는 프로토콜 ex. http
  - **authority** : scheme 다음 :// 다음에 나오는 부분
    1. **domain name** : 어떤 웹 서버가 요구되는 지를 가리키며 직접 IP 주소를 사용하는 것도 가능하지만 외우는 것이 힘들기 때문에 대신 사용 ex. [www.~~~.com](http://www.~~~.com)
    2. **PORT** : 웹 서버의 리소스에 접근하는데 사용되는 기술적인 문(gate), http에서는 80이고 생략 가능 ex. :80
  - **path** : 웹 서버의 리소스 경로, 실제 위치가 아닌 추상화된 형태의 구조를 표현, 무조건 작성하는 부분 아님
  - **parameters** : 웹 서버에 제공하는 추가적인 데이터, key_value 쌍 목록
  - **anchor** : 리소스의 다른 부분에 대한 앵커, 북마크와 비슷한 기능

### REST API

API : application programming interface, 애플리케이션과 프로그래밍으로 소통하는 방법

**REST** : representational state transfer, API server를 개발하기 위한 일종의 소프트웨어 설계 방법론, 자원을 정의하고 자원에 대한 주소를 지정하는 전반적인 방법을 서술, REST 원리를 따르는 시스템을 RESTful 하다고 말함

- 자원의 식별 : URI
- 자원의 행위 : HTTP method
- 자원의 표현 : JSON

JSON : key-value 형태의 구조, 읽고 쓰기 쉽기 때문에 API에서 가장 많이 사용하는 데이터 타입

### Response

```python
1. 모델 migrate
python manage.py migrate
2. fixtures 파일 load
python manage.py loaddata articles.json

1. JsonResponse() 활용 >> application/json
from django.http.response import JsonResponse, HttpResponse

def article_json_1(request):
    articles = Article.objects.all()
    articles_json = []
        # articles_json 리스트에 딕셔너리 형태로 데이터 입력하는 방식
    for article in articles:
        articles_json.append(
            {
                'id' : article.pk,
                'title' : article.title,
                'content' : article.content,
                'created_at' : article.created_at,
                'updated_at' : article.updated_at,
            }
        )
    return JsonResponse(articles_json, safe=False) # false로 설정하면 모든 타입의 객체 가능, true로 하면 dict 타입만 가능

2. serializer 활용 >> aplication/json
from django.http.response import JsonResponse, HttpResponse
from django.core import serializers

def article_json_2(request):
    articles = Article.objects.all()
    data = serializers.serialize('json', articles)
    return HttpResponse(data, content_type='application/json')

# serialization : 어떤 언어나 환경에서도 나중에 다시 쉽게 사용할 수 있는 포맷으로 변환하는 과정

3. Django REST framework 활용 > 전용 템플릿으로 응답함
# rest framework 설치
**pip install djangorestframework**
installed_apps 등록

# 앱 폴더 내에 serializers.py 생성
from rest_framework import serializers
from .models import Article

class ArticleSerializer(serializers.ModelSerializer):

    class Meta:
        model = Article
        fields = '__all__'

# views.py
@api_view()
def article_json_3(request):
    articles = Article.objects.all()
    serializer = ArticleSerializer(articles, **many=True**) # many=true로 하면 여러 데이터가(queryset) 한꺼번에 serialize될 수 있음
    return Response(serializer.data)
```

### Django REST framework(single model)

```python
1. ModelSerializer 작성
#articles/serializers.py
from rest_framework import serializers
from .models import Article, Comment

class ArticleListSerializer(serializers.ModelSerializer):

    class Meta:
        model = Article
        fields = ('id', 'title', 'content',)

class ArticleSerializer(serializers.ModelSerializer):
    #comment_set = serializers.PrimaryKeyRelatedField(many=True, read_only=True)
    #json에서 댓글 pk만 확인 가능함
    comment_set = CommentSerializer(many=True, read_only=True)
        #댓글 개수 확인
    comment_count = serializers.IntegerField(source='comment_set.count', read_only=True)
    # source : 필드를 채우는 데 사용할 속성의 이름
    class Meta:
        model = Article
        fields = '__all__'

2. read
#articles/urls.py
path('articles/', views.article_list),
path('articles/<int:article_pk>/', views.article_detail),

#articles/views.py
from rest_framework.response import Response
from rest_framework.decorators import api_view

from articles.serializers import ArticleListSerializer, ArticleSerializer, CommentSerializer
from . models import Article, Comment

@api_view(['GET', 'POST']) # 빈 리스트 두면 기본값은 GET
def article_list(request):
    if request.method == 'GET':
        #articles = Article.objects.all() 없으면 빈 쿼리셋
        articles = get_list_or_404(Article)
        serializer = ArticleListSerializer(articles, many=True)
        return Response(serializer.data)

@api_view(['GET', 'DELETE', 'PUT']) 
def article_detail(request, article_pk):
    #article = Article.objects.get(pk=article_pk)
    # 데이터 못찾으면 404 에러 발생
    article = get_object_or_404(Article, pk=article_pk)
    if request.method == 'GET':
        serializer = ArticleSerializer(article)
        return Response(serializer.data)

3. create
# url은 따로 생성 안함

#articles/views.py
from rest_framework import status

@api_view(['GET', 'POST']) # 빈 리스트 두면 기본값은 GET
def article_list(request):
    if request.method == 'GET':
        #articles = Article.objects.all() 없으면 빈 쿼리셋
        articles = get_list_or_404(Article)
        serializer = ArticleListSerializer(articles, many=True)
        return Response(serializer.data)

    elif request.method == 'POST':
        serializer = ArticleSerializer(data=request.data)
        if serializer.is_valid(raise_exception=True):
            serializer.save()
                        # 데이터 생성 성공하면 201, 그렇지 않으면 400 코드로 응답
            return Response(serializer.data, status=status.HTTP_201_CREATED)
        #return Response(serializer.errors, status=status.HTTP_400_BAD_REQUEST)
        #raise_exeption=True 입력하면 생략 가능

4. delete
# url은 따로 작성 안함

#articles/views.py
@api_view(['GET', 'DELETE', 'PUT']) 
def article_detail(request, article_pk):
    #article = Article.objects.get(pk=article_pk)
    # 데이터 못찾으면 404 에러 발생
    article = get_object_or_404(Article, pk=article_pk)
    if request.method == 'GET':
        serializer = ArticleSerializer(article)
        return Response(serializer.data)
    elif request.method == 'DELETE':
        article.delete()
                    # 데이터 삭제하면 204코드 응답
        return Response(status=status.HTTP_204_NO_CONTENT)

5. update
# url은 따로 작성 안함

#articles/views.py
@api_view(['GET', 'DELETE', 'PUT']) 
def article_detail(request, article_pk):
    #article = Article.objects.get(pk=article_pk)
    # 데이터 못찾으면 404 에러 발생
    article = get_object_or_404(Article, pk=article_pk)
    if request.method == 'GET':
        serializer = ArticleSerializer(article)
        return Response(serializer.data)
    elif request.method == 'DELETE':
        article.delete()
        return Response(status=status.HTTP_204_NO_CONTENT)
    elif request.method == 'PUT':
        serializer = ArticleSerializer(article, data=request.data)
        if serializer.is_valid(raise_exception=True):
            serializer.save()
            return Response(serializer.data)
```

### Django REST framework(N:1 relation)

```python
1. ModelSerializer 작성
class CommentSerializer(serializers.ModelSerializer):

    class Meta:
        model = Comment
        fields = '__all__'
        read_only_fields = ('article',)
        # 유효성 검사에서는 제외하고 데이터 조회시에는 출력

2. read
#articles/urls.py
path('comments/', views.comment_list),
path('comments/<int:comment_pk>/', views.comment_detail),

#articles/views.py
@api_view(['GET'])
def comment_list(request):
    if request.method == 'GET':
        #comments = Comment.objects.all()
        comments = get_list_or_404(Comment)
        serializer = CommentSerializer(comments, many=True)
        return Response(serializer.data)

@api_view(['GET', 'DELETE', 'PUT']) 
def comment_detail(request, comment_pk):
    #comment = Comment.objects.get(pk=comment_pk)
    comment = get_object_or_404(Comment, pk=comment_pk)
    if request.method == 'GET':
        serializer = CommentSerializer(comment)
        return Response(serializer.data)

3. create
#articles/urls.py
path('articles/<int:article_pk>/comments/', views.comment_create),

#articles/views.py
@api_view(['POST'])
def comment_create(request, article_pk):
    #article = Article.objects.get(k=article_pk)
    article = get_object_or_404(Article, pk=article_pk)
    serializer = CommentSerializer(data=request.data)
    if serializer.is_valid(raise_exception=True): # 저장하기 전에 유효성 검사 먼저 하는데 외래키 없어서 따로 read_only_fields 작성
        serializer.save(**article=article**) # 그냥 save()하면 외래키 없어서 안됨
        return Response(serializer.data, status=status.HTTP_201_CREATED)

4. update & delete
@api_view(['GET', 'DELETE', 'PUT']) 
def comment_detail(request, comment_pk):
    #comment = Comment.objects.get(pk=comment_pk)
    comment = get_object_or_404(Comment, pk=comment_pk)
    if request.method == 'GET':
        serializer = CommentSerializer(comment)
        return Response(serializer.data)

    elif request.method == 'DELETE':
        comment.delete()
        return Response(status=status.HTTP_204_NO_CONTENT)

    elif request.method == 'PUT':
        serializer = CommentSerializer(comment, data=request.data)
        if serializer.is_valid(raise_exception=True):
            serializer.save()
            return Response(serializer.data)
```
