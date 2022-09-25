- ### 순서가 있는 데이터 구조
1. #### string
   
   문자는 immutable
   
   str.**find**(x) : x의 첫번째 위치를 반환. 없으면 -1 ex. ‘happy’.find(’a’) = 1
   
   cf. str.**index**(x) : x의 첫번째 위치를 반환. 없으면 오류.
   
   str.**isalpha**() : 숫자가 아닌 문자 여부. 한글도 True
   
   str.**isupper**() : 대문자 여부, str.**islower**() : 소문자 여부. 글자 전부다 만족해야 true
   
   str.**isspace**() : 문자열이 공백으로 이루어져있는지
   
   str.**istitle**() : 첫글자 대문자 여부. ‘ 나 공백 기준으로 나눔
   
   **isdecimal** 0~9의 숫자로 이루어져있는지< **isdigit** 숫자로 이루어져 있는지< **isnumeric** 수로 볼수 있는지
   
   str.**strip**([chars]) : 공백이나 특정문자 제거. ex. 보통 양쪽에 공백 있을때. lstrip 왼쪽만 제거, rstrip 오른쪽만 제거. str = ‘python’ 일때 [chars] = ‘python’ or ‘nohtyp’ 이면 제거됨
   
   **‘separator’.join([iterable])** : 형식 주의. separator를 글자 사이에 넣어서 iterable 합침. iterable에 문자열 아닌 값 있으면 에러 발생
   
   ‘’.join(str)으로 하면 문자 하나로 합쳐짐
   
   str.**capitalize** : 첫글자만 대문자로 변경 cf. title()은 띄어쓰기 기준으로 앞글자 전부 다 대문자로 변경
   
   str.upper, lower(), str.**swapcase**() : 대소문자 서로 변경
   
   stri.**split**(’나누는 기준값’) : 리스트로 반환. default는 공백 기준으로 나눔.
   
   str.**startswith(x)**, .**endswith(x)** : 문자열이 x로 시작하는지(끝나는지) T/F로 반환. x는 꼭 한글자가 아니라 단어일수도 있음.

2. #### list
   
   list.**insert**(i, x) : 인덱스 i에 x 추가. i가 리스트 길이보다 클경우 맨 마지막에 추가됨. 기존 값은 그대로 있고 추가만 하는 개념임. 마지막에 추가할때 i = len(list)
   
   cf. **.append**(x) : 리스트 마지막에 추가. a[len(a):] = [’x’]와 같음. list.append(len(list), x)
   
   list.**remove**(x) : 가장 첫번째에 있는 x 제거. x가 존재하지 않으면 에러 발생
   
   list.**pop**() : 마지막 값을 빼서 반환 후 제거. pop(i)로 인덱스 지정도 가능
   
   list.**extend**(m) : 순회형 m의 모든 항목들이 기존 리스트 뒤에 붙어서 하나의 리스트 됨. m = [’apple’]이면 ‘apple’이 추가되지만, ‘apple’이면 모든 알파벳 철자들이 각각 추가됨. append(’apple’) = extend([’apple’])
   
   list.**index**(x, start, end): 가장 첫번째에 있는 x의 인덱스를 반환. x 없으면 에러 발생
   
   list.**count**(x) : x 몇개 있는지 셈
   
   ex. 리스트에서 원하는 값 모두 삭제
   
   a = [1, 2, 1, 3, 4]
   target_value = 1
   for i in range(a.**count**(target_value)):
   a.**remove**(target_value)
   print(a)
   
   list.**clear**() : 리스트 요소 전부다 삭제
   
   list.**sort**() : 정렬해서 리스트 원본 바꿈. None 반환
   
   cf.**sorted**() : 정렬해서 새로운 리스트 생성하고 원본은 그대로임.
   
   **reverse**()는 원본 순서 반대로 뒤집음. None 반환

3. #### tuple
   
   튜플은 값 변경 불가능하기 때문에 값에 영향을 미치지 않는 method만 가능
   
   ex. extend는 값을 변경하기 때문에 안되고, 확장연산자 +=를 통해 값을 병합해서 재할당함
- ### 순서가 없는 데이터 구조
1. #### set
   
   set.**copy**() : 얕은 복사본을 반환
   
   set.**add**(x) : x 추가
   
   set.**pop**() : 랜덤하게 하나의 요소 반환 후 제거. set이 비어있으면 error
   
   set.**remove**(x) : x를 삭제, 없으면 에러 cf. **discard**(x) : 없어도 에러안남
   
   set.**update**({t}) : set t에 있는 항목 중 없는 항목들 추가. 괄호 안에 set 여러개 넣어도 됨
   
   set.**isdisjoint**(t) : set t와 겹치는 항목 하나도 없으면 T
   
   set.**issubset**(t), set.**issuperset**(t) : set 기준으로의 포함 관계 T/F

2. #### dictionary
   
   key는 immutable(str,int,float,bool.tuple,range) 만 가능하고, values는 형태 관계없음
   
   dict.**keys**(), **vaules**(), **items**()
   
   dict.**get(k)** : key ‘k’의 값을 반환. k가 dict에 없으면 None
   
   cf. dict.**get(k, v)** : ‘k’가 없을경우 v를 반환
   
   dict.**pop(k)** : key ‘k’의 값을 반환하고 항목 삭제, 없으면 에러 발생
   
   cf. dict.**pop(k, v)** : ‘k’가 없을경우 v를 반환
   
   ex. dict = {’apple’ : ‘사과’ , ‘banana’ : ‘바나나’}
   
   data = dict.pop(’apple’)
   
   print(data) = 사과
   
   print(dict) = {‘banana’ : ‘바나나’}
   
   dict.**update**(others) : others에 주어진 값으로 key, value 덮어씀. others는 key, value 쌍으로 되어있는 iterable이어야함
- ### **shallow copy, deep copy**
  
  a = b assignmnet로 복사하면, 새로운 내용물이 생성되는게 아니라 같은 주소를 공유하기 때문에 출력하면 둘 다 바뀜. 객체참조(=주소)가 복사되는 것.
  
  shallow copy b=a[:] 를 통해 새로운 내용물을 생성해야 각각 관리할 수 있음. cf. 2차원 리스트에서는 불가능
  
  deep copy b = copy.deepcopy(a) 를 통해 새로운 리스트를 생성하여 2차원 리스트에서도 해결할 수 있음


