# 파이썬 - 내장 함수와 표준 라이브러리

> [📚 전체 목차로 돌아가기](../../README.md)

## Table of Contents

- [내장 함수 (Built-in Function)](#1-내장-함수-built-in-function)
- [파이썬 표준 라이브러리](#2-파이썬-표준-라이브러리)
- [operator 라이브러리](#operator-라이브러리)

---

## #1 내장 함수 (Built-in Function)

내장 함수는 별도의 모듈이나 패키지를 `import`하지 않고도 바로 사용할 수 있도록 기본 제공되는 함수를 말한다. 

---

`abs(number)`: number의 절댓값을 반환한다.
```python
abs(3)
abs(-3)

>> 3
>> 3
```
---

`all(iterable)`: iterable을 입력으로 받으며, iterable의 모든 요소가 참이면 (또는 iterable이 비어 있으면) `True`, 하나라도 거짓이 있으면 `False`를 반환한다. 
```python
all([1, 2, 3])
all([1, 2, 3, 0])
all([ ])
all(" ")

>> True
>> False
>> True
>> True
```
- `0`은 `False`이다. 그러므로 `all([1, 2, 3, 0])`는 `False`이다.

원래 빈 리스트(`[ ]`)는 조건문에서 `False`이다. 

그러나 `all`은 조건문처럼 리스트 자체가 참/거짓인지 묻는 게 아니라, iterable인 리스트의 모든 요소가 참/거짓인지 묻는 함수이다. 

그래서 `all([ ])`의 결과로 `True`가 반환된다. 빈 리스트에는 0개 항목이 있다. 즉, 단 하나의 "거짓" 요소가 없는 것이다. `all(" ")`도 같은 원리가 적용된다. 

---

`any(iterable)`: iterable을 입력으로 받으며, iterable의 요소 중 하나라도 참이 있으면 `True`, 모두 거짓일 때만 `False`를 반환한다. 즉, `all(iterable)`과 반대로 작동한다.
```python
any([1, 2, 3])
any([1, 2, 3, 0])
any([ ])
any([0, ""]) # ""는 True이지만, 0은 False로 처리된다 => False

>> True
>> True
>> False
>> False
```

---

`bin(integer)`: 정수(`int`)만 입력으로 받는다. 실수를 넣으면 오류가 발생한다. 정수를 앞에 "0b"가 붙는 2진수 문자열로 바꿔주는 함수이다. 
```python
bin(3)

>> '0b11'
```
- 참고로 prefix "0b"를 빼고 숫자만 얻고 싶다면 문자열 슬라이싱 `bin(integer)[2:]`을 사용하면 된다.

---

`chr(codepoint)`: 유니코드 숫자 값을 받아 그 코드에 해당하는 문자를 반환한다. 
```python
chr(97)

>> 'a'
```

---

`dir(object)`: 객체가 지닌 변수나 함수의 이름을 리스트로 반환한다. 
```python
dir([1, 2])
```

---

`divmod(a, b)`: 2개의 숫자 a, b를 입력으로 받아, a를 b로 나눈 몫과 나머지를 튜플로 반환한다. 
```python
divmod(1, 10)

>> (0, 1)
```
- 즉, `divmod`는 `a // b` 연산(나눈 몫 연산)의 결과와 `a % b` 연산(나머지 연산)의 결과를 튜플로 반환하는 함수이다. 

---

`enumerate(iterable, start=0)`: 입력으로 받는 iterable은 시퀀스, 이터레이터 또는 이터레이션을 지원하는 객체여야 한다. start(기본값은 0)부터 iterable을 이터레이션해서 얻어지는 값을 포함하는 튜플을 얻을 수 있다.  
```python
alphabet = ["a", "b", "c"]

list(enumerate(alphabet))
list(enumerate(alphabet, start=-4))

>> [(0, 'a'), (1, 'b'), (2, 'c')]
>> [(-4, 'a'), (-3, 'b'), (-2, 'c')]
```

---

`eval(expression)`: 문자열로 구성된 expression을 입력으로 받으며, expression을 실행한 결과를 반환한다.
```python
eval("1 + 3")
eval("divmod(1, 10)")
eval("'ab' + 'c'")

>> 4
>> (0, 1)
>> 'abc'
```

---

`filter(function, iterable)`: 첫 번째 인자로 function, 두 번째 인자로 function을 적용할 iterable을 받는다. iterable의 요소 순서대로 function을 적용했을 때 그 결과가 `True`인 것만 필터링하여 filter 객체(이터레이터)를 반환한다. 

`filter`는 보통 iterable에서 특정 조건에 맞는 요소들만 필터링할 때 사용한다.
```python
def is_even(n):
    return n % 2 == 0

numbers = [0, 1, 2, 3]

for i in filter(is_even, numbers):
    print(i)

>> 
0
2
```

이렇게 간단한 함수를 사용하는 경우, `def` 대신 `lambda`를 `filter` 함수와 함께 사용해도 된다. 
```python
list(filter(lambda x: x % 2 == 0, numbers))
>> [0, 2]
```
---

`getattr(object, name, default)`: 객체의 속성이나 메서드의 이름을 전달해 그 값을 가져온다. default는 옵션으로 가져올 속성이 존재하지 않을 때 반환할 기본값이다.
```python
class Person:
    name = "Alice"
    age = 25

p = Person()

# 일반적인 접근
print(p.name)  # Alice

# getattr 사용
print(getattr(p, "name"))  # Alice
print(getattr(p, "address", "No Address"))  # No Address (기본값 반환)

>> Alice
>> Alice
>> No Address
```
---

`globals()`: 현재 실행 중인 모듈(즉, 파이썬 파일)의 전역 변수, 함수, 클래스 등을 딕셔너리 형태로 반환한다.
```python
globals()
```
---

`help(request)`: 도움말 시스템을 호출한다. 특정 모듈, 함수, 클래스, 데이터 타입의 사용법 등을 보여준다.
```python
help(str.swapcase)
>>
Help on method_descriptor:

swapcase(self, /) unbound builtins.str method
    Convert uppercase characters to lowercase and lowercase characters to uppercase.
```

---

`hex(integer)`: 정수를 입력받아 16진수 문자열로 변환하는 함수 
```python
hex(3)

>> '0x3'
```

---

`id(object)`: 객체를 입력받아 object의 고유 주솟값을 반환한다. 
```python
id(1)
id(2)

>> 11645352
>> 11645384
```

---

`input()`: 사용자 입력을 받는 함수이다. 

---

`int(number=0)`: 문자열 형태의 숫자나, 소수가 있는 숫자를 **가장 가까운 정수로 내림**하여 반환하는 함수
```python
int()
int(3.7)
int("13")

>> 0
>> 3
>> 13
```

---

`isinstance(object, classinfo)`: 첫 번째 인자로 객체, 두 번째 인자로 클래스를 받는다. 입력으로 받은 객체가 특정 클래스나 데이터 타입의 인스턴스인지 확인하여 참이면 `True`, 거짓이면 `False`를 반환한다.
```python
class Person:
    name = "Alice"
    age = 25

class Student(Person):
    pass

student = Student()
isinstance(student, Person) 
>> True
```
상속 관계에 있는 부모 클래스인지도 확인할 수 있다. 자식 클래스로 만든 객체는 부모 클래스의 인스턴스
```python
isinstance({"a":1}, dict)
>> True
```
두 번째 인자에 튜플을 전달하면, 첫 번째 인자로 전달된 객체가 튜플에 표현된 것 중 하나라도 일치하는지 확인할 수 있다. 
```python
x = 10

isinstance(x, (str, float, dict, int))
>> True
```

--- 


`issubclass(class, classinfo)`: 첫 번째 인자가 두 번째 인자에 지정된 클래스들의 자식 클래스인지 확인한다. 두 번째 인자에 튜플을 전달하여 여러 클래스들을 비교할 수 있다. 
```python
class A: pass
class B(A): pass
class C: pass

issubclass(B, (C, A))
issubclass(B, (C, int))
>> True
>> False
```

---

`len(object)`: 객체의 길이(item 전체 개수)를 반환한다. 

인자는 시퀀스(문자열, 튜플, 리스트, range) 또는 컬렉션(딕셔너리, 집합, 리스트, 튜플) 자료형일 수 있다. 

`len`은 `range(2 ** 100)`와 같이 `sys.maxsize`보다 긴 길이에서는 OverflowError가 발생한다. 
```python
len(range(2 ** 10)) 

>> 1024
```
```python
import sys

print(sys.maxsize)
print(range(2 ** 100)) 
len(range(2 ** 100))
>> 9223372036854775807
>> range(0, 1267650600228229401496703205376)
>> OverflowError: Python int too large to convert to C ssize_t
```

---

`map(function, iterable, /, *iterables)`: function과 iterable을 입력받아, iterable의 각 요소에 function을 적용한 결과를 `map` 객체로 반환한다. iterable을 추가로 0개 이상 받을 수도 있다. 
```python
def two_times(x):
    return x*2

def add(x, y):
    return x + y

a = [1, 2, 3]
b = [10, 20, 30]

print(list(map(two_times, a)))
print(list(map(add, a, b)))
>> [2, 4, 6]
>> [11, 22, 33]
```
첫 번째 예시는 리스트 `a`의 첫 번째 요소인 1이 `two_times` 함수의 입력으로 들어가고 1*2를 거쳐 2가 된다. 다음으로 `a`의 두 번째 요소인 2가 `two_times` 함수의 입력으로 들어간다. 이러한 과정을 거쳐 결과가 [2, 4, 6]이 된다.

두 번째 예시는 `map` 함수가 하나의 function과 두 개의 iterable을 입력으로 받는다. 

여러 개의 iterable을 `map()`에 전달하면, 각각 동일한 위치에서 값을 하나씩 꺼내 함수에 병렬로 전달한다. 

처음에는 리스트 `a`와 `b`의 값 1과 10이 `add` 함수의 입력으로 들어가고 `add(1, 10)`을 통해 11이 반환된다.

다음으로 두 리스트의 두 번째 요소인 4와 22가 `add` 함수의 입력으로 들어가서 `add(4, 22)`를 통해 26이 반환된다. 

즉, `map(add, a, b)`는 사실상 `add(a[0], b[0])`, `add(a[1], b[1])`, `add(a[2], b[2])` 작업을 처리한다. 

이터레이터 객체를 반환하므로, 값을 보려면 형 변환이 필요하다. 그래서 이 예시에서는 `list()`를 적용했다. 
- 위의 `filter`도 filter 객체(이터레이터 객체)를 반환한다. 값을 보기 위해 `list()`를 적용한 것이다. 

`def` 대신 `lambda`를 사용할 수 있다.
```python
list(map(lambda x: x * 2, a))
list(map(lambda x, y: x + y, a, b))

>> [2, 4, 6]
>> [11, 22, 33]
```

---

`max(iterable, key=None)`: iterable을 받아 iterable 안에서의 최댓값을 반환한다. 
```python
max([1, 2, 3])
max("abc")

>> 3
>> 'c'
```

`max()`에서 중요한 부분은 `key`이다. **`key=함수`로 설정하면, "해당 함수를 적용한 결과를 기준으로 가장 큰 값"을 찾는다.**
```python
words = ["apple", "bananaanaana", "kiwi"]

max(words, key=len) 

>> 'bananaanaana'
```
이 예시는 `words`에 `len()` 함수를 적용한 결과, 가장 큰 `len` 값을 갖는 "bananaanaana"를 반환한다.

`key`에는 함수를 전달하기 때문에 `lambda`도 지정할 수 있다.
```python
lst = [
    ("a", 80),
    ("b", 95),
    ("c", 90)
]

max(lst, key=lambda x: x[1])

>> ('b', 95)
```
이 예시는 튜플의 두 번째 원소를 기준으로 가장 큰 값을 갖는 요소를 반환한다.

`default`는 **iterable이 비어 있을 때 무엇을 반환할지** 지정하면 된다. 

다음과 같이 iterable이 비어 있고 `default`를 지정하지 않으면 ValueError가 발생한다. 
```python
numbers = []

max(numbers)

>> ValueError: max() iterable argument is empty
```
이런 상황에서 `default`를 사용하면
```python
numbers = []

max(numbers, default=10)

>> 10
```

`default`와 `key`를 같이 사용할 수도 있다. 
```python
words = ["apple", "banana", "kiwi"]

while True:
    result = max(words, default="ABC", key=max)
    print(result)

    if not words:
        break
        
    words.pop()

>>
kiwi
apple
apple
ABC
```
이 예시는 각 문자열에 `max()`를 적용한 값을 기준으로 비교한다. 
```python
max("apple")         # 'p'
max("bananaanaana")  # 'n'
max("kiwi")          # 'w'
```
첫 번째 단계에서 각 문자열에 `max()`를 적용한 값을 기준으로 비교하면, `w > p > n`이므로 바깥의 `max()` 함수가 선택하는 원래 값은 `"kiwi"`가 된다. 

`pop()`을 진행하면 `words` 리스트는 빈 리스트가 되어 비교할 요소가 없어진다. `default="ABC"`가 지정되어 있으므로 `ABC`를 출력한다. 

`max(arg1, arg2, /, *args, key=None)`로 **위치 인자를 2개 이상 전달하면, 그 인자들 중 가장 큰 것을 반환**한다.
```python
a = [10, 20]
b = [1, 100]

max(a, b)

>> [10, 20]
```
이 예시는 `max()`에 리스트 2개를 각각 인자로 전달했다: `arg1 = a =[10, 20]`, `arg2 = b = [1, 100]`

파이썬은 두 리스트 자체를 서로 비교한다. 리스트끼리 비교할 때는 앞에서부터 순서대로 비교한다. 첫 번째 요소를 비교하면 `1 < 10`이다. 그러면 이것을 `[10, 20] > [1, 100]`이라고 판단한다. 따라서 결과로 `[10, 20]`이 나온 것이다. 

https://docs.python.org/ko/3/library/functions.html => 이거랑 점프투파이썬 같이 보면서 정리 

---

`min(iterable, /, *, key=None)`, `min(iterable, /, *, default, key=None)`, `min(arg1, arg2, /, *args, key=None)`은 `max` 함수와 반대로 작동한다. 

`min()`에서 `key`를 사용하면 **key 값이 가장 작은 원래 요소**를 반환한다.

---

`next(iterator, default)': 이터레이터(iterator)에서 다음 값을 하나 꺼내는 함수이다. 

`default`는 옵션으로 다음 값이 있으면 그 값을 주고, 더 이상 값이 없으면 `default`를 반환한다.
```python
numbers = [10, 20, 30]

it = iter(numbers)

print(next(it))
print(next(it))
print(next(it))
print(next(it))
>> 10
>> 20
>> 30
>> StopIteration: 
```
iterator가 소진되었으며, `default`가 없으므로 StopIteration 예외가 발생한다. 
```python
print(next(it))
print(next(it))
print(next(it))
print(next(it, -1))
>> 10
>> 20
>> 30 
>> -1
```
마지막 `next`에 `-1`이라는 `default`를 지정했기 때문에 `-1`이 출력된다. 
---

`oct(integer)`: 정수를 8진수 문자열로 반환한다.
```python
oct(10)

>> '0o12'
```
- prefix "0o"를 제거하고 싶으면 문자열 슬라이싱을 사용하면 된다.

---

`open(file, mode='r')`: 파일 이름과 읽기 방법을 입력받아, 파이썬이 읽거나 쓸 수 있는 파일 객체(file object)를 만들어주는 함수이다.
- `open()`에는 더 많은 인자가 있다. https://docs.python.org/ko/3/library/functions.html#isinstance

첫 번째 인자 `file`에는 파일 이름을 지정해 **어떤 파일을 열 것인지**를 지정한다. 
```python
f = open("hello.txt")
f = open("data/hello.txt") # 상대 경로
f = open("/Users/me/data/hello.txt") # 절대 경로도 사용할 수 있다.
```

`mode`에는 **파일을 어떤 방식으로 열 것인지**를 지정한다. 
```python
mode

"r"
# 읽기 모드 (기본값)
# 파일이 없으면 에러 발생

"w"
# 쓰기 모드 
# 파일이 없으면 새로 생성
# 파일이 이미 있으면 기존 내용을 모두 지우고 연다.

"a"
# 추가(append) 쓰기 모드, 이어쓰기
# 파일이 없으면 새로 생성
# 파일이 이미 있으면 기존 내용은 유지하고, 새로 쓰는 내용은 파일 끝에 추가된다.

"x"
# 새 파일 생성 모드
# 파일이 없으면 새로 생성한다.
# 이미 파일이 존재하면 에러가 발생한다.

"b"
# 바이너리(binary) 모드
# 파일을 바이너리 모드로 열어 bytes 단위로 읽고 쓸 때 사용
# 보통 "r", "w" 등과 조합한다.
# 예: "rb" = 바이너리 읽기
#     "wb" = 바이너리 쓰기

"t"
# 텍스트(text) 모드
# 기본값이므로 보통 생략한다.
# "r" == "rt"

"+"
# 읽기와 쓰기를 모두 가능하게 한다.
# 단독으로 사용하지 않고 r, w, a 등과 조합한다.

"r+"
# 읽기 + 쓰기
# "r"의 성격을 유지한다.
# → 파일이 반드시 존재해야 함
# → 기존 내용을 지우지 않음

"w+"
# 읽기 + 쓰기
# "w"의 성격을 유지한다.
# → 파일이 없으면 생성
# → 파일이 있으면 기존 내용을 모두 지움

"a+"
# 읽기 + 쓰기
# "a"의 성격을 유지한다.
# → 파일이 없으면 생성
# → 기존 내용은 유지
# → 쓰기는 파일 끝에 추가
```

---

`ord(character)`: character의 유니코드 숫자 값을 반환한다.
```python
ord('a')

>> 97
```

---

`pow(base, exp, mod=None)`: `base`의 `exp` 거듭제곱을 반환한다.
```python
pow(2, 10)

>> 1024
```
mod가 있는 경우, base의 거듭제곱 값(`base ** exp`)을 구한 뒤, 이를 `mod`로 나눈 나머지('(base ** exp) % mod`)를 계산한다.  
```python
pow(2, 10, mod=10)

>> 4
```
---

`range(start, stop, step=1)`: 입력받은 숫자에 해당하는 정수 범위 값을 iterable 객체로 만들어 반환한다. `start`와 `step`은 생략가능이다. 
```python
range(stop): 0부터 stop - 1까지의 정수를 만든다. 
range(start, stop): start부터 stop - 1까지의 정수를 만든다. 
range(start, stop, step): start부터 stop - 1까지 step 간격으로 정수를 만든다. 
```
```python
tuple(range(10))

>>
(0, 1, 2, 3, 4, 5, 6, 7, 8, 9)
```

---

`reversed(object)`: 원본 데이터를 바꾸지 않고, 객체의 요소를 역순으로 꺼내는 이터레이터 객체를 반환한다.
```python
a = [1, 2, 3]

for i in reversed(a):
    print(i, end="")

>> 321
``` 
```python
b = list(reversed(a))

print(a, b)
>> [1, 2, 3] [3, 2, 1] # 원본 a 유지
```

---

`round(number, ndigits=None)`: `ndigits`를 생략하면 가장 가까운 정수로 반올림하고, `ndigits`를 지정하면 그 자릿수까지 반올림
```python
print(round(3.14159, 2)) # 소수점 둘째 자리까지
print(round(3.14159, 3)) # 소수점 셋째 자리까지
>> 3.14
>> 3.142
```

`round()`는 `ndigits`을 생략하거나 `None`으로 지정하면 정수를 반환하지만, `ndigits`을 지정하면 `number`와 같은 타입을 반환한다. 
```python
r = round(2.6)
print(r, type(r))
>> 3 <class 'int'>
```
```python
r = round(2.6, 0)
print(r, type(r))
>> 3.0 <class 'float'>
```

---

`set(iterable=())`: iterable을 받아 중복이 제거된 집합 객체를 생성한다. 

---

`sorted(iterable, /, *, key=None, reverse=False)`: 위치 인자 `iterable`과 키워드 인자 `key`, `reverse`를 받는다.

`iterable`의 요소들로 정렬된 새 리스트를 반환한다. 원본 iterable 자체는 유지한다.
```python
a = [3, 2, 1]
b = sorted(a)

print(a, b)
>> [3, 2, 1] [1, 2, 3]
```

`reverse`의 기본값은 `False`로 오름차순이다. 내림차순으로 만들고 싶다면 `reverse=True`

`key`에 함수를 지정할 수 있다.
```python
words = ["apple", "banana", "kiwi"]
print(sorted(words, key=len))
>> ['kiwi', 'apple', 'banana']
```
이 예시는 `words`의 요소들에 각각 `len`을 적용한다. 
```python
원래 값       key=len 결과

"apple"       → 5
"banana"      → 6
"kiwi"        → 4
```
`key=len`의 결과를, `reverse=False`이므로 오름차순으로 정렬하면 `4 → 5 → 6` 순으로 정렬되어 `['kiwi', 'apple', 'banana']`가 반환된다. 

---

`str(iterable=())`: 문자열 자료형을 반환한다. 

---

`sum(iterable, /, start=0)`: start 및 iterable의 항목들을 왼쪽부터 오른쪽으로, 차례대로 더해서 그 합계를 반환한다.
```python
a = []
b = [1, 2, 3]

print(sum(a))
print(sum(b))
print(sum(b, 2))
>> 0
>> 6
>> 8
```
- 첫 번째 예시의 경우 `a`는 빈 리스트이기 때문에 `a`에는 더할 item들이 없다. 단, `start=0`이기 때문에 0이 출력된다. `sum(a=[ ], start=0)`이기 때문이다.
- 마지막 예시의 경우 `start=2`이다. 그러므로 `start=2`와 `b`의 items을 왼쪽부터 오른쪽으로 더한 값이 출력된다: `2 + 1 + 2 + 3 = 8`

---

`tuple(iterable=())`: iterable이 입력으로 들어오면, iterable을 튜플로 변환하는 함수이다.

---

`type(object, /)`: 객체의 자료형이 무엇인지 반환하는 함수이다.
```python
type("a")
>> str
```
---

`type(object, /)`: 객체의 자료형이 무엇인지 반환하는 함수이다.

---

`zip(*iterables, strict=False)`: 여러 iterable을 동일한 위치끼리 묶어서 튜플로 만드는 함수이다.

여러 iterable을 병렬로 순회하면서, 각 iterable에서 하나씩 꺼낸 값을 튜플로 만든다.

`zip()`도 iterator를 반환한다. 
```python
a = [1, 2, 3]
b = ["a", "b", "c"]

result = zip(a, b)

print(list(result))
>> [(1, 'a'), (2, 'b'), (3, 'c')]
```
```python
z = zip(a, b)

print(next(z))
print(next(z))
print(next(z))
>> (1, 'a')
>> (2, 'b')
>> (3, 'c')
```

`zip()`의 `strict=False`이면 **여러 iterable 중 가장 짧은 iterable이 끝나는 순간 zip()도 끝난다.**
```python
a = [1, 2, 3] # 길이 3
b = [10, 20] # 길이 2

print(list(zip(a, b)))
>> [(1, 10), (2, 20)]
```
`a`의 마지막 `3`이 사용되지 않은 것을 볼 수 있다. `a`의 3이 남지만 `b`가 끝났기 때문에 `zip`을 종료한 것이다.

그래서 `strict=False`이면, 각 iterable의 길이가 달라도 기본적으로 에러가 발생하지 않는다.

반면, `strict=True`는 각 iterable의 길이가 **반드시 같아야 한다.** 길이가 다르면 ValueError가 발생한다. 

그러므로 iterable들의 길이가 같다고 가정하는 상황에서는 `strict=True` 사용하는 것이 좋다.
```python
print(list(zip(a, b, strict=True)))
>> ValueError: zip() argument 2 is shorter than argument 1
```
```python
a = [1, 2, 3]
b = [10, 20, 30]

print(list(zip(a, b, strict=True)))
>> [(1, 10), (2, 20), (3, 30)]
```

---

## #2 파이썬 표준 라이브러리

---
### #2.1 datetime
```python
import datetime

print(datetime.datetime.now()) # 현재 날짜와 시간
```
다음과 같이 연, 월, 일을 가지는 `datetime.date` 객체를 만들 수 있다. 
```python
day1 = datetime.date(2025, 12, 31)
day2 = datetime.date(2026, 2, 10)
```
이렇게 만든 날짜의 차이는 다음과 같이 뺄셈 `-`으로 구할 수 있으며, 결과로 `datetime` 모듈의 `timedelta`객체가 반환된다. 
```python
diff = day1 - day2
diff
>> datetime.timedelta(days=-41)
```
```python
diff.days
>> -477
```
`datetime.timedelta(days=days)`는 **days라는 시간 간격**을 나타낸다. 
- `days`외에 `weeks`, `hours`, `minutes` 등이 있다. 예를 들어 `datetime.timedelta(weeks=2)`이면 2주, 즉 14일이다. 

위의 예시처럼 날짜를 과거로 이동시키는 것도 가능하고 다음과 같이 날씨를 미래로 이동시키는 것도 가능하다.
```python
day1 + datetime.timedelta(days=10)
>> datetime.date(2026, 1, 10)
```

요일은 `datetime.date` 객체의 `weekday()` 함수를 통해 구할 수 있다.
```python
day1.weekday()
>> 2
```
- 0은 월요일을 의미하며, 순서대로 1은 화요일, 2는 수요일, ... , 6은 일요일이다. 

`isoweekday()` 함수는 `datetime.date` 객체가 무슨 요일인지 숫자로 반환한다. 월요일은 1, 화요일은 2, ... , 일요일은 7로 반환한다.
```python
day1.isoweekday()
>> 3 # 수요일
```
---

### #2.2 time
`time` 라이브러리는 날짜와 시간을 다루는 기능을 제공한다. 

`time.time()`은 협정 세계시를 기준으로 1970년 1월 1일 0시 0분 0초 이후 흘러간 시간을 초 단위 실수로 반환한다. 
```python
import time

time.time()
```

`time.localtime()`은 초 단위 시간 값을 연, 월, 일, 시, 분, 초 등을 담은 `struct_time` 객체로 변환하는 함수이다.

`time.time()`이 반환한 실수도 `struct_time` 객체로 바꿔준다.
```python
time.localtime()

time.localtime(time.time())
```

`time.asctime()`은 현재 지역 시간(`time.localtime()`)을 기준으로 문자열을 만든다.
```python
time.asctime()
```
`time.struct_time` 객체를 입력으로 넣어서 원하는 시간의 문자열을 얻을 수 있다. 
```python
time.asctime(time.localtime(time.time()))
```

`time.ctime()`은 `asctime()`과 거의 동일하다. 다른 점은 항상 현재 시간만을 반환한다는 점이다.
```python
time.ctime()
```

`time.sleep()`은 지정한 초(second) 동안 프로그램의 실행을 일시 중지한다. 
```python 
time.sleep(3) # 프로그램을 3초간 일시 정지
```

---

### #2.3 math

`math.comb(n, k)`: 중복과 순서 없이 `n`개의 항목에서 `k`개의 항목을 선택하는 경우의 수(조합) 

`k <= n`이면 `n! / (k! * (n - k)!)`을 계산하고, `k > n`이면 `0`을 반환한다. 
```python
import math

math.comb(5, 2)
>> 10
```

`math.perm(n, k)`: 중복은 없지만, **순서 있게**  `n`개에서 `k`개의 항목을 선택하는 경우의 수이다. (순열)
```python
math.perm(5, 2)

>> 20
```

`math.factorial(n)`: 음이 아닌 정수 `n`의 factorial `n!`을 계산한다. 
```python
math.factorial(5)

>> 120
```

`math.gcd(*integers)`: 여러 정수의 최대 공약수를 계산한다. 
- 공약수는 두 개 이상의 수에서 공통된 약수를 말하며, 그 공약수 중 가장 큰 수를 최대 공약수라고 한다. 
- 파이썬 3.9부터 여러 개의 정수를 입력으로 받을 수 있다. 이전 버전에서는 두 개의 인자만 지원된다.
```python
math.gcd(12, 18, 24)

>> 6
```

`math.lcm(*integers)`: 여러 정수의 최소 공배수를 반환한다. 
- 최소 공배수는 두 수의 공통 배수 중 가장 작은 수이다.
- 파이썬 3.9부터 사용 가능하다. 
```python
math.lcm(4, 6)

>> 12
```

`math.isqrt(n)`: `n`의 제곱근에서 소수 부분을 버린 정수를 반환한다. 
```
math.isqrt(8)

>> 2
```

이 외에 더 다양한 기능은 https://docs.python.org/ko/3/library/math.html

---


### #2.4 random
`random`은 난수를 발생시키는 모듈이다.

---

`random.seed(a=None)`: 난수 생성기를 초기화한다. `a`가 생략되거나 `None`이면 현재 시스템 시간이 사용된다. 

---

`random.randrange(stop)`, `random.randrange(start, stop, step)`: `range()`가 만들어낼 수 있는 정수들 중 하나를 무작위로 골라 반환하는 함수.
```python
random.randrange(10)
>> 5
```
`randrange(10)`이므로 0 ~ 9 사이의 숫자 중 하나를 무작위로 선택한다.

---

`random.randint(a, b)`: `a`이상 `b` 이하의 정수 중 하나를 무작위로 반환하는 함수
```python
random.randint(1, 3)
>> 3
```

---

`random.choice(seq)`: 시퀀스 `seq`에서 임의의 요소 하나를 반환한다. `seq`가 비어 있으면 IndexError가 발생한다.
```python
lst = ["apple", "banana", "kiwi"]
result = random.choice(lst)

print(result)
>> banana
```

---

`random.choices(population, weights=None, *, cum_weights=None, k=1)`: `population`에서 **복원 추출** 방식으로 `k`개의 요소를 선택해서 리스트로 반환한다.
```python
result = random.choices(lst, k=3)
print(result)

>> ['kiwi', 'kiwi', 'apple']
```

`weights`는 각 요소가 선택될 상대적인 확률의 가중치를 시퀀스로 지정한다. 
```python
result = random.choices(
    lst,
    weights=[1, 8, 1],
    k=10
)
```
이 예시에서 apple, banana, kiwi의 weight는 각각 1, 8, 1이다. 총 가중치는 1 + 8 + 1 = 10이므로 대략적인 선택 비중은 apple은 1/10, banana 8/10, kiwi 1/10이므로 banana가 자주 나올 가능성이 높아진다. 

`cum_weights`는 누적 가중치이다. 

`weights`나 `cum_weights`를 지정하지 않으면, 같은 확률로 선택된다. 

---

`random.shuffle(x)`: 원본 시퀀스 `x`를 섞는다. 
```python
lst = list(range(10))

random.shuffle(lst)
print(lst)
>> [4, 1, 0, 7, 8, 9, 3, 6, 5, 2]
```

원본은 건드리지 않고 새롭게 섞인 리스트를 반환하려면 `random.sample(x, k=len(x))`를 사용하면 된다.

--- 

`random.sample(population, k, *, counts=None)`: `population`에서 중복 없이 `k`개를 무작위로 뽑은 새 리스트를 반환한다. 그래서 보통 비복원 추출 랜덤 샘플링에 사용된다. 
```python
lst = list(range(10))
new_lst = random.sample(lst, k=len(lst))
new_lst_2 = random.sample(lst, k=3)

print(lst)
print(new_lst)
print(new_lst2)
>> [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
>> [2, 3, 4, 7, 9, 1, 5, 8, 6, 0]
>> [8, 2, 3]
```

`counts`에는 각 요소가 `population`에 몇 번 들어 있는 것으로 취급할지를 지정한다.
```python
random.sample(
    ["red", "blue"],
    k=6,
    counts=[4, 2]
)

>> ['red', 'red', 'red', 'blue', 'blue', 'red']
```
이 예시에서는 `counts`을 통해 "red"가 4개, "blue"가 2개 있는 것으로 취급한다. 
---

`random.random()`: 0.0에서 1.0 사이의 실수 난수를 무작위로 생성하여 반환하는 함수
```python
random.random()
>> 0.46258050809294915
```

이외에 다양한 확률분포도 `random`을 통해 생성할 수 있다: https://docs.python.org/ko/3/library/random.html

---

### #2.5 itertools
iterable 데이터를 처리하는 유용한 함수들을 제공한다.

---
`itertools.accumulate(iterable[, function, *, initial=None])`: iterable의 값을 하나씩 누적 계산하면서, 그 중간 결과들을 차례대로 만들어 주는 함수. iterator를 반환한다. 

기본적으로 누적 합을 만든다. `function`을 주면 다른 방식의 누적 결과도 만들 수 있다. 
```python
import itertools

lst = list(range(1, 6)) # 1 ~ 5

list(itertools.accumulate(lst))
list(itertools.accumulate(lst, lambda total, x: total * x)) # 누적 곱
list(itertools.accumulate(lst, min)) # 지금까지 나온 값 중 최솟값을 계속 기록 
>> [1, 3, 6, 10, 15]
>> [1, 2, 6, 24, 120]
>> [1, 1, 1, 1, 1]
```
`initial`은 누적 계산을 시작할 초깃값이다. 
```python
list(itertools.accumulate(lst, initial=100))
>> [100, 101, 103, 106, 110, 115]
```
---

`itertools.batched(iterable, n, *, strict=False)`: iterable의 요소들을 `n`개씩 끊어서 튜플 묶음으로 반환한다.
```python
lst = ["apple", "banana", "kiwi"]

list(itertools.batched(lst, 2)) # lst의 요소들을 2개씩 묶기
>> [('apple', 'banana'), ('kiwi',)]
list(itertools.batched("applebanana", 3)) # 문자열을 3문자씩 묶기
>> [('a', 'p', 'p'), ('l', 'e', 'b'), ('a', 'n', 'a'), ('n', 'a')]
```
두 번째 예시는 `strict=False`라서 마지막 묶음이 `n`개보다 적어도 그냥 그대로 반환한다. 

`strict=True`로 설정하면 모든 묶음이 정확히 `n`개여야 한다. 그렇자 읺으면 TypeError가 발생한다. 
```python
list(itertools.batched("applebanana", 3, strict=True))
>> TypeError: batched() takes at most 2 arguments (3 given)
```

---

`itertools.chain(*iterables)`: 여러 iterable을 앞에서부터 차례대로 이어 붙인 iterator를 만든다.
```python
a = [1, 2, 3]
b = [10, 20]
c = [100, 200]

list(itertools.chain(a, b, c))
>> [1, 2, 3, 10, 20, 100, 200]
```

---

`itertools.combinations(iterable, r)`은 조합, `itertools.combinations_with_replacement(iterable, r)`은 중복을 허용한 조합을 만든다.

```python
data = ["A", "B", "C"]

result = itertools.combinations(data, 2)
result2 = itertools.combinations_with_replacement(data, 2)

print(list(result))
print(list(result2))
>> [('A', 'B'), ('A', 'C'), ('B', 'C')]
>> [('A', 'A'), ('A', 'B'), ('A', 'C'), ('B', 'B'), ('B', 'C'), ('C', 'C')]
```

---

`itertools.compress(data, selectors)`: `data`와 `selectors`을 같은 위치끼리 대응시킨 다음, `selectors`의 값이 참(True)인 위치의 `data`만 반환한다. 

`data`와 `selectors` 둘 중 어느 iterable이든 먼저 끝나면 `compress()`도 종료된다.
```python
data = ["A", "B", "C", "D", "E", "F"]
selectors = [1, 0, 1, 0, 1, 1]

result = itertools.compress(data, selectors)
print(list(result))
>> ['A', 'C', 'E', 'F']
```
```python
data = ["A", "B", "C", "D"]
selectors = [1, 0]

print(list(itertools.compress(data, selectors)))
>> ['A']
```

---

`itertools.count(start=0, step=1)`: `start`부터 시작해서 `step`만큼 계속 증가시키며 값을 만들어낸다. 
```python
cnt = itertools.count() # start=0, step=1

print(next(cnt))
print(next(cnt))
print(next(cnt))
print(next(cnt))
>> 0
>> 1
>> 2
>> 3
```

---

`itertools.cycle(iterable)`: 주어진 iterable의 요소를 끝까지 순회한 다음, 처음부터 다시 반복해서 무한히 반환하는 iterator를 만든다. 
```python
c = itertools.cycle(["A", "B", "C"])

for _ in range(10):
    print(next(c), end=" ")

>> A B C A B C A B C A 
```

---

`itertools.dropwhile(predicate, iterable)`: 은 `predicate`가 처음으로 `False`가 될 때까지 앞부분을 버리고, `False`가되는 요소부터는 조건 검사와 상관없이 전부 반환한다.

즉, 앞에서부터 조건이 `True`인 부분은 버리고, 처음 `False`를 만나는 요소부터는 전부 반환한다.
```python
lst = [1, 2, 3, 7, 4, 2]

result = itertools.dropwhile(lambda x: x < 5, lst)
print(list(result))
>> [7, 4, 2]
```
`lst`의 7부터는 `x<5`가 아니므로 `False`이다. 

---

`itertools.takewhile(predicate, iterable)`: iterable의 앞에서부터 검사하면서 `predicate`가 `True`인 동안만 요소를 가져온다. 처음으로 `False`가 나오면 바로 종료한다. 
```python
lst = [1, 2, 3, 7, 4, 2]

result = itertools.takewhile(lambda x: x < 5, lst)
print(list(result))
>> [1, 2, 3]
```
---

`itertools.filterfalse(predicate, iterable)`: 각 요소마다 `predicate`를 검사해서 결과가 `False`인 요소만 반환한다.
```python
result = itertools.filterfalse(lambda x: x % 2 == 0, lst)
print(list(result))
>>[1, 3, 7]
```

---

`itertools.groupby(iterable, key=None)`: 연속해서 같은 key 값을 가지는 요소들을 하나의 그룹으로 묶는다.
```python
data = ["A", "A", "B", "B", "B", "C"]

result = itertools.groupby(data)
for key, group in result:
    print(key, list(group))

>> 
A ['A', 'A']
B ['B', 'B', 'B']
C ['C']
``` 
`key`는 무엇을 기준으로 같은 그룹인지 판단할 것인가를 정한다. 
```python
data = [
    ("철수", "A"),
    ("영희", "A"),
    ("민수", "B"),
    ("수진", "B"),
    ("준호", "C")
]

for key, group in itertools.groupby(
    data,
    key=lambda x: x[1]
):
    print(key, list(group))

>>
A [('철수', 'A'), ('영희', 'A')]
B [('민수', 'B'), ('수진', 'B')]
C [('준호', 'C')]
``` 
`key=lambda x: x[1]`이므로, 튜플의 두 번째 값을 기준으로 같은 `key`끼리 그룹이 만들어진다. 

---
`itertools.islice(iterable, stop)`, `itertools.islice(iterable, start, stop[, step])`은 iterable에 리스트 슬라이싱과 비슷한 방식으로 일부 구간만 꺼내는 iterator를 만든다.
```python
lst = [10, 20, 30, 40, 50]

result = itertools.islice(lst, 3)
result2 = itertools.islice(lst, 1, 4)

print(list(result))
print(list(result2))
>> [10, 20, 30]
>> [20, 30, 40]
```

---

`itertools.pairwise(iterable)`: 서로 이웃한 요소를 2개씩 겹쳐서 묶는다.
```python
data = [10, 20, 30, 40]

print(list(itertools.pairwise(data)))
>> [(10, 20), (20, 30), (30, 40)]
```

---

`itertools.permutations(iterable, r=None)`: iterable에서 `r`개를 골라 순서를 고려해서 가능한 경우를 모두 생성한다. (순열)
```python
data = ["A", "B", "C"]

print(list(itertools.permutations(data, 2)))
>> [('A', 'B'), ('A', 'C'), ('B', 'A'), ('B', 'C'), ('C', 'A'), ('C', 'B')]
```

---

`itertools.product(*iterables, repeat=1)`: 데카르트 곱을 만든다. 
```python
a = ["A", "B"]
b = [1, 2, 3]

print(list(itertools.product(a, b)))
>> [('A', 1), ('A', 2), ('A', 3), ('B', 1), ('B', 2), ('B', 3)]
```
각각의 iterable에서 하나씩 골라 만들 수 있는 모든 조합이 반환된 것을 볼 수 있다.

---

`itertools.repeat(object[, times])`: 같은 객체를 계속 반복해서 반환하는 iterator를 만든다. `times`를 생략하면 무한히 반복한다.
```python
print(list(itertools.repeat("ABC", 5)))
>> ['ABC', 'ABC', 'ABC', 'ABC', 'ABC']
```

---

`itertools.zip_longest(*iterables, fillvalue=None)`: 여러 iterables의 같은 위치끼리 묶되, 가장 긴 iterable이 끝날 때까지 계속 묶는다.
```python
a = [1, 2, 3]
b = [10, 20]

result = itertools.zip_longest(a, b)
print(list(result))
>> [(1, 10), (2, 20), (3, None)]
```
이 예시에서 `b`는 먼저 끝나지만 `a`는 3이 남아 있다. 

`zip_longest()`는 가장 긴 iterable이 끝날 때까지 계속하기 때문에 부족한 `b` 쪽을 `fillvalue` 기본값 `None`으로 채운다.

```python
result = itertools.zip_longest(a, b, fillvalue=100)
print(list(result))
>> [(1, 10), (2, 20), (3, 100)]
```

https://docs.python.org/ko/3/library/itertools.html

---


### #2.6 operator
## operator 라이브러리
`operator`는 `+`, `-`, `==` 같은 기본 내장 연산자를 함수 형태로 제공하는 표준 라이브러리이다. 

연산자를 함수로 다룰 수 있어 `map()`이나 `sorted()` 등에서 연산자를 인자로 넘길 때 유용하게 쓰인다.

---
https://docs.python.org/ko/3.10/library/operator.html
---

`operator.attrgetter(attr)`는 객체에서 특정 속성(attribute)을 꺼내는 함수를 만들어준다.
```python
f = operator.attrgetter('name')
```
예시의 `f`는 객체의 `.name`을 꺼내는 함수가 된다. 
```python
class Student:
    def __init__(self, name, age):
        self.name = name
        self.age = age

student = Student("Kim", 20)

print(f(student))
>> Kim
```

---

`operator.itemgetter(item)`: 객체에서 특정 item을 꺼내는 함수를 만들어준다.

`attrgetter()`가 속성(attribute)을 꺼낸다면, `itemgetter()`는 `obj[0]`처럼 대괄호 `[]`로 접근하는 값을 꺼낸다.

```python
f = operator.itemgetter(2)
```
이 `f`는 `obj[2]`를 반환하는 함수이다.
```python
data = ["A", "B", "C", "D"]
print(f(data))
>> C
```

`operator.itemgetter(*items)`로 여러 개의 항목도 한 번에 꺼낼 수 있다.
```python
f = operator.itemgetter(0, 2)
print(f(data))
>> ('A', 'C')
```
이 예시의 `f`는 `(obj[0], obj[2])`를 반환한다. 

예를 들어 학생 정보가 `(이름, 나이, 성적)` 순서로 다음과 같다고 하자.
```python
students = [
    ("Kim", 20, 85),
    ("Lee", 22, 95),
    ("Park", 19, 75),
    ("Choi", 21, 90)
]
```
`map()`과 `itemgetter()`를 사용하여 학생들의 이름만 꺼낼 수 있다. 
```python
list(map(operator.itemgetter(0), students))
>> ['Kim', 'Lee', 'Park', 'Choi']
```
`operator.itemgetter(0)`는 사실상 `lambda x: x[0]`와 같다. 

`itemgetter()`에 여러 인덱스를 전달할 수도 있다. 아래 예시는 이름과 성적을 동시에 꺼낸다.
```python
list(map(operator.itemgetter(0, 2), students))
>> [('Kim', 85), ('Lee', 95), ('Park', 75), ('Choi', 90)]
```

아래 예시는 `sorted()`와 `itemgetter()`을 사용해 나이순으로 오름차순 정렬한다.
```python
list(sorted(students, key=operator.itemgetter(1)))
>> [('Park', 19, 75), ('Kim', 20, 85), ('Choi', 21, 90), ('Lee', 22, 95)]
```

마찬가지로 여러 인덱스를 전달할 수 있다.

딕셔너리에도 사용할 수 있다. 단, 딕셔너리의 키를 사용해야 한다.
```python
students = [
    {"name": "Kim", "age": 22, "score": 85},
    {"name": "Lee", "age": 25, "score": 95},
    {"name": "Park", "age": 30, "score": 75},
    {"name": "Choi", "age": 11, "score": 90}
]

list(sorted(students, key=operator.itemgetter('age'), reverse=True))
>> 
[{'name': 'Park', 'age': 30, 'score': 75},
 {'name': 'Lee', 'age': 25, 'score': 95},
 {'name': 'Kim', 'age': 22, 'score': 85},
 {'name': 'Choi', 'age': 11, 'score': 90}]
```

---

### #2.7 pickle
`pickle`을 사용하면 객체의 형태를 그대로 저장하고 불러올수 있다. 

`pickle`의 `dump(obj, file)`함수로 객체를 .pickle이라는 피클 확장자의 파일로 저장할 수 있다.
```python
import pickle
f = open("test.txt", 'wb')
data = {"a": "1", "b": "2"}
pickle.dump(data, f)
f.close()
```
- 저장하는 데이터는 바이너리 형태이다. 그래서 "wb" 형태의 쓰기 모드를 지정한다. 

`load(obj, file)`함수로 저장되어 있는 피클 파일(.pickle)을 불러올 수 있다. 저장한 객체 그대로(딕셔너리를 저장했다면 불러온 파일도 딕셔너리 그대로) 불러온다. 
```python
f = open("test.txt", 'rb')
data = pickle.load(f)
print(data)
>> {"a": "1", "b": "2"}
```
-저장한 피클 데이터가 바이너리이므로 "rb" 형태의 읽기 모드를 지정한다.

---

### #2.8 os

https://docs.python.org/ko/3/library/os.html

`os.getcwd()`: 현재 작업 디렉터리 위치를 문자열로 반환한다.

`os.chdir(path)`: 현재 디렉터리의 위치를 지정한 `path`로 변경한다. 

`os.environ["MY_VAR"]`: 환경 변수에 대한 정보를 반환한다. `os.environ["MY_VAR"] = "value"`를 통해 새로운 환경 변수를 추가할 수 있다. `del os.environ["MY_VAR"]`를 통해 지울 수도 있다. 

`os.getenv("KEY")`: 환경 변수 값을 읽어온다. 

`os.listdir("path")`: 현재 디렉터리에 존재하는 항목들의 이름을 리스트를 반환한다.

os.mkdir("path"): 지정한 "path"의 마지막 폴더 하나만 생성한다. 그래서 상위 폴더가 미리 만들어져 있어야 한다.

os.makedirs("path"): `os.mkdir()`과 달리 재귀적으로 디렉터리를 생성한다. 경로 상에 존재하지 않는 모든 중간 폴더를 자동으로 구성한다. 

동일한 이름의 폴더가 이미 존재하는 경우 두 함수 모두 FileExistsError를 발생시킨다. 이를 안전하게 처리하려면 `os.makedirs()`에서 제공하는 `exist_ok=True`로 설정하면 된다. 이는 이미 폴더가 있어도 오류를 내지 않고 그대로 넘어가도록 해주는 설정 값이다. 

`os.rmdir(path)`:  지정된 `path`의 빈 디렉터리를 삭제하는 함수이다. 디렉터리가 비어 있어야 삭제할 수 있다. 

`os.remove(path)`: 지정한 "path"의 파일을 삭제한다.

`os.removedirs(path)`: 지정한 "path"와 그 상위의 디렉터리들(단, 비어 있는 디렉터리만)을 연쇄적으로 삭제한다. 맨 하위 디렉터리를 지운 뒤, 비어 있는 상위 디렉터리가 있다면 차례대로 함께 삭제한다.
- 예를 들어 `path=a/b/c`라면, `c`를 지운 후, `b`, `c` 순으로 비어 있는지 확인하며 연쇄 삭제를 시도한다. 

`os.rename(src, dst)`: 파일 또는 디렉터리 이름 `src`를 `dst`라는 이름으로 바꾼다. 

---

### #2.9 tempfile
`tempfile`은 임시 파일을 만들고 사용한 후 자동으로 삭제해주는 기능을 제공한다. 그래서 메모리에 다 담기 어려운 대용량 데이터, 테스트 코드, 파일 변환 중 중간 단계 임시 저장 등 임시 파일 및 디렉터리가 필요한 경우 유용하다. 

https://docs.python.org/ko/3/library/tempfile.html

---

### #2.10 traceback
`traceback`은 오류 메시지와 스택 트레이스(stack trace)를 추적하고 출력하는 데 사용되는 라이브러리이다. 

예를 들어, 아래의 `calculate()` 함수는 0으로 나누기 때문에 ZeroDivisionError가 발생한다.
```python
def divide(a, b):
    return a / b

def calculate():
    return divide(10, 0)
```

`traceback.print_tb()`는 객체에 들어 있는 스택 정보만 출력한다. 예외 이름과 메시지 자체는 출력하지 않는다. 
```python
import traceback

try:
    calculate()

except ZeroDivisionError as e:
    traceback.print_tb(e.__traceback__)

>> 
  File "main.py", line 10, in <module>
    calculate()

  File "main.py", line 7, in calculate
    return divide(10, 0)
           ^^^^^^^^^^^^^
  File "main.py", line 4, in divide
    return a / b
           ~~^~~
```
어디서 어떤 함수를 거쳐 오류 지점까지 왔는지 확인할 수 있다.

`traceback.print_exception()`는 스택 트레이스, 예외 종류, 예외 메시지를 전부 출력한다. 
```python
try:
    calculate()

except ZeroDivisionError as e:
    traceback.print_exception(e)

>>
Traceback (most recent call last):
  File "main.py", line 10, in <module>
    calculate()

  File "main.py", line 7, in calculate
    return divide(10, 0)
           ^^^^^^^^^^^^^
  File "main.py", line 4, in divide
    return a / b
           ~~^~~
ZeroDivisionError: division by zero         
```

`traceback.print_exc()`는 `print_exception()`와 같다. 차이는 예외 객체를 직접 전달할 필요가 없다는 것이다.
```python
try:
    calculate()

except ZeroDivisionError as e:
    traceback.print_exc()
```

`traceback.format_tb()`: `print_tb()`와 달리, 출력하지 않고 문자열 리스트로 반환한다.
```python
try:
    calculate()

except ZeroDivisionError as e:
    print(traceback.format_tb(e.__traceback__))
```

`traceback.format_exception()`: `print_exception()`의 문자열 리스트 버전이다. 

`traceback.format_exc()`: 마찬가지로 문자열 버전이다. `print_exc()` 결과를 하나의 문자열로 반환한다. 
```python
try:
    calculate()

except ZeroDivisionError as e:
    print(type(traceback.format_exc()))

>> <class 'str'>
```

### #2.11 json

JSON 데이터를 처리하기 위해 사용한다.

`json.dumps()`: 파이썬 객체를 JSON 문자열로 내보낸다.
```python
import json

student = {
    "name": "Kim",
    "age": 20,
    "score": 85
}

result = json.dumps(student)
print(result)
>> {"name": "Kim", "age": 20, "score": 85}
```

`json.loads()`: JSON 문자열을 파이썬 객체로 불러온다. 
```python
json.loads(result)
>> {'name': 'Kim', 'age': 20, 'score': 85}
```

`json.dump()`: 파이썬 객체를 JSON 파일로 내보낸다. 
```python
student = {
    "name": "Kim",
    "age": 20,
    "score": 85
}

with open("student.json", "w", encoding="utf-8") as f:
    json.dump(student, f)
```

`json.load()`: JSON 파일을 파이썬 객체로 불러온다.
```python
with open("student.json", "r") as f:
    student = json.load(f)
```

---
