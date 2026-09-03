# 파이썬 - 자료형

> [📚 전체 목차로 돌아가기](../../README.md)

## Table of Contents

- [문자열 (String)](#1-문자열-string)
- [리스트](#2-리스트)
- [튜플](#3-튜플)
- [딕셔너리](#4-딕셔너리)
- [집합](#5-집합)

---

## #1 문자열 (String)

문자열은 문자의 모음으로 이루어진 자료형, 작은따음표(`' '`)나 큰따음표(`" "`)로 감싸서 표현할 수 있다. 

문자열은 한 번 생성하면 그 안의 글자를 직접 변경할 수 없는 불변(immutable) 자료형이다. 

문자열은 직접 값을 적어서 생성할 수도 있고, `str()` 함수를 사용하여 생성할 수도 있다. `str()`을 사용하면 다른 데이터 타입을 문자열로 변환할 수 있다. 

여러 줄로 이루어진 문자열은 삼중 따음표 `"""`나 `'''`를 사용한다.

### #1.1 문자열 인덱싱
- 문자열을 이루는 각 문자는 인덱스를 사용하여 접근할 수 있다. 인덱스는 **0부터 시작**한다. 음수 인덱스도 가능하다. 
```python
s1 = "123"

print(s1[0]); print(s2[-1])
>> 1
>> 3
```

### #1.2 문자열 덧셈 (문자열 연결)
- `+` 연산자를 사용하여 두 개 이상의 문자열을 연결할 수 있다. 

- 단, 문자열과 숫자 등 서로 다른 데이터 타입은 바로 연결할 수 없다. 필요하면 먼저 데이터 타입을 변환해야 한다.
```python
s1 = "123"
s2 = "456"
s3 = "789"
s4 = 10

print(s1 + s2 + s3)
print(s1 + s2 + s3 + str(s4))
>> 123456789
>> 12345678910
```

### #1.3 문자열 곱셈 (문자열 반복)
- `*` 연산자를 사용하여 특정 문자열을 지정한 횟수만큼 반복할 수 있다. 
```python
s1 = "H"

print(s1*2)
>> HH
```

### #1.4 문자열 슬라이싱
- 슬라이싱으로 문자열의 특정 부분만을 추출할 수 있. 기본 문법은 다음과 같다.
```python
문자열[start:end]
```
- `start`는 슬라이싱을 시작할 위치의 인덱스이다. 이 인덱스에 해당하는 요소는 슬라이싱 결과에 포함된다.
- `end`는 슬라이싱을 끝낼 위치의 인덱스이다. 이 인덱스에 해당하는 요소는 포함되지 않는다. 즉 `end` 바로 전에 있는 요소까지만 추출된다.
- 그리고 슬라이싱에서는 `start`나 `end`를 생략할 수 있다. 둘 다 생략할 경우 전체 요소를 의미한다.
- 음수 인덱스도 사용할 수 있다.

슬라이싱 문법에서는 `start`와 `end` 외에도 `step`을 지정할 수 있다. `step`의 기본값은 1이며, 2 이상의 값을 설정할 경우, 해당 간격만큼 건너뛰며 요소를 추출한다.
- `step`에 음수 값을 사용하면 리스트를 거꾸로 뒤집어서 요소를 추출할 수 있다.
```python
s1 = "abcd"

print(s1[::1]) # step=1 기본값
print(s1[::2])
print(s1[::3])
print(s1[::4])
>> abcd
>> ac
>> ad
>> a

print(s1[::-1])
print(s1[::-2])
print(s1[::-3])
print(s1[::-4])
>> dcba
>> db
>> da
>> d
```
- `lst[::-1]`은 `start`와 `end`를 생략했기 때문에 리스트 전체를 가져온다. `step`이 -1이므로 `end-1`부터 시작해 `start`순으로 item을 가져온다. 그래서 reverse되는 것이다. 

### #1.5 문자열 포매팅
- 문자열 포매팅은 문자열 안에 어떤 값을 삽입하는 방법을 말한다. 문자열 포맷 코드의 종류는 다음과 같다.
- `%s`: 문자열 
- `%c`: 문자 1개 
- `%d`: 정수
- `%f`: 부동소수
- `%o`: 8진수
- `%x`: 16진수
```python
print("%d apples" % 5)
print("%s apples" % "five")
print("%f apples" % 5.0)
print("%d%%" % 100)
>> 5 apples
>> five apples
>> 5.000000 apples
>> 100%
```
- `%f`를 통해 특정 소수점 자리까지만 나타낼 수 있다.
```python
print("%0.2f" % 1.23456) # 두 번째 자리까지만 표시
>> 1.23
```

#### #1.5.1 문자열의 format 함수
- 문자열의 `format` 함수를 사용하여 문자열 포맷을 지정할 수도 있다. 
```python
print("{0} apples".format(5))
print("{0} apples".format("five"))
print("{0} apples".format("5.0"))
>> 5 apples
>> five apples
>> 5.0 apples
```

- 2개 이상의 값을 넣을 경우, `{0}`, `{1}`과 같은 인덱스를 사용하면 된다. 이 순서에 맞게 값이 대입된다.
```python
print("{1} + {0}".format(10, 20)) # {0}은 10, {1}은 20
>> 20 + 10
```

- `{0}`, `{1}`과 같은 인덱스 대신 `{name}` 형태를 사용하는 방법도 있다. 단, `{name}` 형태를 사용하려면 format 함수에는 반드시 `name=value`와 같은 형태의 입력값이 있어야 한다. 
```python
print("{b} + {a}".format(a=10, b=20))
>> 20 + 10
```
### #1.6 f문자열 포매팅
파이썬 3.6부터 사용할 수 있는 기능이다.
```python
a, b = 10, 20

print(f"{a} + {b} = ")
>> 10 + 20 = 
```
다음과 같이 딕셔너리도 활용할 수 있다.
```python
num = {'a':10, 'b':20}

print(f"{num["a"]} + {num["b"]} = ")
>> 10 + 20 = 
```

### #1.7 문자열 주요 메서드
#### #1.7.1 대소문자 변환: `upper()`, `lower()`
- `upper()`: 문자열의 모든 문자를 대문자로 변환
- `lower()`: 문자열의 모든 문자를 소문자로 변환
```python
s1 = "AbCde"

print(s1.upper())
print(s1.lower())
>> ABCDE
>> abcde
```
#### #1.7.2 공백문자 제거: `strip()`, `rstrip()`, `lstrip()`
- `strip()`: 문자열 양쪽에 있는 공백문자 제거
  - 이때 제거되는 문자는 공백문자뿐만 아니라 줄바꿈 문자(\n), 탭 문자(\t)도 포함
- `rstrip()`: 문자열의 가장 왼쪽 앞에 공백 제거
- `lstrip()`: 문자열의 가장 오른쪽 뒤에 공백 제거
- 지정한 문자를 제거하는 것도 가능하다.
```python
text = "\n Hi! \t"

print(text.strip())
print(text.rstrip())
print(text.lstrip())
>> Hi!
>>  Hi!
>> Hi! 
```
```python
text2 = ".Hi!"

print(text2.strip("."))   
print(text2.strip("!"))  
>> Hi!
>> .Hi
```
#### #1.7.3 특정 문자열 대체: `replace()`
- 문자열 내에서 특정 부분 문자열을 "앞에서부터 확인"하여 다른 문자열로 변경하는 함수
- 첫 번째 인자는 "찾을 문자열", 두 번째 인자로 "대체할 문자열"이며, 세 번째 인자로 "변경 횟수"를 지정할 수 있다. 생략 시 대체 가능한 문자열을 전부 변경한다.
```python
text = "Hello Hi...! hello Hello Hello"

print(text.replace("Hello ", ""))
print(text.replace("Hello", "Hi").replace("Hi", "Bye")) # 최종적으로 발견한 "Hi"를 전부 "Bye"로 변경
print(text.replace("Hello", "Hi", 1)) # 횟수 1회 지정: 앞에서부터 딱 1개만 변경
print(text.replace("Hello", "Hi", 2)) # 횟수 2회 지정: 앞에서부터 2개만 변경
>> Hi...! hello Hello
>> Bye Bye...! hello Bye Bye
>> Hi Hi...! hello Hello Hello
>> Hi Hi...! hello Hi Hello
```

#### #1.7.4 문자열 분할: `split()`
- 문자열을 구분자 기준으로 분할하여 리스트로 반환
- 구분자를 지정하지 않으면 "공백"을 구분자로 사용
```python
print(text.split("hello"))
print(text.split())

>> ['Hello Hi...! ', ' Hello Hello']
>> ['Hello', 'Hi...!', 'hello', 'Hello', 'Hello']
```

cf) `input().split()`
- `input()`을 통해 들어온 사용자의 입력에 대해 `split()` 메서드가 호출되며, 구분자를 지정하지 않은 상태이므로 공백을 기준으로 분할한다.

#### #1.7.5 문자열 결합: `join()`
- `join()` 함수는 문자열, 리스트, 튜플을 입력으로 받아 하나의 문자열로 결합하는 함수이다. 이때 각 요소 사이에는 지정한 구분자가 삽입된다.
```python
print(" ".join("abcd")) # 문자열 # 공백을 구분자로 사용
print("+".join(["1", "2", "3"])) # 리스트
print("_".join(("a", "b"))) # 튜플

>> a b c d
>> 1+2+3
>> a_b
```

#### #1.7.6 문자열 찾기: `find()`, `index()`
- `find()` 함수는 문자열 내에서 특정 문자(혹은 문자열)을 찾아 그 시작 위치의 인덱스를 반환한다. 이때 찾는 문자열이 여러 번 등장한다면 가장 앞에 위치한 문자열의 시작 인덱스를 반환한다.
- 만약 찾는 문자(혹은 문자열)가 존재하지 않는다면 -1을 반환한다. 
- `index()` 함수는 문자열 내에서 특정 문자(혹은 문자열)의 첫 번째 등장 위치를 찾아 반환한다.
- `index()`는 `find()`와 달리 찾지 못하면 `ValueError`를 발생시킨다.
```python
text = "f b abc a c d"

print(text.find("a"))
print(text.find("abc"))
print(text.find("e"))

>> 4
>> 4
>> -1
```
```python
print(text.index("a"))
print(text.index("abc"))
print(text.index("bc"))
print(text.index("e"))

>> 4
>> 4
>> 5
>> ValueError: substring not found
```

#### #1.7.7 문자 개수 세기: `count()`
- `count()` 함수는 문자열 내에서 특정 문자(혹은 문자열)이 몇 번 등장하는지 개수를 세어 반환
```python
print(text.count("a"))

>> 2
```

#### #1.7.8 포함 관계 확인: `in`
- 다음과 같이 `in`으로 문자열 내에 특정 문자(혹은 문자열)가 포함되어 있는지 확인할 수 있으며, 포함 여부에 따라 True/False가 반환된다.
- `count()` 함수는 문자열 내에서 특정 문자(혹은 문자열)이 몇 번 등장하는지 개수를 세어 반환
```python
print("abc" in text)
print("a" in text)
print("A" in text)

>> True
>> True
>> False
```

### #1.8 아스키 코드
파이썬 언어에서 사용할 수 있는 모든 문자들은 전부 하나의 숫자와 대응되며, 이를 **아스키 코드**라고 부른다.

특정 문자의 아스키 코드 값은 `ord()` 함수를 통해 확인할 수 있다.

아스키 코드 값을 알고 있을 때, 대응되는 문자를 확인하고 싶으면 `chr()` 함수를 사용하면 된다.
```python
print(ord("A")); print(chr(65))
print(ord("A")+1); print(chr(ord("A")+1))

>> 65
>> A
>> 66
>> B
```

---
## #2 리스트 
리스트는 여러 개의 값을 순서대로 저장할 수 있는 자료형으로서, 리스트 안에 다양한 자료형을 함께 넣을 수 있다.

`[]`나 `list()`를 사용하여 정의할 수 있다.

리스트의 각 item은 순서대로 위치를 가지며, 이 위치를 **인덱스(index)** 라고 한다. 리스트의 인덱스는 0부터 시작한다.

리스트는 **변경 가능한** 자료형이다. 즉, 리스트 안에 들어있는 item들을 추가/수정/삭제할 수 있다.
```python
list0 = []
list1 = [1, 2, 3]
list2 = list("345")

print(list0); print(list1); print(list2)

>> []
>> [1, 2, 3]
>> ['3', '4', '5']
```

### #2.1 리스트 덧셈 (리스트 연결)
- `+` 연산자를 사용해 두 개 이상의 리스트를 결합하여 새로운 리스트를 만들 수 있다. 
- 여러 리스트를 연결할 때, 원본 리스트는 변경되지 않고 각 리스트의 요소를 순차적으로 병합한 새로운 리스트가 생성된다. 
- 여러 리스트를 하나로 모으거나 순서대로 연결할 때 유용하다.
```python
print(list0 + list1 + list2)
print(list2 + list0 + list1)

>> [1, 2, 3, '3', '4', '5']
>> ['3', '4', '5', 1, 2, 3]
```

### #2.2 리스트 곱셈 (리스트 반복)
- `*` 연산자를 사용하여 특정 리스트를 지정한 횟수만큼 반복한 새로운 리스트를 만들 수 있다.
```python
print(list0*3)
print(list1*2)

>> []
>> [1, 2, 3, 1, 2, 3]
```

### #2.3 리스트 인덱싱
- 리스트의 인덱스는 **0부터 시작**한다.
- 음수 인덱스를 사용하면 마지막 요소에 접근할 수 있다.
- 음수 인덱스는 **-1부터 시작**하여 맨 뒤의 요소부터 접근한다.
- 인덱스가 리스트의 범위를 초과하면 `IndexError`가 발생한다.
- 리스트는 **변경 가능한(mutable) 자료형**이다. 다음과 같이 특정 인덱스의 값을 새로운 값으로 변경할 수 있다.
```python
print(list1)
list1[1] = "a"
print(list1)

>> [1, 2, 3]
>> [1, 'a', 3]
```
### #2.4 리스트 슬라이싱
- 문자열처럼 리스트도 슬라이싱 기법을 적용할 수 있다. 

### #2.5 리스트 주요 메서드와 함수 
- 각 자료형은 자신만의 고유한 **메서드**들을 가지고 있다. 
- **메서드**는 그 자료형의 데이터를 처리하기 위해 만들어진 전용 함수이다.
- 메서드를 사용할 때는 자료형의 이름 뒤에 점(`.`)을 입력한 다음, 사용할 메서드를 이어 붙이면 된다.
    - 예: 리스트이름.append(요소)
- 참고로 일반 함수는 특정 자료형을 위한 것이 아니라, 다양한 자료형에서 사용할 수 있는 함수이다.
    - 예: print()
    - 
#### #2.5.1 `append(x)`: append(x) 메서드는 리스트의 맨 끝에 어떤 x를 추가하는 함수이다.
#### #2.5.2 `insert(a, b)`: insert(a, b) 메서드는 지정한 인덱스 `a`위치에 값 `b`를 삽입하는 함수이다.
```python
list1 = [0, 1, 2]
list1.insert(0, "-1")

print(list1)
>> ['-1', 0, 1, 2]
```

#### #2.5.3 `remove(x)`: remove(x) 메서드는 리스트의 첫 번째로 나오는 x를 삭제하는 함수이다.
```python
list1.remove(0)

print(list1)
>> ['-1', 1, 2]
```

cf) del 함수를 사용해서 리스트의 요소를 삭제할 수도 있다. del list[x]는 list의 x번째 요소를 삭제한다. 
```python
del list1[0]  # 0번 인덱스 요소 제거

print(list1)  
>> [1, 2]
```
#### #2.5.4 `pop()`: pop() 메서드는 
- (1) `pop()`처럼 인덱스를 지정하지 않으면 마지막 요소를 제거한다. 
- (2) `pop(x)`처럼 인덱스를 지정하면, 리스트에서 x번째 요소를 제거한다.
```python
list1.pop()

print(list1)
>> [1]
```

#### #2.5.5 `sort()`: sort() 메서드는 리스트를 오름차순으로 졍랄한다. 
- `reverse=True` 인자를 사용하여 내림차순으로 정렬할 수 있다.
```python
list1 = [1, 10, 0]

list1.sort()
print(list1)

list1.sort(reverse=True)
print(list1)
>> [0, 1, 10]
>> [10, 1, 0]
```

#### #2.5.6 `reverse()`: reverse() 메서드는 리스트를 역순으로 뒤집는 함수이다. 
```python
list1.reverse()

print(list1)
>> [0, 1, 10]
```

#### #2.5.7 `count(x)`: count(x) 메서드는 리스트 안에 x가 몇 개 있는지 그 개수를 반환하는 함수이다.
```python
print(list1.count(10))
>> 1
```

#### #2.5.8 `index(x)`: index(x) 메서드는 리스트 내에서 특정 요소 x가 처음으로 등장하는 위치의 인덱스를 반환한다. 
- x가 존재하지 않으면 `ValueError`가 발생한다.
```python
print(list1.index(10))
print(list1.index(5))
>> 2
>> ValueError: 5 is not in list
```

#### #2.5.9 `extend(x)`: extend(x)에서 **x는 리스트만 올 수 있으며**, 원래의 리스트에 x 리스트를 더하게 된다.
```python
list1.extend(["a", "b", "c"])

print(list1)

>> [0, 1, 10, 'a', 'b', 'c']
```

#### #2.5.10 `len() 함수`: len() 함수를 통해 리스트에 포함된 요소의 개수(즉, 리스트의 길이)를 반환할 수 있다. 

### #2.6 리스트 입력받기
- 사용자로부터 여러 개의 요소를 한 번에 입력받아 리스트로 저장해야 하는 경우, 이를 처리하기 위해 다음과 같이 `input()` 함수와 `map()` 함수를 조합하여 사용하면 된다: `리스트이름 = list(map(변환함수, input().split()))`
- `input().split()`: 사용자 입력에 대해 공백을 기준으로 분리하여 리스트를 만든다. 
- `map(변환함수, ...)`: 공백 기준으로 분리된 문자열들을 지정한 함수(예: int, float, ...)를 사용하여 변환한다.
- `list(...)`: `map` 객체를 리스트로 변환한다.
```python
numbers = list(map(int, input().split()))
```

### #2.7 리스트 컴프리헨션
- 리스트 컴프리헨션은 반복문(for)과 표현식을 사용해서 반복 가능한 객체(iterable)로부터 새로운 리스트를 만드는 방법이다.
- 기본적인 구조는 다음과 같다.
```python
기본형태: 리스트이름 = [표현식 for 항목 in 반복가능객체]
조건문 포함 형태: 리스트이름 = [표현식 for 항목 in 반복가능객체 if 조건문]
```
- 반복 가능한 객체: 리스트, 튜플, 문자열, 세트, 딕셔너리 등이 대표적이며, 반복 가능함은 객체 내부의 요소가 몇 개이든 상관없이, 내부 요소를 한 번에 하나씩 순차적으로 꺼낼 수 있는 구조를 갖추었음을 의미한다.
    - 반복 가능한 객체는 시퀀스 객체를 포함한다: 리스트, 튜플, 문자열은 반복 가능하면 객체이면서 시퀀스 객체이다. 그러나 딕셔너리와 세트는 반복 가능한 객체이지만 시퀀스 객체는 아니다. 
    - 시퀀스 객체는 요소의 순서가 정해져 있고 연속적으로 이어져 있어야 하는데, 딕셔너리와 세트는 요소의 순서가 정해져 있지 않기 때문이다. 
- 항목: 반복 가능한 객체를 순회하며, 현재 차례의 요소 값을 매번 하나씩 전달받아 저장하는 임시 변수이다. 
- 표현식: 각 반복마다 꺼내온 항목을 바탕으로 연산이나 함수를 적용하여, 새로 생성될 리스트의 최종 요소를 결정한다.
```python
arr = [1, 2, 3]
arr_times_2 = [x*2 for x in arr]

print(arr_times_2)
>> [2, 4, 6]
```


### #2.8 2차원 배열
2차원 배열은 복잡한 데이터를 관리하고 처리하는 데 유용한 구조이다. **배열 안에 배열**이 있는 형태로 "행(row)"과 "열(column)"로 데이터를 정리할 수 있다. 행과 열로 구성되기 때문에, 각 요소 $a_{ij}$는 `a[i]][j]`로 접근할 수 있다.

```python
matrix = [
    [1, 2, 3],
    [4, 5, 6]
]

print(matrix[1][2]) # 2행 3열의 요소
```

#### #2.8.1 2차원 배열 생성
- 행과 열의 개수를 입력받아 2차원 배열을 생성할 수 있다. 아래 예시는 `for`문을 사용해 2차원 배열을 입력받는 방식이다.
```python
rows, cols = int(input()), int(input())

matrix = []
for _ in range(rows):
    # 행을 한 줄씩 입력받아서 matrix에 추가
    row_data = list(map(int, input().split())) 
    matrix.append(row_data)
```
- **리스트 컴프리헨션**을 이용하여 더 간단하게 2차원 배열을 생성할 수 있다.
```python
rows, cols = int(input()), int(input())

matrix = [list(map(int, input().split())) for _ in range(rows)]
```
- map(int, input().split())로 문자열 리스트의 모든 요소에 정수 변환을 적용하고 이것을 list()로 감싸 하나의 행으로 만든다.
- 그다음, 완성된 행을 [] 안에 넣는다.
- 이 과정을 rows에 지정된 횟수만큼 반복하며 계속 리스트를 쌓아나간다.

#### #2.8.2 2차원 배열 탐색
- 2차원 배열을 탐색하는 단순한 방법은 이중 for문을 사용하는 것이다.
- 바깥족 반복문은 각 행을 처리하고, 안쪽 반복문은 행 내부의 각 값을 처리한다. 
```python
matrix = [[1, 2], [3, 4]]
rows, cols = 2, 2

for i in range(rows):
    for j in range(cols):
        print(matrix[i][j], end= " ")
    print()
# output
# 1 2 
# 3 4 
```

#### #2.8.3 2차원 배열에서 특정 위치의 값 접근/변경
- 2차원 배열에서 특정 위치의 값을 변경하거나 접근할 때는 행과 열의 인덱스를 알고 있어야 한다.
```python
print(new_matrix[0]); print(new_matrix[0][1])
>> [1, 4]
>> 4
```
---
## #3 튜플
튜플은 item을 바꿀 수 없다는 점만 제외하면 리스트와 완전히 동일하다.

리스트는 리스트 내의 item을 변경하거나 추가, 삭제가 가능하지만 튜플은 불가능하다. 
- 튜플은 값을 직접 추가할 수 없지만, `더하기 연산자(+)`, `리스트로 변환 후 추가`라는 방식을 통해 우회적으로 값을 추가할 수 있다.
```python
t1 = ("a", 1)

print(t1[0])
>> a
```

```python
t1 + (1, 2)

print(t1)
>> ('a', 1, 1, 2)

lst = list(t1)
lst.append(1)
t1 = tuple(lst)
```

그래서 리스트는 데이터가 자주 바뀌는 상황에서, 튜플은 데이터가 고정되어 있어야 하는 상황에서 유용하다.  

```python
t1 = ("a", 1)

print(t1[0])
>> a
```
```python
# 삭제
del t1[0]

>> TypeError: 'tuple' object doesn't support item deletion
```

```python
# 값 변경
t1[0] = "c"

>> TypeError: 'tuple' object does not support item assignment
```

### #3.1 튜플 인덱싱
```python
t1 = (1, 2, "a", "b")

print(t1[0])
>> 1
```

### #3.2 튜플 슬라이싱하기 
```python
print(t1[1:])
>> (2, 'a', 'b')
```

### #3.2 튜플 길이 구하기
```python
print(len(t1))
>> 4
```
---


## #4 딕셔너리
딕셔너리는 `key:value` 형태로 데이터를 저장하는 자료형이다.

그래서 딕셔너리는 리스트나 튜플처럼 순차적으로 값들을 구할 수 없고, key를 통해 value를 얻는다. 
- 리스트가 위치(index)로 값을 찾는다면, 딕셔너리는 `key`로 key에 대응되는 값을 찾는다. 

key는 고유해야 한다. 즉, 딕셔너리는 중복되는 key를 가질 수 없다. 그러나 value는 중복되어도 문제가 없다. 서로 다른 key가 같은 value를 가져도 다.

예를 들어 영어-한글 사전에서 "apple"이라는 단어의 뜻을 찾기 위해선, 사전의 첫 번째 단어부터 단어들을 순차적으로 검색하지 않는다. "apple"이라는 단어가 있는 곳만 펼쳐 본다.  

빈 딕셔너리는 {}로 만들 수 있고, 여기에 "key:value" 쌍을 넣으면 된다.

### #4.1 딕셔너리 key:value 추가하기
```python
d1 = {1:"a"} # 딕셔너리 d1 정의
d1["b"] = 2 # 딕셔너리 d1에 key가 "b"이고, value가 2인 key:value 쌍 추가

print(d1)
>> {1: 'a', 'b': 2}
```

- 주의할 점은, key에는 리스트를 사용할 수 없다. 그러나 튜플은 key로 사용할 수 있다.
- 리스트는 mutable이고 튜플은 immutable이다. 즉, 파이썬 **딕셔너리의 key는 반드시 immutable 자료형**이어야 한다.
- value는 변하는 값이든, 변하지 않는 값이든 아무 값이나 사용할 수 있다. 

### #4.2 딕셔너리 값 조회
- `d[key]`로 key에 대응되는 value를 가져올 수 있다.
    - 딕셔너리 안에 `key`가 없다면 KeyError가 발생한다.
- 다른 방법은 `get(key, default)` 메서드를 사용하는 것이다.
- 그리고 `get(key, default)`은 key가 없을 때 반환할 기본값(default)을 설정할 수 있다. default를 생략하면 `None`을 반환한다.
```python
person = {
    "name": "Alice",
    "age": 30
}

print(person["name"])
print(person.get("name"))
print(person.get("address", "주소 없음"))
print(person.get("address2"))
>> Alice
>> Alice
>> 주소 없음
>> None
```
- key와 value가 딕셔너리 안에 있는지 `in`을 통해 확인할 수 있다.
```python
print("name" in person)
>> True
```
```python
print("Alice" in person.values())
>> True
```

### #4.3 딕셔너리 값 변경·삭제
- del 함수를 사용해, `del d[key]`를 입력하면 지정한 key에 해당하는 key:value 쌍이 삭제된다. 
```python
del person["age"]
```
- `pop()`을 통한 삭제도 가능하다. 
```python
age = person.pop("age")
```
- `popitem()`은 마지막에 들어온 항목을 제거하고 반환한다. 
```python
person.popitem()

>> ('age', 30)
```
- `clear()`는 모든 항목을 제거한다.
```python
person.clear()

print(person)
>> {}
```

### #4.3 딕셔너리 `keys()`, `values()`, `items()`
`keys()`, `values()`, `items()`는 각각 key, value, 그리고 key-value 쌍을 담은 객체를 반환하는 메서드이다.
```python
person = {
    "name": "Alice",
    "age": 30
}

print(person.keys())
print(person.values())
print(person.items())
>> dict_keys(['name', 'age'])
>> dict_values(['Alice', 30])
>> dict_items([('name', 'Alice'), ('age', 30)])
```

### #4.4 딕셔너리 메서드
#### #4.4.1 `update()`: 기존 딕셔너리에 새로운 key-value 쌍을 추가하거나 기존 value를 수정할 수 있다.
```python
person.update({
    "age": 31,
    "job": "developer"
})
print(person)

person.update({'name':"Bob"}) 
print(person)

>> {'name': 'Alice', 'age': 31, 'job': 'developer'}
>> {'name': 'Bob', 'age': 31, 'job': 'developer'}
```
#### #4.4.2 `setdefault(key, default_value)`: 딕셔너리에 특정 key가 있는지 확인하고, key가 있으면 그 key에 대응되는 value를 가져온다. key가 없으면 지정한 기본값(default_value)을 추가한 뒤 해당 값을 반환한다. 기본값을 지정하지 않으면 None을 추가한다. 
```python
d = {}
d.setdefault(
    "name",
    "Alice"
)

print(d)
>> {'name': 'Alice'}
```
```python
d.setdefault(
    "name",
    "Bob"
)
print(d) # 이미 name이라는 key가 있으므로, name에 대응되는 value를 반환
>> 'Alice'
```
#### #4.4.2 `fromkeys()`: 여러 key를 한 번에 만들 수 있다.
```python
keys = [
    "name",
    "age",
    "job"
]
d = dict.fromkeys(keys)

print(d)
>> {'name': None, 'age': None, 'job': None}
```

#### #4.4.3 `copy()`: 얕은 복
```python
d_copy = d.copy()
print(d_copy)
>> {'name': None, 'age': None, 'job': None}
```

### #4.5 딕셔너리 컴프리헨션
리스트 컴프리헨션과 비슷하게 dict도 한 번에 만들 수 있다.
```python
{
    key표현식: value표현식
    for 변수 in iterable
}
```
```python
squares = {x: x ** 2 for x in range(5)}

print(squares)
>> {0: 0, 1: 1, 2: 4, 3: 9, 4: 16}
```

### #4.6 `zip()`으로 딕셔너리 만들기
```python
names = [ "Alice", "Bob", "Charlie"]
scores = [90, 80, 95]

student_scores = dict(
    zip(names, scores)
)

print(student_scores)
>> {'Alice': 90, 'Bob': 80, 'Charlie': 95}
```

---
## #5 집합
집합 자료형은 `set()`을 사용해 만들 수 있다.
```python
s1 = set([1, 2, 3])
s2 = {4, 5, 6}

print(s1)
print(s2)
>> {1, 2, 3}
>> {4, 5, 6}
```
- 참고로 `s = {}`는 빈 집합이 아니다. 이것은 빈 딕셔너리를 생성한 것이다. 
- 빈 집합은 `set()`이 필요하다. 

set의 핵심 특징 2가지는 **중복을 허용하지 않고(즉, 중복되는 원소가 없고), 순서가 없다(원소의 순서/위치를 기록하지 않는다. 인덱싱과 슬라이싱이 불가능하다)**는 것이다.

리스트나 튜플은 순서가 있기 때문에 인덱싱을 통해 원소에 접근할 수 있었지만, set은 순서가 없기 때문에 인덱싱을 통해 원소를 얻을 수 없다.


만약 set 자료형에 저장된 값을 인덱싱으로 접근하려면, set을 리스트/튜플로 변환해서 인덱싱을 사용하면 된다.

변경 불가능한(immutable) 자료형만 넣을 수 있다. 리스트, 딕셔너리, 집합 같은 변경 가능한(mutable) 자료형은 넣을 수 없다. 그 이유는 집합이 중복을 없애고 값을 빠르게 찾기 위해 해시(hash) 값을 사용하기 때문이다. 
- 딕셔너리의 key도 마찬가지이다.

set도 iterable이므로 for문을 통해 반복할 수 있다. 다만, set은 순서가 없기 때문에 출력 순서가 보장되지 않는다.
```python
for skill in person_skills:
    print(skill, end=" ")
```

### #5.1 `in`
집합에 특정 원소가 있는지는 `in`을 통해 확인할 수 있다.
```python
person_skills = {"Python", "Java", "SQL"}

"Python" in person_skills
>> True
```

### #5.2 집합 관련 함수
#### #5.2.1 `add()`: 1개의 원소만 추가할 때 사용한다.
```python
person_skills = {"Python", "Java"}

person_skills.add("SQL")
```

#### #5.2.2 `update()`: 여러 개의 원소를 추가할 때 사용한다.
```python
person_skills = {"Python"}

person_skills.update(
    ["Java", "SQL", "C++"]
)
```
update()는 임의의 iterable을 받으므로 `update("abc")`같은 경우, 위의 `add()`와 다르게 문자 `a`, `b`, `c` 세 개가 들어간다. 

#### #5.2.3 `remove()`: 특정 원소를 제거할 때 사용한다.
```python
person_skills.remove("Java") # "Java" 제거
person_skills.remove("Rust") # KeyError 발생
```

#### #5.2.4 `discard()`: discard()도 원소를 제거한다. remove()와의 차이는 **원소가 없을 때**이다.
```python
person_skills.remove("Java") # "Java" 제거
person_skills.discard("Rust") # KeyError 발생하지 않음
```
discard()는 remove()와 달리, set 안에 제거할 원소가 없어도 에러를 발생시키지 않는다.  

참고로 `pop()`도 가능하다. 단, set의 특성으로 인해 pop()을 사용하면 **임의의 원소 하나를 제거**하고 반환한다.

#### #5.2.5 `clear()`: 모든 원소를 제거한다.
#### #5.2.6 `copy()`: 집합의 shallow copy를 반환한다.

#### #5.2.7 집합 연산 - 합집합, 교집합, 차집합, 대칭 차집합
- 합집합: `s1 | s2`, `s1.union(s2)`
- 교집합: `s1 & s2`, `s1.intersection(s2)`
- 차집합: `s1 - s2`, `s1.difference(s2)`
- 대칭 차집합: `s1 ^ s2`, `s1.symmetric_difference(s2)`
    - 대칭 차집합은 **두 집합 중 한쪽에만 존재하는 원소**를 구하는 연산

#### #5.2.8 집합 연산 - 부분집합, 상위집합
- 부분집합: `s1 <= s2`, `s1.issubset(s2)`는 s1의 모든 원소가 s2 안에 있는지를 검사한다. 
- 진부분집합: `s1 < s2`는 s1의 모든 원소가 s2에 있지만, s1 != s2인 경우
- 상위집합: `s1 >= s2`, `s1.issuperset(s2)`는 반대로 s2의 모든 원소를 s1이 포함하는지를 검사한다.
- 진상위집합: `s1 > s2`는 s2의 모든 원소를 s1이 포함하면서 s1 != s2인 경우

---
