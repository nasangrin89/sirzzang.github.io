---
title:  "[백준] BOJ2231 분해합"
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





> [문제 출처](https://www.acmicpc.net/problem/2231)





# 문제

어떤 자연수 N이 있을 때, 그 자연수 N의 분해합은 N과 N을 이루는 각 자리수의 합을 의미한다.

어떤 자연수 M의 분해합이 N인 경우, M을 N의 생성자라 한다. 예를 들어, 245의 분해합은 256(=245+2+4+5)이 된다. 따라서 245는 256의 생성자가 된다.  

물론, 어떤 자연수의 경우에는 생성자가 없을 수도 있다. 반대로, 생성자가 여러 개인 자연수도 있을 수 있다.

자연수 N이 주어졌을 때, N의 가장 작은 생성자를 구해내는 프로그램을 작성하시오.



(입력)

첫째 줄에 자연수 N(1 ≤ N ≤ 1,000,000)이 주어진다.

(출력)

첫째 줄에 답을 출력한다. 생성자가 없는 경우에는 0을 출력한다.



# 풀이 방법

* 입력의 모든 범위(1~1,000,000)까지에 대해 숫자 `list`를 만든다.
* 입력의 모든 범위(1~1,000,000)까지에 대해 분해합을 넣은 `list`를 만든다.

* 입력으로 주어진 N이 분해합 `list`에 없다면, 0을 출력한다.

* 입력으로 주어진 N이 분해합 `list`에 있다면, 분해합 `list`의 `index` 를 활용하여, 그 `index`에 해당하는 수를 숫자 `list`에서 불러온다.

  * 가장 작은 생성자를 구하는 것이므로, `index`를 구할 때 가장 앞에 있는 수를 구할테니 상관 없다.



# 풀이 코드

```python
numList = list(range(1, 1000000+1))

bunhaehapList = []
for i in range(1, 1000000+1):
    for j in str(i):	# j는 i라는 숫자의 자릿수
        i += int(j)		# i에 j를 합해 나간다.
    bunhaehapList.append(i)	# i는 분해합이 되고, 그 분해합을 bunhaehapList에 넣는다.

n = int(input())
if n in bunhaehapList:
    print(numList[bunhaehapList.index(n)])
else:
    print(0)
```





# 배운 점, 더 생각해 볼 점

* 시간 및 메모리 사용이 더 효율적인 다른 방법을 찾아야 한다. `append` 후 인덱스를 찾는 게 ~~너무~~ 무식한 방법 같다.

  
  
