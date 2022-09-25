### **제어문** control statement

조건문, 반복문. 위에서 아래로 차례대로 명령 수행. 순서도 flowchart 로 표현 가능

- **조건문** conditional statement : T/F 판단할 수 있는 조건식과 함께 사용. if, else(선택적으로 사용)로 표현
  
  1. 복수 조건문 : if, elif, else 사용. 조건 동시에 수행하는 게 아니라 한개씩 순차적으로. ex. 미세먼지 사례
  2. 중첩 조건문 : 조건문 안에 다른 조건문 중첩. if 안에 또 다른 if ex. 윤년 사례

- **조건 표현식** conditional expression : 삼항 연산자. if문을 한줄로 가독성 좋게 줄인 것. num % 2 (== 1) true = 1이기 때문
  
  ‘true 값’ if 조건 else ‘false 값’ 형식으로 나타냄.
  
  ex. value = num if ≥num 0 else -num (절대값 저장하는 코드)

- **반복문** : 특정 조건을 만족할때까지 같은 동작 반복하고 싶을 때 사용
  
  1. while문 : **종료조건**에 해당하는 코드를 통해 종료
  2. for문 : 반복가능한 객체를 모두 순회하면 종료. 종료 조건 필요 없음. 보통 특정 횟수 반복할 때 사용
  3. 반복 제어 : break, continue, for-else

- **while문** : 조건식이 참인 경우 반복적으로 실행. 종료 조건 반드시 필요
  
  복합 연산자 in-place operator : 연산과 할당을 합쳐 놓은 것 ex. cnt += 1. 일정하게 증가하는 수 세로로 출력할 때

- **for문** : string, tuple, list, range 같은 순회가능한 객체 대상으로 수행. 별도의 종료조건 필요 없음. 시퀀스의 마지막 값까지 수행했을 때 종료 **for** 변수명 **in** iterable:
  
  ex. chars = input()
  
  for char in chars:
  
  print(char)
  
  for inx in range(len(chars)):
  
  print(chars[idx])
  
  dictionary는 key 값을 순회함. value 출력 위해서는 추가 method 활용
  
  ex. keys(), values(), **items()** = (key, value)의 튜플로 구성
  
  for students, grade in grade.items():
  
  print(student, grade)

- **enumerate()** : (index, value) 형태의 튜플로 구성된 객체 출력
  
  for idx, number in enumerate(members):
  
  print(idx, number)
  
  print(list(enumerate(members, start=1))) idx 1번째 부터 출력

- **list comprehenstion** : code for 변수 in iterable (if 조건식)
  
  표현식과 제어문을 통해 특정한 값의 리스트를 생성하는 방법
  
  ex. list = [number ** 3 for number in range (1, 4)]
  
  dict = {number : number ** 3 for number in range(1, 4)}

- **반복문 제어**
  
  1. **break** : 완전 멈춤
  2. **continue** : skip. continue 이후의 코드 블록은 수행하지 않고, 다음 반복 수행 ex. 홀수 or 짝수만 출력할 때
  3. for-**else** : 끝까지 반복문 실행한 이후에 else 실행. break 통해 중간에 종료되면 else 실행 안함
  4. **pass** : 아무것도 하지 않음. 문법적으로 필요는 하지만 할 일이 없을 때 사용. 자리 채우는 용도임.

### 함수

- 함수를 사용하는 이유
  
  1. **분해 decomposition** : 기능을 분해하고 재사용 가능하게 함. 코드를 간단명료하게 구현 ex. 평균 구할 때 for문 사용하여 sum, cnt 구해서 계산 > sum과 len 함수 사용하여 간결해짐
  2. **추상화 abstraction** : 내부 구조와 원리를 몰라도 사용할 수 있음. ex. 스마트폰 작동 원리 몰라도 사용할 수 있는 것처럼

- 함수의 종류
  
  1. 내장함수 : 파이썬 개발자가 만들어서 기본적으로 포함된 것
  2. 외장함수 : import문을 사용하여 외부 라이브러리에서 가져오는 것. 파이썬에 없는 함수
  3. 사용자 정의 함수 : 사용자가 만든 함수

- 함수의 정의 : 특정한 기능을 하는 코드의 조각(묶음). 필요시에만 호출하여 사용

- 기본 구조 : 선언(생성)과 호출 **define & call**, **input & output,** 문서화 **docstring**, 범위 **scope**

- 선언 define : def 키워드 활용

- 함수의 output
  
  **void function** : 명시적인 return 값이 없어 none 반환하고 종료
  
  **value returning function** : return하여 값 반환 후 종료
  
  cf. **print**는 함수를 사용하여 값을 출력하는것. 반환값 없음. **return**은 데이터를 처리하는 것.
  
  return은 항상 하나의 값만 반환

- 함수의 input
  
  **parameter** : 함수를 선언할 때 사용되는 변수
  
  **argument** : 함수를 호출할 때 넣어주는 값
  
  - 필수 argumnet : 반드시 전달되어야 하는 argument
  
  - 선택 argument : 값을 전달하지 않아도 되는 경우는 기본값이 전달
  
  - **positional arguments** : 첫번째 argument는 첫번째 parameter에 전달됨. 위치대로
  
  - **keyword arguments** : 직접 위치를 지정해서 전달. keyword 다음에 positional은 안됨
    
    ex. (x=2, y=5), (2, y=5) 가능, (x=2, 5) 불가능
  
  - **default arguments values** : 기본값을 지정하여 함수 호출시 argument 값을 설정하지 않도록 함 ex. (x, y=0) y의 값을 미리 지정해둠
  
  - 가변인자 ***args** : argument 개수가 정해지지 않았을 때 사용. 주로 tuple, list 언패킹시 사용
    
    패킹 : numbers = (1, 2, 3, 4, 5) / 언패킹 : a, b, c, d, e = numbers
    
    언패킹시 변수 개수랑 할당하려는 요소 개수 같아야 오류 안나는데, 변수에 * 붙이면 할당하고 남은 요소를 리스트에 담을 수 있음 a, b, *rest = 1, 2, [3, 4, 5]
    
    ex. 입력되는 number 변수의 개수를 모를 때 전체 합을 구하는 코드
    
    def sum_all(*numbers):
    
    result = 0
    
    for number in numbers:
    
    result += number
    
    return result
  
  - 가변 키워드 인자 ****kwargs** 몇 개의 키워드 인자를 받을지 모르는 함수를 정의할 때 사용. dictionary로 처리됨. 가변 인자와 동시 사용 가능 ex. 가족 구성원, 애완동물 이름 출력하는 예시

- python의 범위. scope
  
  함수는 코드 내부에 local scope를 생성하며, 그 외의 공간은 global scope
  
  이름 검색 규칙 name resolution : **LEGB rule**. **local < enclosed < global < built-in**(모든 것을 담고 있는 범위). 함수 내에서 바깥 scope의 변수에 접근은 가능하나 수정은 불가능 but global, nonlocal 활용하여 상위 scope 변경 가능. global은 global scope에 존재하지 않는 변수도 변경 가능하지만, nonlocal은 가장 가까운 상위 scope에 해당 변수가 존재해야 가능함.

- 함수 응용
  
  map : 모든 요소에 함수 적용 ex. map(int, input().split())
  
  filter : 모든 요소에 함수 적용하고 결과가 true인 것만 반환 ex. 홀수 or 짝수만 남길 때
  
  zip : 리스트 가로로 나열된거 세로로 묶음
  
  lambda : 익명함수. return문, 조건문, 반복문 가질 수 없음. 간결하게 쓸 수 있는 장점이 있음. ex. 도형 넓이 처럼 간단한 계산식에 사용. triangle_area = lambda a, b : 0.5 * a * b
  
  재귀함수 recursive function. base case에 도달할 때까지 함수 호출.
  
  ex. 팩토리얼
  
  def factorial(n):
  
  if n == 0 or n == 1:
  
  return 1
  
  else:
  
  return n * factorial(n-1)

- **모듈** : 다양한 기능을 하나의 파일로 묶은 것. 특정 기능을 하는 코드를 .py **파일** 단위로 작성 from module import* 전부 다
  
  **패키지** : 다양한 파일을 하나의 **폴더**로. package.module
  
  **라이브러리** : 다양한 패키지를 하나의 묶음으로
  
  **pip** : 관리자 ex. pip install jupyter notebook, pip freeze 내가 설치한 리스트 박제
  
  가상환경 : 패키지의 활용 공간. 복수의 프로젝트를 하는데 서로 프로그램의 버전이 다른 경우 가상환경을 만들어 사용할 수 있음
