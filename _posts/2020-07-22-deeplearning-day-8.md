---
layout: post
title:  Deep Learning [Day 8]
date:   2020-07-22 10:00:00-19:00:00
categories: DeepLearning
tag: DeepLearning
use_math: true
---

> [딥 러닝을 이용한 자연어 처리 입문](https://wikidocs.net/31698){:target="_blank"}

# Deep Learning for NLP 1 [TFIDF and NLP Objective]
## 1. TF-IDF
- 언어적인 특성을 반영하여 단어를 수치화하는 방법은 벡터를 사용하는 것이다.
- 데이터를 표현할 때 기본적으로 one-hot encoding 방식을 사용한다.
    + 하지만 이 방식은 단어의 의미나 특성을 표현할 수 없으며, 단어의 수가 매우 많아지면 고차원 저밀도 벡터를 구성한다.
- 벡터의 크기가 작으면서 단어의 의미를 표현해줄 수 있는 방식이 필요하다.
    + 분포가설에 기반하여, 같은 문맥의 단어(비슷한 위치에 나오는 단어)는 비슷한 의미를 가지는 특징을 사용하여 두 가지 표현법이 있다.  
        1) 카운트 기반 방법: 특정 문맥 안에서 단어들이 동시에 등장하는 횟수를 직접 센다.  
        2) 예측 방법: 신경망 등을 통해 문맥 안의 단어들을 예측한다.  

- TF-IDF(Term Frequency-Inverse Document Frequency)는 단어의 빈도와 역문서 빈도(문서의 빈도에 특정 식을 취함)를 사용하여 DTM 내의 각 단어들마다 중요한 정도를 가중치로 주는 방법이다.
- DTM(Document-Term Matrix, 문서 단어 행렬)이란, 다수의 문서에서 등장하는 각 단어들의 빈도를 행렬로 표현하는 것이다.
- tf(d, t): 특정 문장(d)에서 특정 단어(t)가 몇 번 나오는지
- df(t): 특정 단어(t)가 등장한 문장의 수
- idf(d, t): df(t)에 반비례하는 수
- TF-IDF는 모든 문서에서 자주 등장하는 단어는 중요도가 낮다고 판단하며, 특정 문서에서만 자주 등장하는 단어는 중요도가 높다고 판단한다.
- TF-IDF 값이 낮으면 중요도가 낮은 것이고, 값이 크면 중요도가 큰 것이다. 즉, the나 a와 같은 불용어는 모든 문서에서 자주 나타나기 때문에 TF-IDF의 값이 다른 단어에 비해 낮다.

### 1.1. 사이킷런을 이용한 TF-IDF
```python
from sklearn.feature_extraction.text import TfidfVectorizer
corpus = [
    'you know I want your love',
    'I like you',
    'what should I do ',    
]
tfidfv = TfidfVectorizer().fit(corpus)
print(tfidfv.transform(corpus).toarray())
print(tfidfv.vocabulary_)
```

## 2. 문서 유사도
### 2.1. 자카드 유사도
- A와 B 두 개의 집합이 있을 때, 교집합은 두 개의 집합에서 공통으로 가지고 있는 원소들의 집합이다.
- 합집합에서 교집합의 비율을 구한다면, 두 집합 A와 B의 유사도를 구할 수 있다는 것이 자카드 유사도의 아이디어다.
- A / B
- A: 두 집합의 교집합인 공통된 단어의 개수(unique)
- B: 집합이 가지는 단어의 개수
<center><img src="/assets/images/deeplearning/84.PNG" width="50%"></center><br>

- 장점: 벡터를 사용하는 방법을 몰라도 사용 가능하다.
- 단점: 계란이나 달걀은 같은 단어이지만, 자카드 유사도는 0과 1로만 나타내기 때문에 유사도가 떨어진다.

```jupyter
#%%

doc1 = "apple banana everyone like likey watch card holder"
doc2 = "apple banana coupon passport love you"

tokenized_doc1 = doc1.split()
tokenized_doc2 = doc2.split()

print(tokenized_doc1)
print(tokenized_doc2)

#%%

# 문서1과 문서2의 합집합
union = set(tokenized_doc1).union(set(tokenized_doc2))
print(union)

#%%

# 문서1과 문서2의 교집합
intersection = set(tokenized_doc1).intersection(set(tokenized_doc2))
print(intersection)

#%%

print(len(intersection) / len(union))
```


### 2.2. 코사인 유사도
- 코사인 유사도는 두 개의 벡터값에서 코사인 각도를 구하는 방법이다. 두 벡터의 방향이 동일한 경우 1의 값을 가지며, 90도의 각을 가지면 0, 180도로 반대의 방향을 가지면 -1의 값을 갖게 된다.
- 즉, -1과 1사이의 값을 가지며 1에 가까울수록 비슷하다.
<center><img src="/assets/images/deeplearning/85.PNG" width="50%"></center><br>
 
```jupyter
#%%

import numpy as np
from sklearn.feature_extraction.text import TfidfVectorizer

sent = {"휴일인 오늘도 서쪽을 중심으로 폭염이 이어졌는데요, 내일은 반가운 비 소식이 있습니다.", "피해서 휴일에 놀러왔다가 갑작스런 비로 인해 망연자실하고 있습니다."}

tfidf_vectorizer = TfidfVectorizer()
tfidf_matrix = tfidf_vectorizer.fit_transform(sent)

print(tfidf_matrix)
print('First sentence')
print(tfidf_matrix[0])
print('Second sentence')
print(tfidf_matrix[1])

# Cosine similarity
from sklearn.metrics.pairwise import cosine_similarity
print('Cosine similarity: ', cosine_similarity(tfidf_matrix[0], tfidf_matrix[1]))

#%%

sent = {"영희가 눈물을 훔치며, 말을 이어나갔다.", "철수가 닭똥같은 눈물을 흐리며, 말을 멈추었다."}

tfidf_vectorizer = TfidfVectorizer()
tfidf_matrix = tfidf_vectorizer.fit_transform(sent)

print(tfidf_matrix)
print('First sentence')
print(tfidf_matrix[0])
print('Second sentence')
print(tfidf_matrix[1])
print('Cosine similarity: ', cosine_similarity(tfidf_matrix[0], tfidf_matrix[1]))

#%%

sent = {"밥을 먹는다", "밥을 먹다", "밥을 먹었다"}

tfidf_vectorizer = TfidfVectorizer()
tfidf_matrix = tfidf_vectorizer.fit_transform(sent)

print(tfidf_matrix)
print('First sentence')
print(tfidf_matrix[0])
print('Second sentence')
print(tfidf_matrix[1])
print('Third sentence')
print(tfidf_matrix[2])

print('Cosine similarity: ', cosine_similarity(tfidf_matrix[0], tfidf_matrix[1]))
print('Cosine similarity: ', cosine_similarity(tfidf_matrix[0], tfidf_matrix[2]))
print('Cosine similarity: ', cosine_similarity(tfidf_matrix[1], tfidf_matrix[2]))

#%%

from numpy import dot
from numpy.linalg import norm
import numpy as np

def cos_sim(A, B):
  return dot(A, B)/(norm(A)*norm(B))

# 밥을 / 먹는다 / 먹다 / 먹었다
doc1=np.array([1,0,0,0])
doc2=np.array([1,0,1,0])
doc3=np.array([1,0,0,1])

print(cos_sim(doc1, doc2))
print(cos_sim(doc1, doc3))
print(cos_sim(doc2, doc3))
```


### 2.3. 유클리디언 유사도
- 유클리드 거리는 문서의 유사도를 구할 때 자카드 유사도나 코사인 유사도에 비해 유용한 방법은 아니다.
- 다차원 공간에서 두 점 사이의 거리를 계산하는 유클리드 거리 공식
<center><img src="/assets/images/deeplearning/86.PNG" width="50%"></center><br>

```jupyter
#%%

import numpy as np
def dist(x, y):
  return np.sqrt(np.sum((x-y)**2))

doc1 = np.array((2, 3, 0, 1))
doc2 = np.array((1, 2, 3, 1))
doc3 = np.array((2, 1, 2, 2))
docQ = np.array((1, 1, 0, 1))

print(dist(doc1, docQ))
print(dist(doc2, docQ))
print(dist(doc3, docQ))
```
