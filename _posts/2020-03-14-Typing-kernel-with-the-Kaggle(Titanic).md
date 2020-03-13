---
layout: post
title:  Typing kernel with the Kaggle(Titanic)
date:   2020-03-14 00:51:20
categories: typing kernel
tags: Typing Kernel
---





## 노트북 목차

### 파트1: 탐색적 데이터 분석(EDA)
1) 특성 분석  
2) 여러 특성을 고려한 관계 또는 트렌드 찾기   

### 파트2: Feature Engineering 및 데이터 정제
1) 새로운 특성 추가하기  
2) 중복 특성 제거하기  
3) 모델링에 적합한 형태로 특성 변환하기  

### 파트3: 모델링 예측
1) 기본적인 알고리즘  
2) 교차 검증  
3) 앙상블 기법  
4) 중요한 특징 추출  
<br><br>
## 파트1: 탐색적 데이터 분석(EDA)

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
plt.style.use('fivethirtyeight')
import warnings
warnings.filterwarnings('ignore')
%matplotlib inline

data = pd.read_csv('train.csv')
data.head()
```

![img1](https://github.com/star6973/star6973.github.io/blob/master/_posts/typing_kernel_img/titanic/train_data_head.JPG)

  *PassengerId : 탑승객의 고유 아이디*  
  *Survival : 생존유무(0: 사망, 1: 생존)*  
  *Pclass : 선실의 등급*  
  *Name : 이름*  
  *Sex : 성별*  
  *Age : 나이*  
  *Sibsp : 함께 탑승한 형제자매, 아내 남편의 수*  
  *Parch: 함께 탑승한 부모, 자식의 수*  
  *Ticket: 티켓번호*  
  *Fare: 티켓의 요금*  
  *Cabin: 객실번호*  
  *Embarked: 배에 탑승한 위치(C = Cherbourg, Q = Queenstown, S = Southampton)*  


```python
data.isnull().sum() # 결측치 개수 확인
```
![img2](https://github.com/star6973/star6973.github.io/blob/master/_posts/typing_kernel_img/titanic/data_isnull_sum.JPG)

얼마나 많이 살아남았는지 그래프를 통해 확인해보자.
```python
f, ax = plt.subplots(1, 2, figsize=(10, 5))
data['Survived'].value_counts().plot.pie(explode=[0, 0.1], autopct='%1.1f%%', ax=ax[0], shadow=True)
ax[0].set_title('Survived')
ax[0].set_ylabel('')
sns.countplot('Survived', data=data, ax=ax[1])
ax[1].set_title('Survived')
plt.show()
```
![img3](https://github.com/star6973/star6973.github.io/blob/master/_posts/typing_kernel_img/titanic/plt_show_1.JPG)

* 위의 그래프를 통해 훈련셋에 탑승한 891명 중 350명만이 생존하였음을 확인할 수 있다. 더 나아가 데이터에서 생존하지 못한 승객을 파악할 수 있는 많은 정보를 조사해야 한다. 이는 통찰력(Insight)을 얻기 위함이다.
* 데이터셋에서 서로 다른 특성들을 사용하여 생존율을 확인해보자. 이를 위해선 각 특성들을 이해해야 한다.
<br><br>

## 특성의 종류
### 1. 범주형 특성(Categorical Feature)
* 범주형 변수는 둘 이상의 범주가 있는 변수이며, 해당 특성의 각 값을 범주별로 분류할 수 있다. 예를 들어, 성별은 2가지 범주(남성, 여성)를 갖는 범주형 변수이다. 이러한 변수는 정렬하거나 순서를 지정할 수 없기 때문에 명목형 변수라고도 불린다.
* 타이타닉 데이터셋의 범주형 변수: *Sex, Embarked*.

### 2. 순서형 특성(Ordinal Features)
* 순서형 변수는 범주형 변수와 비슷하지만, 값 사이의 상대적인 정렬 혹은 정렬이 가능하다는 점에서 차이가 있다. 예를 들어, Tall, Medium, Short 값을 가진 Height와 같은 변수를 순서형 특성이라 한다.
* 타이타닉 데이터셋의 순서형 변수: *PClass*

### 3. 연속형 특성(Continous Feature)
* 두 지점 사이 혹은 최소값, 최대값 사이의 값을 사용할 수 있는 경우를 연속형 특성이라 한다.
* 타이타닉 데이터셋의 연속형 변수: *Age*  
<br>
### Sex -> Categorical Feature
```python
data.groupby(['Sex', 'Survived'])['Survived'].count()
```
![img4](https://github.com/star6973/star6973.github.io/blob/master/_posts/typing_kernel_img/titanic/data_groupby.JPG)

