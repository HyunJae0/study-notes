# 파이썬 - 이터레이터, 제너레이터, 타입 어노테이션

> [📚 전체 목차로 돌아가기](../../README.md)

## Table of Contents

- [이터레이터 (Iterator)](#1-이터레이터-iterator)
- [제너레이터 (Generator)](#2-제너레이터-generator)
- [타입 어노테이션 (Type Annotation)](#3-타입-어노테이션-type-annotation)

---

## #1. 이터레이터 (Iterator)

이터레이터는 `__next__` 메서드를 통해 값을 차례대로 꺼낼 수 있는 객체(object)이다.

### #1.1 반복 가능한 객체 (Iterable)
iterable이란 값을 한 번에 하나씩 값을 차례대로 반환할 수 있는 반복 가능한 객체를 말한다. 대표적으로 리스트, 튜플, 문자열, 딕셔너리, 세트가 이에 해당된다. 
- 반복 가능한 객체가 가장 넓은 개념이고, 시퀀스 객체는 반복 가능한 객체에 포함되는 하위 개념이다. 
- 모든 시퀀스 객체는 iterable이지만, 모든 iterable이 시퀀스 객체인 것은 아니다.  
    - 딕셔너리와 세트는 iterable이지만, 시퀀스 객체가 아님. 

iterable 객체의 `__iter__()` 메서드를 호출하면 실제로 값을 하나씩 꺼내는 iterator 객체를 얻을 수 있다.
```python
a = [1, 2, 3].__iter__()
type(a)
>> list_iterator
```
- `iter()`함수를 이용해서도 이터레이터로 만들 수 있다. 

그리고 이 iterator의 `__next__()` 메서드를 호출할 때마다 다음 값을 하나씩 가져온다. 모든 값을 꺼낸 뒤 다시 `__next__()` 를 호출하면 StopIteration 예외가 발생한다.
```python
a.__next__()
>> 1
a.__next__()
>> 2
a.__next__()
>> 3
a.__next__()
>> StopIteration
```


`range()`도 대표적인 Iterable이다. 
```python
r = range(3).__iter__()
type(r)
>> range_iterator
```

`for`에 `range`를 사용하면, `for` 문은 이 iterable에서 iterator를 만들어서 값을 하나씩 꺼내는 방식으로 동작한다. 
```python
for i in range(30000):
    print(i)
```
이 예시의 경우, `for` 문은 `iter()`로 iterator를 생성하고 next()를 반복 호출하다가 StopIteration 예외가 발생하면 종료된다. 

정리하면, iterable은 한 번에 하나씩 값을 차례대로 가져올 수 있는 객체이고, iterator는 `__next__` 메서드를 통해 차례대로 값을 가져올 수 있는 객체이다. iterable에서 `__iter__` 메서드로 iterator를 얻을 수 있다.

참고로 위의 `for ... range` 예시를 보면, `range(30000)`은 0부터 29999를 한 번에 만들어서 사용하지 않는다. `for`문에서 iterator를 생성한 다음, `next`로 값이 필요해질 때마다 하나씩 가져온다. 

즉, 실제로 필요한 시점까지 값의 계산을 미루며, 이러한 방식을 지연 평가(lazy evaluation)라고 한다. 
- 파이썬은 값이 실제로 필요할 때까지 데이터 생성을 뒤로 미루는 방식을 사용한다. 이 방식을 지연 평가라고 한다. 
- 미리 모든 값을 만들지 않아 큰 범위를 다룰 때도 메모리를 효율적으로 사용할 수 있다. 

### #1.2 클래스로 이터레이터 만들기

클래스에서 `__iter__`와 `__next__` 메서드를 구현하여 이터레이터를 만들 수 있다. 
```python
class Fibonacci:
    def __init__(self, count):
        self.count = count
        self.generated_count = 0
        self.current = 0
        self.next_value = 1

    def __iter__(self):
        return self

    def __next__(self):
        if self.generated_count >= self.count:
            raise StopIteration

        result = self.current

        self.current, self.next_value = (
            self.next_value,
            self.current + self.next_value
        )

        self.generated_count += 1
        return result


for number in Fibonacci(7):
    print(number, end=' ')
```
`Fibonacci` 클래스는 `__iter__()`와 `__next__()`를 구현하여 iterator로 동작한다.

이 클래스는 `__next__()`가 호출될 때마다 다음 피보나치 수 하나를 반환한다. 
그리고 이미 지정한 개수만큼 값을 반환했다면 StopIteration을 발생시킨다.
파이썬의 iterator는 이런 방식으로`__next__()`를 통해 값을 하나씩 반환하고, 값이 소진되면 StopIteration을 발생시킨다.

한편 `__iter__()`는 반복에 사용할 iterator 객체를 반환하는 역할을 한다.
`__iter__()`는 `__next__()` 메서드를 가진 객체를 반환해야 한다.
`Fibonacci` 객체(`self`)는 이미 `__next__()` 메서드를 가지고 있어, 객체 자신이 곧 iterator 역할을 할 수 있다.
그러므로, `__iter__()`에서 새로운 iterator 객체를 따로 만들 필요 없이 현재 객체인 self를 그대로 반환하면 된다. 

### #1.3 iter, next 함수

`iter(iterable)`로 리스트와 같은 iterable를 `iter()`에 넣으면 iterator가 반환된다. iterator에 `next()` 함수를 이용하면 값을 하나씩 꺼낼 수 있다. 

`iter()`함수는 2개의 인자를 받을 수도 있다. 첫 번째 인자에는 호출 가능한 객체(함수)를 넣고, 두 번째 인자에는 반복을 끝낼 값을 지정한다: `iter(callable, sentinel)`

`iter(callable, sentinel)` 이 방식은 함수를 계속 실행하다가, 특정 값이 나오면 반복을 멈추고 싶을 때 유용하다.
```python
import random

def get_random_number():
    return random.randint(1, 5)

# get_random_number 함수를 계속 실행하다가, 5가 나오면 멈추는 이터레이터 생성
rand_iterator = iter(get_random_number, 5)

for num in rand_iterator:
    print(f"생성된 숫자: {num}")

>>
생성된 숫자: 2
생성된 숫자: 3
생성된 숫자: 4
생성된 숫자: 2
생성된 숫자: 1
생성된 숫자: 2
```
이 예시는 `get_random_number()`를 계속 호출해서 값을 가져오되, 반환값이 5가 되면 반복을 끝낸다. 

`next(iterator, default)`는 iterator에서 다음 원소를 하나씩 꺼내 반환한다. `default`는 옵션이며, `default`를 지정하면 더 이상 꺼낼 값이 없을 때 에러 대신 `default`를 반환한다. 
```python
it = iter(range(2))

next(it, -1)
>> 0

next(it, -1)
>> 1

next(it, -1) # default 반환 
>> -1

next(it, -1) # default 반환 
>> -1
```

---

## #2. 제너레이터 (Generator)
제너레이터는 이터레이터를 생성해 주는 함수이다. 

제너레이터로 생성한 객체는 이터레이터이며, `next()` 함수를 호출할 때마다 값을 하나씩 순차적으로 얻을 수 있다. 이처럼 필요한 시점에 값을 생성하므로 지연 평가가 가능하다.

제너레이터는 값을 생성할 때 `yield` 키워드를 사용하며, `yield`를 만나면 값을 반환한 뒤 함수의 실행 상태를 유지한 채 일시 정지한다. 이후 다시 `next()`가 호출되면 정지했던 위치부터 실행을 계속한다. 

아래는 `yield`를 사용해서 제너레이터를 만든 예시이다.
```python
def gen():
    yield 1
    yield 2

for i in gen():
    print(i)

>> 
1
2
```

`yield`와 다르게 `return`은 실행 자체를 종료시킨다. 
```python
def gen():
    yield 1
    return
    yield 2
    
for i in gen():
    print(i)
>>
1
```
```python
def gen():
    yield 1
    yield 2
    return
    yield 3

g = gen() # g는 제너레이터로 생성한 객체로 이터레이터이다. 

next(g)
>> 1

next(g)
>> 2

next(g)
>> StopIteration: 
```
`return`을 만나 실행이 종료된 후, 다시 `next()`를 호출했을 때 StopIteration이 발생하는 것을 볼 수 있다.

---

## #3. 타입 어노테이션 (Type Annotation)

파이썬은 동적 프로그래밍 언어(dynamicprogramminglanguage)이다.

예를 들어, 다음 코드를 보면:
```python
a = 1
print(type(a))
# <class 'int'>

a = "1"
print(type(a))
# <class 'str'>
```
객체 `a`의 type이 `int`에서 `str`로 바뀐 것을 볼 수 있다. 는 `a`가 가리키는 객체가 `int` 객체에서 `str` 객체로 바뀐 것이다. 

변수는 그저 객체를 가리키는 이름이다. 같은 변수 이름이 실행 중에 서로 다른 타입의 객체를 가리킬 수 있다.

반대로 자바는 한 번 변수에 타입을 지정하면, 지정한 타입 외에 다른 타입은 사용할 수 없는 정적 프로그래밍 언어(static programming language)이다. 

동적 언어는 타입에 자유롭기 때문에 유연한 코딩이 가능하지만, 프로그램의 규모가 커지고 코드가 복잡해질수록 예상과 다른 타입의 값이 사용되는 문제를 파악하기 어려워질 수 있다. 

이러한 문제를 보완하기 위해 파이썬에서는 타입 어노테이션을 사용할 수 있으며, 이를 통해 변수와 함수의 매개변수, 반환값에 예상되는 데이터 타입을 명시할 수 있다: `변수명: 타입 = 값
```python
age: int = 20
name: str = "홍길동"
height: float = 175.5
is_student: bool = True
```
`age: int = 20`은 `age`에는 `int` 타입의 값이 들어올 것으로 예상한다는 뜻이다.


```python
def add(a: int, b: int) -> int:
    return a + b
```
반환값은 `-> 타입`으로 나타낸다. `add`함수가 `int` 두 개를 받아서 `int`를 반환하는 함수라는 의미이다. 

리스트나 딕셔너리도 타입을 적을 수 있다.
```python
numbers: list[int] = [1, 2, 3]
names: list[str] = ["Kim", "Lee"]

person: dict[str, int] = {
    "age": 20,
    "score": 90
}
```
`list[int]`는 정수를 원소로 가지는 리스트, `dict[str, int]`는 키는 문자열이고 값은 정수인 딕셔너리라는 뜻이다. 

참고로 파이썬에는 `mypy`라는 파이썬 표준 라이브러리가 있다. `mypy`로 타입 어노테이션이 올바르게 사용되었는지 검사할 수 있다. `mypy 파일명.py`를 통해 검사할 수 있다. 

---
