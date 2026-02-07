---
title: "[LeetCode] Remove Duplicates from Sorted Array - 읽기/쓰기 포인터로 중복 제거하기 (Python)"
categories: algorithm
tags: [leetcode, python, two-pointers]
---

## 문제 요약

정렬된 정수 배열 `nums`가 주어질 때,  
중복된 원소를 **in-place**로 제거하고  
중복 제거 후 남은 원소의 개수를 반환하는 문제다.

- 배열은 이미 정렬되어 있음
- 추가 배열 사용 ❌
- 반환된 길이까지만 배열이 의미 있음

---

## 핵심 아이디어

이 문제의 가장 중요한 전제는 **배열이 이미 정렬되어 있다는 점**이다.  
정렬되어 있기 때문에 같은 값들은 항상 연속해서 등장한다.

이를 이용해 다음과 같은 전략을 사용할 수 있다.

- 하나의 포인터는 **전체 배열을 읽으며 탐색**
- 다른 포인터는 **중복이 제거된 결과를 쓸 위치**를 가리킨다
- 새로운 값이 등장했을 때만 결과 배열에 기록한다

이 패턴은 흔히 **Read / Write Two Pointers**라고 불린다.

---

## 구현 (Python)
``` python
from typing import List

class Solution:
    def removeDuplicates(self, nums: List[int]) -> int:
        beCompared = nums[0]
        searchingIdx = 1
        currentIdx = 1
        length = len(nums)

        while searchingIdx < length:
            if nums[searchingIdx] != beCompared:
                nums[currentIdx] = nums[searchingIdx]
                beCompared = nums[currentIdx]
                currentIdx += 1
            searchingIdx += 1

        return currentIdx
```
    

---

## 포인터 역할 정리

- `searchingIdx`  
  전체 배열을 순회하며 현재 값을 검사하는 **읽기 포인터**

- `currentIdx`  
  중복이 제거된 값이 저장될 위치를 가리키는 **쓰기 포인터**

- `beCompared`  
  직전에 기록된 값으로, 중복 여부를 판단하는 기준 값

---

## 복잡도 분석

- **Time Complexity:** `O(n)`  
  배열의 각 원소를 한 번씩만 확인한다.

- **Space Complexity:** `O(1)`  
  추가 자료구조 없이 입력 배열을 그대로 수정한다.

---

## 왜 이 방법이 최적일 수밖에 없는가

### 1. 모든 원소는 최소 한 번은 확인해야 한다

중복 여부는 실제 값을 비교하지 않으면 알 수 없기 때문에,  
모든 원소를 한 번씩 확인하는 비용은 피할 수 없다.  
따라서 이 문제의 시간 복잡도 하한은 **Ω(n)**이다.

이 풀이는 그 하한에 정확히 도달한다.

---

### 2. 정렬된 배열이라는 조건을 최대한 활용한다

정렬되어 있지 않다면 중복을 제거하기 위해
- 추가 메모리(set 등)를 사용하거나
- 정렬을 다시 수행해야 한다.

하지만 이미 정렬된 배열이라는 조건 덕분에,
이 풀이는 **단순한 포인터 이동만으로** 중복 제거를 수행할 수 있다.

---

## 백엔드 관점에서의 의미

이 패턴은 실무에서도 자주 등장한다.

- 정렬된 로그에서 중복 이벤트 제거
- 이미 정렬된 결과 집합을 압축해서 저장
- DB 쿼리 결과 후처리

“이미 주어진 조건을 얼마나 잘 활용하느냐”가  
성능과 코드 단순성을 동시에 좌우한다는 점에서,
이 문제는 백엔드 개발자에게도 좋은 연습 문제다.

---

## 정리

- 정렬된 배열에서는 **연속 중복**이라는 특성을 활용할 수 있다.
- 읽기/쓰기 포인터를 분리하면 in-place 처리가 가능하다.
- 이 풀이는 **O(n) 시간, O(1) 공간**으로 문제를 해결한다.