---
title: "[Programmers] 다음 큰 숫자"
excerpt: 3일 1문제-4
toc: false
categories:
  - Programming
tags:
  - Python
  - Programming
  - Programmers
---





> [문제 출처](https://programmers.co.kr/learn/courses/30/lessons/12911)



# 1. 문제



자연수 n이 주어졌을 때, n의 다음 큰 숫자는 다음과 같이 정의 합니다.

- 조건 1. n의 다음 큰 숫자는 n보다 큰 자연수 입니다.
- 조건 2. n의 다음 큰 숫자와 n은 2진수로 변환했을 때 1의 갯수가 같습니다.
- 조건 3. n의 다음 큰 숫자는 조건 1, 2를 만족하는 수 중 가장 작은 수 입니다.

예를 들어서 78(1001110)의 다음 큰 숫자는 83(1010011)입니다.

자연수 n이 매개변수로 주어질 때, n의 다음 큰 숫자를 return 하는 solution 함수를 완성해주세요.



**제한사항**

- n은 1,000,000 이하의 자연수 입니다.



**입출력 예**

| n    | result |
| ---- | ------ |
| 78   | 83     |
| 15   | 23     |



**입출력 예 설명**

입출력 예#1
문제 예시와 같습니다.

입출력 예#2
15(1111)의 다음 큰 숫자는 23(10111)입니다.



---



# 2. 나의 풀이 



## 풀이 방법



 주어진 수를 이진수로 만들고, `1`을 세는 함수를 만든다. 주어진 `n`에 대해서 1의 개수를 세고, `n`보다 큰 수 중 주어진 수와 1의 개수가 같은 숫자를 반환한다.





## 풀이 코드



```python
# 이진수를 만들고 1의 개수를 센다.
def count_one(n):
    n_bin = ""
    while n > 0:
        q, r = divmod(n, 2)
        n_bin = str(r) + n_bin
        n //= 2
    return n_bin.count("1")

def solution(n):
    answer = 0
    for i in range(n+1, 10000001):
        if count_one(i) == count_one(n):
            answer = i
            break
    return answer
```





---



# 3. 다른 풀이



[출처](https://programmers.co.kr/learn/courses/30/lessons/12911/solution_groups?language=python3)



```python
def nextBigNumber(n):
    c = bin(n).count('1')
    for m in range(n+1,1000001):
        if bin(m).count('1') == c:
            return m

#아래 코드는 테스트를 위한 출력 코드입니다.
print(nextBigNumber(78))
```



 굳이 이진수 안 만들고 `bin` 내장함수 쓰면 이진수로 만들어 준다(..!). 다른 풀이 보니까 `bin` 안 쓰고 바이트 형식으로 바꾼 것도 있었다.

```python
int2bin = "{0:b}".format(n)
```

