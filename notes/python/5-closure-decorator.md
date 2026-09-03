# 파이썬 - 변수의 사용 범위, 클로저, 데코레이터

> [📚 전체 목차로 돌아가기](../../README.md)

## Table of Contents

- [변수의 사용 범위](#1-변수의-사용-범위)
- [클로저 (Closure)](#2-클로저-closure)
- [데코레이터 (Decorator)](#3-데코레이터-decorator)
- [이터레이터](#4-이터레이터)
- [제너레이터](#5-제너레이터)
- [어노테이션](#6-어노테이션)

---

## #1 변수의 사용 범위

### #1.1  전역 변수와 지역 변수
```python
x = 10 # 전역 변수
def run_x():
    print(x) # 전역 변수

run_x()
print(x)
>> 10
>> 10
```
`run_x` 함수는 함수 바깥에 있는 변수 `x`의 값을 출력한다. 그리고 함수 바깥에서도 `print(x)`로 `x`의 값을 출력할 수 있다.

이렇게 함수를 포함해 스크립트 전체에서 접근할 수 있는 변수(즉, 모듈의 최상위 레벨에 정의된 변수)를 **전역 변수(global variable)**라고 부르며, 전역 변수에 접근할 수 있는 범위를 **전역 범위(global scope)**라고 한다. 


```python
def run_x():
    x = 10 # run_x 함수의 지역 변수
    print(x) # run_x 함수의 지역 변수

run_x()
print(x)
>> 10
>> NameError: name 'x' is not defined
```
이 예시에서 변수 `x`는 함수 `run_x` 안에서 정의된 변수로, 이렇게 함수와 같은 특정 지역 범위(local scope) 안에서 이름이 묶인 변수를 **지역 변수(local variable)**라고 한다.
- 이 예에서 변수 `x`는 `run_x`의 지역 변수이다. 

**지역 범위**는 그 지역 변수의 이름을 직접 참조할 수 있는 범위이다.

아래 예시에서 `run_x`의 결과로 지역 변수가 출력되고, `print`의 결과로 전역 변수가 출력되는 것을 볼 수 있다.
```python
x = 10 # 전역 변수
def run_x():
    x = 20 # 지역 변수
    print(x) # 지역 변수

run_x() # 지역 변수 출력 
print(x) # 전역 변수 출력
>> 20
>> 10
```

전역 변수 `x`와 지역 변수 `x`는 이름만 같을 뿐 서로 다른 변수이다.

다음과 같이 `global` 키워드를 통해 함수 안에서 전역 변수의 값을 사용할 수 있다.
```python
x = 10  # 전역 변수

def run_x():
    global x # 전역 범위의 x를 사용
    x = 20 # 전역 변수 x: 10 → 20
    print(x) # 전역 변수 x의 값 20 출력

run_x()
print(x) # 변경된 전역 변수 x의 값 20 출력
>> 20
>> 20
```
전역 변수 `x`의 값이 `run_x` 함수 안에서 20으로 변경되었기 때문에 전역 변수를 출력하는 `print`에서도 20이 출력된다. 

이렇게 하면 위의 예시처럼 전역 변수 `x=10`이 없는 상태여도, `global` 키워드를 통해 전역 변수를 설정할 수 있다.

`global`로 지정된 이름이 전역 네임스페이스에 아직 없다면, 이후 그 이름에 값을 대입할 때 전역 네임스페이스에 새로운 변수가 생성된다.

참고로 `globals()`는 현재 모듈의 전역 네임스페이스를 담고 있는 딕셔너리를 반환하고, `locals()`는 현재 지역 네임스페이스에서 유효한 지역 변수들을 딕셔너리로 반환하는 내장 함수이다.

즉, `globals()`는 전역 변수들을 보여준다. `locals()`는 함수 바깥(전역 범위)에서 호출하면 `globals()`와 똑같이 동작하고, 현재 지역 스코프(예: 함수 내부)에서 호출하면 지역 변수들을 보여준다.


### #1.2 함수 안에 함수 
다음과 같이 함수 안에 또 다른 함수가 정의된 형태를 중첩 함수라고 한다. 
```python
def 함수1():
    코드
    ...
    def 함수2():
        코드
        ...
```

바깥 함수가 지닌 변수나 매개변수를 내부 함수가 그대로 가져다 쓸 수 있다.
```python
def outer_f():
    x = 10
    def inner_f():
        print(x) # 바깥 함수의 지역 변수를 사용
```
즉, 바깥 함수의 지역 변수는 그 안에 속한 모든 함수에서 접근할 수 있다. 

```python
def outer_f():
    x = 10 # outer_f의 지역 변수 x
    def inner_f():
        x = 20 # inner_f의 지역 변수 x

    inner_f()
    print(x)

outer_f()
>> 10
```
이 예시의 경우 `outer_f` 함수의 지역 변수 `x`의 값을 내부 함수에서 변경하는 것 같지만, 실제로는 내부 함수 `inner_f`에서 이름이 같은 지역 변수 `x`를 새로 만드는 것이다. 그래서 10이 출력된다. 

```python
def outer_f():
    x = 10
    print(locals())
    def inner_f():
        x = 20
        print(locals())

    inner_f()

outer_f()
>> {'x': 10} # outer_f의 지역 변수 x
>> {'x': 20} # inner_f의 지역 변수 x 
```
즉, `{'x': 10} {'x': 20}`로 나오는 이유는 `outer_f()`와 `inner_f()`가 각각 자기만의 지역 범위를 갖기 때문이다. 

이 예시의 실행 흐름은
- 먼저 `outer_f()`가 호출 -> `x=10`, `outer_f()`의 지역 네임스페이스에 `x=10`이 만들어진다.
- 그다음 `outer_f()` 안에 있는 `inner_f()`가 실행된다.  
- `inner_f()`에서 `x=20`이 실행되며, `inner_f()`의 지역 변수 `x=20`이 새로 만들어진다. 
```python
outer_f() 실행
│
├─ 지역 변수 x = 10
│
├─ inner_f() 호출
│      │
│      └─ 별도의 지역 변수 x = 20
│
└─ outer의 x는 여전히 10
```

### #1.3 `nonlocal`

`nonlocal`은 내부 함수 안에서 바깥 함수의 지역 변수를 수정할 때 사용하는 키워드이다. 

`nonlocal 이름`은 해당 `이름`을 가장 가까운 바깥 함수 범위부터 먼저 찾는다.  
```python
def outer_f():
    x = 10 
    def inner_f():
        x = 20 

    inner_f()
    print(x)

outer_f()
>> 10

------------------

def outer_f():
    x = 10 
    def inner_f():
        nonlocal x
        x = 20 

    inner_f()
    print(x)
    print(locals())

outer_f()
>> 20
>> {'inner_f': <function outer_f.<locals>.inner_f at 0x787a11db5440>, 'x': 20}
```
`nonlocal x`로 선언했기 때문에, `inner_f()` 안에서 `x`를 만들 때 새로운 지역 변수 `x`(즉, `inner_f()`의 지역 변수 `x`)를 만들지 않고, 바깥 함수 `outer_f()`의 지역 변수 `x`를 사용한다.

이 예시의 경우, `inner_f()`의 `nonlocal x`때문에 `x=20`은 `outer_f()`의 지역 변수 `x`를 수정한다. 

그리고 `inner_f()`는 `outer_f()` 안에서 정의된 지역 이름이기 때문에 `outer_f()`에서 `locals()`를 호출하면 `inner_f`와 변경된 `x=20`이 함께 나타난 것이다. 


`global` 키워드를 사용하려면 중첩 함수의 중첩이 몇 단계이든 상관없이 무조건 전역 변수를 사용한다.
```python
x = 1 # 전역 변수
def outer_f():
    x = 10 # outer_f의 지역 변수
    def inner_f_1():
        x = 20 # ineer_f_1의 지역 변수
        def inner_f_2():
            global x # 전역 변수 x를 사용
            print(x)
        inner_f_2()
    inner_f_1()
 
outer_f()
>> 1
```

---
## #2 클로저 (Closure)
파이썬에서 클로저 (closure)는 바깥 함수(외부 함수)의 변수를 기억하고 있는 내부 함수이다. 

내부 함수가 자신을 감싸고 있는 바깥 스코프의 변수를 참조할 수 있다. 

아래 예시는 클로저가 만들어지고 활용되는 대표적인 형태이다.

이 예시는 ① 함수 안에 함수가 있고, ② 내부 함수가 바깥 함수의 변수를 참조하고, ③ 내부 함수를 반환하는 형태로 구성된다. 
```python
def calc():
    a = 2
    b = 4
    def wrapper(x):
        return x + a + b # 내부 함수 바깥쪽에 있는 외부 함수 calc의 지역 변수 a, b를 사용하여 계산
    return wrapper # wrapper 함수를 반환
 
a = add()
print(a(2))
print(a(4))
```

`add()`를 호출하면 먼저 `add`의 지역 범위가 만들어진다.
```python
add의 지역 범위
- a = 2
- b = 4
- wrapper 함수
```

`x`는 `wrapper`의 매개변수이므로 `wrapper`의 지역 변수이다.

반면 `a`와 `b`는 `wrapper()` 안에서 만들어진 것이 아니다. 한 단계 바깥 함수인 `add()`에서 만들어진 지역 변수이다.


이 예시를 보면 `wrapper`를 호출하는 것이 아니라 `wrapper` 함수 객체 자체를 반환한다. 그러므로 `c = add()`를 실행하면
`c`는 `wrapper` 함수 자체를 가리킨다.

이때 `add`의 실행은 끝나지만, `wrapper`는 자신이 사용하는 바깥 함수의 지역 변수 `a`, `b`에 대한 연결을 유지한다.
```python
c = add()

print(c.__code__.co_freevars)
>> ('a', 'b')
```

`__code__.co_cellvars`을 통해 내부 함수(중첩 함수)가 참조하는 외부 함수의 지역 변수 이름들이 들어 있는 튜플을 보면 `a`
, `b`를 볼 수 있다. 

파이썬은 이러한 자유 변수와의 연결을 wrapper 함수 객체의 __closure__에 있는 cell 객체들을 통해 유지한다.
- 여기서 자유 변수(free variable)란 내부 함수 안에서 쓰이지만, 그 내부 함수 안에서 선언하지 않은 외부 함수의 지역 변수를 말한다.

`__closure__`를 통해 `a, b`에 대응하는 두 개의 cell 객체를 확인할 수 있다. 
- `__closure__`는 중첩 함수(내부 함수)가 자신이 속한 외부 함수의 변수(자유 변수) 바인딩을 담고 있는 cell들의 튜플이다.
- 바인딩은 변수, 함수 등의 이름(identifier)에 실제 값이나 객체, 메모리 주소를 연결하는 것을 뜻한다. 
```python
print(c.__closure__)
>> (<cell at 0x784cf116e260: int object at 0xb1b1c8>, <cell at 0x784cf11bb5e0: int object at 0xb1b208>)

print(c.__closure__[0].cell_contents)
print(c.__closure__[1].cell_contents)
>> 2
>> 4 
```
`print(c.__closure__[0].cell_contents)`와 `print(c.__closure__[1].cell_contents)`를 실행하면
각각 `2`, `4`가 출력되어 
`wrapper`가 `a=2`, `b=4`와의 연결을 유지하고 있음을 확인할 수 있다.

`add()`의 실행 자체가 계속 살아 있는 것이 아니라, 반환된 `wrapper` 함수 객체가 
자신에게 필요한 `a`, `b`의 바인딩을 담은 cell들을 계속 참조하고 있기 때문에 
`add()`의 실행이 끝난 뒤에도 `c(2)`와 같이 `wrapper`를 호출하면 `a=2`, `b=4`를 사용할 수 있다.

이처럼 중첩 함수가 자신을 둘러싼 바깥 함수의 변수를 기억하고,
바깥 함수의 실행이 끝난 뒤에도 그 변수들을 사용할 수 있도록 유지하는 것을 클로저라고 한다.
이 예에서는 `c`가 가리키는 `wrapper` 함수가 클로저이다.

### #2.1 lambda로 클로저 만들기
```python
def add():
    a = 2
    b = 4
    return lambda x: x + a + b

c = add()
print(c(2))
print(c(4))
>> 8
>> 10
```
람다 식 자체를 반환하게 하면 된다. 이렇게 람다를 사용하면 클로저를 더 간단하게 만들 수 있다.


### #2.2 클로저의 지역 변수 변경하기
클로저의 지역 변수를 변경하고 싶다면 다음과 같이 `nonlocal`을 사용함녀 된다.
```python
def add():
    a = 2
    b = 4
    def wrapper(x):
        nonlocal a
        a = 10
        return x + a + b
    return wrapper

c = add()
print(c(2))
print(c(4))
>> 16
>> 18
```

---
## #3 데코레이터 (Decorator)

### #3.1

### #3.2

### #3.3 

### #3.4 매개변수를 받는 데코레이터


클래스로 데코레이터 만드는 거랑 함수로 데코레이터 만드는 거랑 결국 만들어지는
데코레이터가 동일한 거라면 굳이 이거는 정리할 필요 x 일단은. 

---
## #4. 이터레이터


---

## #5. 제너레이터


---

## #6. 어노테이션 


---
