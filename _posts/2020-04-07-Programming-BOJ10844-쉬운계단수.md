---
title: "[백준] 쉬운 계단수"
excerpt: 1일 1문제풀이 18일차
header:
  teaser: /assets/images/blog-Programming.jpg
toc: true
categories:
  - Programming
tags:
  - BOJ
  - Python
  - Programming
  - DP

---





> [문제 출처](https://www.acmicpc.net/problem/1463)



# 1. 문제



45656이란 수를 보자.

이 수는 인접한 모든 자리수의 차이가 1이 난다. 이런 수를 계단 수라고 한다.

세준이는 수의 길이가 N인 계단 수가 몇 개 있는지 궁금해졌다.

N이 주어질 때, 길이가 N인 계단 수가 총 몇 개 있는지 구하는 프로그램을 작성하시오. (0으로 시작하는 수는 없다.)



**입력**

첫째 줄에 N이 주어진다. N은 1보다 크거나 같고, 100보다 작거나 같은 자연수이다.



**출력**

첫째 줄에 정답을 1,000,000,000으로 나눈 나머지를 출력한다.



**입출력 예**

```python
입력
1
출력
9
입력
2
출력
17
```



---

# 2. 나의 풀이 



## 풀이 방법



 N번째 계단 수는 N-1번째 계단 수의 맨 뒷자리 수에 +1, -1씩한 숫자를 붙이면 된다. *다만* 마지막 수가 0으로 끝나는 경우는 -1할 수 없고, 9로 끝나는 경우는 +1할 수 없다.

 반대로 생각해 본다면, N번째 계단 수에서 0으로 끝나는 수의 개수는 N-1번째 계단 수에서 1로 끝난 수의 개수, 9로 끝나는 수의 개수는 N-1번째 계단 수에서 8로 끝난 수의 개수와 같다. 나머지 1부터 8까지는 전부 N-1번째 계단 수 중 가장 마지막 자리의 수가 (0, 2)인 수의 개수, (1, 3)인 수의 개수, ..., (6, 8)인 수의 개수, (7, 9)인 수의 개수와 같다.

 



> *시행착오 1*  
>
>  N번째 계단 수의 '개수'와 N-1번째 계단 수의 '개수'가 연관이 있는 줄 알고 그 사이의 점화식을 찾으려고 하루 종일 난리를 쳤다.
>
> ![hoooo....]({{site.url}}/assets/images/stair-num-seq.png)
>
>  결국에 찾기는 했는데, 점화식이 저건 아니라는 생각이... 알고 보니 마지막 자리 수로 접근해야 하는 문제였다. 참...



> *시행착오 2*
>
>  copy deepcopy 여기서 나타날 줄 1도 몰랐다. 진짜. 초기 배열 생성할 때 아무 생각 없이 아래와 같이 했더니, 2번째 배열부터 그 이전 단계 dp 값들이 다 바뀌어 버리는 것이었다..
>
> ```python
> dp = [[0, 0, 0, 0, 0, 0, 0, 0, 0, 0]]*101 # 초기값 설정
> dp[1] = [0, 1, 1, 1, 1, 1, 1, 1, 1, 1]
> ```
>
>  알고 보니 이건 주소값만 참조하도록 해 놓은 거라고... dp 배열 내의 리스트들이 모두 같은 주소값을 공유하고 있으니 한 번 바꾸면 뒤에 것들도 도미노마냥 바뀌어 버린다. 
>
>  복사를 하든, for문 통해 생성하든 해야 한다.
>
> ```python
> # for문 통한 생성
> dp = [[0 for row in range(10)] for col in range(n+1)]
> 
> # shallow copy
> dp = [[copy.deepcopy(0) for x in range(10)] for y in range(n+1)]
> ```



## 풀이 코드

* 정답으로 출력해야 할 수가 1000000000으로 나눈 나머지여야 하므로,  mod 연산의 성질을 이용해 각 단계에서 1000000000으로 나눈 나머지를 dp 배열에 저장한다.

> 29380KB, 64ms.

```python
dp = [[0, 0, 0, 0, 0, 0, 0, 0, 0, 0][:] for _ in range(101)] # 초기값 설정
dp[1] = [0, 1, 1, 1, 1, 1, 1, 1, 1, 1]


N = int(input())

for i in range(2, N+1):
    # print(f"{i-1}th dp: {dp[i-1]}")
    dp[i][0] = dp[i-1][1]%1000000000
    dp[i][1] = (dp[i-1][0] + dp[i-1][2]) %1000000000
    dp[i][2] = (dp[i-1][1] + dp[i-1][3]) %1000000000
    dp[i][3] = (dp[i-1][2] + dp[i-1][4]) %1000000000
    dp[i][4] = (dp[i-1][3] + dp[i-1][5]) %1000000000
    dp[i][5] = (dp[i-1][4] + dp[i-1][6]) %1000000000
    dp[i][6] = (dp[i-1][5] + dp[i-1][7]) %1000000000
    dp[i][7] = (dp[i-1][6] + dp[i-1][8]) %1000000000
    dp[i][8] = (dp[i-1][7] + dp[i-1][9])%1000000000
    dp[i][9] = dp[i-1][8]%1000000000
    
print(sum(dp[N])%1000000000)
```





  



---

  



# 3. 다른 풀이



[풀이 출처](https://www.acmicpc.net/source/17767811)

> 29284KB, 52ms.

```python
n = int(input())

DP = [[0]*10 for i in range(101)]
DP[1] = [0,1,1,1,1,1,1,1,1,1]

for i in range(2,n+1):
    for j in range(10):
        if j == 0:
            DP[i][j] = DP[i-1][j+1]
        elif j == 9:
            DP[i][j] = DP[i-1][j-1]
        else:
            DP[i][j] = DP[i-1][j+1] + DP[i-1][j-1]
result = 0
for i in range(0,10):
    result += DP[n][i]

print(result%1000000000)
```





[풀이 출처](https://www.acmicpc.net/source/13857213)

> 29056KB, 56ms.

```python
a=1;b=c=d=e=2
for i in range(int(input())-1):a,b,c,d,e=b,a+c,b+d,c+e,d+e
print((a+b+c+d+e)%1000000000)
```

* 난 이런 풀이를 정말 생각해 내지도, 당장 이해하지도 못하겠다.. 흐긓ㄱ.....



   



---

# 4. 배운 점, 더 생각해 볼 점



* DP 어렵다 증말... 점화식이 단순한 점화식이 아니다.

* copy, deepcopy... 지금 문제에서는 `[:]` 사용해서 shallow copy로도 초기값 문제 해결할 수 있었지만, 나중에는 배열 안에 배열 넣어서 초기값 설정해야 하는 경우도 있을 수 있다. 그냥 마음 편하게 for문을 쓰든가 하자. 

  >  [참고1](https://medium.com/@daniel.tooke/variables-and-memory-addresses-in-python-6d96d672ed3d), [참고2](https://stackoverflow.com/questions/45911631/why-does-variables-address-change-in-python-when-the-variable-is-assigned-a-dif), [참고3](https://suwoni-codelab.com/python%20%EA%B8%B0%EB%B3%B8/2018/03/02/Python-Basic-copy/), [참고4](https://blueshw.github.io/2016/01/20/shallow-copy-deep-copy/), [참고5: ~~이 문제는 어디에서나 뒷목 잡게 만들 수 있다 핵공감...~~](https://hamait.tistory.com/844)

  > *1. JK의 가르침*
  >
  >  어차피 for문 쓰나, deepcopy 써서 하나 결과적으로 같게 나올 거고, 메모리 차이 크지 않을 것 같다. 그거 가지고 문제 틀리면 애초에 안 되는 메모리 제한입니다!

  > *2. 문쌤의 가르침*
  >
  > ```python
  > from copy import deepcopy
  > 
  > dp = [[0, 0, 0, 0, 0, 0, 0, 0, 0, 0]]*10 # 초기값 설정
  > dp[1] = [100, 1, 1, 1, 1, 1, 1, 1, 1, 1]
  > print("==================== copy 안 했을 때 ====================")
  > print(dp)
  > for d in dp:
  >     print(id(d), id(d[0]))
  > 
  > dp_copy = [[0, 0, 0, 0, 0, 0, 0, 0, 0, 0][:] for _ in range(10)] # 초기값 설정
  > dp_copy[1] = [100, 1, 1, 1, 1, 1, 1, 1, 1, 1]
  > print("==================== copy 했을 때 ====================")
  > print(dp_copy)
  > for d in dp_copy:
  >     print(id(d), id(d[0]))
  > 
  > dp_deepcopy = [deepcopy([0, 0, 0, 0, 0, 0, 0, 0, 0, 0]) for _ in range(10)] # 초기값 설정
  > dp_deepcopy[1] = [100, 1, 1, 1, 1, 1, 1, 1, 1, 1]
  > print("==================== deepcopy 했을 때 ====================")
  > print(dp_deepcopy)
  > for d in dp_deepcopy:
  >     print(id(d), id(d[0]))
  > ```
  >
  > * copy 안 한 건 그냥 reference. 
  > * 배열이 객체고 그 안에 있는 원소들은 integer 객체. 배열 copy해야 다른 주소값 가지게 복사된다.
  > * 안에 있는 원소 주소값 안 바뀌는 건 당연하다. 그 안에 있는 애들은 또 integer 객체니까 또 따로 주소값 가진다. 
  > * 파이썬은 0부터 255까지는 동일한 주로소 할당한다고..(?!)
