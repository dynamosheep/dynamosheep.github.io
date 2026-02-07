---
title: "[LeetCode] Remove Element - 순서를 버리고 효율을 얻는 전략 (Python)"
categories: algorithm
tags: [leetcode, algorithm, python, two-pointers]
---

## 문제 요약

정수 배열 `nums`에서 특정 값 `val`을 제거하고,  
`val`이 아닌 원소들의 개수를 반환하는 문제다.

- 추가 배열 사용 ❌
- 배열을 **in-place**로 수정
- 원소의 **순서는 중요하지 않음**

---

## 핵심 아이디어

이 문제의 중요한 조건은 **원소의 순서를 유지할 필요가 없다는 점**이다.  
이 조건 덕분에, 제거할 값을 만났을 때 배열의 뒤쪽 원소로 덮어쓸 수 있다.

즉,

- 앞에서부터 검사 (`checkIdx`)
- 제거 대상(`val`)을 만나면
    - 배열의 마지막 원소로 덮어쓰기
    - 배열 길이를 하나 줄이기 (`lastIdx -= 1`)

이 방식은 불필요한 이동을 최소화한다.

---

## 구현 (Python)
```python
from typing import List

class Solution:
    def removeElement(self, nums: List[int], val: int) -> int:
        lastIdx = len(nums) - 1
        checkIdx = 0

        while checkIdx <= lastIdx:
            if nums[checkIdx] == val:
                nums[checkIdx] = nums[lastIdx]
                lastIdx -= 1
            else:
                checkIdx += 1

        return checkIdx
```
---

## 복잡도 분석

- **Time Complexity:** `O(n)`  
  각 원소는 최대 한 번씩만 검사된다.  
  `val`을 만났을 경우에도 상수 시간의 대입 연산만 수행한다.

- **Space Complexity:** `O(1)`  
  추가 배열을 사용하지 않고 입력 배열 `nums` 자체를 수정한다.

---

## 왜 이 방법이 최적일 수밖에 없는가

### 1. 시간 복잡도의 하한을 만족한다

배열에서 `val`이 어디에 있는지는 미리 알 수 없기 때문에,  
모든 원소를 최소 한 번은 검사해야 한다.  
따라서 이 문제의 시간 복잡도 하한은 **Ω(n)**이다.

이 풀이는 정확히 그 하한에 도달한다.

---

### 2. 순서를 유지하면 불필요한 비용이 발생한다

원소의 순서를 유지하려면 `val` 이후의 모든 원소를  
한 칸씩 앞으로 이동시켜야 한다.  
이는 최악의 경우 반복적인 이동으로 인해 비효율적이다.

반면 이 풀이는,

- 순서를 포기하고
- 뒤의 값으로 덮어쓴 뒤
- 포인터만 조정하는 방식

을 사용해 이동 비용을 상수 시간으로 제한한다.

---

## 백엔드 관점에서의 의미

이 패턴은 실제 서비스에서도 자주 등장한다.

- 순서가 중요하지 않은 데이터 필터링
- soft delete 이후 in-memory 컬렉션 정리
- 캐시 엔트리 무효화

문제의 조건을 정확히 읽고  
**불필요한 제약을 과감히 버리는 판단**은  
실제 백엔드 시스템에서도 중요한 설계 기준이 된다.

---

## 정리

- 순서가 중요하지 않다면, 뒤에서 덮어쓰는 전략이 효과적이다.
- 이 방식은 **O(n) 시간, O(1) 공간**으로 문제를 해결한다.
- 문제 조건을 정확히 해석하는 것이 알고리즘 설계의 핵심이다.