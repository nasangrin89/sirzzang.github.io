---
title:  "[Python] Matplotlib"
excerpt: "Matplotlib 라이브러리 사용법을 정리합니다."
toc: true
toc_sticky: true
header:
  teaser: /assets/images/blog-Programming.jpg

categories:
  - Python
tags:
  - Python
  - Matplotlib
  - visualization

last_modified_at: 2020-03-03
---







# _"Matplotlib 사용법 정리"_



## 0. 기본 설정

* `%matplotlib inline` : 그래프 결과 출력 세션에 나타나도록 함.
* 한글 설정(윈도우 기준)

```python
from matplotlib import rc, font_manager
import matplotlib.pyplot as plt

plt.rcParams['axes.unicode_minus'] = False

path = "C:/Windows/Fonts/malgun.ttf"
font_name = font_manager.FontProperties(fname=path).get_name()
rc('font', family=font_name)
```



---



## 1. 그래프 설정

* `plt.title()` : 그래프 제목.
* `plt.xlabel()` : X축 이름, `plt.ylabel()` : Y축 이름. 
* `plt.legend()` : 범례 추가. 그림을 그릴 때(`plt.plot()`, `plt.scatter()` 등) label 옵션으로 텍스트를 넘긴 후 적용한다.
* `plt.grid()` : 배경 격자 무늬 적용.
* 그림 옵션
  * `alpha` : 투명도.
  * `cmap` : color map.



### cf) rcParams

* 폰트 크기, 그래프 색깔 등 plot parameter를 조정할 수 있는 class([공식 문서](https://matplotlib.org/3.1.1/api/matplotlib_configuration_api.html#matplotlib.RcParams)).

* 다음의 두 가지 방식을 통해 사용할 수 있다.

  * `.rcParams[]` : 직접적으로 하나씩 지정해 커스텀.

  ```py
  mpl.rcParams['lines.linewidth'] = 2
  mpl.rcParams['lines.color'] = 'r'
  plt.plot(data)
  ```

  * `.rc()` : 여러 세팅을 한 번에 바꿀 수 있음.

  ```python
  mpl.rc('lines', linewidth=4, color='g')
  plt.plot(data)
  ```

* 다음과 같은 옵션을 지정할 수 있다.
  * `axes.labelsize` : x축, y축 이름 폰트 크기.
  * `xtick.labelsize`, `ytick.labelsize` : x축, y축 tick의 폰트 크기.
  * `plt.rcParams['axes.unicode_minus'] = False` : 한글 깨짐 현상 해결.



---





---



## 2. 그래프 종류

* `plot` : 선 그래프.
* `scatter` : 산점도.
* `boxplot` :
* `bar` : 막대그래프(수직). `barh` : 막대그래프(수평).



### 2.1. 선그래프

* `plt.plot()` : 선그래프를 그리는 데 사용하는 명령.
* 인자로 다음의 값을 가질 수 있다.
  * `color` : 그래프 색상.
  * `ls` : 그래프 선 스타일, `lw`: 선의 굵기.
  * `marker` : 데이터가 존재하는 곳에 마킹. 타입을 지정할 수 있다. `markerfacecolor`로 마커의 색상을, `markersize`로 마커의 크기를 지정한다.



### 2.2. 히스토그램

* `plt.hist()` : 히스토그램을 그리는 데 사용하는 명령([공식문서](https://matplotlib.org/api/pyplot_api.html#matplotlib.pyplot.hist))으로, 주로 값의 분포를 시각적으로 확인하기 위해 사용한다.
* 인자로 다음의 값을 갖는다.
  * `n` : 각 구간에 포함된 값의 갯수 혹은 빈도 리스트.
  * `bins` : 구간의 경계값 리스트.
  * `patches` : 각 구간을 그리는 matplotlib patch 객체 리스트.



### 2.3. 산점도

* `plt.scatter()`, `plt.plot(kind='scatter')` : 산점도를 그려 두 변수 간 관계를 확인할 때 사용한다.

* 인자로 다음의 값을 갖는다.
  * `x`, `y`: x축, y축에 들어갈 자료. list, nd.array와 같이 iterable한 자료형을 받는다.
  * `s` : 마커의 크기.
    * 마커의 크기를 바꿀 필요가 있을 때<sup>예) 인구를 나타내야 할 때</sup> 사용한다.
    * 스칼라로 입력할 경우 마커의 크기는 고정이다.
    * 요소수를 가지는 iterable을 입력받는 경우, 각 마커별로 크기를 다르게 설정할 수 있다.
  * c : 마커의 색상.
    * plot에서 선과 동일하게 코드를 입력할 수 있다.
    * iterable을 입력받는 경우, 마커별로 크기를 다르게 설정할 수 있다.



---



## 3. 기타 기능 

### 3.1. 그림에 텍스트 표시하기

* `plt.text()` : 그림에 텍스트를 표시한다. 인자로 다음의 값을 갖는다.
  * `x` : 텍스트가 표시될 x좌표.
  * `y` : 텍스트가 표시될 y좌표.
  * string : 표시할 텍스트.
  * `fontsize` : 텍스트 폰트 사이즈.
* `plt.annotate()` 





## image

> ```python
> import matplotlib.image as mpimg
> ```



* matplotlib에서 이미지를 처리할 때 사용한다.([공식문서](https://matplotlib.org/tutorials/introductory/images.html))

