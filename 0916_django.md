1. 언어 설정 바꿀때 settings.py에 **USE_I18N**이 활성화 상태여야 함

2. html에서 0번글부터 다 출력하고자 할때 : {{ **forloop.counter0** }}

3. {{ today|date: "Y년 m월 d일 (D) A h:i"}} 2022년 09월 16일 (Fri) AM 09:35 형식으로 출력됨

4. 로또 번호 6개 무작위로 뽑아서 보여죽 하려면 numbers = random.sample(range(1, 46), 6) 형식으로 함수 정의

5. 저장된 자료 중 첫번째 자료 가져올 때
   
   ```python
   article = Article.objects.all()[0] / Article.objects.all().first() / Article.objects.all().get(id=1)
   ```

6. Rendering fields manually 혹은 Looping over the form’s fields ({% for %})를 사용하면 modelform에서 화면에 나타나는 각각의 element 위치를 수동으로 변경할 수 있음

7. 사용자가 로그인에 성공할 경우 django_session 테이블에 세션 데이터가 저장되고, 브라우저의 쿠키에 세션 값이 발급되는데 이 세션 값의 key 이름은 sessionid 임

8. User 객체의 password 저장에 사용하는 알고리즘
   
   ### **PBKDF2**
   
   - 해쉬 컨테이너 알고리즘
   - 입력한 암호 기반으로 salt를 정해진 횟수만큼 hash 함수 수행
   
   ### SHA256
   
   - 특정 입력값에 대해 항상 같은 값을 리턴
   
   > 취약점
   > 
   > 1. 레인보우 어택 취약
   > 2. 무차별 대입공격 취약
   > 
   > **보완점**
   > 
   > 1. salting (레인보우 어택 방어)
   >    - 임의의 문자열을 추가하여 다이제스트 생성
   >    - 같은 패스워드라도 각각 다른 salt가 들어가 다이제스트가 다르게 만들어짐
   > 2. key stretching (무차별 대입공격 방어)
   >    - 해시 함수를 여러번 반복
   >    - 즉, 생성된 다이제스트를 입력값으로 다시 다이제스트를 생성. 이를 반복.
