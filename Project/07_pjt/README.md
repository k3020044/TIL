### 1021 PJT_7주차

----

- **개요**
  
  Django의  DRF를 통해 영화 관련 데이터가 담긴 JSON 파일을 이용하여 REST API를 설계하는 활동을 함. Movie, Actor, Review 간의  1:N, M:N 관계를 파악하고, 적당한 model과 serializer를 생성하는 것이 주요한 과제였음

- **4L**
  
  1. **Liked**
     
     지난 주에 학습한 DRF 개념을 활용하여 프로젝트 활동을 하여 개념을 다시 한번 정리할 수 있는 계기가 되었다. 수업 자료를 참고하여 코드를 짜면서 serializer의 구성에 대해 보다 깊이 이해할 수 있었다.
  
  2. **Learned**
     
     - serializer를 nested relationship으로 구성할때, 필드에도 인자로 넣어줘야 한다
       
       ```django
       class ActorDetailSerializer(serializers.ModelSerializer):
            movies = MovieTitleSerializer(many=TRUE, read_only=TRUE)
       
            class Meta:
                model = Actor
                fields = ('id', 'movies', 'name',)
       ```
     
     - M:N 관계에 있는 클래스에서 ManyToManyField를 활용하면 중개테이블이 자동으로 생성되어, 서로간 참조할 수 있다
       
       ```django
       class Movie(models.Model):
           title = models.CharField(max_length=100) # 영화 제목
           overview = models.TextField() # 줄거리
           release_date = models.DateTimeField() # 개봉일
           poster_path = models.TextField() # 포스터 주소
           actors = models.ManyToManyField(Actor, related_name='movies')                   
       ```
  
  3. **Lacked**
     
     처음에 serializer의 구조에 대한 개념이 잘 잡히지 않아서 오랜 시간 헤맸는데, 하나하나 해결해 가면서 이해도를 높일 수 있었다. 하지만 여전히 완전한 이해는 못하고 있어서 공부를 더 해야할 것 같다.
     
     - 내가 작성한 views.py 코드
       
       ```django
       @api_view(['GET'])
       def actor_list(request):
           actors = get_list_or_404(Actor)
           serializer = ActorSerializer(actors, many=True)
           return Response(serializer.data)
       
       @api_view(['GET'])
       def actor_detail(request, actor_pk):
           actor = get_object_or_404(Actor, pk=actor_pk)
           serializer = ActorDetailSerializer(actor)
           return Response(serializer.data)
       
       @api_view(['GET'])
       def movie_list(request):
           movies = get_list_or_404(Movie)
           serializer = MovieSerializer(movies, many=True)
           return Response(serializer.data)
       
       @api_view(['GET'])
       def movie_detail(request, movie_pk):
           movie = get_object_or_404(Movie, pk=movie_pk)
           serializer = MovieDetailSerializer(movie)
           return Response(serializer.data)
       
       @api_view(['GET'])
       def review_list(request):
           reviews = get_list_or_404(Review)
           serializer = ReviewSerializer(reviews, many=True)
           return Response(serializer.data)
       
       @api_view(['GET', 'PUT', 'DELETE'])
       def review_detail(request, review_pk):
           review = get_object_or_404(Review, pk=review_pk)
       
           if request.method == 'GET':
               serializer = ReviewDetailSerializer(review)
               return Response(serializer.data)
       
           elif request.method == 'PUT':
               serializer = ReviewDetailSerializer(review, data=request.data)
               if serializer.is_valid(raise_exception=TRUE):
                   serializer.save()
                   return Response(serializer.data)
       
           elif request.method == 'DELETE':
               review.delete()
               return Response(status=status.HTTP_204_NO_CONTENT)
       ```

       @api_view(['POST'])
       def create_review(request, movie_pk):
           movie = get_object_or_404(Movie, pk=movie_pk)
           serializer = ReviewDetailSerializer(data=request.data)
           if serializer.is_valid(raise_exception=True):
               serializer.save(movie=movie)
               return Response(serializer.data, status=status.HTTP_201_CREATED)
       ```

4. **Longed for**
   
   지금은 수업 자료의 코드를 참고하면서 코드를 작성했는데, 아무 것도 주어지지 않은 상태에서도 스스로 코드를 구현할 수 있는 수준이 되도록 학습해야할 것 같다. 또 DB에서  1:N, M:N 관계의 코드 구현법에 대해 중점적으로 익혀야 할 것 같다.

. **느낀점**

  이번 활동도 코드를 작성하며 여러 시행착오를 겪긴 했으나 문제점들을 스스로 해결할 수 있었다. 앞으로도 스스로 문제점을 해결해 나갈수 있는 개발자로 성장하기 위해 노력해야겠다. 
