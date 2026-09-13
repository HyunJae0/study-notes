# 알고리즘 - 그리디, 백트래킹, DP, 분할 정복

> [📚 전체 목차로 돌아가기](../../README.md)

## Table of Contents

- [Greedy Algorithm](#1-greedy-algorithm)
- [백트래킹 (Backtracking)](#2-백트래킹-backtracking)
- [다이나믹 프로그래밍 (Dynamic Programming, DP)](#3-다이나믹-프로그래밍-dynamic-programming-dp)
- [분할 정복 (Divide and Conquer)](#4-분할-정복-divide-and-conquer)

---

## #1. Greedy Algorithm
그리디 알고리즘은 **현재 상황에서 가장 좋아 보이는 선택**을 하는 알고리즘이다.  

전체 경우를 모두 확인하지 않기 때문에 빠르고 단순하지만, 현재 단계에서 최선인 선택이 전체 문제의 최적해를 항상 보장하는 것은 아니다.

예를 들어 사용할 수 있는 동전이 다음과 같고, 1260원을 거슬러줘야 한다고 하자. 
```python
coins = [500, 100, 50, 10]
```

가장 큰 동전부터 최대한 많이 사용한다면
```python
def greedy_change(money):
    coins = [500, 100, 50, 10]
    count = 0

    for coin in coins:
        count += money // coin # 동전 개수 count
        money %= coin # 남은 금액 계산

    return count


print(greedy_change(1260))
>> 6
```

---

## #2. 백트래킹 (Backtracking)
백트래킹은 가능한 후보들을 탐색하되, **조건에 맞지 않는 경로는 더 이상 탐색하지 않고 포기(가지치기)한 뒤, 이전 단계로 돌아가 다른 선택을 시도하는 방법**이다. 

백트래킹의 기본 구조는 다음과 같다.
```python
선택
 ↓
계속 탐색
 ↓
가능성이 있는가?
 ├─ YES → 계속 진행
 │
 └─ NO
     ↓
   이전 단계로 돌아감
     ↓
   다른 선택
```

---

## #3. 다이나믹 프로그래밍 (Dynamic Programming, DP)
DP는 **문제를 작은 하위 문제로 나누고, 이미 계산한 부분 문제의 답을 저장해서 다시 사용하는 방법**이다. 그래서 동일한 하위 문제가 반복적으로 등장하는 문제에서 효과적이다.

예를 들어 피보나치 수열은 $F(0) = 0, \; F(1) = 1$이고, $F(n) = F(n-1) + F(n-2)$이다. 예를 들어 $F(5)$를 단순 재귀로 계산하면 다음과 같다.
```python
                F(5)
              /      \
           F(4)      F(3)
          /   \      /   \
       F(3)  F(2)  F(2)  F(1)
       / \
    F(2) F(1)
```
F(3), F(2), F(1)이라는 같은 값들이 다시 계산되는 것을 볼 수 있다. DP는 한 번 계산한 결과를 저장해 두었다가 동일한 값이 다시 필요한 경우 이를 재사용함으로써 불필요한 중복 계산을 줄일 수 있다.

DP와 분할 정복은 모두 큰 문제를 여러 개의 작은 하위 문제로 나눈다는 공통점이 있다. 차이점은 하위 문제의 중복 여부이다. 분할 정복은 일반적으로 서로 독립적인 하위 문제를 각각 해결한 뒤 결과를 결합하는 반면, DP는 중복된 하위 문제들의 결과를 테이블에 저장하고 재사용함으로써 중복 계산을 피한다.


DP는 크게 다음 두 방식으로 구현할 수 있다: 상향식 접근법, 하향식 접근법 
- (1) 상향식 접근법: 타뷸레이션(Tabulation)
- bottom-up 방식은 **가장 작은 하위 문제부터 먼저 계산한 뒤, 그 계산 결과를 이용하여 점점 더 큰 문제의 답을 계산하는 방식**이다. 이를 bottom-up DP라고 한다.
- 작은 문제부터 순서대로 계산하여 테이블을 차례대로 채우기 때문에 Tabulation이라고 부르기도 한다.
```python
def fibb(n):
    table = [1] * (n + 1)

    for i in range(2, n + 1):
        table[i] = table[i-1] + table[i-2]

    return table[n]
```

- (2) 하향식 접근법: 메모이제이션(Memoization)
- top-down 방식은 **큰 문제에서 시작해서 필요한 작은 문제를 재귀적으로 호출하는 방식**이다. 다만 단순 재귀와 달리 한 번 계산한 결과를 테이블에 저장한다.
```python
n = 5
table = [0] * (n + 1)

def fibb(n):
    if n <= 1:
        return 1

    if table[n]:
        return table[n]

    table[n] = fibb(n-1) + fibb(n-2)

    return table[n]
```
---

## #4. 분할 정복 (Divide and Conquer)
분할 정복 알고리즘은 **큰 문제를 여러 개의 작은 문제로 나누고(Divide), 각각 해결한 뒤(Conquer), 결과를 합쳐(Combine) 원래 문제를 해결하는 방법**이다. 분할 정복 알고리즘은 보통 재귀를 사용하여 구현한다. 

```python
def fibb(n):
    if n <= 1:
        return 1

    return fibb(n-1) + fibb(n-2)
```

---
