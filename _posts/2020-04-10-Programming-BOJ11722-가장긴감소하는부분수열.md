---
title: "[백준] 가장 긴 감소하는 부분수열"
excerpt: 1일 1문제풀이 21일차..(다른 방법을 생각해 보자)
header:
  teaser: /assets/images/blog-Programming.jpg
toc: true
categories:
  - Programming
tags:
  - BOJ
  - Python
  - Programming
  - DP

---





> [문제 출처](https://www.acmicpc.net/problem/11722)



# 1. 문제



수열 A가 주어졌을 때, 가장 긴 감소하는 부분 수열을 구하는 프로그램을 작성하시오.

예를 들어, 수열 A = {10, 30, 10, 20, 20, 10} 인 경우에 가장 긴 감소하는 부분 수열은 A = {10, **30**, 10, **20**, 20, **10**} 이고, 길이는 3이다.



**입력**

첫째 줄에 수열 A의 크기 N (1 ≤ N ≤ 1,000)이 주어진다. 둘째 줄에는 수열 A를 이루고 있는 Ai가 주어진다. (1 ≤ Ai ≤ 1,000)

**출력**

첫째 줄에 수열 A의 가장 긴 감소하는 부분 수열의 길이를 출력한다.



**입출력 예**

```python
입력
6
10 20 10 30 20 50
출력
3
```



---

# 2. 나의 풀이 



## 풀이 방법



~~(너무 졸리다.. Not Yet)~~





## 풀이 코드

 어제 문제에서 증가하는지 체크하는 부분만 바꿔 주었다.


> 29056KB, 56ms.

```python
import sys

s = sys.stdin.readline

n = int(s())
seq = list(map(int, s().split()))

dp = [1] * n
for i in range(len(seq)):
    for j in range(i):
        if seq[j] > seq[i]:
            dp[i] = max(dp[i], dp[j] + 1)
print(max(dp))
```





---



# 3. 다른 풀이



[풀이 출처](https://www.acmicpc.net/source/14359035)

> 29056KB, 56ms.

```python
def sol(N, seq):
    result = [seq[0]]
    index = 0
    for i in range(1, N):
        if result[index] > seq[i]:
            result.append(seq[i])
            index += 1
        else:
            for j in range(index + 1):
                if result[j] <= seq[i]:
                    result[j] = seq[i]
                    break
    index += 1
    return index


if __name__ == "__main__":
    N = int(input())
    seq = list(map(int, input().split()))
    print(sol(N, seq))

```

 





---

# 4. 배운 점, 더 생각해 볼 점



* 어제 풀었던 비슷한 문제들 중 가장 시간이 적게 걸리는 풀이들이 다 이진 분할 알고리즘을 썼었다. 혹시 이진분할을 쓰지 않고도 내 풀이  시간을 줄일 수 있는 방법이 없나 고민을 했다. 첫 번째 시도로 `input` 받는 방법을 `sys` 모듈을 사용하도록 바꿔 봤는데, 큰 차이가 없었다. ㅎ. 그냥 로직에서 시간 더 줄일 방법을 고민해 보도록 하자...
* 1일 1문제가 힘들어서 그냥 어제랑 똑같은 문제를 *꼼수로* 풀었다. 학원 왔다 갔다 하는 시간에 다 풀 수 있을 줄 알았는데, 지하철에서 다 풀기에는 머리의 한계를 느낀다. 뭔가 다른 방법을 찾자.

