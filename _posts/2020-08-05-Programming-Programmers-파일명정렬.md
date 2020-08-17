---
title: "[프로그래머스] 파일명 정렬"
excerpt: 3일 1문제-15
toc: false
categories:
  - Programming
tags:
  - Python
  - Programming
  - 프로그래머스
  - 정렬
  - 문자열
last_modified_at: 2020-08-05
---





> [문제 출처](https://programmers.co.kr/learn/courses/30/lessons/17686#)



# 1. 문제



세 차례의 코딩 테스트와 두 차례의 면접이라는 기나긴 블라인드 공채를 무사히 통과해 카카오에 입사한 무지는 파일 저장소 서버 관리를 맡게 되었다.

저장소 서버에는 프로그램의 과거 버전을 모두 담고 있어, 이름 순으로 정렬된 파일 목록은 보기가 불편했다. 파일을 이름 순으로 정렬하면 나중에 만들어진 ver-10.zip이 ver-9.zip보다 먼저 표시되기 때문이다.

버전 번호 외에도 숫자가 포함된 파일 목록은 여러 면에서 관리하기 불편했다. 예컨대 파일 목록이 [img12.png, img10.png, img2.png, img1.png]일 경우, 일반적인 정렬은 [img1.png, img10.png, img12.png, img2.png] 순이 되지만, 숫자 순으로 정렬된 [img1.png, img2.png, img10.png, img12.png"] 순이 훨씬 자연스럽다.

무지는 단순한 문자 코드 순이 아닌, 파일명에 포함된 숫자를 반영한 정렬 기능을 저장소 관리 프로그램에 구현하기로 했다.

소스 파일 저장소에 저장된 파일명은 100 글자 이내로, 영문 대소문자, 숫자, 공백(" ), 마침표(.), 빼기 부호(-")만으로 이루어져 있다. 파일명은 영문자로 시작하며, 숫자를 하나 이상 포함하고 있다.

파일명은 크게 HEAD, NUMBER, TAIL의 세 부분으로 구성된다.

- HEAD는 숫자가 아닌 문자로 이루어져 있으며, 최소한 한 글자 이상이다.
- NUMBER는 한 글자에서 최대 다섯 글자 사이의 연속된 숫자로 이루어져 있으며, 앞쪽에 0이 올 수 있다. `0`부터 `99999` 사이의 숫자로, `00000`이나 `0101` 등도 가능하다.
- TAIL은 그 나머지 부분으로, 여기에는 숫자가 다시 나타날 수도 있으며, 아무 글자도 없을 수 있다.

| 파일명             | HEAD  | NUMBER | TAIL         |
| ------------------ | ----- | ------ | ------------ |
| `foo9.txt`         | `foo` | `9`    | `.txt`       |
| `foo010bar020.zip` | `foo` | `010`  | `bar020.zip` |
| `F-15`             | `F-`  | `15`   | (빈 문자열)  |

파일명을 세 부분으로 나눈 후, 다음 기준에 따라 파일명을 정렬한다.

- 파일명은 우선 HEAD 부분을 기준으로 사전 순으로 정렬한다. 이때, 문자열 비교 시 대소문자 구분을 하지 않는다. `MUZI`와 `muzi`, `MuZi`는 정렬 시에 같은 순서로 취급된다.
- 파일명의 HEAD 부분이 대소문자 차이 외에는 같을 경우, NUMBER의 숫자 순으로 정렬한다. 9 < 10 < 0011 < 012 < 13 < 014 순으로 정렬된다. 숫자 앞의 0은 무시되며, 012와 12는 정렬 시에 같은 같은 값으로 처리된다.
- 두 파일의 HEAD 부분과, NUMBER의 숫자도 같을 경우, 원래 입력에 주어진 순서를 유지한다. `MUZI01.zip`과 `muzi1.png`가 입력으로 들어오면, 정렬 후에도 입력 시 주어진 두 파일의 순서가 바뀌어서는 안 된다.

무지를 도와 파일명 정렬 프로그램을 구현하라.



##### 입력 형식

입력으로 배열 `files`가 주어진다.

- `files`는 1000 개 이하의 파일명을 포함하는 문자열 배열이다.
- 각 파일명은 100 글자 이하 길이로, 영문 대소문자, 숫자, 공백(" ), 마침표(.), 빼기 부호(-")만으로 이루어져 있다. 파일명은 영문자로 시작하며, 숫자를 하나 이상 포함하고 있다.
- 중복된 파일명은 없으나, 대소문자나 숫자 앞부분의 0 차이가 있는 경우는 함께 주어질 수 있다. (`muzi1.txt`, `MUZI1.txt`, `muzi001.txt`, `muzi1.TXT`는 함께 입력으로 주어질 수 있다.)



##### 출력 형식

위 기준에 따라 정렬된 배열을 출력한다.



##### 입출력 예제

입력: [img12.png, img10.png, img02.png, img1.png, IMG01.GIF, img2.JPG]
출력: [img1.png, IMG01.GIF, img02.png, img2.JPG, img10.png, img12.png]

입력: [F-5 Freedom Fighter, B-50 Superfortress, A-10 Thunderbolt II, F-14 Tomcat]
출력: [A-10 Thunderbolt II, B-50 Superfortress, F-5 Freedom Fighter, F-14 Tomcat]



<br>



# 2. 나의 풀이 





### 풀이 방법

 문제에서 시키는 대로 문자열을 나누고 정렬해 주면 된다. 문자열을 나누기 위해 `re` 모듈을 활용했다. 나눌 때 주의할 부분은 입력의 두 번째 예시처럼 'foo010bar020.zip'과 같은 문자열이 들어 오는 경우, `NUMBER` 부분에 '010'만 들어 가고, 'bar020'이 들어 가서는 안 된다는 점이다.

 정렬 시에도 `lambda`를 이용해 문제에 제시된 조건을 그대로 구현해 주면 된다. 다만 `두 파일의 HEAD 부분과, NUMBER의 숫자도 같을 경우, 원래 입력에 주어진 순서를 유지한다.`는 조건에 유의해야 하는데, Python의 정렬은 안정적임이 보장되므로([documentation 참고](https://docs.python.org/ko/3/howto/sorting.html)), 크게 유의하지 않고 진행해도 된다.



### 풀이 코드

* `re.findall` : `NUMBER`를 찾기 위해 사용했다. 다만 처음 나오는 숫자만 찾아야 하므로, 인덱싱을 사용해 맨 처음 숫자만 찾아 준다.
* `re.split` : `NUMBER`를 기준으로 나눠주기 위해 사용한다. 다만, 두 번째 예시처럼 `NUMBER` 이후 숫자가 여러 개 있을 경우, 여러 개로 분리된다. 따라서 `maxsplit=1` 옵션을 준다.
* 각각의 파일 명을 `[HEAD, NUMBER, TAIL]` 순으로 구성된 리스트로 만들었다. 정렬한 이후 `join` 메소드를 활용해 다시 문자열로 반환하기 위함이다.

```python
import re

def solution(files):
    answer = []
    for file in files:
        number = re.findall('[0-9]+', file)[0]
        head, tail = re.split('[0-9]+', file, maxsplit=1)
        answer.append([head, number, tail])
    answer.sort(key=lambda x: (x[0].lower(), int(x[1]))) # 정렬
    for i in range(len(answer)): # 문자열로 만들어 주기
        answer[i] = "".join(answer[i])
    return answer
```

<br>



<br>

# 3. 다른 풀이



[출처](https://programmers.co.kr/learn/courses/30/lessons/17686/solution_groups?language=python3)



 `lambda`에 `re.split`, `re.findall`을 넣자.

```python
import re

def solution(files):
    a = sorted(files, key=lambda file : int(re.findall('\d+', file)[0]))
    b = sorted(a, key=lambda file : re.split('\d+', file.lower())[0])
    return b
```



