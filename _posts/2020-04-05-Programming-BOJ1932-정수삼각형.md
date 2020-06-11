---
title: "[백준] 정수 삼각형"
excerpt: 1일 1문제풀이 16일차
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





> [문제 출처](https://www.acmicpc.net/problem/1932)



# 1. 문제

```
        7
      3   8
    8   1   0
  2   7   4   4
4   5   2   6   5
```

위 그림은 크기가 5인 정수 삼각형의 한 모습이다.

맨 위층 7부터 시작해서 아래에 있는 수 중 하나를 선택하여 아래층으로 내려올 때, 이제까지 선택된 수의 합이 최대가 되는 경로를 구하는 프로그램을 작성하라. 아래층에 있는 수는 현재 층에서 선택된 수의 대각선 왼쪽 또는 대각선 오른쪽에 있는 것 중에서만 선택할 수 있다.

삼각형의 크기는 1 이상 500 이하이다. 삼각형을 이루고 있는 각 수는 모두 정수이며, 범위는 0 이상 9999 이하이다.



**입력**

첫째 줄에 삼각형의 크기 n(1 ≤ n ≤ 500)이 주어지고, 둘째 줄부터 n+1번째 줄까지 정수 삼각형이 주어진다.



**출력**

첫째 줄에 합이 최대가 되는 경로에 있는 수의 합을 출력한다.



**입출력 예**

```python
입력
5
7
3 8
8 1 0
2 7 4 4
4 5 2 6 5

출력
30
```



---

# 2. 나의 풀이 



## 풀이 방법



 어제 푼 RGB 문제와 유사한 방식으로 접근했다. 그러나 RGB거리 문제에서는 다음 단계에서 고려해야 할 수열의 길이가 R, G, B에 해당하는 3개밖에 없었지만, 이 문제에서는 수열의 길이가 1씩 증가한다는 점이 달랐다.

 왼쪽 위에서나 오른쪽 위에서 내려오는 방법밖에 없기 때문에, N층 i번째 칸까지 내려오는 경로의 최댓값은 N-1층 i번째 칸이나 i-1번째 칸까지 내려오는 최댓값에 해당 칸의 수를 더해주면 된다. 다만 가장 왼쪽 칸이나 오른쪽 칸의 경우, 위쪽 칸에서 내려올 때 선택할 수 있는 경로가 한 가지밖에 없기 때문에, 조건 처리를 해줘야 한다.



## 풀이 코드

> 33120KB, 168ms

```python
import sys
s = sys.stdin.readline

N = int(s()) # 칠해야 할 집의 수

dp = [[int(s())]] # 각 경로까지의 최댓값 기록. 초기값 설정.

for i in range(1, N):
    tri = list(map(int, s().split())) # 해당 층의 수
    for j in range(len(tri)):
        if j == 0: # 가장 왼쪽
            tri[j] += dp[i-1][0]
        elif j == len(tri)-1: # 가장 오른쪽
            tri[j] += dp[i-1][j-1]
        else:
            tri[j] += max(dp[i-1][j-1], dp[i-1][j])
    dp.append(tri)

print(max(dp[-1]))
        
            
            
```





---



# 3. 다른 풀이



[풀이 출처](https://www.acmicpc.net/source/15481495)

> 33056KB, 112ms.

```python
def solution():
    import sys
    n = int(input())
    triangle = []
    for _ in range(n):
        triangle.append(list(map(int, sys.stdin.readline().rstrip().split())))
        
        accum = []
        for i in range(n):
            accum = [max(a+c, b+c) for a, b, c in zip([0]+accum, accum+[0], triangle[i])]
    
    print(max(accum))

solution()
```



 시간이 월등하게 짧은 풀이였다. 가장 왼쪽에 있는 경우엔 위의 층 대각선 왼쪽의 수를 0으로, 가장 오른쪽에 있는 경우엔 위의 층 대각선 오른쪽의 수를 0으로 봐줘서 다음 층의 최댓값을 계산했다. `zip`을 사용해서 각 경우의 최댓값을 구한 것도 인상적이다...

 ~~대박쓰...~~

 

[풀이 출처](https://www.acmicpc.net/source/14283763)

> 33052KB, 120ms.

```python
import sys


def sol(n, data):
    for i in range(n - 1, 0, -1):
        for j in range(len(data[i]) - 1):
            if data[i][j] > data[i][j + 1]:
                data[i - 1][j] += data[i][j]
            else:
                data[i - 1][j] += data[i][j + 1]
    return data[0][0]


if __name__ == "__main__":
    input = sys.stdin.readline
    n = int(input())
    data = []
    for i in range(n):
        data.append(list(map(int, input().split())))
    print(sol(n, data))
```



 다음 층으로 내려갈 때 더 큰 쪽으로 내려가면 된다는 간단한 논리로 구현한 풀이이다. 시간도 짧다. 모든 경우를 다 기록할 필요가 없다.



---

# 4. 배운 점, 더 생각해 볼 점



* 전부 다 경우를 기록하고 계산할 때 원시적으로 다 하려고 들지 말자!
* `zip` 사용법 복습
* `if __name__ == "__main__"`



