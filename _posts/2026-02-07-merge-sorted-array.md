---
title: "[LeetCode] Merge Sorted Array - 뒤에서 채워야 하는 이유 (Python)"
categories: algorithm
tags: [leetcode, algorithm, python, two-pointers]
---

## 문제 요약
정렬된 두 배열 `nums1`, `nums2`가 주어질 때  
`nums1`의 뒤쪽 빈 공간을 활용해 두 배열을 **in-place**로 병합하는 문제다.

- `nums1`의 길이 = `m + n`
- 앞의 `m`개 원소만 유효
- `nums2`의 길이 = `n`

---

## 접근 방법

### 잘못된 접근
앞에서부터 병합하면 기존 `nums1`의 값이 덮어써져 문제가 발생한다.

### 핵심 아이디어
- `nums1`의 **뒤쪽은 비어 있음**
- 가장 큰 값부터 뒤에서 채우면 안전
- 두 포인터를 배열 끝에서 시작

---

## 구현 (Python)

```python
from typing import List

class Solution:
    def merge(self, nums1: List[int], m: int, nums2: List[int], n: int) -> None:
        pointer1 = m - 1
        pointer2 = n - 1
        insertingAt = m + n - 1

        while pointer2 >= 0:
            if pointer1 < 0 or nums1[pointer1] <= nums2[pointer2]:
                nums1[insertingAt] = nums2[pointer2]
                pointer2 -= 1
            else:
                nums1[insertingAt] = nums1[pointer1]
                pointer1 -= 1

            insertingAt -= 1
```

---

## 복잡도 분석

- **Time:** O(m + n)  
  두 포인터(`pointer1`, `pointer2`)가 각각 최대 `m`, `n`번만 감소하며,
  매 반복마다 `insertingAt`도 1씩 줄어든다.  
  즉, 전체 비교/대입 횟수는 최대 `m + n`번이다.

- **Space:** O(1)  
  추가 배열을 만들지 않고 `nums1`의 뒤쪽 빈 공간을 재사용해 결과를 채운다.
  포인터 변수 몇 개만 사용하므로 추가 공간은 상수다.
