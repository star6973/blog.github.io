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

Age의 결측치 채우기
```python
data.loc[(data['Age'].isnull()) & (data['Initial'] == 'Mr'), 'Age'] = 33
data.loc[(data['Age'].isnull()) & (data['Initial'] == 'Mrs'), 'Age'] = 36
data.loc[(data['Age'].isnull()) & (data['Initial'] == 'Master'), 'Age'] = 5
data.loc[(data['Age'].isnull()) & (data['Initial'] == 'Miss'), 'Age'] = 22
data.loc[(data['Age'].isnull()) & (data['Initial'] == 'Other'), 'Age'] = 46

data['Age'].isnull().any()
```

```python
f, ax = plt.subplots(1, 2, figsize=(10, 5))
data[data['Survived'] == 0]['Age'].plot.hist(ax=ax[0], bins=20, edgecolor='black', color='red')
ax[0].set_title('Survived = 0')
x1 = list(range(0, 85, 5))
ax[0].set_xticks(x1)
data[data['Survived'] == 1]['Age'].plot.hist(ax=ax[1], bins=20, edgecolor='black', color='green')
ax[1].set_title('Survived = 1')
x2 = list(range(0, 85, 5))
ax[1].set_xticks(x2)
plt.show()
```
![img14](https://github.com/star6973/star6973.github.io/blob/master/_posts/typing_kernel_img/titanic/plt_show_10.JPG)

* 나이가 5살 미만인 유아들이 많이 생존했음을 확인할 수 있다.
* 가장 나이가 많은 80세의 승객이 구해졌다.
* 생존하지 못한 승객의 연령 그룹은 30-40세가 가장 많다.

```python
sns.factorplot('Pclass', 'Survived', col='Initial', data=data)
plt.show()
```
![img15](https://github.com/star6973/star6973.github.io/blob/master/_posts/typing_kernel_img/titanic/plt_show_11.JPG)

영화에서도 나왔듯이, 여성과 아이의 구조가 우선이라는 원칙이 그래프를 통해 입증할 수 있다.
<br>

### Embarked -> Categorical Value
```python
pd.crosstab([data['Embarked'], data['Pclass']], [data['Sex'], data['Survived']], margins=True).style.background_gradient(cmap='Oranges')
```
![img16](https://github.com/star6973/star6973.github.io/blob/master/_posts/typing_kernel_img/titanic/plt_show_12.JPG)


승선 항의 위치에 따른 생존율
```python
sns.factorplot('Embarked', 'Survived', data=data)
fig = plt.gcf()
fig.set_size_inches(5, 3)
plt.show()
```
![img16](https://github.com/star6973/star6973.github.io/blob/master/_posts/typing_kernel_img/titanic/plt_show_13.JPG)

C(Cherbourg) 승선 위치에서의 생존율은 55%로 가장 높으며, S(Southampton) 승선 위치에서의 생존율은 가장 낮다.
```python
f, ax = plt.subplots(2, 2, figsize=(10, 5))
sns.countplot('Embarked', data=data, ax=ax[0, 0])
ax[0, 0].set_title('No. Of Passengers Boarded')
sns.countplot('Embarked', hue='Sex', data=data, ax=ax[0, 1])
ax[0, 1].set_title('Male-Female Split for Embarked')
sns.countplot('Embarked', hue='Survived', data=data, ax=ax[1, 0])
ax[1, 0].set_title('Embarked vs Survived')
sns.countplot('Embarked', hue='Pclass', data=data, ax=ax[1, 1])
ax[1, 1].set_title('Embarked vs Pclass')

plt.subplots_adjust(wspace=0.2, hspace=0.5)
plt.show()
```
![img17](https://github.com/star6973/star6973.github.io/blob/master/_posts/typing_kernel_img/titanic/plt_show_14.JPG)

* S 승선 위치에서 온 대다수는 Pclass 3의 사람들이다.
* C 승선의 승객은 다른 위치에 비해 생존율이 높다. 아마도 Pclass 1의 비율이 높기 때문일 것이다.
* S 승선의 승객은 Pclass 3의 승객이 약 81%가 생존하지 못했기에 전체 생존율에서는 낮다.
* Q 승선의 승객은 95%가 Pclass 3의 사람들이다.

```python
sns.factorplot('Pclass', 'Survived', hue='Sex', col='Embarked', data=data)
plt.show()
```
![img18](https://github.com/star6973/star6973.github.io/blob/master/_posts/typing_kernel_img/titanic/plt_show_15.JPG)

* Pclass와 상관없이 Pclass 1, Pclass 2의 여성의 생존 확률은 거의 1이다.
* S 승선의 승객들에서 Pclass 3의 승객들은 생존율이 매우 낮음을 볼 수 있다.
* Q 승선의 승객에서는 전체 남자들의 생존율이 매우 낮음을 볼 수 있다.

Embarked의 결측치 채우기

S 승선의 승객 수가 최대임을 보아, 결측치는 S로 대체하겠다.
```python
data['Embarked'].fillna('S', inplace=True)
data['Embarked'].isnull().any()
```
<br>

### SibSp -> Discrete Feature

이 특성은 가족 구성원 수를 나타내는 것이다.  
* Sibling = 형제, 자매, 의붓형제, 이복 누이  
* Spouse = 남편, 아내 

```python
pd.crosstab(data['SibSp'], data['Survived']).style.background_gradient(cmap='Oranges')
```
![img19](https://github.com/star6973/star6973.github.io/blob/master/_posts/typing_kernel_img/titanic/plt_show_16.JPG)

```python
fig = plt.figure(figsize=(10, 5))
sns.barplot('SibSp', 'Survived', data=data)
fig.suptitle('SibSp vs Survived')

plt.close(2)
plt.show()
```
![img20](https://github.com/star6973/star6973.github.io/blob/master/_posts/typing_kernel_img/titanic/plt_show_17.JPG)

```python
pd.crosstab(data['SibSp'], data['Pclass']).style.background_gradient(cmap='Oranges')
```
![img21](https://github.com/star6973/star6973.github.io/blob/master/_posts/typing_kernel_img/titanic/plt_show_18.JPG)

* 그래프를 통해 승객이 혼자 탑승한 경우 생존율이 34.5% 정도 되며, 형제 수가 증가할 수록 생존율이 줄어드는 것을 확인할 수 있다. 이는 말이 될 수 있는 것이, 가족이 있다면 나보다는 가족을 먼저 살리기 위해서 노력할 것이다.
* 놀라운 부분은 5-8인의 가족의 생존율은 0%이다. 이러한 이유는 무엇일까? 크로스탭으로 Pclass의 비율을 보면 형제 자매의 수가 3인을 초과하는 경우 모두 Pclass 3의 승객임을 알 수 있다. 즉, Pclass 3의 모든 대가족이 생존하지 못했다.
<br>

### Parch

함께 탑승한 부모, 자식의 수
```python
pd.crosstab(data['Parch'], data['Pclass']).style.background_gradient(cmap='Oranges')
```
![img22](https://github.com/star6973/star6973.github.io/blob/master/_posts/typing_kernel_img/titanic/plt_show_19.JPG)

크로스탭으로 Parch를 확인해보면 Pclass 3일 수록 더 많은 수의 인원이 있음을 확인할 수 있다.
```python
fig = plt.figure(figsize=(10, 5))
sns.barplot('Parch', 'Survived', data=data)
plt.suptitle('Parch vs Survived')
plt.close(2)
plt.show()
```
![img23](https://github.com/star6973/star6973.github.io/blob/master/_posts/typing_kernel_img/titanic/plt_show_20.JPG)

* 여기서도 결과가 비슷하다. 부모와 함께 탑승한 승객의 생존율이 더 높다. 그러나 숫자가 증가할 수록 생존율이 낮아진다.
* 1-3명의 부모가 있는 경우에 생존의 기회가 높아지지만, 혼자이거나 4명 이상의 부모가 있는 경우 생존율이 낮아진다.
<br>

### Fare -> Continous Feature

```python
print('가장 비싼 운임료: {:.2f}'.format(data['Fare'].max()))
print('가장 싼 운임료: {:.2f}'.format(data['Fare'].min()))
print('평균 운임료: {:.2f}'.format(data['Fare'].mean()))
```
![img24](https://github.com/star6973/star6973.github.io/blob/master/_posts/typing_kernel_img/titanic/print_3.JPG)

```python
f, ax = plt.subplots(1, 3, figsize=(10, 5))
sns.distplot(data[data['Pclass'] == 1]['Fare'], ax=ax[0])
ax[0].set_title('Fares in Pclass 1')
sns.distplot(data[data['Pclass'] == 2]['Fare'], ax=ax[1])
ax[1].set_title('Fares in Pclass 2')
sns.distplot(data[data['Pclass'] == 3]['Fare'], ax=ax[2])
ax[2].set_title('Fares in Pclass 3')

plt.show()
```
![img25](https://github.com/star6973/star6973.github.io/blob/master/_posts/typing_kernel_img/titanic/plt_show_21.JPG)

Pclass 1의 승객들의 운임료를 보면 큰 분포가 있는 것으로 보인다. 그리고 이 분포는 표준이 감소함에 따라 줄어드는 것을 확인할 수 있다. 이러한 이산형 데이터는 Binning 기법을 사용하여 불연속값으로 변환할 수 있다.

* Binning: 대표적인 변수 가공(Feature Engineering) 기법 중의 하나로 수치형 변수를 범주형 변수로 변형하는 작업.
* 예를 들어, 직원의 나이에 따라 청소년(<20), 청년(20<30), 청장년(30<55), 장년(55>=) 등으로 구분하는 작업이 binning 기법이다.
<br>

### 모든 특성에 대한 요약
* #### Sex: 여성의 생존율이 남성에 비해 높음
* #### Pclass: 1등석 승객이 더 생존율이 높음. 여성의 경우 1등석의 생존율은 거의 1이며, 2등석 역시 매우 높음.
* #### Age: 5-10세 미만의 어린이는 생존율이 높음. 15-35세 그룹의 승객의 사망자 수가 높음.
* #### Embarked: C 승선 위치에서의 생존율이 S 승선 위치에서의 1등석 승객의 생존율 보다 높은 것으로 보임. Q 승선 위치의 대다수 승객은 3등석임.
* #### Parch + SibSp: 1-2명의 형제 자매, 배우자 또는 1-3명의 부모님이 있는 경우 혼자거나 대가족에 비해 생존율이 높음.

### 특성간의 상관관계
```python
sns.heatmap(data.corr(), annot=True, cmap='RdYlGn', linewidths=0.2)
fig = plt.gcf()
fig.set_size_inches(10, 8)
plt.show()
```
![img26](https://github.com/star6973/star6973.github.io/blob/master/_posts/typing_kernel_img/titanic/plt_show_22.JPG)

### 히트맵 해석
* 양의 상관관계: 특성 A가 증가할 때, 특성 B도 증가한다면 양의 상관관계
* 음의 상관관계: 특성 A가 증가할 때, 특성 B가 감소한다면 음의 상관관계

만약 두개의 특성이 서로 상관관계가 있다면, 두 특성 모두 매우 유사한 정보를 포함하고 있다는 것을 의미한다. 이렇게 동일한 정보를 포함하는 특성이 많은 경우를 다중공선성이라 한다.

따라서 특성이 중복되므로 제거를 통해 모델링하거나 훈련하는 시간을 줄일 수 있다. 위의 히트맵에서는 다중공선성은 확인되지 않으며, 가장 높은 상관관계는 SibSp와 Parch라고 할 수 있다.
<br><br>

## 파트2: Feature Engineering 및 데이터 정제

Feature Engineering이란? 모든 특성이 데이터셋에서 중요하지는 않다. 제거해야 할 중복 특성이 많이 있을 수 있다. 또한, 다른 특성에서 정보를 추출하여 새로운 특성을 만들어 추가할 수도 있다. 이러한 과정을 Feature Engineering이라 한다.

### Age_band

* Age의 특성이 가지고 있는 문제: Age는 연속형 변수로 머신러닝 모델에서 사용하기에 부적합하다. 만약 나이별로 그룹화를 하고자 한다면, 나이의 기준을 잡아서 범주형 변수로 변환하는 것이 옳을 것이다.

* Binning 기법 사용: 승객의 최대 연령은 80세이기 때문에 0-80에서 5 bin으로 범위를 나눈다. 따라서 80 / 5 = 16이 구간별 크기이다.

```python
data['Age_band'] = 0
data.loc[data['Age']<=16, 'Age_band'] = 0
data.loc[(data['Age']>16) & (data['Age']<=32), 'Age_band'] = 1
data.loc[(data['Age']>32) & (data['Age']<=48), 'Age_band'] = 2
data.loc[(data['Age']>48) & (data['Age']<=64), 'Age_band'] = 3
data.loc[data['Age']>64, 'Age_band'] = 4

data['Age_band'].value_counts().to_frame().style.background_gradient(cmap='Oranges')
```
![img27](https://github.com/star6973/star6973.github.io/blob/master/_posts/typing_kernel_img/titanic/plt_show_23.JPG)

```python
sns.factorplot('Age_band', 'Survived', data=data, col='Pclass')
plt.show()
```
![img28](https://github.com/star6973/star6973.github.io/blob/master/_posts/typing_kernel_img/titanic/plt_show_24.JPG)
Pclass와 상관없이 나이가 증가함에 따라 생존율이 감소함을 볼 수 있다.

### Family_Size and Alone

새로운 특성인 'Family_Size'와 'Alone'을 추가하여 분석해보자. 이 특성들은 Parch와 SibSp 특성의 요약으로, 생존율이 승객의 가족 규모와 관련이 있는지 확인해볼 수 있는 데이터가 된다.
```python
data['Family_Size'] = 0
data['Family_Size'] = data['Parch'] + data['SibSp']
data['Alone'] = 0
data.loc[data['Family_Size']==0, 'Alone'] = 1

f = plt.figure(figsize=(10, 5))
sns.factorplot('Family_Size', 'Survived', data=data)
f.suptitle('Family_Size vs Survived')
plt.show()
```
![img29](https://github.com/star6973/star6973.github.io/blob/master/_posts/typing_kernel_img/titanic/plt_show_25.JPG)

```python
g = plt.figure(figsize=(10, 5))
sns.factorplot('Alone', 'Survived', data=data)
g.suptitle('Alone vs Survived')
plt.show()
```
![img30](https://github.com/star6973/star6973.github.io/blob/master/_posts/typing_kernel_img/titanic/plt_show_26.JPG)
Family_Size가 0인 경우는 승객이 혼자임을 의미한다. 위의 그래프를 통해서 만약 승객이 혼자이거나 가족의 수가 0이라면, 생존율이 매우 낮음을 확인할 수 있다. 마찬가지로 가족의 수가 4보다 크면, 생존율도 낮아진다. 이러한 결과를 통해 Faimily_Size는 매우 중요한 특성이라고 할 수 있다.





























