---
title:  "[백준] BOJ7568 부녀회장이 될테야"
excerpt:
header:
  teaser: /assets/images/blog-Programming.jpg
categories:
  - Programming
tags:
  - Python
  - Programming
  - BOJ
  - 완전탐색
---



> [문제 출처](https://www.acmicpc.net/problem/2775)



# 문제



평소 반상회에 참석하는 것을 좋아하는 주희는 이번 기회에 부녀회장이 되고 싶어 각 층의 사람들을 불러 모아 반상회를 주최하려고 한다.

이 아파트에 거주를 하려면 조건이 있는데, “a층의 b호에 살려면 자신의 아래(a-1)층의 1호부터 b호까지 사람들의 수의 합만큼 사람들을 데려와 살아야 한다” 는 계약 조항을 꼭 지키고 들어와야 한다.

아파트에 비어있는 집은 없고 모든 거주민들이 이 계약 조건을 지키고 왔다고 가정했을 때, 주어지는 양의 정수 k와 n에 대해 k층에 n호에는 몇 명이 살고 있는지 출력하라. 단, 아파트에는 0층부터 있고 각층에는 1호부터 있으며, 0층의 i호에는 i명이 산다.



(입력)

첫 번째 줄에 Test case의 수 T가 주어진다. 그리고 각각의 케이스마다 입력으로 첫 번째 줄에 정수 k, 두 번째 줄에 정수 n이 주어진다. (1 <= k <= 14, 1 <= n <= 14)



(출력)

각각의 Test case에 대해서 해당 집에 거주민 수를 출력하라.



# 풀이 방법 1_배열 접근

* 전체 아파트 : [[0층], [1층], [2층], ..., [k층]].
* 0층은 초기 케이스이므로 0층 배열을 저장한다.
* 1층부터 k층까지 각 층의 1호는 언제나 1명이 거주하므로, 1을 저장한다.
* 2층부터 k층까지 2호부터 n호까지의 사람을 구해 0층의 배열에 쌓아 나간다.
* T번 반복한 후, 가장 마지막 원소에 접근하면 그게 k층 n호에 사는 사람의 수이다.

```python
for _ in range(T):
    k = int(input())
    n = int(input())
    ground = [[i for i in range(1, n+1)]]
    for i in range(1, k+1):
        floor = [1]
        for j in range(2, n+1):
            floor.append(sum(ground[i-1][:j]))
        ground.append(floor)
     print(ground[-1][-1])
```



# 풀이 방법 2_재귀 접근

![apt]({{site.url}}/assets/images/boj2775_apartment_recursive.jpg)



## 2.1. 시간 초과

* 코드 전부 다 계산해볼 필요 없이, 1호일 때, 0층일 때 초기 조건 잘 정의했으므로 구현됨.
* 답이 나오기는 하는데, 시간 초과.

```python
def apartment(k, n):
    if n == 1:
        return 1
    elif k == 0:
        return n
    else:
        return apartment(k-1, n) + apartment(k, n-1)
    
    
T = int(input())
for _ in range(T):
    k = int(input())
    n = int(input())
    print(apartment(k, n))
```



## 2.2. 메모화

* dictionary 자료형을 활용해 결과값을 메모함으로써 계산 시간 단축. key값이 tuple로 저장되고, 접근할 때도 tuple 형태여야 함에 주의.

```python
dictionary = {}

def apartment(k,n):
    if (k,n) in dictionary:
        return dictionary[(k,n)]
    if n == 0:
        return 1
    elif k == 0:
        return n
    else:
        result = apartment(k-1, n) + aparment(n, k-1)
        dictionary([k,n]) = result
        return result

T = int(input())
for _ in range(T):
    k = int(input())
    n = int(input())
    print(apartment(k, n))    
```



# 배운 점, 더 생각해 볼 점

* `RecursionError`에 부딪히지 않으려면 메모화를 잘 사용하자!