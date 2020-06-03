---
title: "[백준] 피보나치 함수"
excerpt: 피보나치 수열에 동적 프로그래밍을 적용해 보자.
header:
  teaser: /assets/images/blog-Programming.jpg
toc: true
categories:
  - Programming
tags:
  - BOJ
  - Python
  - Programming
  - 그리디

---





> [문제 출처](https://www.acmicpc.net/problem/1003)



# 1. 문제



다음 소스는 N번째 피보나치 수를 구하는 C++ 함수이다.

```c++
int fibonacci(int n) {
    if (n==0) {
        printf("0")
        return 0;
    } else if (n==1) {
        printf("1")
        return 1;
    } else {
        return fibonacci(n-1) + fibonacci(n-2);
    }
}
```



`fibonacci(3)`을 호출하면 다음과 같은 일이 일어난다.

- `fibonacci(3)`은 `fibonacci(2)`와 `fibonacci(1)` (첫 번째 호출)을 호출한다.
- `fibonacci(2)`는 `fibonacci(1)` (두 번째 호출)과 `fibonacci(0)`을 호출한다.
- 두 번째 호출한 `fibonacci(1)`은 1을 출력하고 1을 리턴한다.
- `fibonacci(0)`은 0을 출력하고, 0을 리턴한다.
- `fibonacci(2)`는 `fibonacci(1)`과 `fibonacci(0)`의 결과를 얻고, 1을 리턴한다.
- 첫 번째 호출한 `fibonacci(1)`은 1을 출력하고, 1을 리턴한다.
- `fibonacci(3)`은 `fibonacci(2)`와 `fibonacci(1)`의 결과를 얻고, 2를 리턴한다.

1은 2번 출력되고, 0은 1번 출력된다. N이 주어졌을 때, `fibonacci(N)`을 호출했을 때, 0과 1이 각각 몇 번 출력되는지 구하는 프로그램을 작성하시오.



**입력**

첫째 줄에 테스트 케이스의 개수 T가 주어진다.

각 테스트 케이스는 한 줄로 이루어져 있고, N이 주어진다. N은 40보다 작거나 같은 자연수 또는 0이다.



**출력**

각 테스트 케이스마다 0이 출력되는 횟수와 1이 출력되는 횟수를 공백으로 구분해서 출력한다.



**입출력 예**

```python
입력
3
0
1
3

출력
1 0
0 1
1 2
```



---

# 2. 나의 풀이 



## 풀이 방법



 피보나치 함수의 호출 트리를 통해 ~~약간의 노가다 작업을 거치면~~ 0이 출력되는 횟수와 1이 출력되는 횟수를 다음과 같이 구할 수 있다.



|  n   | 0 호출 | 1 호출 |
| :--: | :----: | :----: |
|  0   |   1    |   0    |
|  1   |   0    |   1    |
|  2   |   1    |   1    |
|  3   |   1    |   2    |
|  4   |   2    |   3    |
|  5   |   3    |   5    |
|  6   |   5    |   8    |
|  7   |   8    |   13   |
|  8   |   13   |   21   |



 조금만 보다 보면, 두 횟수가 모두 피보나치 수열 형식을 따르고 있음을 알 수 있다.

 다만 0이 호출되는 횟수는 초기 베이스 케이스가 n = 0일 때 1, n = 1일 때 0인 피보나치 수열이다. 반대로 1이 호출되는 횟수는 n = 0일 때 , n = 1일 때 1인 피보나치 수열이다.





## 풀이 코드

* 동적 계획법을 적용해 베이스 케이스만 바뀐 피보나치 수열을 구현했다.

  

```python
# 0 호출 횟수는 1, 0으로 시작하는 피보나치 수열.
def fibo_dp_0(n): 
    if n == 0:
        return 1
    else:
        memo = [0] * (n+1) # 메모 리스트 초기화
        
        # 베이스 케이스 설정
        memo[0] = 1
        memo[1] = 0
        for i in range(2, n+1):
            memo[i] = memo[i-1] + memo[i-2]
        return memo[n]

# 1 호출 횟수는 0, 1로 시작하는 피보나치 수열.
def fibo_dp_1(n): 
    if n == 0:
        return 0
    else:
        memo = [0] * (n+1) 
        memo[0] = 0
        memo[1] = 1
        for i in range(2, n+1):
            memo[i] = memo[i-1] + memo[i-2]
        return memo[n]


T = int(input())
for _ in range(T):
    N = int(input())
    print(f"{fibo_dp_0(N)} {fibo_dp_1(N)}")
```



> *의문*
>
>  풀이 시간이 72ms나 걸린다. 왜지?



---



# 3. 다른 풀이



[풀이 출처](https://www.acmicpc.net/source/18173895)

```python
import sys
T = int(input())
dp = [[1,0], [0,1]]
q = [int(sys.stdin.readline()) for _ in range(T)]

for i in range(2,max(q)+1):
    dp.append([dp[i-2][0]+dp[i-1][0], dp[i-2][1]+dp[i-1][1]])
for i in q:
    print(dp[i][0], dp[i][1])
```



  52ms가 걸린 풀이이다.

* 입력을 받을 때 `sys` 모듈을 활용했다.
* 두 리스트를 나누지 않고, 한 리스트 안에 0이 출력되는 횟수와 1이 출력되는 횟수를 묶었다.



[풀이 출처](https://pacific-ocean.tistory.com/107) 

```python
t = int(input())
for i in range(t):
    n = int(input())
    zero = 1
    one = 0
    tmp = 0
    for _ in range(n):
        tmp = one
        one = one + zero
        zero = tmp
    print(zero, one)
```



   1이 등장하는 횟수의 수열을 피보나치 수열로 먼저 생각한다. 그러면 0이 등장하는 횟수의 수열은 위의 수열의 이전 항이다.

> [여기](https://pdache.github.io/python/boj/BOJ-1003/)에서도 해당 규칙을 활용하여 문제를 풀었다. 조금 더 직관적으로 이해가 된다.

 테이블을 활용하지 않고 변수를 기록하는 데에 사용해 업데이트하는 방식으로 푼 것이 인상적이다.







---

# 4. 배운 점, 더 생각해 볼 점



*  다이나믹 프로그래밍 이름이 너무 어려워서 접근하는 것조차 두려웠는데, 자주 나온다고 한다. 열공하자.