# 파이썬 - 함수와 클래스

> [📚 전체 목차로 돌아가기](../../README.md)

## Table of Contents

- [함수](#1-함수)
- [클래스](#2-클래스)

---

## #1 함수

함수(function)란 특정 작업을 수행하기 위한 코드들을 묶어 놓고 필요할 때 호출해서 실행할 수 있도록 만든 코드 블록이라고 생각하면 된다. 

함수를 쓰는 가장 큰 이유는 반복되는 작업을 하나로 묶을 수 있기 때문이다. 

파이썬에서 함수는 `def` 키워드로 정의할 수 있다:
```python
def 함수_이름(매개변수_1, 매개변수_2, ...):
    실행할_코드1
    실행할_코드2
    ...
```

### #1.1 함수의 실행 순서
```python
def hello():
    print('Hello, world!')
 
hello() # hello 함수 호출
```
① hello() 호출 → ② hello 함수 안으로 이동 → ③ print('Hello, world!') 실행 → ④ 함수 종료 → ⑤ 호출했던 곳으로 복귀 → 프로그램 종료

### #1.2 매개변수(Parameter)와 인수(arguments) 
```python
def add(a, b): # a, b는 매개변수
    return a+b

print(add(3, 4)) # 3, 4는 인수
```
- 매개변수:  함수에 입력으로 전달되는 값을 받는 변수
- 인수: 함수를 호출할 때 전달하는 입력값

### #1.3 전역변수와 지역변수
- 전역변수: 함수 밖에서 만든 변수. 프로그램 어디서나(함수 안에서도) 값을 읽을 수 있다.
- 지역변수: 함수 안에서 정의한 변수. 함수 안에서만 사용할 수 있다. 즉, 함수가 끝나면 사라지는 변수 

함수 안에서 전역변수의 값을 직접 바꾸고 싶다면 `global` 키워드를 사용하면 된다. 사용하지 않으면, 함수 안에서 같은 이름의 새로운 지역변수를 정의하는 것이 된다.

```python
x = 10          # x는 전역변수

def hello():
    x = 20      # 함수 안의 x는 hello의 지역변수
    print(x)    # hello의 지역변수 출력
 
hello()
print(x)        # 전역변수 출력
```
```python
x = 10          # x는 전역변수

def hello():
    global x       # 전역변수 x를 사용하겠다고 설정
    print(x)    # x는 전역변수
 
hello()
print(x)        # 전역변수 출력
```

### #1.4 위치 인수와 키워드 인수
```python
person = {
    "name": "Alice",
    "age": 30,
    "job": "developer"
}

def show_person(name, age, job):
    print("이름:", name)
    print("나이:", age)
    print("직업:", job)
```

일반적으로 함수를 호출할 때는 다음과 같이 위치 안수(positional argument) 방식을 사용한다.
- 위치 인수: 함수에 인수를 순서대로 넣는 방식
```python
show_person("Alice", 30, "developer")
```

그리고 함수를 호출할 때 다음과 같이 `이름(키워드)=값` 형식으로 전달하는 방식을 키워드 인수(keyword argument)라고 한다.
```python
show_person(
    name="Alice",
    age=30,
    job="developer"
)
```

키워드 인수 방식은 인수의 순서를 기억할 필요가 없고 코드의 가독성이 높아진다. 

함수 호출 시, 두 방식을 함께 사용할 수 있지만, **위치 인수가 항상 먼저 와야**한다. 키워드 인수가 먼저 나와 버리면 뒤에 오는 값이 몇 번째 자리인지 판단할 근거가 사라지기 때문이다. 


파이썬의 언패킹은 패킹된 데이터를 풀어 여러 개의 변수에 각각 분리하여 할당하는 것을 말한다. `*`, `**`으로 언패킹을 할 수 있다.

위의 딕셔너리 person을 show_person 함수에 다음과 같이 언패킹으로 전달할 수 있다.
```python
show_person(**person)
```
만약 `show_person(*person)`을 한다면 딕셔너리 person의 key들만 전달된다. `*`언패킹은 위치 인수 방식이기 때문에, 그냥 반복해서 나오는 것을 순서대로 위치 인수로 넣기 때문이다. 

`show_person(**person)`와 같은 `**` 언패킹은 키워드 인수로 key=value를 전달하기 때문에 문제가 없다.

이렇게 하면 함수 호출에서 `**expression`을 사용하면 key가 매개변수 이름과 대응되고, 그 key의 value가 그 매개변수에 전달된다. 
- 예를 들어 딕셔너리 person의 "name: Alice"의 경우 key인 name은 show_person의 매개변수인 name에 대응되고 name이라는 key에 매핑된 Alice라는 value는 매개변수 name에 전달(즉, name=Alice).된다.

함수 호출에서 **을 통한 이러한 매핑은 key가 문자열이어야 하며, 함수의 매개변수에 전달되려면 그 이름과 대응해야 한다.

참고로 `*`나 `**`를 **함수 정의부**에 사용하면 방향이 반대가 된다. 즉, 입력으로 키워드 인수를 받는 함수도 가능하다. 이를 **키워드 인수를 사용하는 가변 인수 함수**라고 한다. 

위의 예시에서 `def show_person(name, age, job):`은 정확히 3개의 매개변수를 받는다. 

그런데 키워드 인수가 몇 개 들어올지 모르겠다면 => 다음과 같이 키워드 인수를 사용하는 가변 인수 함수를 사용하면 된다. 이렇게 하면 인수 개수와 이름을 미리 지정하지 않아도 된다. 몇 개의 키워드 인수가 전달될지 미리 알 수 없는 상황에서도 모두 수용하기 때문이다. 
```python
def show_person(**kwargs):
    print(kwargs)
```
- **kwargs에서 kwargs는 관례이다. 다른 이름도 가능하다(예:  **mapping)

여기서 `kwargs`는 딕셔너리이므로 `.items()`, `.get()` 등 dict 메서드를 사용할 수 있다.
```python
def show_person(**kwargs):
    for key, value in kwargs.items():
        print(key, value)

show_person(**person)

>>
name Alice
age 30
job developer
```
**kwargs와 **person의 차이는 같은 `**`지만 **어디에 쓰였느냐**로 역할이 정반대이다.
- 함수 호출부의 show_person(**person)의 **는 dict을 키워드 인수로 언패킹하는 것(즉, dict을 풀어서 키워드 인수를 전달)
- 함수 정의부의 def show_person(**kwargs)의 **는 (남은) 키워드 인수를 dict으로 모으는 것

즉 show_person(**person)의 **는 딕셔너리를 키워드 인수로 펼치고, def show_person(**kwargs)의 **는 여러 키워드 인수를 딕셔너리로 모은다.

`*`도 마찬가지이다.
- 함수 호출부의 show_person(*person)의 *는 iterable을 풀어서(딕셔너리라면 딕셔너리의 key들), 위치 인수로 전달
- 함수 정의부의 def show_person(*args)의 *는 (남은) 위치 인수를 tuple로 모으는 것


함수 정의부에 일반 매개변수와 **kwargs 같이 사용할 수 있다.
```python
def show_person(name, **kwargs):
    print("이름:", name)
    print(kwargs)

show_person(name="Alice", age=30, job="developer")
```
함수의 name은 name="Alice"를 받고, **남은** age와 job은 kwargs = {"age": 30, "job": "developer"}로 **kwargs가 받는다.

위치 인수와 키워드 인수를 같이 사용하는 것도 가능하다.

*args는 위치 인수를 tuple로, **kwargs는 딕셔너리로 받는다.
```python
def show_person(*args, **kwargs):
    print("args:", args)
    print("kwargs:", kwargs)

show_person(
    "hello",
    100,
    name="Alice",
    age=30,
    job="developer"
)
```
이렇게 하면 위치 인수인 "hello", 100를 먼저 args == ("hello", 100)으로 받고, 남은 것들은 kwargs = {"name": "Alice", "age": 30, "job": "developer"}로 받는다.

`def function(a, b, *args, **kwargs):` 이런 형태라면 고정 매개변수 a, b가 위치 인수를 먼저 가져가고, 남는 위치 인수는 *args, 남는 키워드 인수는 **kwargs가 받는다.

a, b, *args, **kwargs 순서대로 정의하는 이유는 앞서 설명한 것처럼 위치 인수때문이다. 위치 인수의 자리 배정을 먼저 끝내야 하기 때문이다. 

### #1.5 lambda

람다 표현식(lambda expression)은 보통 함수를 한 줄로 간결하게 작성할 때 사용한다. 

`lambda`는 주로 `def`를 사용할 정도로 복잡하지 않거나, `def`를 사용할 수 없는 곳에 쓰인다.

```python
함수_이름 = lambda 매개변수1, 매개변수2, ... : 매개변수를_이용한_표현식
```
- 

예를 들어, 두 숫자를 더하는 함수는 `def`를 사용해서 정의할 정도로 복잡한 함수가 아니다. 이런 경우는 다음과 같이 `lambda`를 사용해 간결하게 정의할 수 있다.
```python
add = lambda x, y: x + y

print(add(1, 2))
>> 3
```
- 이 예시의 경우 `def`와 비교하면 다음과 같다.
```python
lambda 방식                          def 방식

lambda x, y : x + y                  def 함수(x, y):
        ↑       ↑                              ↑
     매개변수  표현식                        매개변수
                                               
                                        return x + y
                                                 ↑
                                           lambda의 표현식
```

---
## #2 클래스
클래스는 데이터(속성)와 동작(메서드)을 하나로 묶어 새로운 자료형(타입)을 정의하는 문법이다.

클래스로 만든 객체의 중요한 특징은,  **클래스로 만든 객체는 각자 자신만의 인스턴스 속성을 가지므로 서로 영향을 주지 않는다.**

클래스는 `class` 키워드에 클래스 이름을 지정하고 `:`(콜론)을 붙인 다음, 그 아래에 `def` 메서드를 작성하면 된다. 
- 여기서 메서드는 클래스 안에 들어있는 함수를 의미한다.
```python
class NewClass:
    def do_something(self): # 인스턴스 메서드는 첫 번째 매개변수로 self를 받아야 한다
        코드
        ...

a = NewClass() # 클래스의 객체(= 인스턴스) 생성
b = NewClass()

a.do_something() # 클래스의 do_something이라는 인스턴스 메서드 호출
```
- 클래스로 만든 객체를 인스턴스라고도 한다.
- `a = NewClass()`로 만든 a는 객체이다. 그리고 객체 a는 NewClass의 인스턴스다. 
- "인스턴스"란 말은 특정 객체가 **어떤 클래스의 객체인지**를 설명할 때 사용한다.
    - 즉, "a가 인스턴스다"라고 말할 때 "NewClass 클래스의"가 암시되어 있다.
- `a.do_something()`는 인스턴스 뒤에 .(점)을 붙여서  do_something이라는 메서드를 호출한 것이다. 이렇게 인스턴스를 통해 호출하는 메서드를 **인스턴스 메서드**라고 부른다.
```python
class NewClass:
    pass

a = NewClass()
print(type(a))
>> <class '__main__.NewClass'>
```
- type(a)로 객체 a가 어떤 타입인지 확인할 수 있으며, 객체 a가 NewClass 클래스의 인스턴스임을 알 수 있다.

### #2.1 self
```python
class Calc:
    def set_data(self, first, second):
        self.first = first
        self.second = second

a = Calc()
a.set_data(2, 3)

print(a.first)
print(a.second)
>> 2
>> 3
```
이 클래스를 보면, set_data의 매개변수는 총 3개인데, set_data 메서드를 호출할 때, 2개의 값만 전달한 것을 볼 수 있다. 

self는 무엇일까? => self는 **인스턴스 자기 자신**을 의미한다. 첫 번째 매개변수 `self`에는 set_data 메서드를 호출한 객체 `a`가 자동으로 전달된다. 

이 관계를 그림으로 나타내면 다음과 같다.

<p align="center">
  <img src="../../img/d16.png" alt="" width="600">
</p>

- 관례적으로 self를 사용한다. self말고 다른 이름을 사용해도 상관없다. 

```python
a = Calc()
b = Calc()

a.set_data(2, 3)
b.set_data(4, 5)

print(a.first)
print(b.first)
>> 2
>> 4
```
a 객체의 first 값과 b 객체의 first 값이 다른 것을 볼 수 있다. 즉, 클래스로 만든 객체(인스턴스) 속성은 다른 객체의 속성과 상관없이 독립적인 값을 갖는다. 

```python
class Calc:
    def set_data(self, first, second):
        self.first = first
        self.second = second

    def add_two_numbers(self):
        result = self.first + self.second
        return result


a = Calc()
a.set_data(2, 3)
print(a.add_two_numbers())
>> 5
```
add_two_numbers 메서드의 매개변수는 `self`뿐이고, 반환값은 `result`이다. `result`를 계산하는 부분을 보면 **self.속성** 형식으로 되어 있다. 

그래서 a.add_two_numbers()와 같이 인스턴스에 의해 메서드가 수행되면, 메서드의 `self`에는 **객체 a가 자동으로 전달**되므로 `result = self.first + self.second`는 `result = a.first + a.second`이다. 

이 예시에서 클래스 안에서 속성에 접근하기 위해 **self.속성**을 사용(`self.first`, `self.second`)한 것을 볼 수 있다. 클래스 바깥에서 속성에 접근할 때는 위의 예시처럼 **인스턴스.속성** 형식(예: `a.first`, `b.first`)으로 접근하면 된다.

### #2.1 생성자 메서드 **__init__** 
위의 구현 예시의 경우 `a = Calc()`한 다음, `a.set_data()`로 메서드를 호출하면 매개변수를 전달하지 않았기 때문에 오류가 발생한다.
```python
a = Calc()
a.set_data()
>> TypeError: Calc.set_data() missing 2 required positional arguments: 'first' and 'second' 
```
이렇게 객체에 초깃값을 설정해야 하는 경우, 메서드를 호출하여 초깃값을 설정하는 것보다 **생성자**를 구현하는 것이 안전한 방법이다.

생성자는 **객체가 생성될 때 자동으로 호출되는 메서드**를 의미한다. `__init__` 메서드가 이 역할을 한다.

메서드 이름을 `__init__`으로 하면 생성자로 인식되며, 인스턴스가 생성되는 시점에 자동으로 호출된다.

클래스에 인스턴스 속성을 만들 때는 **__init__** 메서드 안에서 **self.속성**에 값을 할당한다.
```python
class Calc:
    def __init__(self, first, second):
        self.first = first
        self.second = second

    def add_two_numbers(self):
        result = self.first + self.second
        return result


a = Calc(2, 3)
print(a.add_two_numbers())
>> 5
```
- `a = Calc(2, 3)`을 수행하면 `__init__` 메서드의 매개변수에는 각각 다음과 같은 값이 전달된다: self는 생성된 인스턴스 a, first에는 2, second에는 3이 전달.
- `__init__` 메서드도 다른 메서드와 마찬가지로, 첫 번째 매개변수 `self`에는 생성되는 인스턴스가 자동으로 전달된다.

### #2.2 __slots__
`__slots__`은 **클래스의 인스턴스가 가질 수 있는 속성 이름을 미리 정해두는 기능**이다. 

예를 들어 일반 클래스는 다음과 같이 아무 속성이나 나중에 추가할 수 있다.
```python
class Calc:
    def __init__(self, first, second):
        self.first = first
        self.second = second


a = Calc(2, 3)
a.name = "add"
print(a.name)
print(a.__dict__)
>> add
>> {'first': 2, 'second': 3, 'name': 'add'}
```
- 기본적으로 Python 객체는 인스턴스 속성을 `__dict__`라는 딕셔너리에 저장한다. 

`__slots__ = ["속성이름1", "속성이름2", ...]`를 사용하면  **인스턴스의 `__dict__` 생성을 막고 지정된 속성만 저장**하게 할 수 있다.
- 이때 "속성이름"은 반드시 **문자열**로 지정한다.
```python
class Calc:
    __slots__ = ("first", "second")
    
    def __init__(self, first, second):
        self.first = first
        self.second = second


a = Calc(2, 3)
a.name = "add"
>> AttributeError: 'Calc' object has no attribute 'name' and no __dict__ for setting new attributes

print(Calc.__dict__)
print(a.__dict__)
>> {'__module__': '__main__', '__firstlineno__': 1, '__slots__': ('first', 'second'), '__init__': <function Calc.__init__ at 0x000002625E4201A0>, '__static_attributes__': ('first', 'second'), 'first': <member 'first' of 'Calc' objects>, 'second': <member 'second' of 'Calc' objects>, '__doc__': None}
>> AttributeError: 'Calc' object has no attribute '__dict__'
```
- `__slots__`에 지정된 속성 이름 외의 속성을 정의할 수 없는 것을 볼 수 있다. 
- 인스턴스와 클래스에 `__dict__`속성을 출력해보면, 현재 인스턴스와 클래스의 속성을 딕셔너리로 확인할 수 있다. 
- 다만, 위와 같이 __slots__을 정의한 경우, 인스턴스의 `__dict__`가 사라진 것을 볼 수 있다. 클래스의 `__dict__`는 그대로 있다. 
- `__slots__`은 **상속 시 주의해야 한다. 자식 클래스가 `__slots__`를 선언하지 않으면 자식 인스턴스에는 다시 `__dict__`가 생겨 효과가 사라진다.

### #2.3 비공개 속성 (Private Attribute)
Calc 클래스에는 first와 second라는 속성이 있었다. 이 속성들은 클래스 안에서 `self.속성`으로 접근할 수도 있고, 클래스 바깥에서는 `인스턴스.속성` 형식으로 접근할 수 있었다.

클래스 바깥에서 접근할 수 있는 속성을 **공개 속성 (public attribute)**이라 부르고, 클래스 안에서만 접근할 수 있는 속성을 **비공개 속성 (private attribute)**이라 부른다.

private attribute으로 속성을 정의하면 클래스 안에서만 사용할 수 있고, 클래스 바깥에서는 접근할 수 없다. 

속성 이름 앞에 밑줄 두 개(`__속성`)를 붙이면 private attribute가 된다. 
```python
class Calc:
    def __init__(self, first, second):
        self.__first = first
        self.second = second


a = Calc(2, 3)
print(a.__first)
>> AttributeError: 'Calc' object has no attribute '__first'
```
- 클래스 바깥에서 __first에 접근하지 못하는 것을 볼 수 있다. 

속성뿐만 아니라 메서드도 이름 앞에 `_` 두 개를 붙이면 클래스 안에서만 호출할 수 있는 비공개 메서드로 만들 수 있다.
- 정확히 말하면, 파이썬에는 다른 언어 같은 진짜 private이 없다. `__first`는 클래스 안에서 `_클래스이름__first` 형태로 자동 변환될 뿐이다. 이를 **이름 맹글링(name mangling)**이라 한다.
    - name mangling의 주 목적은**상속했을 때 부모/자식의 속성 이름이 충돌하는 것을 막기 위해 사용**. private처럼 보이는 것은 부산물이다. 
- 그래서 "접근할 수 없다"가 아니라 **"실수로 접근하기 어렵게 만든다"**가 정확한 설명이다.
```python
print(a._Calc__first) # 맹글링된 이름으로는 그냥 접근된다.
>> 2
```

### #2.4 클래스 속성
```python
class Calc:
    numbers = [] # 클래스 속성

    def set_data(self, number):
        self.numbers.append(number)


a = Calc()
b = Calc()

a.set_data(2)
b.set_data(10)
print(a.numbers)
print(b.numbers)
>> [2, 10]
>> [2, 10]
```
결과를 보면, a라는 인스턴스와 b라는 인스턴스를 생성한 다음, 각각 set_data로 다른 숫자를 넣었는데 a.numbers와 b.numbers를 보면 둘 다 [2, 10]이 나오는 것을 볼 수 있다. 

이렇게 **클래스 속성은 클래스에 속해 있으며 모든 인스턴스에서 공유**한다.

앞서 설명한 인스턴스 속성과 클래스 속성은 다르다.
- 클래스 속성: 모든 인스턴스가 함께 공유하는 변수
- 인스턴스 속성: 각각의 객체(인스턴스)가 따로 가지는 독자적인 변수

위와 같은 결과는 파이썬의 속성 조회 규칙 때문이다. 이때 **읽을 때와 대입할 때의 동작이 다르다.**
- 규칙 1 - 읽을 때(`self.numbers`): 인스턴스의 `__dict__`에서 먼저 찾는다. 없으면 → 클래스의 `__dict__`에서 찾는다. 없으면 → 부모 클래스에서 찾는다.
- 규칙 2 - 대입할 때(`self.numbers = ...`): 찾는 과정 없이 **인스턴스의 `__dict__`에 새로 만든다.**

- `self.numbers.append(number)` 이 한 줄은 두 단계로 나뉜다.
- (1) `self.numbers`를 읽기(규칙 1): 인스턴스에 `numbers`가 없어서 클래스 속성인 리스트 객체 `numbers`를 찾아온다.
- (2) `.append(number)`: 찾아온 `numbers`라는 이름의 리스트 객체를 변경(이 예에서는 number를 append)한다. 

즉, 규칙 2가 적용될 일이 없다. 객체 `a`와 `b`는 자기만의 `numbers`가 없으니(즉, 자신만의 인스턴스 속성이 없으니) 둘 다 `numbers`라는 이름의 같은 리스트 객체를 찾아서 변경(이 예에서는 `append()`를 수행하는 것)한다. 그래서 결과가 둘 다 [2, 10]이 된다. 

그렇다면 동일한 클래스에서 생성된 인스턴스들끼리 이 클래스 속성을 공유하지 않게 하려면 어떻게 해야 할까? => 클래스 속성이 아닌 인스턴스 속성으로 정의하면 된다. 그러면 서로 영향을 주지 않으니까 

### #2.4 비공개 클래스 속성
클래스 속성도 비공개 속성으로 만들 수 있다. 마찬가지로 `_` 두 개를 붙이면 된다. 

`__클래스속성`으로 정의된 클래스 속성은, 해당 속성이 클래스 안에서만 접근할 수 있고, 클래스 바깥에서는 접근할 수 없다는 것을 드러내기 위해 사용한다.
```python
class Calc:
    __numbers = []

    def set_data(self, number):
        self.__numbers.append(number) # 클래스 안에서는 접근 가능

print(Calc.__numbers)
>> AttributeError: type object 'Calc' has no attribute '__numbers'
```
- 사실 `Calc._Calc__numbers`로 접근 가능하다. 그래서 파이썬에서 비공개 속성은 **클래스 바깥에서는 접근하지마라**는 의도를 드러내는 장치이다.

### #2.5 정적 메서드 (Static Method)
지금까지는 클래스의 메서드를 사용하려면, 인스턴스를 통해서 메서드를 호출했다.

정적 메서드, 클래스 메서드를 사용하면 인스턴스를 통하지 않고 클래스에서 바로 호출할 수 있다.

정적 메서드는 다음과 같이 메서드 위에 **@staticmethod**를 붙이고, 매개변수에 self를 지정하지 않는다.
```python
class ClassName:
    @staticmethod
    def do_something(매개변수1, 매개변수2):
        코드
        ...
```
-  `@`는 **데코레이터**라고 하며, 메서드에 추가 기능을 구현할 때 사용한다. 
```python
class Calc:
    @staticmethod
    def add(a, b):
        return a + b
 
    @staticmethod
    def mul(a, b):
        return a * b

# 클래스에서 바로 메서드 호출
print(Calc.add(1, 2))
print(Calc.mul(1, 2))
>> 3
>> 2
```
이렇게 정적 메서드를 호출할 때는 **클래스에서 바로 메서드를 호출**하면 된다. 이는 해당 메서드가 **인스턴스와 무관한 기능**임을 잘 보여준다. 

단, 정적 메서드는 매개변수에 self를 지정하지 않아, self를 받지 않기 때문에 인스턴스 속성에는 접근할 수 없다. 

그래서 보통 정적 메서드는 인스턴스 속성, 인스턴스 메서드가 필요 없을 때 사용한다.

이 예시도 add, mul 메서드는 숫자 두개를 받아서 더하거나 곱할 뿐 인스턴스의 속성은 필요하지 않다.


그럼 무엇을 정적 메서드로 만들어야 할까? => 정적 메서드는 메서드의 실행이 외부 상태에 영향을 끼치지 않는 순수 함수(pure function)를 만들 때 사용한다. 

순수 함수는 부수 효과(side effect)가 없고  **입력 값이 같으면 언제나 같은 출력 값을 반환**한다.
- 여기서 말하는 side effect는 함수가 결과값을 돌려주는 것 외에 **바깥 세상의 상태를 바꾸는 모든 것**을 말한다.
    - side effect는 예를 들어, 인스턴스 속성이나 전역변수를 바꾸는 것, 파일 쓰기나 수정 등 함수 실행 결과가 반환하는 값 말고 다른 것도 건드리는 것을 말한다. 
```python
# 순수 함수 — 입력만 보고 결과를 만들고, 바깥을 건드리지 않는다
def add(a, b):
    return a + b

# 순수하지 않은 함수 — 바깥 변수를 바꾼다
total = 0
def add_to_total(x):
    global total
    total += x
```
일반적으로, 정적 메서드는 인스턴스의 상태를 변화시키지 않는 메서드를 만들 때 사용한다.

사실 정적 메서드는 그냥 모듈 수준의 함수로 따로 빼도 상관없다. 

그럼에도 클래스 안에 두는 이유는 **그 기능이 이 클래스와 논리적으로 한 묶음임을 드러내기 위해서**다. 

### #2.6 클래스 메서드 (Class Method)
클래스 메서드는 정적 메서드와 비슷하지만 약간의 차이점이 있다. 

클래스 메서드는 다음과 같이 메서드 위에 **@classmethod**를 붙인다. 그리고 첫 번째 매개변수에 `cls`를 지정해야 한다. 
```python
class ClassName:
    @classmethod
    def do_something(cls, 매개변수1, 매개변수2):
        코드
        ...
```
`cls`에는 **호출한 클래스**가 들어온다. 그래서 `cls.속성`으로 클래스 속성에 접근할 수 있다. 

인스턴스 없이 호출할 수 있다는 점은 정적 메서드와 같다. 

단, 클래스 메서드는 **클래스 속성이나 클래스 자체가 필요할 때(예: 클래스 속성에 접근) 쓴다.**
```python
class Calc:
    count = 0 # 클래스 속성 # 지금까지 만들어진 인스턴스 수

    def __init__(self, first, second):
        self.first = first
        self.second = second
        Calc.count += 1 # 클래스 속성에 대입 → 클래스 이름으로 접근해야 한다

    @classmethod
    def get_count(cls):
        return cls.count


a = Calc(1, 2)
b = Calc(3, 4)
print(Calc.get_count())
>> 2
```


특히 `cls`를 사용하면 **메서드 안에서 현재 클래스의 인스턴스를 만들 수도 있다.**  `cls`는 클래스이다. 예를 들어, 이 예시에  `cls()`는 클래스 `Calc()`와 같다.

이 성질은 __init__ 말고 다른 방식으로도 객체를 만들고 싶을 때 사용할 수 있다.
```python
class Calc:
    def __init__(self, first, second):
        self.first = first
        self.second = second

    @classmethod
    def from_string(cls, text):
        first, second = map(int, text.split(","))
        return cls(first, second) # cls는 Calc이므로, cls(4, 5) == Calc(4, 5)


a = Calc(2, 3) # 기본 생성 방식
b = Calc.from_string("4,5")  # 클래스 메서드로 생성
print(b.first, b.second)
>> 4 5
```


### #2.7 클래스 상속
클래스 상속은 부모 클래스(Super/Parent/Base Class)로부터 물려받은 속성과 기능을 자식 클래스(Sub/Child/Derived Class)가 이를 유지한채로 다른 속성과 기능을 추가할 때 사용한다.

즉, 클래스 상속은 새로운 클래스를 만들 때 기존의 (다른) 클래스의 속성과 기능을 물려받을 수 있게 만드는 것이다.

클래스 상속은 다음과 같이 새로운 자식 클래스를 만들 때 ( )를 붙이고 ( )안에 부모 클래스 이름을 넣는다.
```python
class ParentClass:
    코드
    ...

class ChildClass(ParentClass):
    코드
    ...
```
- ChildClass는 ParentClass를 상속했으므로 ParentClass의 모든 기능을 사용할 수 있다.

```python
class Calc:
    def add(self, a, b):
        return a + b

class ChildCalc(Calc):
    pass

cc = ChildCalc()
print(cc.add(1, 2))
>> 3
```
- `ChildCalc` 클래스에는 `add` 메서드가 없지만 `Calc` 클래스를 상속받아서 `add` 메서드를 호출할 수 있다.

이러한 상속은 **기존 클래스가 라이브러리 형태로 제공되거나 수정이 허용되지 않는 상황에서 새로운 기능을 추가하거나 기존 기능을 변경하려고 할 때** 사용한다.

```python
class Calc:
    def add(self, a, b):
        return a + b

class ChildCalc2(Calc):
    def sub(self, a, b):
        return a - b

cc2 = ChildCalc2()
print(cc2.add(1, 2))
print(cc2.sub(1, 2))
>> 3
>> -1
```
- 위의 예시는 `Calc` 클래스를 상속한 `ChildCalc2` 클래스에 `sub`라는 새로운 메서드를 추가한 것이다. 
- 이렇게 클래스 상속은 부모 클래스의 기능(이 예에서는 `add` 메서드)을 유지하면서 새로운 기능(`sub` 메서드)을 추가할 수 있다.

클래스의 상속 관계를 확인하고 싶을 때는 `issubclass` 함수를 사용한다. 자식 클래스가 부모 클래스의 자식이 맞으면 `True`, 아니면 `False`를 반환한다.
```python
print(issubclass(Calc, ChildCalc2))
print(issubclass(ChildCalc2, Calc))
>> False 
>> True
```
 
### #2.8 상속 관계와 포함 관계
#### #2.8.1 상속 관계 (is-a 관계)
아래와 같은 `Animal` 클래스가 있다고 하자.
```python
class Animal:
    def eat(self):
        print("먹습니다.")

    def sleep(self):
        print("잡니다.")


class Cat(Animal):
    def meow(self):
        print("야옹")
```
`Animal`은 아주 넓은 개념이다. 동물이라면 보통 (1) "먹을 수 있다" (2) "잘 수 있다"라는 공통적인 특징이 있다고 생각해보자.

여기서 고양이를 생각하면, 고양이도 동물이다. 이렇게 **명확히 한 종류라고 말할 수 있다**면 상속 관계를 고려할 수 있다. 

이렇게 "고양이는 동물이다.", "Cat is an Animal."라고 했을 때 말이 되는 것을 **is-a** 관계라 하며, 이런 관계들은 자연스럽게 상속으로 표현할 수 있다.

#### #2.8.2 포함 관계 (has-a 관계)
이번에는 자동차를 생각해보자.
```python
class Engine:
    def start(self):
        print("엔진이 작동합니다.")
```

자동차에는 엔진이 들어 있다. 그런데 다음 문장은 이상하다: "자동차는 엔진이다.", "Car is an Engine."

자동차에는 **엔진이 들어 있는 것**이다. 그래서 자연스러운 문장은: 자동차는 엔진을 가지고 있다. "Car has an Engine."

이럴 때 **포함 관계(has-a)**를 사용한다.
```python
class Engine:
    def start(self):
        print("엔진이 작동합니다.")


class Car:
    def __init__(self):
        self.engine = Engine()
```
여기서 중요한 부분은 `self.engine = Engine()`이다.

`self.engine = Engine()`을 하나씩 보면 다음과 같다.
- `Engine()`: 엔진 클래스의 객체를 하나 만든다.
- `self.engine = Engine()`: 그 엔진 객체를 `Car` 객체의 `engine`이라는 속성에 넣는다. 

즉 **Car 안에 Engine 클래스의 객체가 들어 있는 것**이다. Car가 Engine인 게 아니라, Car는 Engine 객체를 가지고 있다.

그래서 자동차 객체를 만들면 `car = Car()`, 내부적인 구조는 대략 다음과 같다:
```python
car
 │
 ▼
Car 객체
 │
 └── engine
              │
              ▼
        Engine 객체
```
그래서 이렇게 접근할 수 있습니다.
```python
car.engine.start()
```

동물과 고양이, 차와 엔진 예시의 구조를 비교하면 다음과 같다.
```python
Animal
   ↑
   │ 상속
   │
  Cat
```
```python
Car
 │
 └── Engine
```

정리하면, **is-a 관계이면 상속을 사용하고, 그 이외에는 속성에 인스턴스를 넣는 포함 방식을 사용**하면 된다. 

또 하나 중요한 점은, **하나의 클래스가 상속 관계와 포함 관계를 동시에 가질 수 있다**는 것이다.

예를 들어 고양이는 동물이고(Cat is an Animal), 고양이는 꼬리를 가지고 있다(Cat has a Tail). 
```python
class Animal:
    def eat(self):
        print("먹습니다.")


class Tail:
    def move(self):
        print("꼬리를 흔듭니다.")


class Cat(Animal):
    def __init__(self):
        self.tail = Tail()

    def meow(self):
        print("야옹")
```

이 구조는 다음과 같다.
```python
             Animal
                ↑
│
                │ # is-a
                │
               Cat
                │
                │ # has-a
│
                ▼
               Tail
```

### #2.9 부모 클래스의 속성 사용하기
아래 예시는 `Animal` 클래스를 상속받아 `Cat` 클래스를 정의한 것이다.

그러나 `Cat` 클래스의 인스턴스로 부모 클래스의 속성인 `food` 속성에 접근을 하면 에러가 발생한다. 그리고 그 `food` 속성을 사용하는 부모 클래스의 메서드를 호출해도 에러가 발생한다.
```python
class Animal:
    def __init__(self):
        self.food = "사료"

    def eat(self):
        print(f"{self.food}를 먹습니다.")

    def sleep(self):
        print("잡니다.")


class Cat(Animal):
    def __init__(self):
        self.name = "black"

    def meow(self):
        print("야옹")

cat = Cat()
print(cat.name)
print(cat.food) # food는 부모 클래스의 속성
print(cat.eat())
>> black
>> AttributeError: 'Cat' object has no attribute 'food'
>> AttributeError: 'Cat' object has no attribute 'food'
```
에러의 내용은 `Cat`의 객체에 `food`라는 속성이 없다는 것이다.  

이는 부모 클래스 `Animal`의 생성자 메서드인 `__init__` 메서드가 호출되지 않았기 때문이다. 

부모 클래스의 `__init__` 메서드가 호출되지 않았기 떄문에, 부모 클래스의 `__init__` 메서드에 정의된 속성인 `self.food = "사료"`가 실행되지 않아 속성이 만들어지지 않은 것이다.

그래서 자식 클래스에서 `super().__init__()`으로 부모 클래스의 `__init__` 메서드를 호출해줘야 한다. 이를 통해 부모 클래스의 `__init__` 메서드를 호출해주면, 부모 클래스가 초기화되어 부모 클래스의 속성이 만들어진다.

```python
class Animal:
    def __init__(self):
        self.food = "사료"

    def eat(self):
        print(f"{self.food}를 먹습니다.")

    def sleep(self):
        print("잡니다.")


class Cat(Animal):
    def __init__(self):
super().__init__() # super()로 부코 클래스의 __init__ 메서드 호출
        self.name = "black"

    def meow(self):
        print("야옹")
```

만약, 자식 클래스에서 `__init__` 메서드를 생략한다면, 부모 클래스의 `__init__`이 자동으로 호출되므로 `super()`를 사용하지 않고도 부모 클래스의 속성을 사용할 수 있다. 
```python
class Animal:
    def __init__(self):
        self.food = "사료"

    def eat(self):
        return f"{self.food}를 먹습니다."

    def sleep(self):
        print("잡니다.")


class Cat(Animal):
    def print_name(self):
        return "black"

    def meow(self):
        print("야옹")

cat = Cat()
print(cat.print_name())
print(cat.food) # food는 부모 클래스의 속성
print(cat.eat())
>> black
>> 사료
>> 사료를 먹습니다.
```

### #2.10 메서드 오버라이딩
다음과 같이 두 개의 숫자를 받아 나누기 연산을 다루는 클래스가 있다고 하자.
```python
class Calc:
    def __init__(self, a, b):
        self.a = a
        self.b = b

    def div(self):
        return self.a / self.b

calc = Calc(4, 0)
print(calc.div())
>> ZeroDivisionError: division by zero
```
이 예시는 `4 / 0`을 하기 때문에 ZeroDivisionError가 발생한다. 

만약, 자식 클래스가 `Calc`를 상속해서 부모 클래스인 `Calc`의 `div` 메서드를 그대로 사용하면 동일하게 ZeroDivisionError가 발생할 것이다.

이런 문제는 부모 클래스의 `div` 메서드를 수정하거나, 아니면 부모 클래스를 상속받은 자식 클래스에서 **메서드 오버라이딩(method overriding)**을 수행하면 된다. 
```python
class Calc:
    def __init__(self, a, b):
        self.a = a
        self.b = b

    def div(self):
        return self.a / self.b

class SafeCalc(Calc):
    def __init__(self, a, b):
        super().__init__(a, b)

    def div(self):
        if self.b == 0:
            return 0
        else:
            return self.a / self.b

safe_calc = SafeCalc(4, 0)
print(safe_calc.div())
>> 0
```


메서드 오버라이딩은 **부모 클래스에 정의된 메서드를 자식 클래스에서 같은 이름으로 다시 정의하여, 자식 클래스에 맞는 동작으로 변경하는 것**이다. 

그렇다면 메서드 오버라이딩을 사용하는 이유는 무엇일까? => 부모와 자식 클래스에서 **같은 기능을 같은 메서드 이름으로 사용하되, 클래스에 따라 동작을 다르게 구현하고 싶을 때** 사용한다.


만약, 부모 클래스의 **기존 기능을 그대로 활용하면서 일부 기능만 추가하거나 변경하고 싶다면 `super()`를 사용해 부모 클래스의 메서드를 호출**할 수 있다. 이렇게 하면 공통된 코드를 다시 작성할 필요가 없어 코드 중복을 줄일 수 있다.


아래는 "부모 메서드를 오버라이딩하면서 부모 메서드의 기존 기능은 super()로 재사용"하는 예시이다.
```python
class Calc:
    def __init__(self, a, b):
        self.a = a
        self.b = b

    def div(self):
        return self.a / self.b


class SafeCalc(Calc):
    def __init__(self, a, b):
        super().__init__(a, b)

    def div(self):
        if self.b == 0:
            return 0
        return super().div()
```
자식 클래스인 `SafeCalc`의 `div` 메서드에서 `super().div()`로 부모 클래스인 `Calc`의 `div()` 메서드를 호출하여 부모의 나눗셈 기능을 재사용한다. 

이 예시의 경우, 자식 클래스는 `super()`를 두 곳에서 사용했다. 
- 하나는 `super().__init__(a, b)`: 부모의 초기화 기능을 재사용
- 다른 하나는 `super().div()`: 부모의 나눗셈 기능을 재사용

### #2.11 다중 상속
다중 상속은 **하나의 자식 클래스가 여러 개의 부모 클래스로부터 상속받는 방법**이다.
```python
class ParentClass1:
    코드
    ...
 
class ParentClass2:
    코드
    ...
 
class ChildClass(ParentClass1, ParentClass2):
    코드
    ...
```

```python
class DivCalc:
    def div(self, a, b):
        return a / b


class MulCalc:
    def mul(self, a, b):
        return a * b


class DivMulCalc(DivCalc, MulCalc):
    pass


calc = DivMulCalc()

print(calc.div(10, 2))
print(calc.mul(10, 2))
```
이 예시에서 `DivMulCalc` 클래스는 두 클래스 `DivCalc`와 `MulCalc`를 동시에 상속받는다.

그래서 `DivMulCalc` 클래스는 안에 `div()`나 `mul()`을 작성하지 않았지만, `calc.div(10, 2)`를 하면 `DivCalc`에서 물려받은 `div()`를 사용할 수 있고, `calc.mul(10, 2)`를 하면 `MulCalc`에서 물려받은 `mul()`을 사용할 수 있다.

### #2.12 다이아몬드 상속과 MRO(Method Resolution Order)
파이썬은 다중 상속에서 어떤 클래스의 메서드를 먼저 찾을지 MRO로 결정한다. 다이아몬드 상속에서도 일관된 탐색 순서를 만들기 위해 MRO를 사용한다.
```python
class Person:
    def introduce(self):
        print("저는 사람입니다.")


class Student(Person):
    def introduce(self):
        print("저는 학생입니다.")


class Employee(Person):
    def introduce(self):
        print("저는 직원입니다.")


class WorkingStudent(Student, Employee):
    pass

james = WorkingStudent()
james.introduce()
>> 저는 학생입니다.
```

이 예시에서 클래스 간의 관계는 다음과 같은 다이아몬드 구조이다.
```python
              Person
             /      \
            /        \
       Student      Employee
            \        /
             \      /
          WorkingStudent
```
이 예시에서는 `Person` 클래스를 상속받아 `Student`와 `Employee` 클래스를 만들고, `Student`와 `Employee` 클래스를 다중 상속하여 `WorkingStudent` 클래스를 만들었다. 

여기서 문제는  `Person`, `Student`, `Employee` 클래스 모두 `introduce`라는 같은 메서드를 가지고 있다. 그리고 `WorkingStudent` 자신에게는 `introduce()`가 없다.  

그래서 다음과 같은 문제가 발생한다: 
```python
`WorkingStudent`에는 `introduce()`가 없음 → 부모 클래스에 있는지 확인 → `Student`에도 `introduce()`가 있고 `Employee`에도 introduce()가 있음. → 둘 중 누구 것을 실행해야 하는가?
```
이 문제를 해결하기 위해 MRO가 메서드를 탐색할 순서를 정해 놓는다. `WorkingStudent`에 메서드 `mro()`를 사용하면 **메서드 탐색 순서**를 확인할 수 있다. 
```python
print(WorkingStudent.mro())
>> 
[<class '__main__.WorkingStudent'>, <class '__main__.Student'>, <class '__main__.Employee'>, <class '__main__.Person'>, <class 'object'>]
```

mro 결과를 보면 WorkingStudent → Student → Employee → Person → object이다. 

즉, ① WorkingStudent → ② Student → ③ Employee → ④ Person → ⑤ object 순서로 `introduce()`를 찾는다.

그래서 `james.introduce()`를 실행했을 때,  `저는 학생입니다.`가 반환된 이유는:

① WorkingStudent에는 `introduce()`가 없으므로 다음 순서인 ② Student로 넘어가 `introduce()`를 찾는다.

② Student에는 `introduce()`가 존재하므로, `Student` 클래스의  `introduce()` 메서드를 사용한 것이다. 그 결과로 `james.introduce()`는 학생입니다.`를 반환한 것이다

참고) object 클래스
- 파이썬에서 object 클래스는 모든 클래스의 조상이다. 그래서 위의 mro 결과에서도 `<class 'object'>`가 출력된 것이다. 
- 파이썬 3에서 모든 클래스는 object 클래스를 상속받는다. 그리고 기본적으로 object를 생략한다. 
- 즉, `class X`는 사실 `class X(object)`인 것이다. 

### #2.13 추상 클래스
파이썬은 **추상 클래스(abstract class)**라는 기능을 제공합니다. 추상 클래스는 메서드의 목록만 가진 클래스이며 상속받는 클래스에서 메서드 구현을 강제하기 위해 사용한다.

추상 클래스를 만들려면, import로 `abc` 모듈을 가져와야 한다.
- `abc`는 abstract base class의 약자

그리고 클래스의 ( ) 안에 `metaclass=ABCMeta`를 지정하고, 메서드를 만들 때 위에 @abstractmethod를 붙여서 추상 메서드로 지정해야 한다.
```python
from abc import *
 
class AbstractClass(metaclass=ABCMeta):

    @abstractmethod
    def do_something(self):
        코드
```

예를 들어 `Animal` 클래스가 있다고 하자.
```python
from abc import *
 
class Animal(metaclass=ABCMeta)):

    @abstractmethod
    def sound(self):
        pass
```

여기서
```python
@abstractmethod
def sound(self):
    pass
```
는 **`Animal`을 상속받는 클래스라면 반드시 `sound()`를 구현해야 한다.**를 의미한다.

그래서 자식 클래스를 만들 때 다음과 같이 `sound()`를 구현해야 한다.
```python
class Dog(Animal):
    def sound(self):
        print("멍멍")


class Cat(Animal):
    def sound(self):
        print("야옹")
```

핵심은 `Dog`, `Cat`이 둘 다 `sound()`라는 동일한 메서드를 반드시 가지도록 강제했다는 것이다.

추상 메서드를 모두 구현하지 않은 클래스는 객체(인스턴스)를 만들 수 없다. 객체를 만들려고 하면 TypeError가 발생한다. 
```python
from abc import *

class Animal(metaclass=ABCMeta)):

    @abstractmethod
    def sound(self):
        pass

class Dog(Animal):
    pass

dog = Dog()
>> TypeError: Can't instantiate abstract class Dog without an implementation for abstract method 'sound' 
```
그래서 추상 클래스는 **자식 클래스가 반드시 구현해야 하는 메서드의 규칙을 정해두는 부모 클래스**로 볼 수 있다.

그렇다면 추상 클래스는 왜 사용할까? => 추상 클래스가 없다면 개발자가 실수로 메서드를 빼먹을 수 있다. 그래서 **“메서드 구현을 강제”**하기 위해 사용한다.

---
