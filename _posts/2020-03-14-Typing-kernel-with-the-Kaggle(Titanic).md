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

![data.head()](https://github.com/star6973/star6973.github.io/blob/master/_posts/typing_kernel_img/titanic/train_data_head.JPG)

PassengerId : 탑승객의 고유 아이디  
Survival : 생존유무(0: 사망, 1: 생존)  
Pclass : 선실의 등급  
Name : 이름  
Sex : 성별  
Age : 나이  
Sibsp : 함께 탑승한 형제자매, 아내 남편의 수  
Parch: 함께 탑승한 부모, 자식의 수  
Ticket: 티켓번호  
Fare: 티켓의 요금  
Cabin: 객실번호  
Embarked: 배에 탑승한 위치(C = Cherbourg, Q = Queenstown, S = Southampton)  


```

```



