---
title:  "[ML/DL] KL Divergence"
excerpt: "KL Divergence, 상대 엔트로피."
toc: true
toc_sticky: true
categories:
  - Lecture
tags:
  - ML
  - DL
  - Entropy
  - KL Divergence
use_math: true
last_modified_at: 2020-06-17
---



<sup>출처가 명시되지 않은 모든 자료(이미지 등)는 [조성현 강사님](https://blog.naver.com/chunjein)의 강의 및 강의 자료를 기반으로 합니다.</sup> <sup>[Github Repo](https://github.com/sirzzang/LECTURE/blob/master/인공지능-자연어처리(NLP)-기반-기업-데이터-분석/조성현 강사님/[20200617] KLDivergence.pdf)</sup>

# _KL Divergence_



## 1. 개념



 두 확률 분포의 차이를 '수치화'한다. 확률 분포의 차이를 수치화한다면, 실제 확률 분포에 가까운 분포와 그렇지 않은 분포의 차이를 수치화할 수 있기 때문에, *어떤 것이 더 예측을 잘 하는 확률 분포* 인지 알 수 있다.

 



$$
D_{KL}(P||Q) = - \Sigma \ P(x) log(\frac {Q(x)} {P(x)})
$$

 

 정보량 측면에서 KL Divergence 식을 파헤쳐 보자. 크로스 엔트로피 식을 전개하다 보면, KL Divergence 식을 도출할 수 있다!

$$
H(p, q)(a.k.a \ Cross \ Entropy) \\
= -\Sigma p_i logq_i \\
= - \Sigma p_i logq_i \ -\Sigma p_i logp_i \ + \Sigma p_i logp_i \\
= H(p) + \Sigma p_i logp_i - \Sigma p_i logq_i \\
= H(p) + \Sigma p_i log \frac {p_i} {q_i}
$$





 결국 크로스 엔트로피 `H(p, q)`는 원래 P의 엔트로피 `H(p)`의 값에 마지막 항의 값 만큼을 더해준 것임을 알 수 있다. 이 때, 더해진 *'무언가'* 가 바로 정보량 차이이고, 이 정보량 차이의 식이 자세히 살펴 보면 KL Divergence와 같음을 알 수 있다.



*즉* , KL Divergence는 불확실한 분포를 통해 확실한 분포를 추정해 내고자 할 때, 두 분포의 크로스 엔트로피 값에서 실제 엔트로피 값을 만들기 위해 더해 주어야 하는 정보량 차이를 나타낸다. 이러한 의미에서 KL Divergence를 **상대 엔트로피**라고도 한다.





## 2. 특징



 KL Divergence는 다음과 같은 특징을 갖는다. 수학적인 증명 과정은 Github 강의 자료 정리를 참고하고, 여기서는 다음과 같은 특징을 갖는다는 것만 알아두자.

* 0 이상이다.
* KL Divergence가 0이라는 것은, 두 확률 분포가 동치임을 나타낸다. 
* 기준으로 두는 확률 분포에 따라 비대칭적인 값을 갖는다.



 KL Divergence 값을 이해할 때 가장 중요한 것은, 두 확률 분포가 비슷할 수록 더 작은 값을 갖는다는 것이다. 결국 확률 분포의 차이를 수치화하고 싶다는 아이디어에서 출발한 개념이므로, 수치화되어 나타난 값이 작을수록 더 비슷한 확률 분포임을 보여 준다.

 한편 마지막 특징은 KL Divergence를 검색하다 보면 항상 따라 나오는 '거리 개념이 아니므로 주의한다'는 말과도 연관된다. 만약 KL Divergence가 두 확률 분포 사이의 거리를 나타냈다면 P와 Q의 위치가 바뀌어도 같은 값을 가져야 하지만, 전혀 그렇지 않다. 참고로, 이러한 관점에서 KL Divergence를 대칭적으로 바꾼 `Jenson-Shannon Divergence `식이 있다. 나중에 GAN에서 핵심적으로 등장하는 개념인 만큼, 지금은 그 존재를 알아두고, 이후 해당 단계에서 학습하도록 하자. 







## 3. KL Divergence와 손실 함수



 머신러닝 모델링의 목적이 예측 확률 분포 Q를 실제 확률 분포 P와 가깝게 만들어 가는 것임을 상기해 본다면, 손실 함수로서 왜 KL Divergence를 이용하지 않고 Cross Entropy를 이용하는지 궁금해 진다.

 **결론적으로, KL Divergence와 Cross Entropy, 무엇을 최소화하든 그 결과는 같다.**

 위에서 KL Divergence와 Cross Entropy가 밀접한 관련이 있음을 식을 통해 알 수 있었다. 그런데, 머신러닝 학습 과정에서 예측 분포 Q의 파라미터를 조정해나갈 때 원래 확률 분포 P의 엔트로피는 변경되지 않는다. 원래 정해진 상수 값이다. 학습 과정에서 파라미터의 최적화를 통해 변경되는 것은 오로지 `H(p, q)`, 즉, 크로스 엔트로피 값일 뿐이므로, KL Divergence값을 최소화하나, Cross Entropy를 최소화하나 *거기서 거기다!*

