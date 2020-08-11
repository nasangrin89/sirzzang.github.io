---
title: "[프로그래머스] H-Index"
excerpt: 3일 1문제-6
toc: false
categories:
  - Programming
tags:
  - Python
  - Programming
  - 프로그래머스
  - 정렬
---





> [문제 출처](https://programmers.co.kr/learn/courses/30/lessons/42747)



# 1. 문제



H-Index는 과학자의 생산성과 영향력을 나타내는 지표입니다. 어느 과학자의 H-Index를 나타내는 값인 h를 구하려고 합니다. 위키백과[1](https://programmers.co.kr/learn/courses/30/lessons/42747#fn1)에 따르면, H-Index는 다음과 같이 구합니다.

어떤 과학자가 발표한 논문 `n`편 중, `h`번 이상 인용된 논문이 `h`편 이상이고 나머지 논문이 h번 이하 인용되었다면 `h`의 최댓값이 이 과학자의 H-Index입니다.

어떤 과학자가 발표한 논문의 인용 횟수를 담은 배열 citations가 매개변수로 주어질 때, 이 과학자의 H-Index를 return 하도록 solution 함수를 작성해주세요.



##### 제한사항

- 과학자가 발표한 논문의 수는 1편 이상 1,000편 이하입니다.
- 논문별 인용 횟수는 0회 이상 10,000회 이하입니다.



**입출력 예**

| citations       | return |
| --------------- | ------ |
| [3, 0, 6, 1, 5] | 3      |



##### 입출력 예 설명

이 과학자가 발표한 논문의 수는 5편이고, 그중 3편의 논문은 3회 이상 인용되었습니다. 그리고 나머지 2편의 논문은 3회 이하 인용되었기 때문에 이 과학자의 H-Index는 3입니다.

※ 공지 - 2019년 2월 28일 테스트 케이스가 추가되었습니다.





---



# 2. 나의 풀이 



## 2.1. 1일차 : 정확성 테스트 실패



### 풀이 방법



 너무 단순하게 생각했다. 큰 순서대로 정렬한 뒤 `enumerate`를 사용하면, 해당 `index`까지가 `value` 번 이상 인용된 논문이라고 생각했다. 그러나 2개만 맞은 처참한 결과가 나왔다.



### 풀이 코드



```python
def solution(citations):
    answer = 0
    citations.sort()
    # print(citations)
    for i, v in enumerate(citations):
        if len(citations)-i == v:
            answer = v
	 return answer
```





### 더 생각해 볼 점



* 애초에 문제를 잘못 이해했다. 생각하지 못한 테스트 케이스가 너무 많다. 
* 내일 다음과 같은 테스트 케이스에 대해 생각해 봐야겠다.



## 2.2. 2일차



### 풀이 방법



* `citations` 안에 들어 있지 않더라도 정답이 될 수 있다.
* 경우의 수가 좀 많은 것 같아서, 생각하다가 `에라 모르겠다` 하는 마음으로 완전 탐색을 구현했다.
  * 정렬한 뒤, `h`의 초기값을 첫째 값으로 잡았다.
  * 이후 `h`를 1씩 빼면서, 문제 정의에 충실하게 `citations` 배열 안에 남아 있는 원소 중 `h` 이상인 것의 개수를 셌다. 
  * 인용 횟수가 `h` 이상인 `h_cnt`가 `h` 이상이면 반복문 순회를 멈추도록 했다.
* 다만 모든 논문 횟수가 똑같은 경우는 예외이므로, 다음과 같이 처리했다.
  * 모든 논문 인용 횟수가 논문 개수보다 많다면, 논문 개수가 H-Index이다.
  * 그렇지 않다면, 논문 인용 횟수가 H-Index이다.



### 풀이 코드

```python
def solution(citations):
    answer = 0
    if len(set(citations)) == 1:
        if citations[-1] > len(citations):
            answer = len(citations)
        else:
            answer = citations[-1]
    else:
        citations.sort(reverse=True) # 인용횟수 많은 순 정렬
        h = citations.pop(0) # 초기
        while h > 0:
            h -= 1            
            h_cnt = 1 # 초기
            for c in citations:
                if c >= h:
                    h_cnt += 1
            if h_cnt >= h:
                answer = h
                break                
    return answer
```



 정렬, `pop`, `while` 안에 `for` 등 시간이 매우 오래 걸릴 것이라고 생각은 했으나, 막상 눈으로 확인하니 *이거 실화인가* 하게 된다.



![image-20200716230310893]({{site.url}}/assets/images/image-20200716230310893.png){: width="500" height="300"}{: .align-center}

<center><sup>진짜 시간 실화인가..</sup></center>

<br>

 JK한테 C++ 풀이 받아 보고 다시 생각해 봐야 겠다. 아무리 생각해도 언어의 문제가 아니라 내 머리가.. ㅎ;;



---

# 3. 다른 풀이



[출처](https://programmers.co.kr/learn/courses/30/lessons/42747/solution_groups?language=python3)



```python
def solution(citations):
    citations = sorted(citations)
    l = len(citations)
    for i in range(l):
        if citations[i] >= l-i:
            return l-i
    return 0
```



 굳이 while문 for문 안 쓰고 이렇게 하면 되는 것이었다.(...) 





*출처: JK*



 JK가 `C++` 로 푼 걸 파이썬으로 옮기기만 했는데, 시간이 매우 빠르다. 내 로직의 문제였다 ㅎ...

```python
def solution(citations):
    answer = 0
    citations.sort()
    i = 0
    h = 0
    while i < len(citations):
        if citations[i] == h:
            if len(citations) - i >= h:
                answer = h
                h += 1
                i += 1
            else:
                break
        elif citations[i] < h:
            i += 1
            continue
        else:
            if len(citations) - i >= h:
                answer = h
                h += 1
            else:
                break            
    return answer
```



* 오름차순 정렬
* `h` : 논문 인용 횟수 체크. `i` : citations 배열에서 인덱스로 움직이면서 비교.
* 경우의 수 3가지
  * 논문 인용 횟수가 현재 `h`보다 작다면, 계속해서 비교.
  * 논문 인용 횟수가 현재 `h`와 같다면,
    * 마지막까지 검사하지 않았다면 `h` 늘려서 검사하고 `i`도 늘리고,
    * 마지막까지 검사 완료했으면 검사 중단.
  * 논문 인용 횟수가 현재 `h`보다 크다면,
    * 마지막까지 검사하지 않았을 때는 그 인덱스 그대로 놓고 `h`만 늘리고,
    * 마지막까지 검사 완료했으면 검사 중단.