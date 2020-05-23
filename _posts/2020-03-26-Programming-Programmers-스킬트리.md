---
title:  "[Programmers] 스킬트리"
excerpt: "1일 1문제풀이 7일차"
header:
  teaser: /assets/images/blog-Programming.jpg
toc: true
categories:
  - Programming
tags:
  - Python
  - Programming
  - Programmers
---



> [문제 출처](https://programmers.co.kr/learn/courses/30/lessons/49993)



# 1. 문제



선행 스킬이란 어떤 스킬을 배우기 전에 먼저 배워야 하는 스킬을 뜻합니다.

예를 들어 선행 스킬 순서가 `스파크 → 라이트닝 볼트 → 썬더`일때, 썬더를 배우려면 먼저 라이트닝 볼트를 배워야 하고, 라이트닝 볼트를 배우려면 먼저 스파크를 배워야 합니다.

위 순서에 없는 다른 스킬(힐링 등)은 순서에 상관없이 배울 수 있습니다. 따라서 `스파크 → 힐링 → 라이트닝 볼트 → 썬더`와 같은 스킬트리는 가능하지만, `썬더 → 스파크`나 `라이트닝 볼트 → 스파크 → 힐링 → 썬더`와 같은 스킬트리는 불가능합니다.

선행 스킬 순서 skill과 유저들이 만든 스킬트리[1](https://programmers.co.kr/learn/courses/30/lessons/49993#fn1)를 담은 배열 skill_trees가 매개변수로 주어질 때, 가능한 스킬트리 개수를 return 하는 solution 함수를 작성해주세요.



**제한사항**

- 스킬은 알파벳 대문자로 표기하며, 모든 문자열은 알파벳 대문자로만 이루어져 있습니다.
- 스킬 순서와 스킬트리는 문자열로 표기합니다.
  - 예를 들어, `C → B → D` 라면 CBD로 표기합니다
- 선행 스킬 순서 skill의 길이는 1 이상 26 이하이며, 스킬은 중복해 주어지지 않습니다.
- skill_trees는 길이 1 이상 20 이하인 배열입니다.
- skill_trees의 원소는 스킬을 나타내는 문자열입니다.
  - skill_trees의 원소는 길이가 2 이상 26 이하인 문자열이며, 스킬이 중복해 주어지지 않습니다.



**입출력 예**

| skill   | skill_trees                         | return |
| ------- | ----------------------------------- | ------ |
| `"CBD"` | `["BACDE", "CBADF", "AECB", "BDA"]` | 2      |



**입출력 예 설명**

- BACDE: B 스킬을 배우기 전에 C 스킬을 먼저 배워야 합니다. 불가능한 스킬트립니다.
- CBADF: 가능한 스킬트리입니다.
- AECB: 가능한 스킬트리입니다.
- BDA: B 스킬을 배우기 전에 C 스킬을 먼저 배워야 합니다. 불가능한 스킬트리입니다.



---



# 2. 나의 풀이 

## 풀이 방법



 핵심은 두 가지다. 첫째, 스킬 트리에 있는 스킬을 일단 배우기 시작했으면, 무조건 그 순서대로 배워야 한다는 것. 둘째, 스킬 트리에 없는 스킬은 아무렇게나 배워도 된다는 것.

 첫째 사항을 기준으로 생각해 보면, 선행 스킬 순서에서 1칸씩 늘려 나간 순서로만 스킬을 습득할 수 있다.

 둘째 사항을 기준으로 생각해 보면, 선행 스킬 순서에 구애받지 않는 스킬들로만 이루어지면, 습득할 수 있는 스킬이다.



>*시행 착오*
>
>처음에 둘째 사항을 캐치하지 못했다. 그랬더니 테스트 4/14라는 처참한 결과가...





## 풀이 코드

* `skills` : 가능한 선행트리 순서 조합.
* `st_skill` : 주어진 스킬트리 중 선행 스킬이 필요한 스킬들만 모아 놓은 문자열.

```python
def solution(skill, skill_trees):
    skills = [skill[0:i] for i in range(1, len(skill)+1)]
    
    answer = 0
    
    for st in skill_trees:
        st_skill = ""
        for s in st:
            if s in skill:
                st_skill += s
    
    if (st_skill in skills) or st_skill == "":
        answer += 1
    
    return answer
```





---





# 3. 다른 풀이



[풀이 출처](https://programmers.co.kr/learn/courses/30/lessons/49993/solution_groups?language=python3)

```python
def solution(skill, skill_trees):
    answer = 0
    
    for skills in skill_trees:
        skill_list = list(skill)
        
        for s in skills:
            if s in skill:
                if s != skill_list.pop(0):
                    break
    
    else:
        answer += 1
    
    return answer
```

* `for ~ else` 문을 사용했다.
* 선행 스킬 트리에 있는 스킬이 주어진 skill의 첫 번째와 일치하는지 판단하도록 했다.
  * 이를 위해 skill을 list로 만들었다.
  * 첫 번째와 일치하는지 판단하기 위해 `.pop(0)`을 사용한다.
  * 첫 번째와 일치하지 않아진다면, 즉시 `for` 문을 빠져 나온다.



```python
def solution(skill, skill_tree):
    answer = 0
    for i in skill_tree:
        skilllist=''
        for z in i:
            if z in skill:
                skilllist += z
        if skilllist == skill[0:len(skilllist)]:
            answer += 1
    return answer
```

* 내 방법과 유사하지만, 다음의 방법에서 다르다.
  * 전체 스킬트리 경우의 수를 구하지 않아도 된다.
  * 선행 스킬 순서에 속한 스킬 문자열의 개수만큼 slicing하여 비교한다.(*왜 나는 이걸 생각하지 못했나...*)



---



# 4. 배운 점, 더 생각해볼 점

* `for ~ else` : 중간에 끊기지 않고 끝까지 for문이 돌면, else가 실행된다.
* `pop(0)`으로 비교하는 구현이 간단하다. 시간복잡도, 효율성은 어떨지 궁금하다.
