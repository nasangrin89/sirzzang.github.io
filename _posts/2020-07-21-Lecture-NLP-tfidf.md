---
title:  "[NLP] TF-IDF"
excerpt: "<<NLP>> 빈도 기반의 단어 수치화 방법인 TF-IDF 행렬을 알아 보자."
toc: true
toc_sticky: true
categories:
  - Lecture
tags:
  - NLP
  - TF-IDF
use_math: true
last_modified_at: 2020-07-21
---



<sup> [조성현 강사님](https://blog.naver.com/chunjein)의 강의 및 강의 자료를 기반으로 합니다.</sup> <sup>[Github Repo](https://github.com/sirzzang/LECTURE/tree/master/인공지능-자연어처리(NLP)-기반-기업-데이터-분석/조성현 강사님/DL/DL 실습/20200721)</sup>

<sup>Tensorflow : 2.2.0</sup>



# _TF-IDF_



## 1. 개요



 여러 개의 문서(= 문장)가 있을 때, 각각의 문서 내에 있는 단어에 **수치적 중요도**를 부여하여 단어를 수치화하는 방법이다. **빈도**_(count)_를 기반으로 단어에 중요도를 부여하고, 그것을 수치로 나타낸다.



 TF-IDF를 엑셀로 구현해 놓은 교재 파일을 보자.



![colab-tfidf](sirzzang.github.io/assets/images/tf-idf.png)

<center><sup>제일 오른쪽이 TF-IDF 행렬이다.</sup></center>

 

### TF(Term Frequency)



 문서별 단어의 등장 빈도를 나타내는 행렬이다.

* 어휘 집합(Vocabulary)을 만든다. 모든 문서에 사용된 단어를 `행 방향`으로 나타낸 것이다. 문서 corpus에 쓰인 모든 단어의 개수가 곧 행의 개수이다.
* `열 방향`으로 단어의 등장 빈도를 기록한다. 즉, 각 문서별로 해당 단어가 몇 번 쓰였는지 나타낸다. 



 행렬의 마지막 칸에 해당 단어가 총 몇 번 등장했는지 합을 구하고, 그 합을 문서에 등장한 총 단어 수로 나눠 준다. 문서가 길수록 단어의 등장 빈도가 높기 때문에, 문서 길이로 **표준화**하는 작업이다.



### IDF(Inverse Document Frequency)



 **DF**란, 단어의 문서별 등장 빈도를 나타내는 행렬이다. Vocabulary에 있는 모든 단어가 문서 중 몇 개에 등장했는지를 나타낸다.



>  위의 엑셀 예시에서 'silver'의 경우는 두 번째 문서에 2번 등장했지만, 문서 1, 2, 3 중 총 2번 문서에만 등장하므로 DF가 1이 된다.



 DF가 클수록 여러 문서에 나타나는 **일반적**_(general)_인 단어라고 본다. 특정 문서 안에서 단어가 많이 나타나면 그 단어는 그 문서 안에서 중요한 단어이긴 하나, 모든 문서에서 그 단어가 많이 나타난다면 그렇지 않을 수 있다. 예컨대, 영어로 치면 `a`, `the`와 같이 중요하지 않은 단어일 수 있다.

 따라서 *specific*한 단어일수록 중요도가 크다고 보고, `1/DF`에 비례할수록 단어의 중요도가 커진다고 본다. 따라서 IDF 행렬은 Document Frequncy에 역수를 취해 그에 비례하도록 수치를 계산한다. 정확한 계산 공식은 다음과 같다.


$$
IDF = log(\frac {1} {DF})
$$


 ### TF-IDF



 결과적으로 TF-IDF 알고리즘은 단어의 중요도는 **TF와 IDF에 비례**한다고 본다. 이 때, IDF에 비례한다는 말은, 곧 DF에 반비례한다는 말이기도 하다. 따라서 단어의 중요도를 나타내기 위해 TF와 IDF를 곱한 행렬을 만든다. 이를 `TF-IDF` 행렬이라고 한다.



### 활용



 TF-IDF 행렬을 구하면, 각 문서별로 단어가 몇 번 등장하는지를 수치화함으로써 해당 문서를 수치화할 수 있게 된다. 이렇게 수치화한 문서 벡터를 다른 작업에 활용한다. 예컨대, 각 문서 벡터 간 유사도를 계산해 검색 문장과 가장 유사한 문서를 추천하는 데에 사용할 수 있다.





## 2. 구현



 파이썬의 Scikit-learn 라이브러리에 `TfidfVectorizer` 함수를 사용하면 된다. 그러나 원리를 익히기 위해, 교재 엑셀 파일과 같은 결과가 나오도록 직접 코딩해보자. 검색 문장을 입력했을 때 1, 2, 3번 문서 중 가장 유사도가 높은 문서가 검색되도록, 문서별 검색 순위를 구해 보자.



### 나의 풀이



 정말 직관적으로 하라는 대로 구현했다. 그런데 행렬 인덱싱 이용해 하려다 보니 코드가 ~~*약간*~~ 지저분해 보인다. 내장 함수, 라이브러리 등 다른 함수 적극적으로 사용했으면 더 쉬웠을 것 같다. 곧이 곧대로 하려다 보니, 재현성이 떨어지고(`transpose` 등을 많이 썼다) 코드가 효율적이지 못해 보이는 (느낌적인) 느낌이다.



```python
import numpy as np

# vocabulary 생성
def build_dictionary(corpus):
    vocab_dict = {}
    for c in corpus:
        vocabs = c.strip().split() # 각 문장 어휘 리스트
        for vocab in vocabs:
            vocab_key = vocab.lower()
            if vocab_key not in vocab_dict: # dict 내 어휘, 등장 빈도 색인 생성
                vocab_dict[vocab_key] = 0

    return sorted(vocab_dict)

# TF 계산
def calc_TF(corpus, vocab_dict):
    flag = len(vocab_dict)
    
    # 각 문장별로 단어 등장 빈도 표시한 행렬
    TF_matrix = [[v, 0] for _ in range(len(corpus)) for v in vocab_dict] # 초기화
    for i in range(len(corpus)):
        sent = corpus[i].strip().lower().split()
        vocab_cnt = 0 # 어휘 사전에 있는 단어 총 등장 빈도 수
        for s in sent:
            if vocab in TF_matrix[i*flag : (i+1)*flag]:
                if s == vocab[0]: 
                    vocab[1] += 1 # 해당 단어 등장 빈도 업데이트
                    vocab_cnt += 1 # 총 등장 빈도 업데이트
        
        # 문장 길이로 TF 표준화
        for j in range(flag):
            TF_matrix[i*flag : (i+1)*flag][j][1] /= vocab_cnt
    
    return TF_matrix

# IDF 계산
def cacl_IDF(corpus, vocab_dict):
    IDF_matrix = [[vocab, 0] for vocab in vocab_dict] # IDF 행렬 초기화
    
    for j in range(len(IDF_matrix)):
        for i in range(1, len(corpus)):
            sent = corpus[i].strip().lower().split()
            if IDF_matrix[j][0] in sent: # 문장에 등장하면 등장 빈도 업데이트
                IDF_matrix[j][1] += 1
     
    # 공식에 따라 IDF 계산
    for i in range(len(IDF_matrix)):
        IDF_matrix[i][1] = np.log((len(corpus)-1) / IDF_matrix[i][1]) # 검색 문장 제외
        
    return IDF_matrix * len(corpus) # 이후 계산 편의 위해 형태 변경하여 return

# TF-IDF 계산
def calc_TFIDF(tf, idf, corpus, vocab_dict):
    flag = len(vocab_dict)
    
    TFIDF_matrix = []
    for i in range(len(corpus)): # 문장별로 tf, idf 구해서 곱하기
        tf = np.array([tf[1] for tf in TF[i*flag : (i+1)*flag]], dtype=np.float32) 
        idf = np.array([idf[1] for idf in IDF[i*flag : (i+1)*flag]], dtype=np.float32)
        TFIDF_matrix.append(tf*idf)
    
    return np.array(TFIDF_matrix).T

# 코사인 유사도 계산
def calc_cos_sim(A, B):
    # 분자
    bunja = A*B
    
    # 분모
    norm_A, norm_B = 0, 0
    for a in A:
        norm_A += a**2
    for b in B:
        norm_B += b**2
    bunmo = np.sqrt(norm_A) * np.sqrt(norm_B)
    
    return (bunja/bunmo).T.sum()

# 각 문서별 코사인 유사도 계산
sentences = [
             'gold silver truck',
             'Shipment of gold damaged in a fire',
             'Delivery of silver arrived in a silver truck',
             'Shipment of gold arrived in a truck'
             ]

dictionary = build_dictionary(sentences)
tf_matrix = calc_TF(sentences, dictionary)
idf_matrix = calc_IDF(sentences, dictionary)
tfidf_matrix = calc_TFIDF(tf_matrix, idf_matrix, sentences, dictionary)

cos_sim_docs = {} # 검색어 문장과의 코사인 유사도 계산해서 저장
for i in range(1, len(sentences)):
    cos_sim_docs[sentences[i]] = calc_cos_sim(tfidf_matrix.T[0], tfidf_matrix.T[i])
```



### 다른 풀이



**강사님 해설**



**중헌쓰 풀이**



