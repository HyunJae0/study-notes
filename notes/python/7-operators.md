# 파이썬 - 이터레이터, 제너레이터, 타입 어노테이션

> [📚 전체 목차로 돌아가기](../../README.md)

## Table of Contents

- [연산자의 기본 개념과 산술 연산자](#1-연산자의-기본-개념과-산술-연산자)
- [대입 연산과 복합 대입](#2-대입-연산과-복합-대입)
- [비교 연산자와 값 동등성](#3-비교-연산자와-값-동등성)
- [논리 연산자](#4-논리-연산자)
- [객체 동일성 연산자](#5-객체-동일성-연산자)
- [멤버십 연산자](#6-멤버십-연산자)
---

## #1 연산자의 기본 개념과 산술 연산자

### #1.1 연산자, 피연산자, 표현식
Python에서는 **객체(objects)와 연산자(operators)를 결합해 표현식(expression)을 만든다.**

예를 들어 `3 + 2`에서 `3`과 `2가` 피연산자(operand)이고, `+`가 연산자(operator)이며, `3 + 2` 전체가 표현식(expression)이다. 

Python은 `3 + 2`라는 expression을 평가하여 하나의 값 `5`를 만든다. 그리고 그 결과의 type도 `int`이다.

### #1.2 기본 산술 연산자

| 연산자       | 의미             | 예               |
| --------- | -------------- | --------------- |
| `+`       | 덧셈             | `5 + 2` → `7`   |
| `-`       | 뺄셈             | `5 - 2` → `3`   |
| `*`       | 곱셈             | `5 * 2` → `10`  |
| `/`       | 나눗셈            | `5 / 2` → `2.5` |
| `//`      | floor division | `5 // 2` → `2`  |
| `%`       | 나머지            | `5 % 2` → `1`   |
| `**`      | 거듭제곱           | `5 ** 2` → `25` |
| unary `-` | 부호 반전          | `-5`            |

`int`와 `float` 모두 기본적으로 `+`, `-`, `*`, `/` 연산을 지원한다.

`int`끼리 `+`, `-`, `*`를 하면 일반적으로 결과 역시 `int`이다. 

반면 `2 + 3.0` 이렇게 float가 포함되면 결과는 `5.0`으로 `float`이 된다. 

`/`의 경우 **나눗셈 결과는 항상 float**이다. 두 `int`가 정확히 나누어떨어지는 경우에도 마찬가지이다.
```python
7 / 2
>> 3.5

8 / 2
>> 4.0
```

정수 결과가 필요한 경우 Python에는 `//`가 있다. 나눗셈의 몫을 아래쪽 정수 방향으로 내린다.
```python
7 // 2
>> 3

-6 // 4
>> -2
```
`-6 / 4`의 정확한 나눗셈은 `-1.5`이고, 아래쪽 정수는 `-2`이다. 그러므로 `//` 연산은 몫을 아래쪽 정수로 내린다.

`%`는 나눗셈의 나머지를 구하는 연산이다.
```python
17 % 5
>> 2
```

`%` 연산은 특히 배수 판별에 많이 쓰인다. 예를 들어 `x % 5`의 결과가 `0`이라는 것은 `x`가 `5`로 정확하게 나누어진다는 뜻이다. 

`**`는 거듭제곱 연산자이다. 

`-`는 상황에 따라 두 가지 역할을 할 수 있다: 뺄셈, 부호 반전
```python
x = 10
y = 5

print(-x)
print(x-y)
>> -10
>> 5
```

이 연산자들의 **우선순위**는 다음과 같다.
```
( )
**
-x
*  /  //  %
+  -
```

괄호 `( )`가 있으면, 괄호 안의 연산을 먼저 한다. 그다음 순위는 `**`이며, 그다음은 `-x`이다. 그리고 곱셈·나눗셈 계열, 덧셈·뺄셈 계열 순서이다. 

**같은 우선순위**는 보통 왼쪽부터 표현식을 평가한다. 예를 들어 `60 / 2 * 3`에서 `/`와 `*`는 우선순위가 동일하다. 따라서 왼쪽부터 `60 / 2`, `30 * 3`으로 연산을 수행한다.


---

## #2 대입 연산과 복합 대입

### #2.1 대입 연산
Python에서 `=`는 오른쪽의 값을 왼쪽의 변수에 저장하는 할당 연산자이다. 정확히 말하면, 이름을 해당 객체에 바인딩한다. 예를 들어 `x = 10`은 이름 `x`가 객체 `10`을 가리키도록 한다.

```python
x = 10
x = x + 5
```
이 예시의 경우 현재 `x`의 값을 가져와서 `10 + 5`를 계산하고, `x`가 계산 결과인 `15`를 가리키도록 다시 대입하는 과정이다. 

`=`와 `==`는 완전히 다르다. `x = 10`은 `x`에 `10`을 대입한다는 의미이며, binding 수행된다. `x == 10`은 `x`의 값이 `10`과 같은지를 의미하며, 결과는 `True`/`False`가 된다. 

Python에서는 이미 존재하는 변수에 새로운 값을 다시 대입할 수 있다. 예를 들어 `x = 10`을 정의한 다음, `x = 20`을 수행하면, `x = 20`이 된다. 
```python
x = 10
x = 20

print(x)
>> 20
```
이는 변수 `x`가 가리키는 객체가 `10`이었다가 `x = 20` 후 `x`라는 이름이 객체 `20`을 가리키도록 한 것이다. 즉, 변수 `x`가 **가리키는 객체가 달라진 것**이다. 

이번에는 `x = 10`을 정의한 다음, `x = y`를 수행하면, `x`와 `y`는 동일한 객체를 가리키게 된다.
```python
x = 10
y = x

print(id(x) == id(y))
>> True
```
`y = x`에서 오른쪽의 `x`를 평가하면 현재 `x`가 가리키는 값 `10`이 나온다. 이렇게 `y = x`와 같은 할당은 그 순간 `x`가 가리키는 값을 `y`도 가리키도록 만든다. 다만, 두 변수 사이에 지속적인 연결을 만드는 것은 아니다. `y = x` 이후 `y = 20`을 할당한다면, `y`는 객체 `20`을 가리키게 된다.

### #2.2 복합 대입 연산자

예를 들어 다음과 같은 연산은
```python
x = 10
x = x + 5
```

다음과 같이 복합 대입(augmented assignment) 연산자 `+=`로 작성할 수 있다.

```python
x = 10
x += 5
```

산술 연산에서 주로 사용하는 것은 다음과 같다.
| 복합 대입 | 기본적인 의미             | 예         |
| ----- | ------------------- | --------- |
| `+=`  | 더한 뒤 대입             | `x += 2`  |
| `-=`  | 뺀 뒤 대입              | `x -= 2`  |
| `*=`  | 곱한 뒤 대입             | `x *= 2`  |
| `/=`  | 나눈 뒤 대입             | `x /= 2`  |
| `//=` | floor division 후 대입 | `x //= 2` |
| `%=`  | 나머지를 구한 뒤 대입        | `x %= 2`  |
| `**=` | 거듭제곱 후 대입           | `x **= 2` |

복합 대입 연산은 immutable 객체와 mutable 객체에서 차이가 있다.

예를 들어 `int`와 같은 immutable 객체에서는 `x += 3`과 `x = x + 3`은 같다고 생각해도 되지만, 

`list`와 같은 mutable 객체에서 `a += [3]`은 기존 list를 in-place로 변경하고, `a = a + [3]`은 `a`를 새로운 list에 rebinding한다.
```python
a = [1, 2]
b = a
print(id(a) == id(b))

a = a + [3]

print(a)
print(id(a) == id(b))
>> True
>> [1, 2, 3]
>> False
```
처음에는 `a`와 `b`가 같은 리스트 객체를 가리킨다.
```
a ──┐
    ├──→ [1, 2]
b ──┘
```
그런데 `a = a + [3]`은 `a + [3]`으로 새로운 리스트 `[1, 2, 3]`을 생성하고, 왼쪽에 있는 이름 `a`가 그 새 객체를 가리키도록 재대입한다. 
```
a ─────→ [1, 2, 3]    ← 새 객체

b ─────→ [1, 2]       ← 기존 객체
```

반면, `a += [3]`은 list의 in-place 덧셈 동작을 사용해서 현재 리스트 객체를 변경하는 것에 불과하다.
```python
a = [1, 2]
b = a

a += [3]

print(a)
print(id(a) == id(b))
print(b)
>> [1, 2, 3]
>> True
>> [1, 2, 3]
```
그러므로 `a`와 `b`가 여전히 같은 객체를 가리킨다. 
```
a ──┐
    ├──→ [1, 2, 3]
b ──┘
```

---

## #3 비교 연산자와 값 동등성

비교 연산자(comparison operator)는 값을 비교하여 **Boolean 값 True 또는 False를 반환**한다. Python의 기본 비교 연산자는 다음 6개이다.
| 연산자  | 의미     | 예                 |
| ---- | ------ | ----------------- |
| `==` | 같다     | `5 == 5` → `True` |
| `!=` | 같지 않다  | `5 != 3` → `True` |
| `<`  | 작다     | `3 < 5` → `True`  |
| `<=` | 작거나 같다 | `5 <= 5` → `True` |
| `>`  | 크다     | `5 > 3` → `True`  |
| `>=` | 크거나 같다 | `5 >= 5` → `True` |

`==`와 같은 비교 연산자에서 중요한 점은 **같은 객체인지 확인하는 것이 아니라 값의 동등성을 비교한다**는 것이다.
- 즉, 객체가 아니라 객체의 값이 같은지 비교한다. 

이 6가지 비교 연산자들은 **모두 같은 우선순위이다.**
> `+`, `-`, `*`, `/` 등의 산술 연산자는 비교 연산자보다 우선순위가 높다. 예를 들어 `1 + 1 == 2`는 `+`연산을 먼저 수행한 뒤, `2 == 2`라는 비교 연산을 수행하게 된다. 


---

## #4 논리 연산자
논리 연산자는 여러 Boolean 조건을 결합하거나 뒤집을 때 사용한다. Python의 기본 논리 연산자는 다음 세 개이다.

| 연산자   | 의미            |
| ----- | ------------- |
| `and` | 두 조건이 모두 참인가? |
| `or`  | 하나 이상 참인가?    |
| `not` | 참/거짓을 반전      |

`and`는 양쪽 조건이 모두 참일 때만 참이다.
```python
True and True
# True

True and False
# False

False and True
# False

False and False
# False
```
`or`는 둘 중 하나 이상이 참이면 참이다.
```python
True or True
# True

True or False
# True

False or True
# True

False or False
# False
```
`not`은 Boolean 값을 반대로 뒤집는다.
```python
not True
# False

not False
# True
```

논리 연산자의 **우선순위는 `not`, `and`, `or` 순**이다. `not`이 우선순위가 가장 높고, `or`이 가장 낮다.
> 산술 연산자, 비교 연산자, 논리 연산자의 우선순위는 산술 연산자, 비교 연산자, 논리 연산자 순이다. 산술 연산자의 우선순위가 가장 높고, 논리 연산자의 우선순위가 가장 낮다. 

그리고 Python에서는 Boolean만 조건으로 쓸 수 있는 것이 아니다. 다음과 같은 값들은 empty 값들로 `False`로 취급한다. 
```
0
0.0
None
""
[]
{}
```
```python
bool(0)
# False

bool("")
# False

bool([])
# False
```

반대로 일반적인 non-empty/non-zero 값은 `True`로 취급된다.
```python
bool(10)
# True

bool("hello")
# True

bool([1, 2])
# True
```

그래서 `if len(lst) > 0` 대신 `if lst`도 가능하다. list가 비어 있지 않으면 true이기 때문이다.

`not`은 De Morgan 법칙을 다룰 때 매우 유용하다. 
```
not (A and B) ↔ (not A) or (not B)

not (A or B) ↔ (not A) and (not B)
```
예를 들어 `not (age >= 18 and has_id)`는 논리적으로 `age < 18 or not has_id`와 같다.
- `not (age >= 18 and has_id)`는 `not (age >= 18) or not has_id`이며, `not (age >= 18)`은 곧 `age < 18`이므로, 최종적으로 `age < 18 or not has_id`가 된다.

또 `not (x < 0 or x > 100)`은 `x >= 0 and x <= 100`와 같다. 

### #4.1 단락 평가(Short-circuit evaluation)

Python의 `and`, `or`는 항상 양쪽을 전부 평가하지 않는다. **필요한 결과를 이미 알았다면 뒤쪽 expression을 평가하지 않는다.**

예를 들어 `False and something()`은 첫 번째 값이 이미 `False`이다. `and`는 둘 다 참이어야 전체가 참인데, 이미 첫 번째 값이 `False`이므로 무조건 전체 결과는 `False`이다. 그러므로 `something()`을 실행할 필요가 없다.

예를 들어 `if i < len(s) and s[i].isalpha():`에서 `True/False` 평가 순서는 `i < len(s)`가 먼저이다. 그러므로 `i < len(s)`가 `False`가 되는 순간 전체 결과는 `False`이므로, Python은 뒤의 `s[i].isalpha()`를 평가하지 않는다.

`or`의 short-circuit은 `and`의 short-circuit과 반대이다. 

예를 들어 `True or something()`의 경우, 첫 번째 값이 이미 `True`이기 때문에 `or`에서는 이미 전체 결과가 무조건 `True`이다. 그러므로 `or` 뒤의 `something()`은 평가할 필요가 없다.

### #4.2 `and`, `or`는 항상 `True`나 `False`를 반환하는 것은 아니다

`and`와 `or`는 **operand 자체를 반환할 수 있다.**

예를 들어 `"hello" and "world"`의 결과는 `"world"`이며, `"hello" or "world"`의 결과는 `"hello"`이다. 

`A and B`에서는 기본적으로, `A`가 false이면 `A`를 반환하고, `A`가 true이면 `B`를 평가하고 `B`를 반환한다. 예를 들어 `0 and 100`는 `0`을 반환하게 된다.

반대로 `A or B`는 `A`가 true이면 `A`를 반환한다. 그리고 `A`가 false이면 `B`를 평가하고 `B`를 반환한다. 예를 들어 `0 or []`는 `[]`를 반환하게 된다. 

> `not`은 다르다. `not`은 결과를 Boolean으로 반환한다. 예를 들어 `not 0`은 `True`, `not "hello"`는 `False`를 반환한다.


---


## #5 객체 동일성 연산자
`is`는 **두 변수가 정확히 같은 객체를 가리키는가?**를 검사한다.
> `==`가 두 객체의 값이 동일한지 검사했다면, `is`는 **두 변수가 같은 메모리상의 객체를 가리키는지** 확인한다. `a == b`는 동등성(equality)을 비교하는 반면, `a is b`는 동일성(identity)을 비교한다. 

예를 들어 `a = [1, 2, 3]`이고, `b = a`이면 `a is b`는 `True`이다.
```
a ───┐
     ├────→ [1, 2, 3]
b ───┘
```
두 이름 `a`와 `b`가 하나의 동일한 list 객체를 가리키므로 `a is b`는 `True`이다. 이렇게 `is`는 두 변수가 alias 관계인지 확인하는 데 사용될 수 있다. 
> 두 이름이 같은 mutable object를 가리키는 현상을 aliasing 또는 alias라고 한다. 

`is not`은 `is`의 반대이다. **두 변수가 서로 다른 객체를 가리키는가?**를 검사한다.

예를 들어
```python
a = [1, 2]
b = [1, 2]

print(a is b)
print(a is not b)
>> False
>> True
```
```
a ─────→ [1, 2]
          object A


b ─────→ [1, 2]
          object B
```
두 list는 **서로 독립적으로 생성**된 상황으로 `a`와 `b`라는 이름이 서로 다른 객체를 가리킨다. 
> 이 예시의 경우, `a`와 `b`의 값이 동일하기 때문에 `a == b`는 `True`이다.

Python에는 객체의 identity를 확인하기 위한 `id()` 함수가 있다.
> Python에서 identity란 메모리 상에서 객체가 차지하고 있는 고유한 위치(주소)를 뜻한다.
```python
a = [1, 2, 3]
b = a

print(id(a) == id(b))
>> True
```

cf) `is None`과 `is not None`은 Python에서 변수가 `None` 객체인지 아닌지 확인하는 표현이다. `None`은 특정한 하나의 객체를 나타내므로 값의 동등성 `==`보다 객체 동일성 `is`로 검사하는 것이 Python의 표준적인 방식이다. 

---


## #6 멤버십 연산자
멤버십 연산자는 어떤 값이 **특정 컨테이너 안에 포함되어 있는지 검사**한다. Python에는 두 가지가 있다: `in`, `not in`
> 컨테이너(container)는 여러 개의 데이터를 하나의 자료형에 담아 관리할 수 있는 자료형을 말한다. 

`in`과 `not in`의 기본 형태는 다음과 같으며, 결과는 `True/False`이다.
```python
x in container
x not in container
```

`x in container`은 container 안에 x가 존재하는가?이며, `not in`은 정확히 그 반대로 container 안에 x가 존재하지 않는가?를 검사한다.
```python
numbers = [10, 20, 30]

20 in numbers
# True

50 in numbers
# False
```

list의 `in`은 substring/subsequence를 찾는 것이 아니라 **element 단위 membership을 검사**한다. 예를 들어 `[1, 2] in [[1, 2], [3, 4]]`에서는 `[1, 2]` 자체가 하나의 element이므로 `True`이다. 반면, `[1, 2] in [1, 2, 3, 4]`는 `False`가 된다. 

`x in lst`는 개념적으로 생각하면 list 안을 살펴보면서, `x == lst[0] ?`, `x == lst[1] ?`, `x == lst[2] ?`와 같은 equality 관계를 찾는 것이다. 


string에서 `in`은 조금 다르다. **문자열에서는** `in`이 **substring이 존재하는지** 검사한다. 
```python
"c" in "abcd"
# True

"bd" in "abcd" # "bd"라는 연속된 substring이 존재해야 True이다. 
# False
```

또한, 문자열에서는 **대소문자를 구분**한다.
```python
"Dog" in "CatDogBird"
# True

"dog" in "CatDogBird"
# False
```

즉, string의 `in`에서는 **문자가 정확하게 일치해야 하며 uppercase/lowercase가 서로 다르다.**

tuple에서도 `in`을 사용할 수 있다.
```python
t = ("a", "b", "c")

"b" in t
# True

"x" in t
# False
```

dictionary에서 `in`은 **key를 검사**한다.
```python
d = {
    "name": "Alice",
    "age": 20
}

"name" in d # dict d의 key에 "name"이 있는지
# True

"Alice" in d # dict d의 key에 "Alice"가 있는지
# False
```

dictionary에서 value가 존재하는지 검사하려면 다음과 같이 `d.values()`를 사용하면 된다.
```python
"Alice" in d.values()
# True
```

dictionary의 key-value pair도 다음과 같이 `d.items()`를 사용하면 검사할 수 있다. `dict.items()`가 `(key, value)` tuple들을 제공하기 때문이다. 
```python
("name", "Alice") in d.items()
# True
```

여기서의 `in`은 `for x in iterable`의 `in`과 다르다. `x in numbers`과 `for x in numbers:`는 역할이 다르다. 

`x in numbers`는 membership test이므로 하나의 expression이며 결과가 `True/False`이다.

반면, `for x in numbers:`의 `in`은 for statement의 반복 대상을 지정하는 문법의 일부이다.

---




