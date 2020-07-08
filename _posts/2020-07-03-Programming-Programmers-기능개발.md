---
title: "[Programmers] 기능 개발"
excerpt: 3일 1문제-2
toc: false
categories:
  - Programming
tags:
  - Python
  - Programming
  - Programmers
---





> [문제 출처](https://programmers.co.kr/learn/courses/30/lessons/42586)



# 1. 문제



프로그래머스 팀에서는 기능 개선 작업을 수행 중입니다. 각 기능은 진도가 100%일 때 서비스에 반영할 수 있습니다.

또, 각 기능의 개발속도는 모두 다르기 때문에 뒤에 있는 기능이 앞에 있는 기능보다 먼저 개발될 수 있고, 이때 뒤에 있는 기능은 앞에 있는 기능이 배포될 때 함께 배포됩니다.

먼저 배포되어야 하는 순서대로 작업의 진도가 적힌 정수 배열 progresses와 각 작업의 개발 속도가 적힌 정수 배열 speeds가 주어질 때 각 배포마다 몇 개의 기능이 배포되는지를 return 하도록 solution 함수를 완성하세요.



**제한사항**

- 작업의 개수(progresses, speeds배열의 길이)는 100개 이하입니다.
- 작업 진도는 100 미만의 자연수입니다.
- 작업 속도는 100 이하의 자연수입니다.
- 배포는 하루에 한 번만 할 수 있으며, 하루의 끝에 이루어진다고 가정합니다. 예를 들어 진도율이 95%인 작업의 개발 속도가 하루에 4%라면 배포는 2일 뒤에 이루어집니다.



**입출력 예**

| progresses | speeds   | return |
| ---------- | -------- | ------ |
| [93,30,55] | [1,30,5] | [2,1]  |



**입출력 예 설명**

첫 번째 기능은 93% 완료되어 있고 하루에 1%씩 작업이 가능하므로 7일간 작업 후 배포가 가능합니다.
두 번째 기능은 30%가 완료되어 있고 하루에 30%씩 작업이 가능하므로 3일간 작업 후 배포가 가능합니다. 하지만 이전 첫 번째 기능이 아직 완성된 상태가 아니기 때문에 첫 번째 기능이 배포되는 7일째 배포됩니다.
세 번째 기능은 55%가 완료되어 있고 하루에 5%씩 작업이 가능하므로 9일간 작업 후 배포가 가능합니다.

따라서 7일째에 2개의 기능, 9일째에 1개의 기능이 배포됩니다.



---



# 2. 나의 풀이 



## 풀이 방법



 문제에서 하라는 대로 직관적으로 풀었다. `progresses` 배열을 100까지 완료하기까지 남은 기간으로 바꿔 주었다. 이 과정에서 남은 작업량이 하루에 가능한 작업량의 배수가 아니라면 하루가 더 필요하게 되므로, 경우를 나누어 주었다. 



> *참고*
>
>  `math.ceil`을 사용하면 위의 코드가 더 간단해진다.



 이렇게 남은 기간으로 바꾸고 난 뒤, `max_day`라는 변수를 사용했다. 해당 기능이 배포되기까지 필요한 최대 시간을 나타낸다. 이 변수의 초기값은 `progresses` 배열의 초기값으로 할당했다.  `answer` 배열의 초기값은 1로 초기화한다. 첫 번째 기능이 배포되는 날에는 반드시 한 개의 기능이 배포되기 때문이다. 

 이후 `progresses` 배열을 두 번째 값부터 마지막까지 순회한다. 기능이 배포되기까지 남은 기간을 나타내기 때문에, 해당 값이 `max_day`보다 작다면 `answer` 배열의 top 값에 1을 더해 주었다. `max_day`보다 더 많은 기간이 필요하다면 `max_day`를 해당 값으로 바꿔 주고, `answer` 배열에 1을 append한다. 







## 풀이 코드



```python
def solution(p, s):
    # 남은 기간으로 바꿔 주기
    for i in range(len(p)):
        if (100-p[i]) % s[i] == 0:
            p[i] = (100-p[i]) // s[i]
        else:
            p[i] = (100-p[i]) // s[i]
            p[i] += 1
    
    # 작업 기간 비교
    max_day = p[0]
    answer = [1]
    for i in range(len(p)):
        if p[i] <= max_day:
            answer[-1] += 1
        else:
            max_day = p[i]
            answer.append(1)
    
    return answer
```





---



# 3. 다른 풀이



출처: https://programmers.co.kr/learn/courses/30/lessons/42586/solution_groups?language=python3



```python
def solution(progresses, speeds):
    Q = []
    for p, s in zip(progresses, speeds):
        if len(Q) == 0 or q[-1][0] < -((p-100)//s):
            Q.append([-((p-100)//s), 1])
        else:
            Q[-1][1] += 1
    return [q[1] for q in Q]
```



 내가 나눠서 생각했던 단계를 한 번에 진행했다.

* 남은 시간 구하는 방법
  * `i` 사용하지 않고 `zip` 사용하면 된다.
  * `p-100`으로 음수를 만들어 주면 된다.
* `answer` 배열 구하는 방버
  * 초기값 설정하지 않고 `len(Q) == 0`을 이용하면 된다.
  * `Q` 안에 남은 기간과 해당 기간 동안 배포되는 기능의 수를 함께 리스트로 저장했고, 업데이트해야 하는 경우 리스트의 두 번째 요소만 업데이트한다.



---



```python
def solution(progresses, speeds):
    answer = []
    time = 0
    count = 0
    while len(progresses) > 0:
        if (progresses[0] + time*speeds[0]) >= 100:
            progresses.pop(0)
            speeds.pop(0)
            count += 1
        else:
            if count > 0:
                answer.append(count)
                count = 0
            time += 1
    answer.append(count)
```



 직관적으로 문제를 간결하게 잘 구현한 풀이 같다.

* `progresses` 배열의 맨 앞 원소를 빼 나가면서 비교해주는 방식이다.
* 기능 개발이 완료되었는지 여부를 `(progresses[0] + time*speeds[0]) >= 0`으로 검사했다.
* 내 코드에서의 `max_day`를 `time`으로 이해하고, 그 `time`까지 개발된 기능의 수를 `count`로 이해하면 된다.



