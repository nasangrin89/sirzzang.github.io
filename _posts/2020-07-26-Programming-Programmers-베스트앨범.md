---
title: "[Programmers] 베스트앨범"
excerpt: 3일 1문제-11
toc: false
categories:
  - Programming
tags:
  - Python
  - Programming
  - Programmers
  - 해시
last_modified_at: 2020-07-26
---





> [문제 출처](https://programmers.co.kr/learn/courses/30/lessons/42579)



# 1. 문제



스트리밍 사이트에서 장르 별로 가장 많이 재생된 노래를 두 개씩 모아 베스트 앨범을 출시하려 합니다. 노래는 고유 번호로 구분하며, 노래를 수록하는 기준은 다음과 같습니다.

1. 속한 노래가 많이 재생된 장르를 먼저 수록합니다.
2. 장르 내에서 많이 재생된 노래를 먼저 수록합니다.
3. 장르 내에서 재생 횟수가 같은 노래 중에서는 고유 번호가 낮은 노래를 먼저 수록합니다.

노래의 장르를 나타내는 문자열 배열 genres와 노래별 재생 횟수를 나타내는 정수 배열 plays가 주어질 때, 베스트 앨범에 들어갈 노래의 고유 번호를 순서대로 return 하도록 solution 함수를 완성하세요.



##### 제한 사항

- genres[i]는 고유번호가 i인 노래의 장르입니다.
- plays[i]는 고유번호가 i인 노래가 재생된 횟수입니다.
- genres와 plays의 길이는 같으며, 이는 1 이상 10,000 이하입니다.
- 장르 종류는 100개 미만입니다.
- 장르에 속한 곡이 하나라면, 하나의 곡만 선택합니다.
- 모든 장르는 재생된 횟수가 다릅니다.



##### 입출력 예제

| genres                                | plays                      | return       |
| ------------------------------------- | -------------------------- | ------------ |
| [classic, pop, classic, classic, pop] | [500, 600, 150, 800, 2500] | [4, 1, 3, 0] |



##### 입출력 예 설명

classic 장르는 1,450회 재생되었으며, classic 노래는 다음과 같습니다.

- 고유 번호 3: 800회 재생
- 고유 번호 0: 500회 재생
- 고유 번호 2: 150회 재생

pop 장르는 3,100회 재생되었으며, pop 노래는 다음과 같습니다.

- 고유 번호 4: 2,500회 재생
- 고유 번호 1: 600회 재생

따라서 pop 장르의 [4, 1]번 노래를 먼저, classic 장르의 [3, 0]번 노래를 그다음에 수록합니다.

<br>

# 2. 나의 풀이 



## 풀이 방법



 딕셔너리의 키로 장르명을 사용한다. 해당 장르에 해당하는 노래의 플레이 횟수 총합을 value의 맨 앞에 저장하고, 그 뒤로 튜플을 사용해 노래 고유번호와 해당 노래의 플레이 횟수를 쌍으로 저장했다.

 이후 value의 맨 첫 번째 값(해당 장르 플레이 횟수 총합)을 기준으로 딕셔너리의 value를 정렬한 뒤, 해당 노래의 플레이 횟수를 기준으로 큰 2개의 값을 찾는다.

<br>



## 풀이 코드

 ~~풀고 나니까 코드가 너무 길고, 이렇게 푸는 게 맞는 건가 싶었는데 JK가 원래 코드 길어져도 된다고 조언해 줬다. 호호.~~

```python
def solution(genres, plays):
    answer = []
    
    songs = {} # 저장할 딕셔너리
    for i, g in enumerate(genres):
        if g not in songs:
            songs[g] = [plays[i], (i, plays[i])]
        else:
            songs[g][0] += plays[i] # 총 장르 재생 횟수 업데이트.
            songs[g].append((i, plays[i])) # 노래 고유번호, 재생 횟수 업데이트.
            
    # 장르 총 재생 횟수로 정렬.
    songs = sorted(songs.values(), key=lambda x:x[0], reverse=True) 
    
    # 각 장르의 노래 별로 재생 횟수 많은 2개의 고유번호 answer 배열에 추가
    for song in songs:
        for s in sorted(song[1:], key=lambda x:x[1], reverse=True)[:2]:
        	answer.append(s[0])
    
    return answer
```



<br>

# 3. 다른 풀이



[출처](https://programmers.co.kr/learn/courses/30/lessons/42579/solution_groups?language=python3)



```python
def solution(genres, plays):
    answer = []
    d = {e:[] for e in set(genres)} 
    for e in zip(genres, plays, range(len(plays))):
        d[e[0]].append([e[1], e[2]])
    genreSort = sorted(list(d.keys()), key=lambda x: sum(map(lambda y: y[0], d[x])), reverse=True)
    for g in genreSort:
        temp = [e[1] for e in sorted(d[g], key=lambda x: (x[0], -x[1]), reverse=True)]
        answer += temp[:min(len(temp), 2)]
```

 `d`에서 set을 이용해 고유 장르 값을 찾아 초기화한다. 그리고 `zip`에 `range(len(plays))`를 써서 고유 번호까지 한 번에 처리해 `d`의 value에 append했다.

 내가 처음 구현하고 싶었으나 안 된 부분에 집중하자.

*  `lambda` 2개 쓰고 싶으면 처음에 내가 하다가 오류났던 방식처럼 `lambda` 안에 `for`문을 쓸 게 아니라, 위에서처럼 두 개의 `lambda` 표현식을 사용해주어야 한다.
* `d`의 각 요소에 대해, `map` 함수를 이용해 `sum`해줬다. 

 2개까지만 찾아서 answer 배열에 append할 때는 `min`을 이용했다.

 전체적으로 코드 논리 자체는 비슷한 것 같은데, 표현을 어떻게 하느냐의 차이인 것 같기도 하다. (+ lambda를 잘 써야겠다!)

<br>

```python
def solution(genres, plays):
    answer = []
    dic = {}
    album_list = []
    for i in range(len(genres)):
        dic[genres[i]] = dic.get(genres[i], 0) + plays[i]
        album_list.append(album(genres[i], plays[i], i))

    dic = sorted(dic.items(), key=lambda dic:dic[1], reverse=True)
    album_list = sorted(album_list, reverse=True)



    while len(dic) > 0:
        play_genre = dic.pop(0)
        print(play_genre)
        cnt = 0;
        for ab in album_list:
            if play_genre[0] == ab.genre:
                answer.append(ab.track)
                cnt += 1
            if cnt == 2:
                break

    return answer

class album:
    def __init__(self, genre, play, track):
        self.genre = genre
        self.play = play
        self.track = track

    def __lt__(self, other):
        return self.play < other.play
    def __le__(self, other):
        return self.play <= other.play
    def __gt__(self, other):
        return self.play > other.play
    def __ge__(self, other):
        return self.play >= other.play
    def __eq__(self, other):
        return self.play == other.play
    def __ne__(self, other):
        return self.play != other.play
```

<br>

 어떻게 이렇게 풀 수 있는 건지 모르겠지만. 어쨌든. 나중에 꼭! 클래스 이해하고! 다시 봐야지! (…)