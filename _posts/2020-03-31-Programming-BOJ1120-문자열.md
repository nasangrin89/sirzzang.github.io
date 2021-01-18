---
title: "[백준] 문자열"
excerpt: 1일 1문제풀이 12일차
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
  - 브루트포스
  - 문자열
  - 시뮬레이션

---





> [문제 출처](https://www.acmicpc.net/problem/1120)



# 1. 문제



길이가 N으로 같은 문자열 X와 Y가 있을 때, 두 문자열 X와 Y의 차이는 X[i] ≠ Y[i]인 i의 개수이다. 예를 들어, X=”jimin”, Y=”minji”이면, 둘의 차이는 4이다.

두 문자열 A와 B가 주어진다. 이때, A의 길이는 B의 길이보다 작거나 같다. 이제 A의 길이가 B의 길이와 같아질 때 까지 다음과 같은 연산을 할 수 있다.

1. A의 앞에 아무 알파벳이나 추가한다.
2. A의 뒤에 아무 알파벳이나 추가한다.

이때, A와 B의 길이가 같으면서, A와 B의 차이를 최소로 하는 프로그램을 작성하시오.



**입력**

첫째 줄에 A와 B가 주어진다. A와 B의 길이는 최대 50이고, A의 길이는 B의 길이보다 작거나 같고, 알파벳 소문자로만 이루어져 있다.

**출력**

A와 B의 길이가 같으면서, A와 B의 차이를 최소가 되도록 했을 때, 그 차이를 출력하시오.



**입출력 예**

```python
입력 : adaabc aababbc
출력 : 2
```



---



# 2. 나의 풀이 



## 풀이 방법



 간단한 문제였다.

 A의 앞이나 뒤에 문자열을 어떻게든 붙일 수 있다. **따라서!**

* 기존에 A의 문자열의 부분 중 B와의 차이가 가장 적은 곳에 A를 위치시키고,
* 그 앞과 뒤에 B와 똑같은 알파벳들을 붙이면 된다.

*결과적으로*  A와 B의 차이의 최솟값은 **최초 A와, A의 길이만큼의 B의 substring 중 차이의 최솟값**과 일치한다.

 

## 풀이 코드

* 입력 문자열의 최대 길이가 50이다. 따라서 두 문자열의 차이의 초기값을 50으로 설정해 놓는다.
* A를 B의 substring과 비교하며, 둘의 문자열 차이를 계산한다. 그 값이 초기의 차이보다 작다면, 차이를 업데이트한다.

```python
a, b = input().split()

answer = 50 # 초기값 설정

for i in range(len(b)-len(a)+1):
    flag = b[i:i+len(a)] # 비교할 substring
    diff_cnt = 0
    
    for j in range(len(flag)):
        if a[j] != flag[j]:
            diff_cnt += 1
    
    if diff_cnt < answer:
        answer = diff_cnt

print(answer)
```



---



# 3. 다른 풀이



[풀이 출처](https://www.acmicpc.net/source/18198158)

```python
x, y = input().split()
minimum = 50

for i in range(len(y) - len(x) + 1):
    temp = 0
    for j in range(len(x)):
        if x[j] != y[i + j]:
            temp += 1
    if temp < minimum:
        minimum = temp

print(minimum)
```



 논리는 똑같다. 그런데 내 풀이의 소요 시간이 64ms였던 데 비해, 시간이 52ms로 훨씬 짧았다.

 substring에서 차이를 계산하기 위해 나는 substring 길이만큼 `for문`을 돌렸는데, 이 코드는 A의 길이만큼만 반복하고, B를 체크하는 부분은 인덱스로 처리했다. 생각해 보니, 어차피 앞에서부터 뒤로 한 칸씩 이동하며 모든 부분을 검사하기 때문에 인덱스로 사용하면 되는 것이었다!





---

# 4. 배운 점, 더 생각해 볼 점



*  시간 효율성을 생각하자.