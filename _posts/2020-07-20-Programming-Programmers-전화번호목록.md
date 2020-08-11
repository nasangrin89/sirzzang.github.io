---
title: "[프로그래머스] 전화번호 목록"
excerpt: 3일 1문제-8
toc: false
categories:
  - Programming
tags:
  - Python
  - Programming
  - 프로그래머스
  - 해시
last_modified_at: 2020-07-20
---





> [문제 출처](https://programmers.co.kr/learn/courses/30/lessons/42577)



# 1. 문제



전화번호부에 적힌 전화번호 중, 한 번호가 다른 번호의 접두어인 경우가 있는지 확인하려 합니다.
전화번호가 다음과 같을 경우, 구조대 전화번호는 영석이의 전화번호의 접두사입니다.

- 구조대 : 119
- 박준영 : 97 674 223
- 지영석 : 11 9552 4421

전화번호부에 적힌 전화번호를 담은 배열 phone_book 이 solution 함수의 매개변수로 주어질 때, 어떤 번호가 다른 번호의 접두어인 경우가 있으면 false를 그렇지 않으면 true를 return 하도록 solution 함수를 작성해주세요.



##### 제한 사항

- phone_book의 길이는 1 이상 1,000,000 이하입니다.
- 각 전화번호의 길이는 1 이상 20 이하입니다.



##### 입출력 예제

| phone_book                  | return |
| --------------------------- | ------ |
| [119, 97674223, 1195524421] | false  |
| [123,456,789]               | true   |
| [12,123,1235,567,88]        | false  |



##### 입출력 예 설명

입출력 예 #1
앞에서 설명한 예와 같습니다.

입출력 예 #2
한 번호가 다른 번호의 접두사인 경우가 없으므로, 답은 true입니다.

입출력 예 #3
첫 번째 전화번호, “12”가 두 번째 전화번호 “123”의 접두사입니다. 따라서 답은 false입니다.

<br>

# 2. 나의 풀이 



## 풀이 방법



 주어진 `phone_book` 리스트에서 전화번호를 하나씩 선택해 다른 전화번호의 접두사인지 확인한다. **맨 앞에 나오는 경우만** 체크해야 한다. 그렇지 않으면 몇 개의 테스트 케이스를 통과하지 못한다.



## 풀이 코드



```python
def solution(phone_book):
    for i in range(len(phone_book)):
        flag = phone_book[i] # 검사할 전화번호
        for v in phone_book[:i] + phone_book[i+1:]:
            if flag in v and v.find(flag) == 0: # 맨 앞에 나오는 경우만 인덱스 체크해서 검사
                return False
    return True
```







<br>

# 3. 다른 풀이



[출처](https://programmers.co.kr/learn/courses/30/lessons/42577/solution_groups?language=python3)

```python
def solution(phoneBook):
    phoneBook = sorted(phoneBook)

    for p1, p2 in zip(phoneBook, phoneBook[1:]):
        if p2.startswith(p1):
            return False
    return True
```



* 어차피 `phoneBook`이 string으로 이루어졌으므로 앞의 글자 순서대로 정렬하면 된다. 
* `zip` 사용해서 하나씩 체크했다.
* `startswith` 사용해서 접두사인지 검사한다.

<br>

```python
def solution(phone_book):
    answer = True
    hash_map = {}
    for phone_number in phone_book:
        hash_map[phone_number] = 1
    for phone_number in phone_book:
        temp = ""
        for number in phone_number:
            temp += number
            if temp in hash_map and temp != phone_number:
                answer = False
    return answer
```



 파이썬에서 해쉬로 구현해서 풀려면 이렇게 풀어야 하는 듯하다. 딕셔너리 안에 `key`, `value` 로 무엇을 설정해주어야 하는지 잘 파악하자. 나는 `key`로 검사할 전화번호, `value`로 남은 전화번호를 놓고, `key`가 `value` 안에 있는지 검사하려고 하다 보니 시간 초과가 났다.