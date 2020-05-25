---
title: "[Programmers] 더 맵게"
excerpt: 1일 1문제풀이 9일차
header:
  teaser: /assets/images/blog-Programming.jpg
toc: true
categories:
  - Programming
tags:
  - Python
  - Programming
  - Programmers
  - 자료구조
  - 힙
---





> [문제 출처](https://programmers.co.kr/learn/courses/30/lessons/42626)



# 1. 문제



매운 것을 좋아하는 Leo는 모든 음식의 스코빌 지수를 K 이상으로 만들고 싶습니다. 모든 음식의 스코빌 지수를 K 이상으로 만들기 위해 Leo는 스코빌 지수가 가장 낮은 두 개의 음식을 아래와 같이 특별한 방법으로 섞어 새로운 음식을 만듭니다.

```
섞은 음식의 스코빌 지수 = 가장 맵지 않은 음식의 스코빌 지수 + (두 번째로 맵지 않은 음식의 스코빌 지수 * 2)
```

Leo는 모든 음식의 스코빌 지수가 K 이상이 될 때까지 반복하여 섞습니다.
Leo가 가진 음식의 스코빌 지수를 담은 배열 scoville과 원하는 스코빌 지수 K가 주어질 때, 모든 음식의 스코빌 지수를 K 이상으로 만들기 위해 섞어야 하는 최소 횟수를 return 하도록 solution 함수를 작성해주세요.



**제한사항**

- scoville의 길이는 2 이상 1,000,000 이하입니다.
- K는 0 이상 1,000,000,000 이하입니다.
- scoville의 원소는 각각 0 이상 1,000,000 이하입니다.
- 모든 음식의 스코빌 지수를 K 이상으로 만들 수 없는 경우에는 -1을 return 합니다.



**입출력 예**

| scoville             | K    | return |
| -------------------- | ---- | ------ |
| [1, 2, 3, 9, 10, 12] | 7    | 2      |



**입출력 예 설명**

1. 스코빌 지수가 1인 음식과 2인 음식을 섞으면 음식의 스코빌 지수가 아래와 같이 됩니다.
   새로운 음식의 스코빌 지수 = 1 + (2 * 2) = 5
   가진 음식의 스코빌 지수 = [5, 3, 9, 10, 12]
2. 스코빌 지수가 3인 음식과 5인 음식을 섞으면 음식의 스코빌 지수가 아래와 같이 됩니다.
   새로운 음식의 스코빌 지수 = 3 + (5 * 2) = 13
   가진 음식의 스코빌 지수 = [13, 9, 10, 12]

모든 음식의 스코빌 지수가 7 이상이 되었고 이때 섞은 횟수는 2회입니다.



---



# 2. 나의 풀이 

## 풀이 방법



 스코빌 지수가 최솟값인 음식부터 차례로 섞어 가며 스코빌 지수를 업데이트한다. **스코빌 지수의 최솟값이 주어진 `K`보다 크거나 같다면** 모든 음식의 스코빌 지수가 `K` 이상이라는 의미이므로, 과정을 종료한다.

 계속해서 최솟값을 사용해 스코빌 지수를 업데이트하는 과정을 구현하기 위해 리스트를 사용해 정렬하는 방법, `heapq` 모듈을 사용해최소힙을 구하는 방법을 사용해 봤는데, 전자는 효율성 측면에서 자꾸 통과하지 못해서 최소 힙을 구현했다.

 **모든 음식을 합했을 때 스코빌 지수가 `K`보다 작다면, -1을 구하는 조건**을 꼭 처리해야 한다. 이를 잊으면, 정확성 테스트에서 난리가 난다.



## 풀이 코드

* 예외 조건을 처리하기 위해 `try ~ except` 구문을 사용했다. 모든 음식을 다 섞었는데도 스코빌 지수가 `K`보다 작다면, 남은 음식이 1개 밖에 없으므로 `IndexError`가 나게 된다. 이 경우만 `-1`을 반환하면 된다.

```python
import heapq

def solution(scoville, K):

    h = []
    for s in scoville:
        heapq.heappush(h, s)

    answer = 0
    try: # 가장 스코빌지수가 작은 음식이 K 이상이도록 만듦.
        while h[0] < K:
            score = heapq.heappop(h) + 2*heapq.heappop(h)
            answer += 1
            heapq.heappush(h, score)
    except IndexError: # 만들 수 없을 때
        answer = -1
```





---



# 3. 다른 풀이



[풀이 출처](https://programmers.co.kr/learn/courses/30/lessons/42626/solution_groups?language=python3)

```python
import heapq as hq

def solution(scoville, K):

    hq.heapify(scoville)
    answer = 0
    while True:
        first = hq.heappop(scoville)
        if first >= K:
            break
        if len(scoville) == 0:
            return -1
        second = hq.heappop(scoville)
        hq.heappush(scoville, first + second*2)
        answer += 1  

    return answer
```

* `heapify`를 이용해 바로 힙으로 만들었다.
* `scoville` 배열이 비게 되면 `-1`을 반환한다.





# 4. 배운 점, 더 생각해 볼 점



* 질문 검색해보니 다른 사람들도 다 효율성에 대한 고민이 있었다. heapq 모듈을 사용하지 않고 리스트를 사용하면 시간복잡도가 늘어나는 문제인 듯하다.