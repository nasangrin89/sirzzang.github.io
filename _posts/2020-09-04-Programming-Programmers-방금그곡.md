---
title: "[프로그래머스] 방금 그 곡"
excerpt: 3일 1문제-22
toc: false
categories:
  - Programming
tags:
  - Python
  - Programming
  - 프로그래머스
last_modified_at: 2020-09-03
---





> [문제 출처](https://programmers.co.kr/learn/courses/30/lessons/17683)



# 1. 문제



라디오를 자주 듣는 네오는 라디오에서 방금 나왔던 음악이 무슨 음악인지 궁금해질 때가 많다. 그럴 때 네오는 다음 포털의 '방금그곡' 서비스를 이용하곤 한다. 방금그곡에서는 TV, 라디오 등에서 나온 음악에 관해 제목 등의 정보를 제공하는 서비스이다.

네오는 자신이 기억한 멜로디를 가지고 방금그곡을 이용해 음악을 찾는다. 그런데 라디오 방송에서는 한 음악을 반복해서 재생할 때도 있어서 네오가 기억하고 있는 멜로디는 음악 끝부분과 처음 부분이 이어서 재생된 멜로디일 수도 있다. 반대로, 한 음악을 중간에 끊을 경우 원본 음악에는 네오가 기억한 멜로디가 들어있다 해도 그 곡이 네오가 들은 곡이 아닐 수도 있다. 그렇기 때문에 네오는 기억한 멜로디를 재생 시간과 제공된 악보를 직접 보면서 비교하려고 한다. 다음과 같은 가정을 할 때 네오가 찾으려는 음악의 제목을 구하여라.

- 방금그곡 서비스에서는 음악 제목, 재생이 시작되고 끝난 시각, 악보를 제공한다.
- 네오가 기억한 멜로디와 악보에 사용되는 음은 C, C#, D, D#, E, F, F#, G, G#, A, A#, B 12개이다.
- 각 음은 1분에 1개씩 재생된다. 음악은 반드시 처음부터 재생되며 음악 길이보다 재생된 시간이 길 때는 음악이 끊김 없이 처음부터 반복해서 재생된다. 음악 길이보다 재생된 시간이 짧을 때는 처음부터 재생 시간만큼만 재생된다.
- 음악이 00:00를 넘겨서까지 재생되는 일은 없다.
- 조건이 일치하는 음악이 여러 개일 때에는 라디오에서 재생된 시간이 제일 긴 음악 제목을 반환한다. 재생된 시간도 같을 경우 먼저 입력된 음악 제목을 반환한다.
- 조건이 일치하는 음악이 없을 때에는 `(None)`을 반환한다.



##### 입력 형식

입력으로 네오가 기억한 멜로디를 담은 문자열 `m`과 방송된 곡의 정보를 담고 있는 배열 `musicinfos`가 주어진다.

- `m`은 음 `1`개 이상 `1439`개 이하로 구성되어 있다.
- `musicinfos`는 100개 이하의 곡 정보를 담고 있는 배열로, 각각의 곡 정보는 음악이 시작한 시각, 끝난 시각, 음악 제목, 악보 정보가 `,`로 구분된 문자열이다.
  - 음악의 시작 시각과 끝난 시각은 24시간 `HH:MM` 형식이다.
  - 음악 제목은 '`,`' 이외의 출력 가능한 문자로 표현된 길이 `1` 이상 `64` 이하의 문자열이다.
  - 악보 정보는 음 `1`개 이상 `1439`개 이하로 구성되어 있다.



##### 출력 형식

조건과 일치하는 음악 제목을 출력한다.



##### 입출력 예시

| m                | musicinfos                                             | answer |
| ---------------- | ------------------------------------------------------ | ------ |
| ABCDEFG          | [12:00,12:14,HELLO,CDEFGAB, 13:00,13:05,WORLD,ABCDEF]  | HELLO  |
| CC#BCC#BCC#BCC#B | [03:00,03:30,FOO,CC#B, 04:00,04:08,BAR,CC#BCC#BCC#B]   | FOO    |
| ABC              | [12:00,12:14,HELLO,C#DEFGAB, 13:00,13:05,WORLD,ABCDEF] | WORLD  |



##### 입출력 예 설명

첫 번째 예시에서 HELLO는 길이가 7분이지만 12:00부터 12:14까지 재생되었으므로 실제로 CDEFGABCDEFGAB로 재생되었고, 이 중에 기억한 멜로디인 ABCDEFG가 들어있다.
세 번째 예시에서 HELLO는 C#DEFGABC#DEFGAB로, WORLD는 ABCDE로 재생되었다. HELLO 안에 있는 ABC#은 기억한 멜로디인 ABC와 일치하지 않고, WORLD 안에 있는 ABC가 기억한 멜로디와 일치한다.

<br>

# 2. 나의 풀이 





### 풀이 방법

 언급된 조건 중 주의해야 할 것은 다음의 두 가지이다.

* 조건이 일치하는 음악이 여러 개일 때에는 라디오에서 재생된 시간이 제일 긴 음악 제목을 반환한다. 
* 재생된 시간도 같을 경우 먼저 입력된 음악 제목을 반환한다.

 위의 조건을 고려하기 위해, 입력된 음악 정보 `musicinfos`를 안의 문자열을 음악 재생 시간 순으로 정렬했다. 재생된 시간이 가장 긴 음악 제목만 고려하기 위함이며, 파이썬의 정렬은 안정 정렬이므로 재생된 시간이 같다면 어차피 먼저 입력된 제목이 반환될 것이다.

 언급되지 않은 조건 중 주의해야 할 것은 음계에 `#`이 들어갔는지 여부이다. `#`이 들어가면 다른 음이고, 이를 고려하여 각 음악의 멜로디에서 `#`이 들어있을 때 다른 음으로 바꿔주어야 한다.

 `(None)`을 반환한다는 것은 `return None`이 아니라, `return 'None'`으로, 말 그대로 문자열 'None'을 반환한다는 의미이다(… ~~*테스트 케이스 4개만 계속 틀려서 한 동안 고생했다*~~) .

<br>



### 풀이 코드

 기능 별로 나눠서 구현했다.

* `get_duration`: 음악 재생 시간을 분 단위로 구한다.
* `change_melody` : 멜로디에서 `#`이 들어 있는 음계를 바꾼다.
  * 각 멜로디의 음계를 stack에 push한다.
  * push해야 할 음계가 `#`이라면, 스택의 top을 소문자로 바꾸어, 다른 음으로 간주하도록 한다.
* `solution`
  * 음악 재생 시간 순으로 각 음악 정보가 들어 있는 문자열을 정렬한다.
  * 가장 재생 시간이 긴 음악부터 `m`이 들어 있는지 검사한다.
    * 네오가 들은 음악 `m`의 음계를 바꾼다.
    * 음악 재생 시간과 길이를 이용해 원래 음악이 어디까지 재생되었는지 구한다. 물론 음계를 바꿔 준다.
    * 네오가 들은 음악 `m`이 있다면, 그 음악의 제목을 반환한다.
  * 모든 음악을 검사했는데도 `m`이 없다면, `'(None)'`을 반환한다.



```python
from datetime import datetime
# 음악 재생 시간(분) 구하기
def get_duration(start_time, end_time):
    return int((datetime.strptime(end_time, '%H:%M')-datetime.strptime(start_time, '%H:%M')).total_seconds() / 60)

# 멜로디에서 '#'이 붙은 음계 바꾸기
def change_melody(melody):
    melody = list(melody)    
    stack = []
    for m in melody:
        if m == '#':
            stack[-1] = stack[-1].lower()
        else:
            stack.append(m)
    return ''.join(stack)

def solution(m, musicinfos):
    # 음악 재생 시간에 따라 정렬
    musicinfos.sort(reverse=True, key=lambda x:get_duration(x.split(',')[0], x.split(',')[1]))
    for musicinfo in musicinfos:
        start = musicinfo.split(',')[0]
        end = musicinfo.split(',')[1]
        title = musicinfo.split(',')[2]
        melody = change_melody(musicinfo.split(',')[3])
        q, r = divmod(get_duration(start, end), len(melody))
        music = melody*q + melody[:r] # 총 재생된 음악 멜로디
        if change_melody(m) in music:
            return title
    return '(None)' 
```

 풀긴 했는데, 실행 시간이 그닥 마음에 들지 않는다.

<br>

# 3. 다른 풀이



[출처](https://programmers.co.kr/learn/courses/30/lessons/17683/solution_groups?language=python3)

```python
import io


def convert_sharp(music):
    music = music.replace('C#', 'c')
    music = music.replace('D#', 'd')
    music = music.replace('F#', 'f')
    music = music.replace('G#', 'g')
    music = music.replace('A#', 'a')
    return music


def solution(m, musicinfos):
    m = convert_sharp(m)
    longest_play_time = 0
    that_title = '(None)'
    for info in musicinfos:
        begin, end, title, music = info.split(',')
        music = convert_sharp(music)
        begin = int(begin[:2])*60 + int(begin[3:])
        end = int(end[:2])*60 + int(end[3:])
        length = end - begin
        if length < len(m):
            continue
        played_music = io.StringIO()
        for i in range(length):
            played_music.write(music[i % len(music)])
        played_music = played_music.getvalue()

        index = played_music.find(m)
        if index == -1:
            continue

        if longest_play_time < length:
            longest_play_time = length
            that_title = title

    return that_title
```

  ~~(*왜 항상 나는 돌아가는 듯한 느낌일까*)~~

* 굳이 스택 사용하지 않고, 문자열을 바로 바꿔주면 되는 것이었다!
* 음악 재생 시간 굳이 `datetime` 이용하지 않아도, 정직하게 구하면 된다.

 네오가 들은 음악의 길이가 더 길 경우에는 검사하지 않는다. 문자열 입출력, dictionary 구조 사용, `longest_play_time` 변수 사용한 것을 배우자. **실행 시간이 훨씬 빠르다**. ~~돌아가지 말자 제발~~.

