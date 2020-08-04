---
title: "[Programmers] 완주하지 못한 선수"
excerpt: 3일 1문제-10
toc: false
categories:
  - Programming
tags:
  - Python
  - Programming
  - Programmers
  - 해시
last_modified_at: 2020-07-24
---





> [문제 출처](https://programmers.co.kr/learn/courses/30/lessons/42576)



# 1. 문제



수많은 마라톤 선수들이 마라톤에 참여하였습니다. 단 한 명의 선수를 제외하고는 모든 선수가 마라톤을 완주하였습니다.

마라톤에 참여한 선수들의 이름이 담긴 배열 participant와 완주한 선수들의 이름이 담긴 배열 completion이 주어질 때, 완주하지 못한 선수의 이름을 return 하도록 solution 함수를 작성해주세요.



##### 제한 사항

- 마라톤 경기에 참여한 선수의 수는 1명 이상 100,000명 이하입니다.
- completion의 길이는 participant의 길이보다 1 작습니다.
- 참가자의 이름은 1개 이상 20개 이하의 알파벳 소문자로 이루어져 있습니다.
- 참가자 중에는 동명이인이 있을 수 있습니다.



##### 입출력 예제

| participant                             | completion                       | return |
| --------------------------------------- | -------------------------------- | ------ |
| [leo, kiki, eden]                       | [eden, kiki]                     | leo    |
| [marina, josipa, nikola, vinko, filipa] | [josipa, filipa, marina, nikola] | vinko  |
| [mislav, stanko, mislav, ana]           | [stanko, ana, mislav]            | mislav |



##### 입출력 예 설명

예제 #1
leo는 참여자 명단에는 있지만, 완주자 명단에는 없기 때문에 완주하지 못했습니다.

예제 #2
vinko는 참여자 명단에는 있지만, 완주자 명단에는 없기 때문에 완주하지 못했습니다.

예제 #3
mislav는 참여자 명단에는 두 명이 있지만, 완주자 명단에는 한 명밖에 없기 때문에 한명은 완주하지 못했습니다.

<br>

# 2. 나의 풀이 



## 풀이 방법



 참여자 명단과 완주자 명단을 정렬한다. 어차피 완주하지 못한 사람은 1명이므로, 완주자 명단을 기준으로 봤을 때 참여자 명단에서와 인덱스가 다르다면, 참여자 명단에서 해당 인덱스에 있는 사람이 완주하지 못한 것이다. 완주자 명단을 모두 순회했는데도 인덱스가 다른 위치를 찾지 못한다면, 마지막 사람이 완주하지 못한 것이다. (~~풀고 나서 다른 사람 풀이 보다가 똑같은 게 너무 많아서 소름 돋았다.~~)

>  근데 이 문제를 어떻게 해쉬로 푸는 걸까.

<br>



## 풀이 코드



 `for ~ else …` 문을 사용하여 `completion`을 전부 다 순회했는지 검사했다.

```python
def solution(participant, completion):
    # 이름 순 정렬
    participant.sort()
    completion.sort()    
    for i in range(len(completion)):
        if completion[i] != participant[i]:
            return participant[i]
    else:
        return participant[-1]
```



<br>

# 3. 다른 풀이



[출처](https://programmers.co.kr/learn/courses/30/lessons/42576/solution_groups?language=python3)



```python
import collections

def solution(participant, completion):
    answer = collections.Counter(participant) - collections.Counter(completion)
    return list(answer.keys())[0]
```

 `Counter`를 사용해서 완주자 명단에 없는 참여자 명단을 알아 냈다. `Counter` 객체는 빼기가 가능하다.

<br>

```python
def solution(participant, completion):
    answer = ''
    temp = 0
    dic = {}
    for part in participant:
        dic[hash(part)] = part
        temp += int(hash(part))
    for com in completion:
        temp -= hash(com)
    answer = dic[temp]
    return answer
```



 진짜 hash 함수를 사용하여 수의 가감을 이용해서 풀었다. 해쉬 자료 구조를 어떻게 활용해야 할지만 생각했는데, 진짜 hash 값을 사용했다는 게 신박하다. 한편으로는 댓글에 해시 충돌 생기면 바로 에러날 수 있을 것 같다는 의견도 있었다!

