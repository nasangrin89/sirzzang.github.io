---
title:  "[백준] BOJ1018 체스판 다시 칠하기"
excerpt:
header:
  teaser: /assets/images/blog-Programming.jpg
categories:
  - Programming
tags:
  - Python
  - Programming
  - BOJ
  - 완전탐색
---





> [문제 출처](https://www.acmicpc.net/problem/1018)





# 문제

지민이는 자신의 저택에서 MN개의 단위 정사각형으로 나누어져 있는 M x N 크기의 보드를 찾았다. 어떤 정사각형은 검은색으로 칠해져 있고, 나머지는 흰색으로 칠해져 있다. 지민이는 이 보드를 잘라서 8 x 8 크기의 체스판으로 만들려고 한다.

체스판은 검은색과 흰색이 번갈아서 칠해져 있어야 한다. 구체적으로, 각 칸이 검은색과 흰색 중 하나로 색칠되어 있고, 변을 공유하는 두 개의 사각형은 다른 색으로 칠해져 있어야 한다. 따라서 이 정의를 따르면 체스판을 색칠하는 경우는 두 가지뿐이다. 하나는 맨 왼쪽 위 칸이 흰색인 경우, 하나는 검은색인 경우이다.

보드가 체스판처럼 칠해져 있다는 보장이 없어서, 지민이는 8 x 8 크기의 체스판으로 잘라낸 후에 몇 개의 정사각형을 다시 칠해야겠다고 생각했다. 당연히 8 x 8 크기는 아무데서나 골라도 된다. 지민이가 다시 칠해야 하는 정사각형의 최소 개수를 구하는 프로그램을 작성하시오.

[입력]

첫째 줄에 N과 M이 주어진다. N과 M은 8보다 크거나 같고, 50보다 작거나 같은 자연수이다. 둘째 줄부터 N개의 줄에는 보드의 각 행의 상태가 주어진다. B는 검은색이며, W는 흰색이다.

[출력]

첫째 줄에 지민이가 다시 칠해야 하는 정사각형 개수의 최솟값을 출력한다.



# 풀이 방법

* 주어진 입력에서, 체스판을 만들 수 있는 모든 경우를 나눈다.

  ![chess]({{site.url}}/assets/images/chess.jpg)

  * 각 경우는 8개의 글자로 이루어져 있다.
  * 가로로 끝까지, 세로로 끝까지 이동시켜가는데, 항상 가로 8개, 세로 8개여야 한다.

  ```python
  (예시)
  ['BBBBBBBB', 'BBBBBBBB', 'BBBBBBBB', 'BBBBBBBB', 'BBBBBBBB', 'BBBBBBBB', 'BBBBBBBB', 'BBBBBBBB', 'BBBBBBBW', 'BBBBBBBB', 'BBBBBBBW', 'BBBBBBBB', 'BBBBBBBW', 'BBBBBBBB', 'BBBBBBBW', 'BBBBBBBB', 'BBBBBBWB', 'BBBBBBBW', 'BBBBBBWB', 'BBBBBBBW', 'BBBBBBWB', 'BBBBBBBW', 'BBBBBBWB', 'BBBBBBBW', 'BBBBBWBW', 'BBBBBBWB', 'BBBBBWBW', 'BBBBBBWB', 'BBBBBWBW', 'BBBBBBWB', 'BBBBBWBW', 'BBBBBBWB', 'BBBBWBWB', 'BBBBBWBW', 'BBBBWBWB', 'BBBBBWBW', 'BBBBWBWB', 'BBBBBWBW', 'BBBBWBWB', 'BBBBBWBW', 'BBBWBWBW', 'BBBBWBWB', 'BBBWBWBW', 'BBBBWBWB', 'BBBWBWBW', 'BBBBWBWB', 'BBBWBWBW', 'BBBBWBWB', 'BBBBBBBB', 'BBBBBBBB', 'BBBBBBBB', 'BBBBBBBB', 'BBBBBBBB', 'BBBBBBBB', 'BBBBBBBB', 'WWWWWWWW', 'BBBBBBBB', 'BBBBBBBW', 'BBBBBBBB', 'BBBBBBBW', 'BBBBBBBB', 'BBBBBBBW', 'BBBBBBBB', 'WWWWWWWW', 'BBBBBBBW', 'BBBBBBWB', 'BBBBBBBW', 'BBBBBBWB', 'BBBBBBBW', 'BBBBBBWB', 'BBBBBBBW', 'WWWWWWWW', 'BBBBBBWB', 'BBBBBWBW', 'BBBBBBWB', 'BBBBBWBW', 'BBBBBBWB', 'BBBBBWBW', 'BBBBBBWB', 'WWWWWWWB', 'BBBBBWBW', 'BBBBWBWB', 'BBBBBWBW', 'BBBBWBWB', 'BBBBBWBW', 'BBBBWBWB', 'BBBBBWBW', 'WWWWWWBW', 'BBBBWBWB', 'BBBWBWBW', 'BBBBWBWB', 'BBBWBWBW', 'BBBBWBWB', 'BBBWBWBW', 'BBBBWBWB', 'WWWWWBWB', 'BBBBBBBB', 'BBBBBBBB', 'BBBBBBBB', 'BBBBBBBB', 'BBBBBBBB', 'BBBBBBBB', 'WWWWWWWW', 'WWWWWWWW', 'BBBBBBBW', 'BBBBBBBB', 'BBBBBBBW', 'BBBBBBBB', 'BBBBBBBW', 'BBBBBBBB', 'WWWWWWWW', 'WWWWWWWW', 'BBBBBBWB', 'BBBBBBBW', 'BBBBBBWB', 'BBBBBBBW', 'BBBBBBWB', 'BBBBBBBW', 'WWWWWWWW', 'WWWWWWWW', 'BBBBBWBW', 'BBBBBBWB', 'BBBBBWBW', 'BBBBBBWB', 'BBBBBWBW', 'BBBBBBWB', 'WWWWWWWB', 'WWWWWWWB', 'BBBBWBWB', 'BBBBBWBW', 'BBBBWBWB', 'BBBBBWBW', 'BBBBWBWB', 'BBBBBWBW', 'WWWWWWBW', 'WWWWWWBW', 'BBBWBWBW', 'BBBBWBWB', 'BBBWBWBW', 'BBBBWBWB', 'BBBWBWBW', 'BBBBWBWB', 'WWWWWBWB', 'WWWWWBWB']
  ```

* 해당 `list`를 8개씩 chunk 개념으로 자른다. 자른 8개의 한 단위가 하나의 체스판이 된다.
* 체스판의 시작이 B라면, 체스판의 홀수번째 줄(`python`에서 `index`를 2로 나눈 몫으로 봤을 때는 짝수번째 줄이 된다.)은 "BWBWBWBW"가, 짝수번째 줄은 "WBWBWBWB"가 되어야 한다.
* 체스판의 시작이 W라면, 반대로 체스판의 홀수번째 줄은 "WBWBWBWB"가, 짝수번째 줄은 "BWBWBWBW"가 되어야 한다.
* 각 줄에서 `index`를 이용해 색을 바꾸어 칠해주어야 할 칸의 개수를 구한다. 그 중 최솟값을 출력한다.
* 반드시, **`index`가 0부터 시작**함에 주의하자!!!!



# 풀이 코드

* BW로 칠하거나, WB로 칠하거나로 경우를 나눴다.

```python
n, m = map(int, input().split())

chess = []
for _ in range(n):
    chess.append(input())

i = 0
j = 0
chessList = []

while i+8 <= n:
    while j+8 <= m:
        k = i
        for k in range(i, i+8):
            chessList.append(chess[k][j:j+8])
        j += 1
    j = 0
    i += 1

countList = []
 
# BW로 시작하는 체스판을 만들기 위한 경우
    
for i in range(0, len(chessList), 8):
    nCount = 0
   
    for j in range(len(chessList[i:i+8])):
        if j % 2 == 0:
            # 1, 3, 5, 7번째 줄에서 
            # 1, 3, 5, 7번째 칸이 B가 아니라면 바꾸고, 2, 4, 6, 8번재 칸이 W가 아니면 바꿔라.
            for k in range(len(chessList[i:i+8][j])):
                if k % 2 == 0 and chessList[i:i+8][j][k] != "B":
                    nCount += 1
                elif k % 2 == 1 and chessList[i:i+8][j][k] != "W":
                    nCount += 1
        else:
            # 2, 4, 6, 8번째 줄에서
            # 1, 3, 5, 7번째 칸이 W가 아니라면 바꾸고, 2, 4, 6, 8번째 칸이 B가 아니면 바꿔라.
            for k in range(len(chessList[i:i+8][j])):
                if k % 2 == 0 and chessList[i:i+8][j][k] != "W":
                    nCount += 1
                elif k% 2 == 1 and chessList[i:i+8][j][k] != "B":
                    nCount += 1
    countList.append(nCount)

# WB로 시작하는 체스판을 만들기 위한 경우

for i in range(0, len(chessList), 8):
    nCount = 0

    for j in range(len(chessList[i:i+8])):
        if j % 2 == 0:
            for k in range(len(chessList[i:i+8][j])):
                if k % 2 == 0 and chessList[i:i+8][j][k] != "W":
                    nCount += 1
                elif k  % 2 == 1 and chessList [i:i+8][j][k] != "B":
                    nCount += 1
        else:
            for k in range(len(chessList[i:i+8][j])):
                if k % 2 == 0 and chessList[i:i+8][j][k] != "B":
                    nCount += 1
                elif k  % 2 == 1 and chessList [i:i+8][j][k] != "W":
                    nCount += 1
    countList.append(nCount)
    
print(min(countList))               
```



# 다른 풀이







# 배운 점, 더 생각해 볼 점

* 더 생각해 볼 점 : 코드 바꿔야 하지 않을까?
  * 단순히, 길이가 너무 길다.
  * 모든 경우를 언급해줘야 하는데, 하드코딩 아닌가?
  * 이거 말고 더 쉽게 생각할 수 있는 경우가 있을 것 같은데, 고민해 보자.