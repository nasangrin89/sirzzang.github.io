---
title: "[백준] 가장 긴 증가하는 부분수열"
excerpt: 1일 1문제풀이 20일차..(1일 1문제 너무 힘든데?)
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





> [문제 출처](https://www.acmicpc.net/problem/11053)



# 1. 문제



수열 A가 주어졌을 때, 가장 긴 증가하는 부분 수열을 구하는 프로그램을 작성하시오.

예를 들어, 수열 A = {10, 20, 10, 30, 20, 50} 인 경우에 가장 긴 증가하는 부분 수열은 A = {**10**, **20**, 10, **30**, 20, **50**} 이고, 길이는 4이다.



**입력**

첫째 줄에 수열 A의 크기 N (1 ≤ N ≤ 1,000)이 주어진다. 둘째 줄에는 수열 A를 이루고 있는 Ai가 주어진다. (1 ≤ Ai ≤ 1,000)

**출력**

첫째 줄에 수열 A의 가장 긴 증가하는 부분 수열의 길이를 출력한다.



**입출력 예**

```python
입력
6
10 20 10 30 20 50
출력
4
```



---

# 2. 나의 풀이 



## 풀이 방법



~~(너무 졸리다.. Not Yet)~~





## 풀이 코드


> 29056KB, 56ms.

```python
N = int(input())
seq = list(map(int, input().split()))
dp = [1] * N
for i in range(len(seq)):
    for j in range(i):
        if seq[j] < seq[i]:
            dp[i] = max(dp[i], dp[j]+1)
print(max(dp))
```





---



# 3. 다른 풀이



[풀이 출처](https://www.acmicpc.net/source/12868262)

> 29284KB, 56ms.

```python
from sys import*
input = stdin.readline

def lower(l,r,x):
    while l<=r:
        m = (l+r)//2
        if v[m] >= x:
            r = m - 1
        else:
            l = m + 1
    return l
n=int(input())
arr=list(map(int,input().split()))
v=[arr[0]]
for i in range(1,n):
    if v[-1] < arr[i]:
        v.append(arr[i])
    else:
        v[lower(0,len(v)-1,arr[i])]=arr[i]

print(len(v))
```

 

[풀이 출처](https://www.acmicpc.net/source/17559044)

> 29284KB, 56ms.

```python
def lower_bound(start, end, list_, find):
     
    while (end-start)>0:
        mid = (end+start)//2

        if list_[mid] < find:
            start = mid + 1
        else:
            end = mid
    
    return start

n = int(input())
n_list = list(map(int, input().split()))


result = list()
result.append(n_list[0])

for i in range(1, n):
    if result[-1] < n_list[i]:
        result.append(n_list[i])
    else:
        sorted_ = sorted(result)
        seq = lower_bound(0, len(sorted_), sorted_, n_list[i])
        result[seq] = n_list[i]
print(len(result))
```



 여기까지 이진분할 알고리즘으로 구현한 풀이들이다.

---



[풀이 출처](https://www.acmicpc.net/source/18341659)

> 31340KB, 56ms.

```python
import bisect


def lis(arr):
    lis_arr = [-987654]

    for n in arr:
        if lis_arr[-1] < n:
            lis_arr.append(n)
        else:
            where = bisect.bisect_left(lis_arr, n)
            lis_arr[where] = n
    return len(lis_arr)-1


input()
print(lis(list(map(int, input().split()))))
```



 애초에 이진 분할 알고리즘을 구현해 주는 `bisect` 모듈을 썼다.





---

# 4. 배운 점, 더 생각해 볼 점



* 이진탐색을 써야 하나?

