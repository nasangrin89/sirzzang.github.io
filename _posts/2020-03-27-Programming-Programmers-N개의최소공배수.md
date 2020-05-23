---
title:  "[Programmers] N개의 최소공배수"
excerpt: "1일 1문제풀이 9일차"
header:
  teaser: /assets/images/blog-Programming.jpg
toc: true
categories:
  - Programming
tags:
  - Python
  - Programming
  - Programmers
  - 수학
---



> [문제 출처](https://programmers.co.kr/learn/courses/30/lessons/12953)



# 1. 문제



두 수의 최소공배수(Least Common Multiple)란 입력된 두 수의 배수 중 공통이 되는 가장 작은 숫자를 의미합니다. 예를 들어 2와 7의 최소공배수는 14가 됩니다. 정의를 확장해서, n개의 수의 최소공배수는 n 개의 수들의 배수 중 공통이 되는 가장 작은 숫자가 됩니다. n개의 숫자를 담은 배열 arr이 입력되었을 때 이 수들의 최소공배수를 반환하는 함수, solution을 완성해 주세요.



**제한사항**

- arr은 길이 1이상, 15이하인 배열입니다.
- arr의 원소는 100 이하인 자연수입니다.



**입출력 예**

| arr        | result |
| ---------- | ------ |
| [2,6,8,14] | 168    |
| [1,2,3]    | 6      |



---



# 2. 나의 풀이 

## 풀이 방법



 두 수의 곱은 최소공배수와 최대공약수의 곱과 같다는 공식을 이용하면 된다.

 배열의 수들을 앞에서부터 두 개씩 잘라 가며, 최소공배수를 구한 후, 두 개의 수 중 뒤의 수를 최소공배수로 교체한다. 최소공배수와 그 다음 수에 대해 해당 과정을 반복한다. 결국 마지막에 업데이트되는 배열의 가장 마지막 수가 모든 수의 최소공배수가 된다.





## 풀이 코드

```python
def gcd(a, b):
    while b:
        a, b = b, a%b
    return a

def lcm(a, b):
    return a * b // gcd(a, b)

def solution(arr):
    arr = sorted(arr)
    i = 1
    while i < len(arr):
        arr[i] = lcm(arr[i-1], arr[i])
        i += 1
    answer = arr.pop()
    return answer
```





---





# 3. 다른 풀이



[풀이 출처](https://programmers.co.kr/learn/courses/30/lessons/12953/solution_groups?language=python3)

```python
from math import gcd

def nlcm(num):
    answer = num[0]
    for n in num:
        answer = n * answer / gcd(n, answer)
    return answer
```

 내 풀이와 논리는 비슷하다. 다만, 제일 앞의 수부터 자기 자신과의 최소공배수를 구하는 방식으로 구현했다.