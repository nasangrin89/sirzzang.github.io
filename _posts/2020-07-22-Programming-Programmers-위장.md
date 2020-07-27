---
title: "[Programmers] 위장"
excerpt: 3일 1문제-9
toc: false
categories:
  - Programming
tags:
  - Python
  - Programming
  - Programmers
  - 해시
last_modified_at: 2020-07-22
---





> [문제 출처](https://programmers.co.kr/learn/courses/30/lessons/42578)



# 1. 문제



스파이들은 매일 다른 옷을 조합하여 입어 자신을 위장합니다.

예를 들어 스파이가 가진 옷이 아래와 같고 오늘 스파이가 동그란 안경, 긴 코트, 파란색 티셔츠를 입었다면 다음날은 청바지를 추가로 입거나 동그란 안경 대신 검정 선글라스를 착용하거나 해야 합니다.

| 종류 | 이름                       |
| ---- | -------------------------- |
| 얼굴 | 동그란 안경, 검정 선글라스 |
| 상의 | 파란색 티셔츠              |
| 하의 | 청바지                     |
| 겉옷 | 긴 코트                    |

스파이가 가진 의상들이 담긴 2차원 배열 clothes가 주어질 때 서로 다른 옷의 조합의 수를 return 하도록 solution 함수를 작성해주세요.



##### 제한 사항

- clothes의 각 행은 [의상의 이름, 의상의 종류]로 이루어져 있습니다.
- 스파이가 가진 의상의 수는 1개 이상 30개 이하입니다.
- 같은 이름을 가진 의상은 존재하지 않습니다.
- clothes의 모든 원소는 문자열로 이루어져 있습니다.
- 모든 문자열의 길이는 1 이상 20 이하인 자연수이고 알파벳 소문자 또는 '_' 로만 이루어져 있습니다.
- 스파이는 하루에 최소 한 개의 의상은 입습니다.



##### 입출력 예제

| clothes                                                      | return |
| ------------------------------------------------------------ | ------ |
| [[yellow_hat, headgear], [blue_sunglasses, eyewear], [green_turban, headgear]] | 5      |
| [[crow_mask, face], [blue_sunglasses, face], [smoky_makeup, face]] | 3      |





##### 입출력 예 설명

예제 #1
headgear에 해당하는 의상이 yellow_hat, green_turban이고 eyewear에 해당하는 의상이 blue_sunglasses이므로 아래와 같이 5개의 조합이 가능합니다.

```
1. yellow_hat
2. blue_sunglasses
3. green_turban
4. yellow_hat + blue_sunglasses
5. green_turban + blue_sunglasses
```

예제 #2
face에 해당하는 의상이 crow_mask, blue_sunglasses, smoky_makeup이므로 아래와 같이 3개의 조합이 가능합니다.

```
1. crow_mask
2. blue_sunglasses
3. smoky_makeup
```

<br>

# 2. 나의 풀이 



## 풀이 방법



 주어진 `clothes`에 의상 이름과 의상 종류의 리스트가 들어 있다. 따라서 해당 리스트의 두 번째 요소를 key로, 의상 이름들의 list를 value로 갖는 dictionary를 만든다. 의상 이름을 key에 맞는 dictionary의 value에 저장한다.

 `answer`는 각 경우의 수를 모두 구해 주면 된다. 다만 제한 사항에 나오는 것처럼 의상을 아예 안 입는 경우는 있을 수 없으므로, 1을 빼 준다.

<br>



## 풀이 코드



```python
def solution(clothes):
    # 의상 dictionary
    outfit = {}
    for c in clothes:
        if c[1] in outfit:
            outfit[c[1]].append(c[0])
        else:
            outfit[c[1]] = [c[0]]
            
    # 모든 가능한 조합 구하기
    answer = 1
    for v in outfit.values():
        answer *= (len(v) + 1)
    return answer - 1
```



<br>

# 3. 다른 풀이



[출처](https://programmers.co.kr/learn/courses/30/lessons/42578/solution_groups?language=python3)



 아래와 같이 `reduce` 함수를 이용해 푼 경우가 많았다.

```python
from collections import Counter
from functools import reduce

def solution(clothes):
    cnt = Counter([kind for name, kind in clothes])
    answer = reduce(lambda x, y: x*(y+1), cnt.values(), 1) - 1
    return answer
```



<br>

```python
def solution(clothes):
    clothes_type = {}

    for c, t in clothes:
        if t not in clothes_type:
            clothes_type[t] = 2
        else:
            clothes_type[t] += 1

    cnt = 1
    for num in clothes_type.values():
        cnt *= num

    return cnt - 1
```



 dictionary의 value로 숫자를 저장한 것이 인상적이다. 나처럼 `len`을 사용하지 않아도 되고, 더 효율적일 것 같다. 특히 초기값을 2로 해 저장했다. 특히 경우의 수를 구할 때 1을 더해주지 않아도 되는 차이가 있다.