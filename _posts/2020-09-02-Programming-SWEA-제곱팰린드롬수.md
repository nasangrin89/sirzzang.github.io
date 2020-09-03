---
title: "[SWEA] 제곱 팰린드롬 수"
excerpt: (오랜만에 돌아온) 3일 1문제-21
toc: false
categories:
  - Programming
tags:
  - Python
  - Programming
  - SW Expert Academy
last_modified_at: 2020-09-02
---





> [문제 출처](https://swexpertacademy.com/main/code/problem/problemDetail.do)



# 1. 문제



- 앞으로 읽어도 뒤로 읽어도 똑같은 문자열을 **팰린드롬** 혹은 회문이라고 부른다. 어떠한 실수 N이 양의 정수이며, 십진수로 표현했을 때 팰린드롬이면 이 수를 **팰린드롬 수**라고 부른다.

  어떠한 양의 정수 N에 대해서, N과 √N이 모두 팰린드롬이면 이 수를 **제곱 팰린드롬 수** 라고 부른다.

  예를 들어, 121은 제곱 팰린드롬 수인데, 121이 팰린드롬이며, 121의 제곱근인 11 역시 팰린드롬이기 때문이다.

   

  A 이상 B 이하 제곱 팰린드롬 수는 모두 몇 개인가?



##### 입력 형식

- 첫 번째 줄에 테스트 케이스의 수 TC가 주어진다. 이후 TC개의 테스트 케이스가 새 줄로 구분되어 주어진다. 각 테스트 케이스의 첫 번째 줄에 A, B 가 주어진다. 



##### 출력 형식

각 테스트 케이스 마다 한 줄씩, 제곱 팰린드롬 수의 개수를 출력하라.



##### 입출력 예시

```
# 입력
3 // 전체 테스트케이스 수
1 9 // 첫 번째 테스트케이스 A = 1, B = 9
10 99 // 두 번째 테스트케이스 A = 10, B = 99
100 1000	

# 출력
#1 3 // 첫 번째 테스트 케이스 답
#2 0 // 두 번째 테스트 케이스 답
#3 2	
```





<br>

# 2. 나의 풀이 





### 풀이 방법

 주어진 수 `A`, `B`의 범위 안에 있는 수 중, 제곱근이 있는 수여야 제곱 팰린드롬이 된다. 따라서 `A`, `B`의 제곱근 범위 안에 있는 정수들에 대해, 해당 수와 그 수의 제곱이 팰린드롬인지 검사한다.

<br>



### 풀이 코드



```python
# 팰린드롬인지 검사
def is_palindrome(x):
    if str(x) == str(x)[::-1]:
        return True
    return False
 
def solution(a, b):
    # 제곱근 범위 
    if a**(1/2) == int(a**(1/2)):
        start = int(a**(1/2))
    else:
        start = int(a**(1/2)) +1
    end = int(b**(1/2))+1
    
    # 팰린드롬이면 answer 1씩 더하기
    answer = 0
    for n in range(start, end):
        if is_palindrome(n) and is_palindrome(n**2):
            answer += 1
    return answer
 
TC = int(input())
for tc in range(TC):
    a, b = map(int, input().split())
    answer = solution(a, b)
    print("#{0} {1}".format(tc+1, answer))
```


