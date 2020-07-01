---
title:  "[ML/DL] Entropy"
excerpt: "<<Loss Function>> 정보 이론 관점에서 엔트로피(섀넌 엔트로피)를 알아보자."
toc: true
toc_sticky: true
categories:
  - Lecture
tags:
  - ML
  - DL
  - Entropy
use_math: true
last_modified_at: 2020-06-17
---



<sup>출처가 명시되지 않은 모든 자료(이미지 등)는 [조성현 강사님](https://blog.naver.com/chunjein)의 강의 및 강의 자료를 기반으로 합니다. </sup> <sup>[Github Repo](https://github.com/sirzzang/LECTURE/blob/master/인공지능-자연어처리(NLP)-기반-기업-데이터-분석/조성현 강사님/ML/[20200617] CrossEntropy.pdf)</sup>

# _Entropy_





## 1. 정보 엔트로피



 정보 이론에서의 Entropy는 1948년 Claude Shannon이 제안한 개념으로, 다음의 아이디어를 반영한 개념이다.

* 확률이 큰 사건일수록 정보량은 작다.
* 독립적인 두 사건에서 얻을 수 있는 정보량은 두 사건에서 얻을 수 있는 정보량의 합이다.
* 정보량은 비트로 표현된다.



 이 아이디어를 모두 반영하기 위해서 Shannon은 어떤 사건에서 얻을 수 있는 정보의 양을 해당 사건의 로그 확률로 정의했다. 그리고 어떠한 확률 분포에서 얻을 수 있는 정보량을 확률 이론의 기댓값 계산 방식에 따라 기댓값으로 나타냈다.



![entropy]({{site.url}}/assets/images/entropy.png)



>  *참고*
>
>  교재에 명시되어 있는 정보 엔트로피 식은 정보량이 비트로 표현된다는 아이디어를 반영하지 않은 식이다. 이 부분은 강의에서 진행하지 않았으나, 추후 검색을 통해 찾아 반영해 작성한 내용이다. 원래 Shannon의 정보 엔트로피 식에서는 로그 확률의 밑이 2로 표현된다.
>
>  강사님께서는 정보 엔트로피와 열역학 엔트로피 사이의 연관 관계 및 그로부터 얻을 수 있는 정보 가치에 더 중점을 두어 설명하신 듯하다.





## 2. 엔트로피 공식 도출



 섀넌이 정의한 엔트로피 공식을 다시 한 번 보자.


$$
H(x) = - \Sigma_{i=1}^n p_ilog(p_i)
$$



 위의 엔트로피 공식을 변형해 보자.

$$
H(x) = \Sigma_{i=1}^n p_i*(-log(p_i)) \\
H(x) = \Sigma_{i=1}^n p_i*log(p_i^{-1}) \\
H(x) = \Sigma_{i=1}^n p_i*log(\frac {1} {p_i}) \\
H(x) = \Sigma_{i=1}^n log(\frac {1} {p_i})*p_i
$$



 한편, 확률 변수의 기댓값을 나타내는 공식을 보자.
$$
E(X)  = \Sigma_{i=1}^n x_i * p_i
$$


 `x_i` 자리에 `log` 값이 들어간 것 빼고는 전부 다 동일하다. 결국 엔트로피는 기댓값과 같은 형태를 갖고 있다.



 여기서 `log` 부분은 **정보의 양**을 나타낸다. 섀넌의 아이디어에 따라 다음의 두 가지 조건을 만족하면 정보의 양을 나타내는 함수로 사용할 수 있다.


$$
I(x) : 정보량을 \ 나타내는 \ 함수 \\
1. \ p(x_1) > p(x_2) \rightarrow I(x_1) < I(x_2) \\
2. \ I(x_1, x_2) = I(x_1) + I(x_2)
$$


 두 조건을 만족하는 모든 함수로 `log`만큼 좋은 게 없다(!). 



> *참고*
>
>  강사님의 교재에 설명되지 않은 부분이지만, 원래 섀넌 엔트로피 공식에서는 `log`의 밑이 2이기 때문에, 정보의 양에 따라 필요한 `bit 수`를 구할 수 있게 된다.



## 3. 머신러닝에서의 의미



 정보 이론에서의 엔트로피는 정보의 불확실성에 대한 척도를 의미한다. 그리고, 머신러닝은 모델링을 통해 불확실한 정보를 줄이고, *혹은* 불확실한 정보로부터 확실한 정보를 예측하고자 하는 작업이다. 

 따라서 불확실성을 수치로 표현하고, 그것을 줄이고자 하는 작업에 엔트로피 개념을 활용한다.



>  When estimating a model from the data, one has to assume a certain data generating process. The parameters of such a model are the values that maximize the agreement between the model and the observed data. 
>
> _출처: https://amethix.com/entropy-in-machine-learning/_



 이 외에, 뒤에서 볼 크로스 엔트로피 개념을 활용해 손실함수로 사용하기도 한다. 추가적으로 [이 링크](http://www.inference.org.uk/itprnn_lectures/)에는 어떻게 엔트로피를 머신 러닝, 데이터 사이언스에서 활용할 수 있는지 소개한 강의들이 있다.





*참고: 의사결정나무 알고리즘에서의 적용*

  [Decision Tree 수업 내용](https://github.com/sirzzang/LECTURE/blob/master/%EC%9D%B8%EA%B3%B5%EC%A7%80%EB%8A%A5-%EC%9E%90%EC%97%B0%EC%96%B4%EC%B2%98%EB%A6%AC(NLP)-%EA%B8%B0%EB%B0%98-%EA%B8%B0%EC%97%85-%EB%8D%B0%EC%9D%B4%ED%84%B0-%EB%B6%84%EC%84%9D/%EC%A1%B0%EC%84%B1%ED%98%84%20%EA%B0%95%EC%82%AC%EB%8B%98/ML/%5B20200616-17%5D%20ML-Classification-DecisionTree.pdf) 및 아래 Udacity 강의 내용을 참고하자.



|                           Entropy                            |                           Impurity                           |
| :----------------------------------------------------------: | :----------------------------------------------------------: |
| ![entropy_udacity1]({{site.url}}/assets/images/udacity1.png) | ![entropy-udacity2]({{site.url}}/assets/images/udacity2.png) |

<center><sup> https://www.youtube.com/watch?v=NHAatuG0T3Q </sup></center>




