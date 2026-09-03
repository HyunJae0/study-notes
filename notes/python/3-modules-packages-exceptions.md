# 파이썬 - 모듈, 패키지, 예외 처리

> [📚 전체 목차로 돌아가기](../../README.md)

## Table of Contents

- [모듈](#1-모듈)
- [패키지](#2-패키지)
- [예외 처리](#3-예외-처리)

---

## #1 모듈

모듈(module)은 **변수, 함수, 클래스 등을 모아놓은 하나의 파이썬 파일**을 말한다.

패키지(package)는 이러한 **모듈들을 계층적으로 묶어 관리하는 디렉터리(폴더) 구조**를 의미한다.


### #1.1 모듈 불러오기 import
mod1.py에 다음과 같은 함수가 있으며, 이 py가 "C:\"라는 경로에 저장되어 있다고 하자. 
```python
def add(a, b):
    return a + b

def sub(a, b):
    return a - b

c = 10
```

cmd와 같은 대화형 인터프리터에서 위와 같이 만든 mod1.py을 파이썬으로 불러와 사용하려면, 먼저 mod1.py가 저장된 디렉터리로 이동해야 한다.

모듈을 불러오려면 `import` 키워드를 사용하면 된다: `import 모듈명`
- 이 예에서는 `import mod1`

`import`는 현재 디렉터리에 있는 파일이나 파이썬 라이브러리가 저장된 디렉터리에 있는 모듈을 가져와서 사용할 수 있게 해주는 명령어이다. 

mod1.py에 있는 `add`나 `sub` 함수를 사용하려면, mod1.add처럼 모듈명 뒤에 .(점)을 붙이고 함수 이름을 쓰면 된다. 모듈에 있는 변수도 마찬가지이다. 
```python
import mod1

print(mod1.add(1, 2))
print(mod1.sub(1, 2))
print(mod1.c)
>> 3
>> -1
>> 10
```

정리하면, 모듈을 불러오려면 `import 모듈명1, 모듈명2, ...`, 모듈에 있는 변수를 가져오려면 `모듈명.변수`, 모듈에 있는 함수나 클래스는 `모듈.함수()`, `모듈.클래스()`

`import 모듈명`으로 모듈을 불러오면, 사용할 때 일일이 모듈명을 입력해야 한다. `import 모듈명 as 이름`을 사용하면 모듈의 이름을 지정할 수 있다. 
```python
import mod1 as m1

print(m1.add(1, 2))
print(m1.sub(1, 2))
>> 3
>> -1
```

`import as`보다 더 간단하게 사용할 수 있는 방법이 있다. `from 모듈명 import 모듈에 있는 변수, 함수, 클래스`를 사용하면 된다.
```python
from mod1 import c

print(c)
>> 10
```
```python
from mod1 import add, sub

print(add(1, 2))
print(sub(1, 2))
>> 3
>> -1
```

`from 모듈명 import *`를 하면, 모듈에 있는 것들을 모두 불러와 사용할 수 있다.
- 여기서 * 문자는 "모든 것"이라는 의미이다.
```python
from mod1 import *

print(add(1, 2))
print(sub(1, 2))
print(c)
>> 3
>> -1
>> 10
```

`from import`에도 `as`를 사용하여 이름을 지정할 수 있다.
```python
from mod1 import add as a, sub as s

print(a(1, 2))
print(s(1, 2))
>> 3
>> -1
```

`import`로 가져온 모듈은 `del`로 해제할 수 있다.
```python
import mod1
del mod1
```
### #1.2 if __name__ == "__main__":
파이썬 파일(즉, 모듈)은 크게 두 가지 방식으로 사용될 수 있다.
- (1) **파일을 (해당 파일에서) 직접 실행하는 경우**
- (2) **다른 파일에서 `import`해서 사용하는 경우**

`if __name__ == "__main__":`은 바로 이 두 경우를 구분하기 위해 사용한다. 

파이썬의 각 모듈에는 `__name__`이라는 **현재 모듈의 이름을 담는 내장 변수**가 있다. 이 `__name__` 변수의 값은 해당 파이썬 파일(모듈)이 프로그램의 시작점인지 아닌지에 따라 그 값이 달라진다.

(1) **파일을 터미널이나 직접 실행한 경우**
- 터미널에서 다음과 같이 파이썬을 직접 실행하면, (혹은 해당 파일에서 직접 실행하면)
```python
python mod1.py
```
- **프로그램의 시작점이 된 파일**의 `__name__` 값은 `"__main__"`이 된다. 
- 다시 말해, `__name__ == "__main__"`이라는 것은 이는 프로그램의 시작점이라는 뜻이다. 

(2) **다른 파일(.py)에서 `import`로 불러온 경우**
- 어떤 파이썬 파일을 다른 파이썬 파일에서 import하면, 가져온 파일의 `__name__`에는 `"__main__"`이 아니라 **그 파일의 이름(즉, 모듈의 이름)**이 저장된다.
- 예를 들어 mod2.py에서 mod1.py를 다음과 같이 `import`로 불러왔다고 하자.
```python
# mod2.py
import mod1
```
- 이 경우 mod1.py의 `__name__`은 "mod1"이 된다. 

즉, 
```python
if __name__ == "__main__"
```
이라는 조건문은,  `__name__` 변수의 값이 `"__main__"`인지 확인을 통해 **현재 이 파일이 메인 프로그램으로 사용되고 있는지(즉, 이 파일이 프로그램의 시작점인지), 아니면 다른 파일에서 모듈로 불러와진 것인지 구분**하기 위한 용도로 사용된다. 


예를 들어 mod1.py에 다음 내용이 있다고 하자.
```python
# mod1.py 

print("mod1.py의 __name__:", __name__)
```
mod1.py를 mod1.py에서 실행시키면, 출력 결과는 `"__main__"`이 된다. 

mod1.py를 직접 실행했으므로 프로그램의 시작점이 mod1.py가 되었기 때문에, mod1.py의 `__name__` 값은 `"__main__"`이 된 것이다. 

이번에는 다음과 같은 mod2.py에서 `import`로 mod1.py를 불러오고, mod2.py에서 실행하자. 
```python
# mod2.py
import mod1

print("mod2.py의 __name__:", __name__)
>> mod1.py의 __name__: mod1
>> mod2.py의 __name__: __main__
```
이번에 직접 실행한 파일은 mod2.py이므로 프로그램의 시작점은 mod2.py이다. 그러므로 mod2.py의 `__name__` 값은 `"__main__"`이 된다. 

그리고 mod1.py는 직접 실행된 것이 아니라 mod2.py에서 `import mod1`을 통해 불러온 모듈이다. 그러므로 mod1.py의 `__name__` 값에는 `"__main__"`이 아니라 모듈 이름인 "mod1"이 저장된다. 


반대로 프로그램의 시작점을 mod1.py로 하자. 즉, mod1.py에서 실행시키는 것이다. 이번에는 mod1.py에서 `import`로 mod2.py를 불러오자.
```python
# mod1.py
import mod2

print("mod1.py의 __name__:", __name__)
>> mod2.py의 __name__: mod2
>> mod1.py의 __name__: __main__
```
프로그램의 시작점인 mod1.py의 `__name__` 값이 `__main__`이 되며, mod1.py에서 import된 모듈인 mod2.py는 `__name__` 값이 "mod2"가 된다.


이번에는 예를 들어 mod1.py가 다음과 같다고 하자.
```python
# mod1.py
def add(a, b):
    return a + b

def sub(a, b):
    return a - b

print(add(1, 4))
print(sub(4, 2))
```
mod1.py를 직접 실행하면 당연히 다음과 같이 출력된다.
```python
5
2
```
그런데 파이썬에서는 다른 파일에서 모듈을 import할 때에도 **그 모듈에 작성되어 있는 최상위 코드가 실행된다.**

다음과 같이 mod2.py에 단순히 `import mod1`만 있다고 하자.
```python
# mod2.py
import mod1
```
이 상태에서 mod2.py를 실행하면 mod1.py에 작성되어 있던 두 출력문이 모두 실행된다. 그래서 mod2.py에는 단순히 import문만 있을 뿐인데 5와 2가 출력된다. 

만약, mod1.py를 import하는 mod2.py에서 mod1.py의 두 프린트 문의 결과를 보고 싶지 않다면 어떻게 해야 할까? => mod1.py에 있는 두 출력문이 mod1.py의 `__name__` 변수의 값이 `"__main__"`일 때만 실행하도록 하면 된다. 즉, mod1.py를 직접 실행해야 두 print문이 출력되게 하는 것이다.
```python
# mod1.py
def add(a, b):
    return a + b

def sub(a, b):
    return a - b

if __name__ == "__main__":
    print(add(1, 4))
    print(sub(4, 2))
```
이렇게 하면 mod2.py에서 mod1.py를 import하고, mod2.py를 실행하더라도 mod1.py의 두 출력문이 실행되지 않는다. 

그 이유는 반복해서 말하지만 mod2.py를 직접 실행하면 프로그램의 시작점은 mod2.py가 되므로 mod2.py의 `__name__` 값은 `"__main__"`이 되고, import된 mod1.py의 `__name__` 값은 모듈 이름인 "mod1"이 되기 때문이다. 따라서 mod1.py의 `if __name__ == "__main__":` 조건은 거짓이 되어 두 출력문이 실행되지 않는다.

### #1.3 sys.path.append
`sys.path`는 파이썬이 모듈이나 패키지를 찾을 때(`import`할 때) 검색하는 디렉터리들의 경로를 문자열로 담고 있는 리스트이다.

`sys.path`에 포함된 디렉터리 안에 저장된 파이썬 모듈은, 사용자가 해당 모듈이 저장된 디렉터리로 이동하지 않아도  바로 불러와 사용할 수 있다. 


`sys.path`는 리스트이기 때문에 `append()`를 통해 사용자가 원하는 모듈이 담겨 있는 디렉터리를 추가할 수 있다. 

예를 들어 "C:/mod" 디렉터리 안에 mod1.py가 저장되어 있다고 하자. 이를 다음과 같이 `sys.path`에 추가하면
```python
import sys

sys.path.append("C:/mod")
```
"C:/mod"가 파이썬의 모듈 검색 경로에 추가되며, 이후 파이썬은 모듈을 찾을 때, "C:/mod" 디렉터리도 함께 검색한다. 

그러므로 사용자가 현재 위치를 "C:/mod"로 변경하지 않아도, 그 안에 저장된 모듈을 바로 불러와 사용할 수 있다.

---
## #2 패키지
패키지(package)는 **모듈들을 계층적으로 묶어 관리하는 디렉터리(폴더) 구조**를 의미한다.

예를 들어 온라인 쇼핑몰 프로그램을 다음과 같은 패키지 구조로 만들 수 있다.
```python
shop/
    __init__.py

    product/
        __init__.py
        item.py
        category.py

    order/
        __init__.py
        cart.py
        payment.py

    user/
        __init__.py
        account.py
        address.py
```
여기서
- `shop`은 전체 패키지의 **루트 디렉터리**
- `product`, `order`, `user`는 기능별로 나눈 **서브 디렉터리**
- `item.py`, `cart.py`, `payment.py`, `account.py` 등 `.py` 파일은 각각 **파이썬 모듈**

패키지 구조로 파이썬 프로그램을 만들면, 기능별로 여러 모듈과 디렉터리를 나누어 관리할 수 있어 유지보수가 편해지고, 다른 모듈과 이름이 겹치더라도 이름 충돌을 방지할 수 있다.

디렉터리(폴더) 안에 `__init__.py` 파일이 있으면 해당 폴더는 패키지로 인식된다. `__init__.py` 파일의 내용은 비워 둘 수 있다. 
- 파이썬 3.3 이상부터는 `__init__.py`파일이 없어도 패키지로 인식된다. 

`.py`에 있는 내용들이 다음과 같다고 하자. 

`shop/__init__.py`
```python
SHOP_NAME = "쇼핑몰"
```

`shop/product/item.py`
```python
def get_item_name():
    return "노트북"
```

`shop/product/category.py`
```python
def get_item_name():
    return "노트북"
```

`shop/order/cart.py`
```python
def add_to_cart(item):
    return f"{item}을(를) 장바구니에 담았습니다."
```

`shop/order/payment.py`
```python
def pay(price):
    return f"{price}원을 결제했습니다."
```

`shop/user/account.py`
```python
class User:
    def __init__(self, name):
        self.name = name

    def introduce(self):
        return f"안녕하세요. 저는 {self.name}입니다."
```
`shop/user/address.py`
```python
class User:
    def __init__(self, name):
        self.name = name

    def introduce(self):
        return f"안녕하세요. 저는 {self.name}입니다."
```
---

### #2.1 패키지.모듈 경로를 통해 패키지의 모듈에서 변수, 함수, 클래스 불러오기 
다음과 같은 경로로 모듈이나 모듈에 있는 변수, 함수, 클래스를 불러올 수 있다. 
```python
import 패키지.모듈1, 패키지.모듈2, ... 
from 패키지.모듈 import 모듈의 변수, 함수, 클래스
from 패키지.모듈 import *
```

가장 간단한 방법은 패키지 경로를 전부 적어서 import하는 것이다.
```python
# 모듈 전체 경로로 import
import shop.product.item
import shop.order.cart

shop.product.item.get_item_name()
shop.order.cart.add_to_cart("노트북")
>> 노트북
>> 노트북을(를) 장바구니에 담았습니다.
```
- `import shop.product.item`는 `shop` 패키지 안의 `product` 패키지 안에 있는 `item.py` 모듈을 불러오는 것이다. 

이렇게 패키지의 .(점)으로 구분된 이름은 **패키지와 하위 모듈의 계층 관계**를 나타낸다.

두 번째 방법은 `from ... import`로 모듈을 가져오는 것이다. 

아래 예시는 `item` 모듈이 들어 있는 `shop.product`까지 `from`에 작성한다.
```python
# 패키지에서 모듈을 import
from shop.product import item
from shop.order import cart

item.get_item_name()
cart.add_to_cart("노트북")
>> 노트북
>> 노트북을(를) 장바구니에 담았습니다.
```
- 이번에는 `item`이라는 이름이 현재 코드에서 직접 사용할 수 있기 되었기 때문에, `shop.product.item.get_item_name()`처럼 전체 경로를 적을 필요가 없다. 

세 번째 방법은 모듈 안의 함수 자체를 가져오는 것이다.
```python
# 모듈에서 함수를 직접 import
from shop.product.item import get_item_name
from shop.order.cart import add_to_cart

get_item_name()
add_to_cart("노트북")
>> 노트북 
>> 노트북을(를) 장바구니에 담았습니다.
```
- 이 경우에는 `item`이라는 모듈 이름조차 붙일 필요가 없다. 

단, `from shop.product.item.get_item_name`처럼 모듈의 함수를 모듈 경로로 import할 수 없다.


모듈 안의 클래스를 불러오는 방법도 동일하다. 

`account.py`에는 `User` 클래스가 있다.
```python
import shop.user.account

user1 = shop.user.account.User("철수")
user1.introduce()
>> 안녕하세요. 저는 철수입니다.
```
```python
from shop.user import account

user1 = account.User("철수")
user1.introduce()
>> 안녕하세요. 저는 철수입니다.
```
```python
from shop.user.account import User

user1 = User("철수")
user1.introduce()
>> 안녕하세요. 저는 철수입니다.
```

참고로 패키지의 모듈(즉, 어떤 모듈이 패키지 안에 들어 있을 경우)은, 그 모듈의 `__name__` 값은 **패키지.모듈**, 서브 디렉터리가 있으면 **패캐지.서브패키지.모듈** 형식의 전체 이름이 들어간다. 

예를 들어, `shop/product/item.py`에 `print("item.py의 __name__:", __name__)`를 추가하고, 다른 곳에서 `item` 모듈을 import하면 이 `__name__`은 `shop.product.item`이 된다. 

### #2.2 __init__.py
`__init__.py` 파일은 해당 디렉터리가 패키지의 일부임을 알려주는 역할뿐 아니라, 패키지를 초기화하는 역할도 한다. 그래서 `__init__.py`에는 패키지와 관련된 설정이나 초기화 코드를 포함시킬 수 있다. 

#### #2.2.1 패키지 변수 및 함수 정의, 초기화
패키지의 `__init__.py` 파일에 공통 변수나 함수를 정의할 수 있다. 

`shop/__init__.py`
```python
SHOP_NAME = "쇼핑몰"

def print_shop_name():
    print(f"shop name: {SHOP_NAME}")

print(f"Initializing {SHOP_NAME} ...")
```
```python
import shop

print(shop.SHOP_NAME)
shop.print_shop_name()
>> Initializing 쇼핑몰 ...
>> 쇼핑몰
>> shop name: 쇼핑몰
```
그리고 패키지를 처음 import할 때 초기화 코드가 실행되는 것을 볼 수 있다.


#### #2.2.2 패키지 내 모듈을 미리 import하기
`import shop` 이렇게 패키지만 가져와서 `shop.product.item.get_item_name()` 이런 식으로 패키지 내 모듈의 함수를 바로 사용할 수는 없을까?

이를 위해서는 **`shop`을 import할 때 필요한 하위 패키지와 모듈도 함께 import되도록 `__init__.py`를 구성**해야 한다. 

`__init__.py`는 패키지가 import 될 때 실행되며, 여기에 하위 패키지나 모듈을 import하는 코드를 넣을 수 있다. 

예시의 `shop`은 하위 패키지가 존재하므로 다음처럼 구성해보자.

`shop/__init__.py`
```python
from . import product
from . import order
from . import user

SHOP_NAME = "쇼핑몰"
print(f"Initializing {SHOP_NAME} ...")
```

여기서 `.`은 **현재 패키지**, 즉 `shop`을 의미한다. 상대 경로 import에서는 `.`(점)을 이용하여 현재 패키지를 기준으로 모듈이나 하위 패키지를 지정할 수 있다. 
- `.`은 현재 패키지를 의미하며, `..`은 현재 패키지에서 한 단계 위의 패키지를 의미한다. 

그러나 이것만으로는 `product` 패키지 내부의 `item` 모듈까지 자동으로 가져올 수 없다. 

이 예시에는 `product`, `order`, `user` 패키지에도 `__init__.py`가 있다. 여기에 이 서브 패키지들의 모듈들을 import하면 된다.

`shop/product/__init__.py`
```python
from . import item
from . import category
```
`shop/order/__init__.py`
```python
from . import cart
from . import payment
````
shop/user/__init__.py`
```python
from . import account
from . import address
```

이제 `import shop`을 실행하면 순서상 필요한 하위 패키지와 모듈들도 함께 import되어, 다음처럼 사용할 수 있다.

```python
import shop

print(shop.product.item.get_item_name())
print(shop.product.category.get_category())

print(shop.order.cart.add_to_cart("노트북"))
print(shop.order.payment.pay(1500000))

>> Initializing 쇼핑몰 ...
>> 노트북
>> 전자제품
>> 노트북을(를) 장바구니에 담았습니다.
>> 1500000원을 결제했습니다.
```

`import shop`만 했는데도 `shop.product.item.get_item_name()`을 사용할 수 있는 이유는 `shop/__init__.py`가 `product`를 가져오고, 다시 `product/__init__.py`가 `item`을 가져오도록 구성했기 때문이다.


그런데 매번 `shop.product.item.get_item_name()` 처럼 긴 경로를 사용하는 것이 불편할 수 있다.

이럴 때 `shop/__init__.py`에서 **하위 모듈의 함수나 클래스를 직접 가져오게 만들 수 있다.**

`shop/__init__.py`
```python
from .product.item import get_item_name
from .product.category import get_category

from .order.cart import add_to_cart
from .order.payment import pay

from .user.account import User
```
이렇게 하면 `get_item_name`, `pay`, `User` 등이 `shop` 패키지의 네임스페이스(namespace)에 들어오게 된다.

예를 들어 `from .product.item import get_item_name`은 현재 `shop` 패키지의 `product.item` 모듈에서 `get_item_name` 함수를 가져와서 `shop`의 네임스페이스에 넣는다. 

그래서 원래 `shop.product.item.get_item_name()`처럼 사용하던 것을 `shop.get_item_name()`처럼 더 짧게 사용할 수 있게 된다.


이 디렉터리 예시는 상위 패키지가 있고 하위패키지에 모듈로 기능들이 정의되어 있기 때문에

상위 패키지의 `__init__.py`에 **from .하위패키지.모듈 import 변수, 함수, 클래스** 또는 **from .하위패키지.모듈 import *** 형식으로 작성하면 패키지를 가져오는 파일에서는 다음과 같이 **패키지.함수()** 형식으로 사용할 수 있다. 
```python
import shop

print(shop.get_item_name())
print(shop.get_category())
print(shop.add_to_cart("노트북"))
print(shop.pay(1500000))

user = shop.User("철수")
print(user.introduce())

>> Initializing 쇼핑몰 ...
>> 노트북
>> 전자제품
>> 노트북을(를) 장바구니에 담았습니다.
>> 1500000원을 결제했습니다.
>> 안녕하세요. 저는 철수입니다.
```

이번에는 `shop/__init__.py`가 다음과 같다고 하자.
```python
from . import product
from . import order
from . import user
```

그리고 외부에서 `from shop import *`을 실행하고, `print(get_item_name())`을 사용하려고 하면 `get_item_name()`을 찾을 수 없다.

왜냐하면 `shop/__init__.py`에서는 `from . import product`로 `product`라는 하위 패키지를 가져왔을 뿐, `product.item` 안의 `get_item_name` 함수 자체를 `shop`의 네임스페이스로 가져온 것은 아니기 때문이다.

즉, 현재 `shop/__init__.py`가 저렇다면 `shop`의 네임스페이스에는 `product`, `order`, `user` 같은 네임이 존재하지만, `get_item_name`, `add_to_cart`, `pay`, `User`가 존재하는 것은 아니다.

그래서 위에처럼
```python
from .product.item import get_item_name
from .product.category import get_category

from .order.cart import add_to_cart
from .order.payment import pay

from .user.account import User
```
이렇게 해야 `shop`의 네임스페이스에 들어가며, `from shop import *`가 가능해진다. 

그리고 다음과 같이 작성해도 된다.
```python
from .product.item import *
from .product.category import *
from .order.cart import *
from .order.payment import *
from .user.account import *
```
이렇게 하면 각 모듈에서 공개되는 이름이 `shop`의 네임스페이스로 들어오게 된다.

#### #2.2.3 __all__
패키지의 `__init__.py`에서 `from ... import *`로 모든 변수, 함수, 클래스를 가져오게 된다. 만약, 외부에 공개하고 싶지 않은 것이 있다면, 이럴 때 `__all__`을 사용하면 된다.

`__all__`에 공개할 모듈, 변수, 함수, 클래스를 문자열 리스트로 지정하면 된다. 예를 들어 `shop/__init__.py`가 다음과 같다고 하자.
```python
from .product.item import get_item_name
from .product.category import get_category
from .order.cart import add_to_cart
from .order.payment import pay
from .user.account import User

__all__ = [
    "get_item_name",
    "pay",
]
```

`shop` 내부에는 다음 이름들이 존재하게 된다. 
```python
get_item_name
get_category
add_to_cart
pay
User
```
이때 `__all__`에는 `__all__ = ["get_item_name", "pay"]`만 지정했다.

그러므로 외부에서 `from shop import *`을 하더라도 `__all__`에 지정된 `get_item_name`과 `pay`만 사용할 수 있다.

만약 `__all__`에 지정되지 않는 것을 사용한다면 NameError가 발생한다. 
```python
print(get_category())
>> NameError: name 'get_category' is not defined
```
- 단, __all__에 없다고 해서 `shop.get_category()` 방식의 접근까지 막는 것은 아니다.

### #2.3 같은 계층의 다른 하위 패키지 가져오기
이번에는 `shop/order/cart.py`에서 `shop/product/item.py`에 있는 `get_item_name()` 함수를 사용한다고 해보자. 두 파일의 관계는 다음과 같다. 
```python
shop/
├── product/
│   └── item.py
│
└── order/
    └── cart.py
```
`cart.py`는 `shop.order` 패키지에 속해 있고, `item.py`는 옆에 있는 `shop.product` 패키지에 속해 있다.

`cart.py`에서 `item.py`를 가져오려면, `cart.py`의 위치를 기준으로 `item.py`까지의 경로를 상대 import로 표현하면 된다.

`item.py`는 `shop/product/item.py`에 있으므로, `cart.py`에서는 `from ..product import item`으로 불러오면 된다.

여기서 `..`은 현재 패키지인 `order`에서 한 단계 위, 즉 상위 패키지인 `shop`으로 올라간다는 의미다. 그렇게 `shop`까지 올라간 다음, 그 아래의 `product` 패키지를 다시 찾아 들어가는 것이다.

더 깊은 패키지 구조라면 `...`처럼 `.`(점)의 개수가 더 늘어날 수도 있다. 


### #2.4 모듈과 패키지의 독스트링
모듈은 파일 첫 줄에 """ """ 또는 ''' '''로 docstring을 작성할 수 있다. 패키지는 `__init__.py`에 같은 방식으로 작성하면 된다. 

모듈이나 패키지의 docstring을 확인하려면 `__doc__`를 출력하면 된다.
```python
# shop/__init__.py
"""상품, 주문, 회원 기능을 제공하는 쇼핑몰 패키지입니다."""

from .product.item import get_item_name
from .order.cart import add_to_cart
from .order.payment import pay
from .user.account import User
```
```python
import shop

print(shop.__doc__)
>> 상품, 주문, 회원 기능을 제공하는 쇼핑몰 패키지입니다.
```

docstring은 모듈이나 패키지뿐 아니라 함수나 클래스에도 넣을 수 있다. 만약, 어떤 패키지의 모듈에 있는 함수의 독스트링을 출력하고 싶다면, 그 경로를 작성하고 `__doc__`를 출력하면 된다. 

---
## #3 예외 처리
예외(exception)란 **코드를 실행하는 중에 발생한 에러**를 말한다. 
 
AttributeError, ZeroDivisionError, NameError, TypeError 등 다양한 에러들도 모두 예외이다.  

### #3.1 try‑except 
#### #3.1.1 try‑except만 사용
아래는 예외 처리를 위한 `try-except` 문의 기본 구조이다.
```python
try:
    실행할 코드
except:
    예외가 발생했을 때 처리하는 코드
```
`try-except` 문은 **try 블록 수행에서 예외가 발생하면 `except` 블록이 수행된다. `try` 블록에서 예외가 발생하지 않으면 `except` 블록은 실행되지 않는다.**

`except 예외이름:`을 사용하면 해당 예외가 발생했을 때만 `except` 블록이 실행된다. 

즉, 위와 같이 `try-except`만 사용하는 경우, **예외의 종류에 상관없이 예외가 발생하면 except 블록이 실행된다.**

#### #3.1.2 try-except에서 특정 예외만 처리하기
다음과 같이 `except`에 **예외 이름을 지정하면, 해당 예외가 발생했을 때만 except가 실행**된다.
```python
try:
    실행할 코드
except 예외이름:
    예외가 발생했을 때 처리하는 코드
```
```python
try:
    x = 10 / 0
    print(x)

    numbers = [1, 2, 3]
    print(numbers[10])  # IndexError 발생
except ZeroDivisionError:
    print("zero division") # ZeroDivisionError일 때만 실행

>> zero division
```
예시의 `try` 블록은 두 가지 문제가 있다. 하나는 0으로 나누는 ZeroDivisionError, 다른 하나는 존재하지 않는 인덱스에 접근할 때 발생하는 IndexError이다. 

그러나 이 예시에서 except는 IndexError를 잡지 못한다. 
- `try`블록에서 `10 / 0`을 실행하는 시점에 ZeroDivisionError가 먼저 발생한다. 
- 예외가 발생하면 `try` 블록의 나머지 코드는 더 이상 실행되지 않으므로, `numbers[10]`까지 도달하지 않는다.
- 즉, 예외가 여러 개 발생하더라도 먼저 발생한 예외의 처리 코드만 실행된다. 
- 그러므로 이 실행에서는 IndexError 자체가 발생하지 않는다.

발생한 ZeroDivisionError는 `except ZeroDivisionError`와 일치하므로 해당 `except` 블록에서 처리되어 "zero division"이 출력된다. 

#### #3.1.3 예외의 에러 메시지 받아오기
다음과 같이 `except`에서 `as` 뒤에 변수를 지정하면, **발생한 예외의 에러 메시지를 그 변수에 받을 수 있다.** 
```python
try:
    실행할 코드
except 예외이름 as 변수:
    예외가 발생했을 때 처리하는 코드
```
```python
try:
    x = 10 / 0
    print(x)

except ZeroDivisionError as e:
    print("zero division:", e) # ZeroDivisionError일 때만 실행

>> zero division: division by zero
```


#### #3.1.4 `except Exception:`
`Exception`은 파이썬의 많은 예외들이 상속하는 **예외 클래스**이다. 일반적으로 새로운 예외를 만들 때는 `Exception`을 상속받아서 구현한다.

`except Exception:`으로 지정하면, Exception을 상속받아 만든 예외들을 처리할 수 있다. 

```python
try:
    x = 10 / 0
    print(x)

except Exception as e:
    print(e)
    print(type(e))

>> division by zero
>> <class 'ZeroDivisionError'>
```
이 예시의 경우 ZeroDivisionError는 `Exception`의 하위 클래스이다. 그래서 `except Exception:`이 잡을 수 있는 것이다.
- https://docs.python.org/3/library/exceptions.html에서 Exception을 통해 만들어진 예외들을 확인할 수 있다.

#### #3.1.5 try-else
다음처럼 `else`를 사용할 수도 있다. `else`는 `except` 바로 다음에 와야 하며 `except`를 생략할 수 없다.
```python
try:
    실행할 코드
except:
    예외가 발생했을 때 처리하는 코드
else:
    예외가 발생하지 않았을 때 실행할 코드
```
이 구조는 **try 블록에서 예외가 발생하면 except 블록이 실행되고, try 블록에서 예외가 발생하지 않으면  else 절이 실행된다.**
```python
try:
    x = int(input(""))
    y = 10
except:
    print('잘못된 입력')
else:
    if x > y:
        print(x)
    else:
        print(y)

5
>> 10
```
이 예시의 경우 입력으로 숫자가 아닌 다른 값(예: 문자)을 입력하면 예외가 발생하여 `except`가 실행되고, 올바른 입력을 넣었다면 `else` 절이 실행된다. 

#### #3.1.6 try-finally
`finally` 절을 사용하면 `try` 블록에서의 예외 발생 여부와 상관없이 항상 코드를 실행할 수 있다. 

```python
try:
    실행할 코드
except:
    예외가 발생했을 때 처리하는 코드
else:
    예외가 발생하지 않았을 때 실행할 코드
finally:
    예외 발생 여부와 상관없이 항상 실행할 코드
```
```python
x = "a"

try:
    x = int(input(""))
    y = 10
except:
    print('잘못된 입력')
else:
    if x > y:
        print(x)
    else:
        print(y)
finally:
    print(5 * x)

3
>> 10
>> 15

"d"
>> 잘못된 입력
>> aaaaa
```

처음에는 `x="a"`이므로 `x`는 문자열이다. 그런데 `3`을 입력하면 `x=int(input(""))`이 정상적으로 실행되면서 `x`에 정수 3이 새로 저장된다.

이 경우에는 예외가 발생하지 않았으므로, `else` 절이 실행된다. 그리고 `finally` 절도 실행되기 때문에, 결과로 10과 15가 출력되는 것이다.

만약, "d"를 입력하면 `x = int("d")`를 실행하려 할 것이다. 그러나 `"d"`는 정수로 변환할 수 없기 떄문에 ValueError가 발생한다. 

` int("d")`가 실패했기 때문에 새로운 값이 `x`에 저장되지 않는다. `x="a"`로 유지된다.

이 경우에는 예외가 발생했으므로 `except`가 실행되고, 예외 발생 여부와 상관없이  `finally` 절은 실행되기 때문에, 결과로 "잘못된 입력"과 "aaaaa"가 출력된다. 

#### #3.1.7 여러 개의 예외 처리하기
하나의 `try` 블록에서 발생할 수 있는 여러 종류의 예외를 같은 방식으로 처리하고 싶다면, 예외 클래스들을 튜플 ( )로 묶어 하나의 `except`에서 지정할 수 있다.
```python
try:
    ...
except (ZeroDivisionError, IndexError) as e:
    print(e)
```
단, 한 번의 `try` 실행에서 예외가 발생하면 그 시점에서 `try` 블록이 중단되므로 이후 코드에서 발생할 수 있는 다른 예외까지 연달아 처리하는 것은 아니다.
- 즉, 이 방법이 2개 이상의 예외를 동시에 발생시켜서 모두 처리한다는 뜻이 아니다. 
- 이 예시의 정확한 의미는, `try` 에서 발생한 예외가 ZeroDivisionError이거나 IndexError라면, 둘 중 어느 것이든 `except`에서 처리하겠다는 것이다.

### #3.2 예외 발생시키기 
상황에 따라 예외를 일부러 발생시켜야 할 경우도 생긴다. 이럴 때는 `raise`를 사용하면 된다.

`raise`에 예외를 지정하면 된다. 예외를 지정하고, 해당 예외가 발생했을 때 출력할 에러 메시지를 넣을 수도 있다. (에러 메시지는 optional)
```python
class Calc:
    def add(self):
        raise NotImplementedError("add 메서드를 구현하라.")

class CalcAdd(Calc):
    pass

calc_add = CalcAdd()
calc_add.add()

>> NotImplementedError: add 메서드를 구현하라.
```
이 예시에서 `CalcAdd` 클래스는 `Calc` 클래스를 상속받았다. 단, `Calc` 클래스의 `add` 메서드를 오버라이딩하여 구현하지 않았다. 

이 상황에서 `CalcAdd` 클래스의 인스턴스가 `add` 메서드를 수행하는 순간 부모 클래스인 `Calc`의 `add` 메서드가 수행되어 NotImplementedError가 발생한다.

아래는 `try-except`에서 `raise`를 사용하는 예시이다.
```python
def is_multiple_of_2():
    x = int(input(""))
    if x % 2 != 0:
        # x가 2의 배수가 아니면 예외를 발생시킴
        raise Exception("2의 배수가 아닙니다.")
    print(x)


try:
    is_multiple_of_2()
except Exception as e:
    print(e)

3
>> 2의 배수가 아닙니다.
```
이 예시의 경우 `raise`로 발생한 예외를 `is_multiple_of_2` 함수 안에서 처리하지 않는다. 

이렇게  `raise`로 발생한 예외를 현재 함수 안에서 처리하지 않으면, 그 함수를 호출한 쪽으로 예외가 전달된다. 

이 예시의 경우 `is_multiple_of_2` 안에서 발생한 예외가 바깥으로 전달되어 `except Exception as e:`와 만난다. 발생한 예외가 `Exception`이므로 이 `except`가 예외를 잡는다.

참고로 `assert`를 사용해서 예외를 발생시킬 수도 있다.

`assert`는 지정된 조건식이 거짓이면 AssertionError 예외를 발생시키며, 조건식이 참이면 넘어간다. 

`assert`에는 조건식뿐 아니라 에러 메시지도 지정할 수 있다. 에러 메시지 지정은 옵션이다. 
```python
n = 5
lst = input().split()

aassert len(lst) % n == 0, "길이가 5의 배수가 아닙니다"

abc
>> AssertionError: 길이가 5의 배수가 아닙니다
```

### #3.3 예외 만들기 

**파이썬 내장 클래스인 `Exception` 클래스를 상속하여 예외를 만들 수 있다.**
```python
class 예외이름(Exception):
    def __init__(self):
        super().__init__("에러메시지")
```

```python
class NotMultipleOfTwoError(Exception):
    def __init__(self):
        super().__init__("2의 배수가 아닙니다.")

if int(input()) % 2 != 0:
    raise NotMultipleOfTwoError

3 
>> NotMultipleOfTwoError: 2의 배수가 아닙니다.
```

위의 예시는 예외를 발생시킬 때 에러 메시지를 출력하기 위해 `raise`를 사용했다. 

다음과 같이 `Exception`만 상속받고 `pass`를 넣어서 아무것도 구현하지 않아도 된다.
```python
class NotMultipleOfTwoError(Exception):
    pass

if int(input()) % 2 != 0:
    raise NotMultipleOfTwoError

3 
>> NotMultipleOfTwoError:
```

---
