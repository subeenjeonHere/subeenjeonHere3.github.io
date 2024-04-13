---
title: 선택 정렬(Selection Sort), 삽입 정렬 (Insertion Sort)
description: Sorting algorithms
author: subeenjeon
date: 2024-04-14
categories: [ Algorithms ]
tags: [ Algorithms, 알고리즘 ]
pin: true
math: true
mermaid: true
#image:
#  path: /commons/devices-mockup.png
#  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
#  alt: Responsive rendering of Chirpy theme on multiple devices.
---

# 선택 정렬(Selection Sort), 삽입 정렬 (Insertion Sort)

태그: ★ 알고리즘
날짜: 2024년 4월 11일 오전 7:19
상태: 진행 중

---

# ◼︎ 선택 정렬(Selection Sort)

배열(리스트)의 정렬되지 않은 부분의 **최댓값이나 최솟값을 찾아서** 정렬되지 않은 부분의 첫 번째 원소와 자리를 교환하는 방식으로 정렬한다.

---

## ► 선택 정렬 특징

1. 원리가 간단하여 구현이 쉽다.
2. 비교적 항목이 적을 때 잘 작동한다.
3. 이중 반복문을 사용하므로, 시간 복잡도는 O(n2)이다.
  1. *입력 데이터 크기에 따라 실행 시간이 제곱 수준으로 증가*

---

## ► Python으로 구현하기

```python
import random

def selection_sort(my_list):
    for i in range(len(my_list) - 1):
        min_idx = i
        for j in range(i + 1, len(my_list)):
            if my_list[j] < my_list[min_idx]:
                min_idx = j
        my_list[i], my_list[min_idx] = my_list[min_idx], my_list[i]
    return my_list

my_list = list()
for _ in range(10):
    my_list.append(random.randrange(1, 15))

print("정렬 전: ")
print(my_list)

print("정렬 후:")
print(selection_sort(my_list))
```

```python
정렬 전: 
[8, 4, 5, 8, 3, 1, 5, 11, 1, 4]
정렬 후:
[1, 1, 3, 4, 4, 5, 5, 8, 8, 11]
```

---

# ◼︎ 삽입 정렬 (Insertion Sort)

삽입 정렬은 배열의 각 요소를 적절한 위치에 삽입하는 방법으로 정렬하는 알고리즘이다. 삽입 정렬은 두 번째 요소부터 시작하여 왼쪽에 있는 요소들과 비교하며 적절한 위치에 삽입한다.

---

## ► 삽입 정렬 특징

1. 원리가 간단하여 구현이 쉽다.
2. 이미 정렬되어 있는 경우 효율적이다.
3. 이중 반복문을 사용하므로, 시간 복잡도는 O(n2)이다.
  1. *입력 데이터 크기에 따라 실행 시간이 제곱 수준으로 증가*

---

## ► Python으로 구현하기

삽입 정렬: DESC

```python
//입력값
53421
```

```python
def insertion_sort(li):
    n = len(li)  # Get the length of the list
    if n <= 1:
        return
    for i in range(1, n):
        key = li[i]
        j = i - 1
        while j >= 0 and key > li[j]:
            li[j + 1] = li[j]
            j -= 1
        li[j + 1] = key

li = list(map(int, input()))
insertion_sort(li)

for i in li:
    print(i, end='')
```

### 삽입 정렬의 동작 방식을 표 형식으로

| 정렬 과정 | 배열 상태 | 정렬한 요소 | 수정된 위치 |
| --- | --- | --- | --- |
| 초기 상태 | 5 3 4 2 1 | - | - |
| 첫 번째 반복 | 3 5 4 2 1 | 5와 3 비교 | 5의 위치는 2로 이동 |
| 두 번째 반복 | 3 4 5 2 1 | 5와 4 비교 | 5의 위치는 3로 이동 |
| 세 번째 반복 | 2 3 4 5 1 | 5와 2 비교 | 5의 위치는 4로 이동 |
| 네 번째 반복 | 1 2 3 4 5 | 5와 1 비교 | 5의 위치는 5로 이동 |

### 결과

```python
# 전
45321
# 후
54321
```
