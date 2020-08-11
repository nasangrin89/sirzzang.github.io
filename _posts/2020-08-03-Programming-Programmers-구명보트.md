---
title: "[프로그래머스] 구명보트"
excerpt: 3일 1문제-13
toc: false
categories:
  - Programming
tags:
  - Python
  - Programming
  - 프로그래머스
  - 정렬
---





> [문제 출처](https://programmers.co.kr/learn/courses/30/lessons/42885)



# 1. 문제



무인도에 갇힌 사람들을 구명보트를 이용하여 구출하려고 합니다. 구명보트는 작아서 한 번에 최대 **2명**씩 밖에 탈 수 없고, 무게 제한도 있습니다.

예를 들어, 사람들의 몸무게가 [70kg, 50kg, 80kg, 50kg]이고 구명보트의 무게 제한이 100kg이라면 2번째 사람과 4번째 사람은 같이 탈 수 있지만 1번째 사람과 3번째 사람의 무게의 합은 150kg이므로 구명보트의 무게 제한을 초과하여 같이 탈 수 없습니다.

구명보트를 최대한 적게 사용하여 모든 사람을 구출하려고 합니다.

사람들의 몸무게를 담은 배열 people과 구명보트의 무게 제한 limit가 매개변수로 주어질 때, 모든 사람을 구출하기 위해 필요한 구명보트 개수의 최솟값을 return 하도록 solution 함수를 작성해주세요.



##### 제한사항

- 무인도에 갇힌 사람은 1명 이상 50,000명 이하입니다.
- 각 사람의 몸무게는 40kg 이상 240kg 이하입니다.
- 구명보트의 무게 제한은 40kg 이상 240kg 이하입니다.
- 구명보트의 무게 제한은 항상 사람들의 몸무게 중 최댓값보다 크게 주어지므로 사람들을 구출할 수 없는 경우는 없습니다.



**입출력 예**

| people           | limit | return |
| ---------------- | ----- | ------ |
| [70, 50, 80, 50] | 100   | 3      |
| [70, 80, 50]     | 100   | 3      |





<br>



# 2. 나의 풀이 



## 2.1. 1일차 : 효율성 테스트 실패





### 풀이 방법

 보트에 **2명**까지만 태울 수 있으므로, 가장 가벼운 사람과 가장 무거운 사람을 태워야 한다고 생각했다. 

> 문제 제대로 읽자. 처음에 2명까지 태울 수 있다는 조건 못 보고 풀었다.



 정확성 테스트는 다 맞아서 풀이 논리 자체는 틀리지 않은 것 같은데, 효율성 테스트 1번이 자꾸 걸린다.





### 풀이 코드

 무게를 오름차순으로 정렬하고, 보트에 태울 수 있는 사람을 `people` 리스트에서 pop해 나갔다. 시간 초과 요소가 좀 많아 보이긴 하는데, (정렬, `while`문 안의 `for`문)다른 건 다 괜찮아도 한 개만 걸린다는 게 신기하다.

```python
def solution(people, limit):
    answer = 0
    people.sort()  # 오름차순 정렬
    while people:
        temp = people.pop() # 가장 무거운 사람 먼저 태우고,
        answer += 1
        for i in range(len(people)):
            if temp + people[i] > limit: # 어차피 못 태움
                break
            # 가벼운 사람 더 태울 수 있는지 검사
            people.pop(i) 
            break

    return answer
```

<br>



## 2.2. 2일차



### 풀이 방법



 결국 구글링을 통해 힌트를 얻었다. 반복문을 사용해 굳이 다 리스트를 순회하지 않고, 인덱스만을 이용하면 된다. 보트가 몇 개 필요한지만 알면 되기 때문이다.

* 오름차순으로 정렬한다.
* `l`, `r`로 왼쪽, 오른쪽 인덱스를 지정한다.
* 보트를 1개 사용한다. 무거운 사람만 태울 수 있으면 `r`을 왼쪽으로 1칸 옮기고, 가벼운 사람과 무거운 사람 2명을 모두 태울 수 있으면 `r`을 왼쪽으로 1칸, `l`을 오른쪽으로 1칸 옮긴다.
* 마지막에 `l`과 `r`이 일치한다면, 가운데 사람이 못 타고 남았다는 것이므로, 보트의 개수를 1만큼 증가시킨다.



### 풀이 코드

```python
def solution(people, limit):
    answer = 0
    people.sort() # 오름차순 정렬
    
    l, r = 0, len(people)-1
    while l < r:
        answer += 1 # 보트 1개 추가.
        if people[l] + people[r] > limit: # 둘 다 태울 수 없는 경우: 무거운 사람만 태움.
            r -= 1    
        else: # 둘 다 태울 수 있는 경우.
            l += 1
            r -= 1
    if l == r: # 다 안 탔을 때
        answer += 1
        
    return answer
```



<br>

# 3. 다른 풀이



[출처](https://programmers.co.kr/learn/courses/30/lessons/42885/solution_groups?language=python3)



 다른 풀이들도 대부분 비슷한 방식이었으나, 아래의 방식이 `while`문을 사용하는 데도 불구하고 통과되어서 신기했다.

```python
def solution(people, limit):
    answer = 0
    poo = sorted(people)
    while poo:
        if len(poo) == 1:
            answer += 1
            break
        if poo[0] + poo[-1] > limit:
            poo.pop()
            answer += 1
        else:
            poo.pop(0)
            poo.pop()
            answer += 1
    return answer
```



