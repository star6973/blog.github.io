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

```python
f, ax = plt.subplots(1, 2, figsize=(10, 5))
data[['Sex', 'Survived']].groupby(['Sex']).mean().plot.bar(ax=ax[0])
ax[0].set_title('Survived vs Sex')
sns.countplot('Sex', hue='Survived', data=data, ax=ax[1])
ax[1].set_title('Sex: Survived vs Dead')
plt.show()
```
![img5](https://github.com/star6973/star6973.github.io/blob/master/_posts/typing_kernel_img/titanic/plt_show_2.JPG)

* 타이타닉 호의 승선한 남성의 수는 여성의 수보다 훨씬 많음에도 불구하고 여성의 생존율은 75%, 남성의 생존율은 약 18~19%임을 확인할 수 있다.  
* 위의 결과를 보았을 때, 성별은 중요한 특성임을 파악할 수 있지만, 과연 이것만으로도 충분할까? 다른 특성도 확인해보자.  
<br>

### Pclass -> Ordinal Feature
```python
pd.crosstab(data['Pclass'], data['Survived'], margins=True).style.background_gradient(cmap='Oranges')
```
![img6](https://github.com/star6973/star6973.github.io/blob/master/_posts/typing_kernel_img/titanic/plt_show_3.JPG)

```python
f, ax = plt.subplots(1, 2, figsize=(10, 5))
data['Pclass'].value_counts().plot.bar(color=['#CD7F32', '#FFDF00', '#D3D3D3'], ax=ax[0])
ax[0].set_title('Number of Passengers By Pclass')
ax[0].set_ylabel('Count')
sns.countplot('Pclass', hue='Survived', data=data, ax=ax[1])
ax[1].set_title('Pclass: Survived vs Dead')
plt.show()
```
![img7](https://github.com/star6973/star6973.github.io/blob/master/_posts/typing_kernel_img/titanic/plt_show_4.JPG)

* Pclass 변수는 선실의 등급으로, 비행기 좌석의 클래스와 같은 개념이라고 생각하면 된다. Pclass 3의 승객 수가 가장 많지만 생존 비율을 확인했을 때, 구조 순서가 선실의 등급의 우선 순위가 부여되어 있음을 파악할 수 있다.
* Pclass 1의 생존율은 약 63%, Pclass 2의 생존율은 약 48%, Pclass 3의 생존율은 약 25%를 보이고 있다.
* 다른 특성들도 더 파악해보기 전에 Sex와 Pclass를 함께 살펴보자.

```python
pd.crosstab([data['Sex'], data['Survived']], data['Pclass'], margins=True).style.background_gradient(cmap='Oranges')
```
![img8](https://github.com/star6973/star6973.github.io/blob/master/_posts/typing_kernel_img/titanic/plt_show_5.JPG)

```python
sns.factorplot('Pclass', 'Survived', hue='Sex', data=data)
plt.show()
```
![img9](https://github.com/star6973/star6973.github.io/blob/master/_posts/typing_kernel_img/titanic/plt_show_6.JPG)

* FactorPlot을 사용하여 범주형 변수를 쉽게 분리할 수 있다.
* 위의 그래프를 통해 Pclass 1에서 94명의 여성 중 3명만이 사망한 것처럼 Pclass 1 여성의 생존율은 약 95~96%임을 파악할 수 있다.
* Pclass 1의 남성의 생존율이 낮은 것을 보아, 여성에게 최우선 구조 순위가 부여됨을 확인할 수 있다.
<br>

### Age -> Continous Feature
```python
print('가장 늙은 사람의 나이: {:.2f}'.format(data['Age'].max()), '세')
print('가장 어린 사람의 나이: {:.2f}'.format(data['Age'].min()), '세')
print('평균 나이: {:.2f}'.format(data['Age'].mean()), '세')
```
![img10](https://github.com/star6973/star6973.github.io/blob/master/_posts/typing_kernel_img/titanic/print_1.JPG)

```python
f, ax = plt.subplots(1, 2, figsize=(10, 5))
sns.violinplot('Pclass', 'Age', hue='Survived', data=data, split=True, ax=ax[0])
ax[0].set_title('Pclass and Age vs Survived')
ax[0].set_yticks(range(0, 110, 10))
sns.violinplot('Sex', 'Age', hue='Survived', data=data, split=True, ax=ax[1])
ax[1].set_title('Sex and Age vs Survived')
ax[1].set_yticks(range(0, 110, 10))
plt.show()
```
![img11](https://github.com/star6973/star6973.github.io/blob/master/_posts/typing_kernel_img/titanic/plt_show_7.JPG)

그래프를 통해 확인할 수 있는 점(Pclass와 Age의 생존율, Sex와 Age의 생존율)
* Pclass의 경우, 10세 미만의 어린이의 생존율이 양호해 보인다.  
* Pclass1에서 20-50세의 승객의 생존율이 높고, 여성의 경우 남성보다 생존율이 더 높다.  
* 남성의 경우 생존율은 나이가 증가할 수록 감소함을 보인다.  
<br>

앞에서 살펴본 것처럼, Age의 특성은 177개의 결측치를 가지고 있다. NaN 값을 대체하기 위해선 데이터셋의 평균 나이로 할당할 수 있다.

하지만, 연령층이 높은 사람들이 꽤 많다는 것을 알 수 있다. 때문에 실제 4살의 아이를 평균 연령 29살로 할당하는 것은 바람직하지 않다. 그렇다면 어떻게 승객이 어떤 연령대인지를 확인할 수 있을까?

정답은 바로 이름에서 힌트를 얻을 수 있다. 이름에는 Mr. 또는 Mrs.와 같은 호칭이 붙어있기에 이 평균값을 각 그룹에 할당할 수 있다.

```python
data['Initial'] = 0
for i in data:
    data['Initial'] = data['Name'].str.extract('([A-Za-z]+)\.')
```
위의 코드에서 정규표현식을 사용하여 이름의 A-Z 또는 a-z 사이에 있고, .(점)이 있는 문자열을 추출하는 것이다. 따라서 이름에서 호칭을 성공적으로 추출할 수 있다.

```python
pd.crosstab(data['Initial'], data['Sex']).T.style.background_gradient(cmap='Oranges')
```
![img12](https://github.com/star6973/star6973.github.io/blob/master/_posts/typing_kernel_img/titanic/plt_show_8.JPG)

Mlle이나 Mme와 같은 맞춤법이 틀린 이니셜은 Miss를 의미하며, Dr의 경우 Mr로 변경할 수 있다.

```python
data['Initial'].replace(['Mlle', 'Mme', 'Ms', 'Dr', 'Major', 'Lady', 'Countess', 'Jonkheer', 'Col', 'Rev', 'Capt', 'Sir', 'Don'], 
                        ['Miss', 'Miss', 'Miss', 'Mr', 'Mr', 'Mrs', 'Mrs', 'Other', 'Other', 'Other', 'Mr', 'Mr', 'Mr'], inplace=True)
data.groupby('Initial')['Age'].mean()
```
![img13](https://github.com/star6973/star6973.github.io/blob/master/_posts/typing_kernel_img/titanic/plt_show_9.JPG)

### Age의 결측치 채우기
```python
data.loc[(data['Age'].isnull()) & (data['Initial'] == 'Mr'), 'Age'] = 33
data.loc[(data['Age'].isnull()) & (data['Initial'] == 'Mrs'), 'Age'] = 36
data.loc[(data['Age'].isnull()) & (data['Initial'] == 'Master'), 'Age'] = 5
data.loc[(data['Age'].isnull()) & (data['Initial'] == 'Miss'), 'Age'] = 22
data.loc[(data['Age'].isnull()) & (data['Initial'] == 'Other'), 'Age'] = 46

data['Age'].isnull().any()
```
![img14](https://github.com/star6973/star6973.github.io/blob/master/_posts/typing_kernel_img/titanic/print_2.JPG)





