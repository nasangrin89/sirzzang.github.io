---
title: "[Programmers] 야근 지수"
excerpt: 3일 1문제 - 1
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
  - 우선순위 큐
---





> [문제 출처](https://programmers.co.kr/learn/courses/30/lessons/12927)



# 1. 문제



회사원 Demi는 가끔은 야근을 하는데요, 야근을 하면 야근 피로도가 쌓입니다. 야근 피로도는 야근을 시작한 시점에서 남은 일의 작업량을 제곱하여 더한 값입니다. Demi는 N시간 동안 야근 피로도를 최소화하도록 일할 겁니다.Demi가 1시간 동안 작업량 1만큼을 처리할 수 있다고 할 때, 퇴근까지 남은 N 시간과 각 일에 대한 작업량 works에 대해 야근 피로도를 최소화한 값을 리턴하는 함수 solution을 완성해주세요.



**제한사항**

- `works`는 길이 1 이상, 20,000 이하인 배열입니다.
- `works`의 원소는 50000 이하인 자연수입니다.
- `n`은 1,000,000 이하인 자연수입니다.



**입출력 예**

| works     | n    | result |
| --------- | ---- | ------ |
| [4, 3, 3] | 4    | 12     |
| [2, 1, 2] | 1    | 6      |
| [1,1]     | 3    | 0      |



**입출력 예 설명**

1. 입출력 예 #1
   n=4 일 때, 남은 일의 작업량이 [4, 3, 3] 이라면 야근 지수를 최소화하기 위해 4시간동안 일을 한 결과는 [2, 2, 2]입니다. 이 때 야근 지수는 22 + 22 + 22 = 12 입니다.
2. 입출력 예 #2
   n=1일 때, 남은 일의 작업량이 [2,1,2]라면 야근 지수를 최소화하기 위해 1시간동안 일을 한 결과는 [1,1,2]입니다. 야근지수는 12 + 12 + 22 = 6입니다.



---



# 2. 나의 풀이 



## 2.1. 1일차 : 효율성 테스트 실패



### 풀이 방법



 제곱합의 특성 상, 큰 수가 작아지도록 줄여야 한다. 최대한 모든 수가 비슷해지도록 큰 수부터 빼야 한다고 생각했다. `works` 배열을 정렬한 뒤, 큰 수일수록 1부터 빼줬다. 



### 풀이 코드

 `n`이 `works`의 합보다 클 경우에는 굳이 야근할 필요가 없으므로, 0을 반환한다.

```python
def solution(n, works):
    answer = 0
    if n >= sum(works):
        return answer
    else:
        while n > 0:
            works.sort()
            works[-1] -= 1
            n -= 1        
        for work in works:
            answer += work*work
        return answer
```





### 더 생각해볼 점



* 스킬 테스트 레벨3 풀면서 처음으로 20분 안에 로직 짰고, 실제로 정확성 테스트도 다 맞았는데, 효율성에서 걸렸다. `for`, `while`, `sort` 등 너무 효율적이지 못한 요소가 많은 것 같다. 

  

![image-20200701231617318]({{site.url}}/assets/images/image-20200701231617318.png)



* 채점 결과 다시 살펴 보니 테스트 7, 8에서 시간이 좀 많이 걸린다. 질문 게시판 검색해 보면서 해당 테스트에서 실패한 사례가 있나 봐야 겠다. 

> *추가*
>
>  스킬 테스트 동안에는 생각 못 했는데, 그 다음 날에 다시 생각해보니 그리디하기 그지 없는 접근이라는 생각이 들어서, 집에 아침에 학원 가면서 전철에서 나름대로 `y=x^2` 그래프와 `x^2 + y^2 = r^2` 그래프 그려 가면서 생각했는데, 맞는 접근인지는 모르겠다. 



## 2.2. 2일차 : 효율성 테스트 실패



### 풀이 방법



 어제 풀었던 풀이에서 굳이 1씩 전부 다 빼줄 필요가 없다고 생각했다. 정렬한 뒤 가장 큰 수와 그 다음으로 큰 수만을 비교했다. 

 그리고 다음과 같이 경우를 분기했다.

* 두 수가 같다면 1을 빼줘서 가장 큰 수를 작게 만들어 비교할 수 있게 해준다.
* 두 수가 같지 않은 경우,
  * 가장 큰 수를 두 번째로 큰 수보다 1만큼 작게 만들면서 `n`이 남는다면, 그렇게 해주고,
  * 가장 큰 수를 두 번째로 큰 수보다 1만큼 작게 만들 때 `n`이 남지 않는다면 가장 큰 수에서 `n`을 빼 줬다.



### 풀이 코드

 `n`을 빼줄 때 어제처럼 아무 생각 없이 아래서 빼면 안 된다. 오늘 바꾼 코드에서는 `works[-1]`, `works[-2]`를 바꾸는 경우가 있기 때문에, `works` 배열의 수가 바뀐 뒤 `n`을 아래에서 빼면 업데이트가 안 된다.



```python
def solution(n, works):
    answer = 0
    if n > = sum(works):
        return answer
    else:
        while n > 0:
            works.sort()
            if works[-1] == works[-2]:
                n -= 1
                works[-1] -= 1
            else:
                if works[-1] - works[-2] + 1 <= n:
                    n -= (works[-1] - works[-2] + 1)
                    works[-1] -= (works[-1] - works[-2] + 1)
                else:
                    works[-1] -= n
                    break
         
        for work in works:
            answer += work * work
            
        return answer
```



### 더 생각해볼 점



 결국 JK에게 가르침을 구했다. *(~~JK는 한 방에 풀더라.. 멋진 JK... 존경해...~~)*

* 어제 코드 문제점
  * `while`문 한 번씩 돌아야 하니까 거기서 시간 복잡도 `N`
  * `sort`하는 게 아마 파이썬에서는 `NlogN`
  * 결과적으로 아무리 못해도 시간 복잡도 `O(N^2 x logn)`. 효율성 테스트에서는 일부러 엄청 큰 수 넣어 놓으니까 틀릴 수밖에 없는 풀이인 듯하다.
* 굳이 1씩 안 빼도 될 것 같은데, 그렇게 하고도 `sort`가 문제이면 자료구조의 문제일 수도 있을 듯? JK는 `C++`에서 `priority queue` 써서 풀었는데 파이썬에서 우선순위 큐 사용해 보면 어떨까?
* 아니면 `sort` 하지 말고 반복문 돌면서 제일 큰 원소, 두 번째로 큰 원소를 찾는 방법도 고려해 보면 어떨까?



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




