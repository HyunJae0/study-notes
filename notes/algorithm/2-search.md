# 알고리즘 - 탐색

> [📚 전체 목차로 돌아가기](../../README.md)

## Table of Contents

- [Search Algorithm](#1-search-algorithm)
  - [순차 탐색 (Sequential Search)](#11-순차-탐색-sequential-search)
  - [이진 탐색 (Binary Search)](#12-이진-탐색-binary-search)
  - [보간 탐색 (Interpolation Search)](#13-보간-탐색-interpolation-search)

---

## #1. Search Algorithm
간단한 탐색 알고리즘으로 순차 탐색, 이진 탐색, 보간 탐색이 있으며, 빠른 탐색을 위한 자료구조로 맵과 해쉬(hash) 테이블이 있다.

---

### #1.1 순차 탐색 (Sequential Search)
순차 탐색은 처음부터 끝까지 하나씩 차례대로 확인하면서 원하는 값을 찾는 탐색 알고리즘이다. 

구현이 간단하고 데이터가 정렬되어 있을 필요가 없다는 장점이 있지만, 데이터가 많아질수록 검색 시간이 데이터 개수에 비례하여 증가한다는 단점이 있다. 

**시간복잡도**

데이터 개수가 $n$개라면 순차 탐색의 시간복잡도는 데이터 개수에 비례한 $O(n)$이다.

**순차 탐색 구현**
```python
def sequential_search(arr, key, low, high):
    for i in range(low, high + 1):
        if arr[i] == key:
            return i
    return None
```

---

### #1.2 이진 탐색 (Binary Search)

이진 탐색은 정렬된 배열의 탐색에 적합한 방법이다. **정렬된 데이터에서 탐색 범위를 절반씩 줄여가며 원하는 값을 찾는다.**

동작 방식은 다음과 같다.
- 탐색 범위의 시작 위치를 `low`, 끝 위치를 `high`라고 한다면, 탐색 범위의 중앙 index인 `middle`을 구한다: $\dfrac{\text{low index} + \text{high index}}{2}$
- `middle`의 값과 찾고자 하는 값인 `key`를 비교하여 다음과 같이 탐색 범위를 줄인다.
    - `array[middle] = key`: 원하는 값을 찾았으므로 탐색 성공
    - `array[middle] < key`: 찾는 값은 `middle`보다 오른쪽에 있으므로, `low`를 업데이트한다: `low = middle + 1`로 설정하고 절반만 다시 탐색한다.
    - `array[middle] > key`: 찾는 값은  `middle`보다 왼쪽에 있으므로, `high`를 업데이트한다: `high = middle - 1`로 설정하고 절반만 다시 탐색한다.
    - 이 과정을 반복하다가 `low > high`가 되면 더 이상 탐색할 범위가 없으므로 찾는 값이 배열에 없다고 판단한다.

**시간복잡도**

이진 탐색은 매 단계마다 탐색 범위가 절반으로 줄어든다: $n \rightarrow \dfrac{n}{2} \rightarrow \dfrac{n}{4} \rightarrow \dfrac{n}{8} \rightarrow ...$

그러므로 이진 탐색의 시간복잡도는 $O(\log_2 n)$으로 효율적인 방법이다. 단, 반드시 배열이 정렬되어 있어야 한다. 

**이진 탐색 구현**
```python
def binary_search(arr, key, low, hight):
    while low <= high:
        middle = (low + high) // 2

        if arr[middle] == key:
            return middle
        elif arr[middle] < key:
            low = middle + 1
        else:
            high = middle - 1 

    return None
```

---

### #1.3 보간 탐색 (Interpolation Search)
보간 탐색은 이진 탐색의 일종으로 배열이 정렬된 상태일 때 값을 탐색하며, **찾고자 하는 값이 있을 것으로 예상되는 위치를 계산한 뒤, 그 위치를 기준으로 탐색 범위를 줄여 가는 탐색 방법**이다. 

탐색할 값의 크기에 따라 예상 위치를 계산하기 때문에 탐색 범위가 항상 정확히 절반으로 나뉘지는 않는다.

이진 탐색은 항상 탐색 범위의 가운데 위치를 선택하는 것과 달리, 보간 탐색은 `low`, `high` 위치의 값과 `key`의 크기를 이용하여 다음과 같이 **key가 있을 법한 위치를 추정**한다.
$$\text{middle} = \text{low} + \text{(high-low)} \cdot \dfrac{\text{key - array[low]}}{\text{array[high] - array[low]}}$$

`array[low]`는 현재 범위에서 가장 작은 값, `array[high]`는 현재 범위에서 가장 큰 값, `key`가 찾고자 하는 값이라고 하면, 먼저 `key`가 현재 값의 범위에서 어느 정도 위치에 있는지를 추정한다: $\dfrac{\text{key - array[low]}}{\text{array[high] - array[low]}}$
- 예를 들어 `array[low] = 10`, `array[high] = 100`, `key = 80`이라면 $frac{70}{90} ≈ 0.78$이 된다. 이는 `80`이 `10~100` 범위에서 약 78% 지점에 있다고 추정한 것이다.

이 비율에 실제 탐색 범위의 인덱스 간격인 `high - low`에 곱한다: $\text{(high-low)} \cdot \dfrac{\text{key - array[low]}}{\text{array[high] - array[low]}}$

마지막으로 `low`를 더해서 실제 배열에서의 예상 위치를 계산한다: $\text{low} + \text{(high-low)} \cdot \dfrac{\text{key - array[low]}}{\text{array[high] - array[low]}}$

예상 위치 `middle`을 이렇게 구한 다음에는 이진 탐색처럼 탐색 범위를 줄여 나간다. 
- `array[middle] == key`: 탐색 성공
- `array[middle] < key`: 찾고자 하는 값이 오른쪽에 있으므로, `low = middle + 1`
- `array[middle] > key`: 찾고자 하는 값이 왼쪽에 있으므로, `high = middle - 1`

**시간복잡도**
평균 $O(\log(\log n))$, 최악의 경우는 $O(n)$이다. 


**보간 탐색 구현**
```python
def interpolation_search(arr, key, low, high):
    while low <= high: 
        middle = int(low + (high - low) * (key - lst[low]) / (lst[high] - lst[low]))

        if arr[middle] == key:
            return middle
        elif arr[middle] < key:
            low = middle + 1
        else:
            high = middle - 1 

    return None
```
