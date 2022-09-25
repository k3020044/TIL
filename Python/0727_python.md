# 7/27 파이썬강의

### OOP 객체지향 프로그래밍 object-oriented programming

컴퓨터 프로그래밍의 패러다임 중 하나로서, 컴퓨터 프로그램을 명령어의 목록이 아닌 객체들의 모임으로 파악하고자 하는 것. 독립된 객체들과 객체 간의 상호작용. 객체에는 정보(변수)와 행동(함수)이 담겨있음. 추상화를 위해 필요함.

장점 : 클래스 단위로 모듈화 시켜서 필요한 부분만 수정하기 쉽고, 대규모 소프트웨어 개발에 적합함

단점 : 설계시 많은 노력과 시간이 필요. 실행 속도가 상대적으로 느림

cf. 절차지향 프로그래밍 : 하나의 global data를 여러 함수들로 엮어나가는 방식. 수정이 힘들다는 단점이 있음.

- **객체** : **속성(변수)과 행동(함수)**으로 구성된 모든 것. 파이썬의 모든 것은 객체. 모든 객체는 특정 타입의 인스턴스 ex. ‘banana’.upper(). 1,23,456은 int의 인스턴스.
  
  ==는 내용이 같은 경우 T, is는 동일한 객체를 가리킬 경우 T

- 인스턴스 : 클래스로 만든 객체. ex. 아이유는 가수의 인스턴스(o), 아이유는 인스턴스(x)

- 속성 attribute : 객체들이 가지게 될 상태, 데이터. 클래스 변수와 인스턴스 변수가 있음

- **인스턴스 변수** : 인스턴스가 개인적으로 가지고 있는 속성. 인스턴스 변수 없으면 클래스 변수에서 찾음.
  
  생성자 method ‘**init**’(self, name)으로 정의
  
  인스턴스 변수 변경할 때는 ‘인스턴스.인스턴스 변수’ 형식으로 변경

- **클래스 변수** : 클래스 선언 내부에서 정의. 인스턴스 변수와 달리 개인용이 아니라 공용임. 한 클래스의 모든 인스턴스가 공유하는 값.
  
  클래스 변수 변경할 때는 ‘클래스.클래스 변수’ 형식으로 변경

- method : 특정 클래스의 객체에 공통적으로 적용 가능한 행위
1. **instance method** : 개별 행동. 인스턴스 변수 처리. 호출시 첫번째 인자로 인스턴스 self가 전달됨. 인스턴스, 클래스 변수 모두 사용 가능
   1. 매직 method : __ 특정 상황에서 자동으로 불러와져 특수한 동작을 하도록 만들어짐 ex. ‘**str**’ : print하면 ‘**str**’의 return 값이 출력됨
   2. 생성자 constructor method : 인스턴스 객체가 생성될때 자동으로 호출되도록 설계. 인스턴스 변수의 초기값 설정
   3. 소멸자 destructor method : ‘**del**’ 인스턴스 객체가 소멸되기 직전에 호출
2. **class method** : 클래스 변수 처리. 호출시 첫번째 인자로 클래스가 전달
   1. @classmethod 라는 decorator(함수 꾸며주는 기능) 사용.
3. **static method** : 인스턴스나 클래스와 상관 없는 나머지. @staticmethod. 클래스 이름공간에 귀속됨.

---

### 객체지향의 핵심 4가지

- **추상화**

- **상속** : 두 클래스 사이에서 부모-자식 관례 정립하는 것. 하위 클래스는 상위 클래스에 정의된 속성, 행동, 관계, 제약조건 모두 상속 받음. method 재사용할 수 있음. 이름공간은 인스턴스>자식>부모 순으로 탐색
  
  - isinstance(object, classinfo) : object가 class로부터 만들어졌는지
  - issubclass(class, classinfo) : class가 classinfo의 subclass인지
  - super() : 자식클래스에서 부모클래스를 호출할 때
  
  다중상속 : 두개 이상의 클래스를 상속. 중복되는 속성이나 method는 상속 순서에 의해 결정

- **다형성** : 동일한 method가 클래스에 따라 다르게 행동할 수 있음
  
  method overriding 부모 클래스의 기본 기능은 그대로 사용하지만, 특정 기능을 자식 클래스에서 변경할 수 있음. 부모 클래스의 method 실행할 때 super() 활용
  
  cf. 파이썬에 overloading은 없음. 가변인자 활용이 가능하기 때문에

- **캡슐화** : 객체의 일부 내용에 대해 외부의 직접적인 엑세스를 차단
  
  - **public member** : _ 없이 시작. 어디서나 호출 가능
  - **protected member** : _ 1개로 시작. 부모 클래스 내부, 자식클래스에서만 호출 가능
  - **private member** : — 2개로 시작. 본 클래스 내부에서만 사용 가능. 외부 호출 불가능
  
  변수에 direct로 접근하는 것 막기 위해 method 활용
  
  **getter()** method로 변수에 접근. @property 데코레이터 사용.
  
  **setter()** method로 변수의 값 설정

---

### 에러와 예외 처리

- 문법에러 **syntax error** : 가장 먼저 에러 발생한 곳 표시해줌(캐럿 기호 ^)
  
  ex, invalid syntax, caanot assign to literal, EOL(end of line), EOF(end of file) 주로 괄호 안닫았을 때 발생,

- 예외 **exception** : 실행 도중 예상치 못한 상황 발생하여 실행 멈춤. 문법적으로 올바른데 발생하는 에러. name error, type error 처럼 여러 종류로 나뉨.
  
  zero division error
  
  name error : 이름공간에 이름 없는 경우
  
  type error : 요소 타입이 불일치 할 때, argument 누락 or 초과할 때
  
  value error : 타입은 올바른데 값이 적절하지 않거나 없는 경우
  
  index error : 인덱스가 존재하지 않거나 범위 벗어나는 경우
  
  key error : 해당 키가 존재하지 않는 경우
  
  module not found error : import error모듈은 있으나 존재하지 않는 함수 가져오는 경우
  
  keyboard interrupt : 임의로 프로그램 종료할 때
  
  indentation error

- try문과 except절을 이용하여 예외 처리
  
  try문에서 코드 실행. 문제 발생하면 except절 실행하고 예외 발생하지 않으면 except 실행 안함
  
  except 여러번 쓸 수 있는데, 순차적으로 실행되므로 가장 작은 범주부터 예외 처리 해야함. except는 반드시 한 개 이상은 있어야함.
  
  as 키워드로 원본 에러 메세지 출력 가능. ex. except indexerror as err: print(f’{err}’)
  
  **try** - **except** - **else** (예외 발생 안할때 실행) - (**finally)** (예외 여부 관계없이 무조건 실행)
