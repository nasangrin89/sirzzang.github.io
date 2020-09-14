---
title:  "[HuggingFace] Transformers 라이브러리 사용하기"
excerpt: "이제 HuggingFace Transformers 라이브러리를 Keras Functional API처럼 사용할 수 있다!"
toc: true
toc_sticky: true
categories:
  - SELF
tags:
  - 자연어처리
  - NLP
  - Transformers
  - Keras
  - 
last_modified_at: 2020-08-19
---





# _HuggingFace Transformers 사용하기_

<sup>[:hugs:][huggingface.co/transformers](https://huggingface.co/transformers)</sup>

<br>

 HuggingFace의 Transformers 트랜스포머 기반의 모델들을 쉽게 사용할 수 있도록 해 놓았다. 예전에 프로젝트할 때도 유용하게 썼었는데, 그 때에는 PyTorch로만 사용할 수 있었다. 그런데 이제는 **Tensorflow 2.x 버전에서, 특히나 사전학습된 모델을 Keras 모델처럼** 사용할 수 있다! 



> *참고* 
>
> * **Models** are standard [torch.nn.Module](https://pytorch.org/docs/stable/nn.html#torch.nn.Module) or [tf.**keras**.Model](https://www.tensorflow.org/api_docs/python/tf/keras/Model) so you can use them in your usual training loop. 🤗 
> * **Model classes** such as [`BertModel`](https://huggingface.co/transformers/model_doc/bert.html#transformers.BertModel), which are 30+ PyTorch models ([torch.nn.Module](https://pytorch.org/docs/stable/nn.html#torch.nn.Module)) or Keras models ([tf.keras.Model](https://www.tensorflow.org/api_docs/python/tf/keras/Model)) that work with the pretrained weights provided in the library.



사전학습한 모델을 불러 와서, Keras의 *functional API*처럼 컴파일하고, 훈련시킬 수 있다.

 [이 글](https://towardsdatascience.com/working-with-hugging-face-transformers-and-tf-2-0-89bf35e3555a)을 참고하여, 어떻게 HuggingFace의 Transformers 라이브러리를 Keras 모델처럼 사용할 수 있는지 정리해 본다.

 <br>



## 1. 개요



 기본적으로 Transformers 라이브러리의 모델을 사용할 때 적용되는 큰 작업 흐름은 모두 동일하다. 사전학습된 모델로부터 토크나이저를 불러 와 문서를 토크나이징하고, 사전학습된 모델을 불러온 뒤 그 출력인 임베딩을 활용해 파인튜닝하면 된다.

<br>

### Tokenizer

 토크나이저를 불러 오고, `encode` 메소드를 사용하면 바로 사전학습된 모델의 `word2idx`에 따라 인코딩된 수치 벡터가 나온다. 이 때, 파라미터로 `padding`, `truncating` 등의 옵션을 줄 수 있다. 토크나이징 단계에서 한 번에 문장의 길이를 맞출 수 있어 편리하다.

 원래 문장으로 복구하고 싶으면, `decode` 메소드를 사용한다. 사전학습된 모델의 어휘집에 무엇이 있는지 알고 싶다면, `get_vocab()` 메소드를 사용한다. 추가적인 파라미터, 메소드에 대해 알고 싶다면 [이 문서](https://huggingface.co/transformers/main_classes/tokenizer.html)를 참고하자.

```python
# 
```













```python
import os

# KoNLPy 라이브러리 설치
!pip install konlpy

# jdk, JPype1-py3 설치
!apt-get install openjdk-8-jdk-headless -qq > /dev/null
!pip3 install JPype1-py3

# automake 설치
os.chdir('/tmp')
!curl -LO http://ftpmirror.gnu.org/automake/automake-1.11.tar.gz
!tar -zxvf automake-1.11.tar.gz
os.chdir('/tmp/automake-1.11')
!./configure
!make
!make install

# automake 설치 오류 시
os.chdir('/tmp/') 
!wget -O m4-1.4.9.tar.gz http://ftp.gnu.org/gnu/m4/m4-1.4.9.tar.gz
!tar -zvxf m4-1.4.9.tar.gz
os.chdir('/tmp/m4-1.4.9')
!./configure
!make
!make install
os.chdir('/tmp')
!curl -OL http://ftpmirror.gnu.org/autoconf/autoconf-2.69.tar.gz
!tar xzf autoconf-2.69.tar.gz
os.chdir('/tmp/autoconf-2.69')
!./configure --prefix=/usr/local
!make
!make install
!export PATH=/usr/local/bin
```

<br>

 강의에서는 `Okt` 형태소 분석기를 기준으로 해서 `mecab`을 깔지는 않았지만, 위 문서에서는 다음과 같은 방식으로 `mecab`을 설치할 수 있다고 설명한다.

```python
import os

# mecab-ko 설치
os.chdir('/tmp/')
!curl -LO https://bitbucket.org/eunjeon/mecab-ko/downloads/mecab-0.996-ko-0.9.1.tar.gz
!tar zxfv mecab-0.996-ko-0.9.1.tar.gz
os.chdir('/tmp/mecab-0.996-ko-0.9.1')
!./configure
!make
!make check
!make install

# mecab-ko-dic 설치
os.chdir('/tmp')
!curl -LO https://bitbucket.org/eunjeon/mecab-ko-dic/downloads/mecab-ko-dic-2.0.1-20150920.tar.gz
!tar -zxvf mecab-ko-dic-2.0.1-20150920.tar.gz
os.chdir('/tmp/mecab-ko-dic-2.0.1-20150920')
!./autogen.sh
!./configure
!make
# !sh -c 'echo "dicdir=/usr/local/lib/mecab/dic/mecab-ko-dic" > /usr/local/etc/mecabrc'
!make install

# mecab-python 설치: python3 기준
os.chdir('/content')
!git clone https://bitbucket.org/eunjeon/mecab-python-0.996.git
os.chdir('/content/mecab-python-0.996')
!python3 setup.py build
!python3 setup.py install
```

<br>

## 2. KoNLPy 설치 위치로 이동



 구글 드라이브에서 `내 드라이브`의 상위 경로로 이동하는 게 핵심이었다. ~~*(예전에는 이걸 몰랐다)*~~ 그 상위 경로로만 이동하면, 로컬 환경에서 진행하는 것과 똑같다! (다만, Google Colabaratory가 리눅스 환경 기반이어서 조금씩 폴더 명이 다르긴 하다.)

 아래의 사진에서처럼 `konlpy` 폴더를 찾아 가자. `/usr/local/lib/python3.6/dist-packages/konlpy`를 따라 가면 된다.



|               내 드라이브 상위 경로로 이동                |                  KoNLPy 폴더로 이동                  |
| :-------------------------------------------------------: | :--------------------------------------------------: |
| ![change-path]({{site.url}}/assets/images/user-dic-1.png) | ![konlpy]({{site.url}}/assets/images/user-dic-2.png) |



> *참고* 
>
>  로컬 환경(아나콘다 기준)에서 사용자 사전을 추가하려면, `C/user/anaconda3/Lib/site-packages/konlpy/java` 경로를 찾아 가면 된다.

<br>

## 3. 사용자 사전 추가



> *참고* 
>
>  아래의 작업을 로컬 환경에서 진행하려면 윈도우에서 cmd 창을 관리자 권한으로 열어 실행하면 된다. 명령어는 전부 동일하다. 사용자 사전을 추가하고자 하는 경우는 메모장, notepad 등의 텍스트 편집기를 열어서 추가하면 된다.



 `konlpy` 폴더에서 `java` 폴더에 들어 가면 아래와 같이 `open-korean-text-2.1.0.jar` 파일이 있는 것을 확인할 수 있다. 이 묶음 파일 안에 `Okt`의 사전이 들어 있다. 



![okt-jar]({{site.url}}/assets/images/user-dic-3.png){: width="400"}{: .align-center}

<br>

 `java` 폴더로 이동해 아래에 임시 폴더를 만든다. 나는 'aaa'라는 이름으로 만들었다. `os.makedirs`를 이용하긴 했으나, 사실 colabaratory 상에서는 옆의 GUI 환경 상에서 폴더를 만드는 게 훨씬 편하다. `os` 모듈을 이용한다면, 항상 현재 경로를 확인하자.

```python
import os

os.chdir('/usr/local/lib/python3.6/dist-packages/konlpy/java')
os.getcwd() 
os.makedirs('./aaa')
```

<br>

 임시 폴더에 `Okt` 사전 파일의 압축을 풀어 준다.

```python
!jar xvf ../open-korean-text-2.1.0.jar
```

 콘솔 창에 아래와 같은 출력이 나타나면 된다.

```python
created: META-INF/
 inflated: META-INF/MANIFEST.MF
  created: org/
  created: org/openkoreantext/
  created: org/openkoreantext/processor/
  created: org/openkoreantext/processor/normalizer/
  ...
```

<br>

 이후 원하는 품사의 파일을 열어 사용자 사전에 추가한다. 나는 `Nouns`에서 `names.txt`에 추가했다. 

```python
# 사용자 사전 열기
with open(f"/usr/local/lib/python3.6/dist-packages/konlpy/java/aaa/org/openkoreantext/processor/util/noun/names.txt") as f:
    data = f.read()
```



 사용자 사전을 열어 보면, 개행 문자로 각 단어가 구분되어 있다. 

```python
# names.txt 예시
가몽\n가온\n갓세븐\n강새이\n게임닉가\n관우\n귀여미...
```



  따라서 `\n`으로 구분하여 새로운 단어를 추가하고, 해당 파일을 `쓰기 모드`로 열어 새롭게 쓴다.

```python
# 새로운 단어 추가
data += '김재경자경\n송이레만세\n'

# 파일 새롭게 저장
with open("/usr/local/lib/python3.6/dist-packages/konlpy/java/aaa/org/openkoreantext/processor/util/noun/names.txt", 'w') as f:
    f.write(data)
```

<br>

 이제 `java` 폴더의 `open-korean-text-2.1.0.jar`로 다시 압축해 준다.

```python
!jar cvf ../open-korean-text-2.1.0.jar * 
```



 콘솔 창에 아래와 같은 출력이 나타나면 된다.

```python
added manifest
adding: mecab-python-0.996/(in = 0) (out= 0)(stored 0%)
adding: mecab-python-0.996/build/(in = 0) (out= 0)(stored 0%)
adding: mecab-python-0.996/build/lib.linux-x86_64-3.6/(in = 0) (out= 0)(stored 0%)
adding: mecab-python-0.996/build/lib.linux-x86_64-3.6/MeCab.py(in = 15733) (out= 2743)(deflated 82%)
    ...
```



<br>

> *참고* : `startJVM()` 오류 발생 시
>
>  형태소 분석기를 사용하기 위해 로드하면, JVM 오류가 발생하는 경우도 있다. 구글링을 통해 [이 글](https://i-am-eden.tistory.com/9)을 찾아 냈다. Colabaratory에서는 콘솔 창에서 `jvm.py` 파일을 그대로 열어서 `convertStrings=True` 옵션을 없애 주면 된다.
>
> ![JVM-error]({{site.url}}/assets/images/user-dic-4.png){: width="600"}{: .align-center}

<br>



 런타임을 초기화하자. 그리고 추가한 사용자 사전이 제대로 작동되는지 확인한다. 성공했다!



```python
from konlpy.tag import Okt
okt = Okt()
print(okt.nouns('송이레만세')) # ['송이레만세']
print(okt.morphs('김재경자경')) # ['김재경자경']
```

