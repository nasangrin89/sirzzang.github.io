---
title: "[Programmers] 모의고사"
excerpt: 3일 1문제-5
toc: false
categories:
  - Programming
tags:
  - Python
  - Programming
  - Programmers
  - 완전탐색
---





> [문제 출처](https://programmers.co.kr/learn/courses/30/lessons/42840#)



# 1. 문제



수포자는 수학을 포기한 사람의 준말입니다. 수포자 삼인방은 모의고사에 수학 문제를 전부 찍으려 합니다. 수포자는 1번 문제부터 마지막 문제까지 다음과 같이 찍습니다.

1번 수포자가 찍는 방식: 1, 2, 3, 4, 5, 1, 2, 3, 4, 5, ...
2번 수포자가 찍는 방식: 2, 1, 2, 3, 2, 4, 2, 5, 2, 1, 2, 3, 2, 4, 2, 5, ...
3번 수포자가 찍는 방식: 3, 3, 1, 1, 2, 2, 4, 4, 5, 5, 3, 3, 1, 1, 2, 2, 4, 4, 5, 5, ...

1번 문제부터 마지막 문제까지의 정답이 순서대로 들은 배열 answers가 주어졌을 때, 가장 많은 문제를 맞힌 사람이 누구인지 배열에 담아 return 하도록 solution 함수를 작성해주세요.



**제한사항**

- 시험은 최대 10,000 문제로 구성되어있습니다.
- 문제의 정답은 1, 2, 3, 4, 5중 하나입니다.
- 가장 높은 점수를 받은 사람이 여럿일 경우, return하는 값을 오름차순 정렬해주세요.



**입출력 예**

| answers     | return  |
| ----------- | ------- |
| [1,2,3,4,5] | [1]     |
| [1,3,2,4,2] | [1,2,3] |



**입출력 예 설명**

입출력 예 #1

- 수포자 1은 모든 문제를 맞혔습니다.
- 수포자 2는 모든 문제를 틀렸습니다.
- 수포자 3은 모든 문제를 틀렸습니다.

따라서 가장 문제를 많이 맞힌 사람은 수포자 1입니다.

입출력 예 #2

- 모든 사람이 2문제씩을 맞췄습니다.



---



# 2. 나의 풀이 



## 풀이 방법

 1번 수포자는 5개를 주기로, 2번 수포자는 8개를 주기로, 3번 수포자는 10개를 주기로 똑같은 답을 찍는다. 주기가 반복되므로 인덱스 사용해서 똑같은지 비교하는 완전탐색을 구현한다.

 각 수포자가 정답을 맞출 때마다 맞춘 개수를 저장하는 배열을 만들고, 그 배열에서 최댓값을 갖는 사람의 번호를 반환하면 된다.



## 풀이 코드

* 모든 조건을 검사할 때 `if`, `elif`로 쓰면 안 된다!

```python
def solution(answers):
    res = [0, 0, 0, 0]
    first = [1, 2, 3, 4, 5]
    second = [2, 1, 2, 3, 2, 4, 2, 5]
    third = [3, 3, 1, 1, 2, 2, 4, 4, 5, 5]        
    for i in range(len(answers)):
        if first[i%5] == answers[i]:
            res[1] += 1
        if second[i%8] == answers[i]:
            res[2] += 1
        if third[i%10] == answers[i]:
            res[3] += 1
    answer = []
    for i, v in enumerate(res):
        if v == max(res):
            answer.append(i)
    return answer
```





---



# 3. 다른 풀이



[출처](https://programmers.co.kr/learn/courses/30/lessons/42840/solution_groups?language=python3)



```python
def solution(answers):
    p = [[1, 2, 3, 4, 5],
         [2, 1, 2, 3, 2, 4, 2, 5],
         [3, 3, 1, 1, 2, 2, 4, 4, 5, 5]]
    s = [0] * len(p)

    for q, a in enumerate(answers):
        for i, v in enumerate(p):
            if a == v[q % len(v)]:
                s[i] += 1
    return [i + 1 for i, v in enumerate(s) if v == max(s)]
```

  로직 자체는 똑같은데, `enumerate` 안에 `enumerate`를 넣었다.

