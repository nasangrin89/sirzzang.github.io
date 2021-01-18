---
title: "[프로그래머스] 카펫"
excerpt: 3일 1문제-12
toc: false
categories:
  - Programming
tags:
  - Python
  - Programming
  - 프로그래머스
  - 완전탐색
last_modified_at: 2020-07-28
---





> [문제 출처](https://programmers.co.kr/learn/courses/30/lessons/42842)



# 1. 문제



Leo는 카펫을 사러 갔다가 아래 그림과 같이 중앙에는 노란색으로 칠해져 있고 테두리 1줄은 갈색으로 칠해져 있는 격자 모양 카펫을 봤습니다. Leo는 집으로 돌아와서 아까 본 카펫의 노란색과 갈색으로 색칠된 격자의 개수는 기억했지만, 전체 카펫의 크기는 기억하지 못했습니다.

Leo가 본 카펫에서 갈색 격자의 수 brown, 노란색 격자의 수 yellow가 매개변수로 주어질 때 카펫의 가로, 세로 크기를 순서대로 배열에 담아 return 하도록 solution 함수를 작성해주세요.



##### 제한 사항

- 갈색 격자의 수 brown은 8 이상 5,000 이하인 자연수입니다.
- 노란색 격자의 수 yellow는 1 이상 2,000,000 이하인 자연수입니다.
- 카펫의 가로 길이는 세로 길이와 같거나, 세로 길이보다 깁니다.



##### 입출력 예제

| brown | yellow | return |
| ----- | ------ | ------ |
| 10    | 2      | [4, 3] |
| 8     | 1      | [3, 3] |
| 24    | 24     | [8, 6] |



<br>

# 2. 나의 풀이 



## 풀이 방법



 총 타일의 개수는 갈색 타일과 노란색 타일을 합친 것과 같다. 세로가 가로 길이보다 더 짧고, 세로의 길이가 적어도 3 이상이어야 하기 때문에, 3 이상 총 타일의 개수의 제곱근 이하의 범위에서 가능한 모든 경우의 수를 찾는다.

 노란색 타일의 개수는 `(가로 길이 - 2) x (세로 길이 -2)`와 같기 때문에, 해당 조건을 만족하는 경우를 반환한다.

<br>



## 풀이 코드



```python
def solution(brown, yellow):
    total = brown + yellow
    for h in range(3, int(total**(1/2))+1):
        if total % h == 0: # 약수인 경우
            w = total // h
            if (h-2)*(w-2) == yellow:
                return [w, h]
```



<br>

# 3. 다른 풀이



[출처](https://programmers.co.kr/learn/courses/30/lessons/42579/solution_groups?language=python3)



```python
def solution(brown, red):
    for i in range(1, int(red**(1/2))+1):
        if red % i == 0:
            if 2*(i + red//i) == brown-4:
                return [red//i+2, i+2]
```

 약수가 아니라, 둘레 길이를 이용해서 풀었다.