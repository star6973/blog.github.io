---
layout: post
title:  Typing Kernel with the Kaggle(Home Credit PART 2)
date:   2020-03-25 05:30:20
tags: Kernel
---

# 캐글 커널 필사하기
[Introduction: Home Credit Default Risk Competition - Will Koehrsen](https://www.kaggle.com/willkoehrsen/start-here-a-gentle-introduction)
<br>
## Feature Engineering 메뉴얼 파트 2 (Manual Feature Engineering Part Two)

Home Credit에서는 `application` 데이터로만 사용하여 모델을 만들었다. 가장 최상의 모델의 점수는 LightGBM을 사용하여 0.75 정도였다. 이 점수를 더 향상시키기 위해서, 다른 데이터프레임으로부터 더 많은 정보를 추가할 것이다.

+ `bureau`: Home Credit에 보고된 다른 금융기관의 이전 대출에 대한 정보
+ `bureau_balance`: 이전 대출에 대한 월간 정보. 각 달은 한 행씩.

우리는 최대한 많은 정보를 가져와서 모델에 사용하는 것이 목표이다. 이후에 우리는 PCA와 같은 기법으로부터의 특성의 중요도를 사용하기 위해 차원축소를 수행할 것이다.

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
import warnings
warnings.filterwarnings('ignore')
plt.style.use('fivethirtyeight')
```
<br>

### Counts of a client's previous loans

우리는 먼저 고객이 다른 금융기관으로부터 이전의 대출금의 수를 간단히 파악할 것이다. 이를 위해선 pandas의 다양한 명령을 사용해야 한다.

+ `groupby`: `SK_ID_CURR` 컬럼과 같은 고유한 고객을 그룹핑한다.
+ `agg`: 컬럼들의 평균을 구하는 것과 같은 그룹핑된 데이터들을 계산하는 것이다.
+ `merge`: 집계된 통계를 해당 고객에게 일치시킨다. 우리는 `SK_ID_CURR` 컬럼으로 계산된 통계를 기존의 훈련 데이터에 병합시켜야 할 필요가 있다. `SK_ID_CURR` 컬럼은 고객들에게 해당 통계가 없으면 `NaN` 값을 넣을 것이다.

```python
bureau = pd.read_csv('../input/home-credit-default-risk/bureau.csv')
bureau.head()
```
<img src="/assets/images/typing/home-credit2/1.PNG" width="100%"><br>

```python
# Group by the client id (SK_ID_CURR)
previous_loan_counts = bureau.groupby('SK_ID_CURR', as_index=False)['SK_ID_BUREAU'].count().rename(columns = {'SK_ID_BUREAU': 'previous_loan_counts'})
previous_loan_counts.head()
```
<img src="/assets/images/typing/home-credit2/2.PNG" width="60%"><br>

```python
train = pd.read_csv('../input/home-credit-default-risk/application_train.csv')
train = train.merge(previous_loan_counts, on='SK_ID_CURR', how='left')

train['previous_loan_counts'] = train['previous_loan_counts'].fillna(0)
train.head()
```
<img src="/assets/images/typing/home-credit2/3.PNG" width="100%"><br>
<br><br>

## Assessing Usefulness of New Variable with r value

새로운 변수가 유용하다는 것을 결정하기 위해, 우리는 이 변수와 타겟 사이에 Pearson 상관관계 계수(r-value)를 계산할 수 있다. 이 값은 두 개의 변수들 사이에 선형적인 관계와 -1(완벽한 음의 상관관계)에서 1(완벽한 양의 상관관계)사이에 범위를 측정할 수 있다.

r-value는 새로운 변수의 "유용하지 않음"을 측정하기 위한 가장 좋은 방법은 아니지만, 처음으로 머신러닝 모델을 만드는 데 도움을 줄 수 있는 근사치를 제공할 수 있다. 타겟에 대한 변수의 r-value의 값이 클수록 이 변수가 타겟에 영향을 미칠 가능성이 더 높아진다. 그러므로, 대상과 비교하여 절댓값 r-value가 가장 큰 변수를 찾아야 한다.

### Kernel Density Estimate Plots

`previous_loan_count`가 `TARGET`이 1인지 0인지에 따라 색이 바뀌도록 만들어준다. KDE 그림의 결과는 사람들이 자신의 대출금을 갚을 수 없는지(`TARGET == 1`)와 있는지(`TARGET == 0`) 사이에 있는 변수의 분포에서 유의미한 차이를 보여줄 것이다.

```python
def kde_target(var_name, df):
    
    # 새로운 변수와 타겟 사이에 상관관계 계수 계산하기
    corr = df['TARGET'].corr(df[var_name])
    
    # 제때 갚을 수 있는지 vs 없는지에 대한 평균값 계산하기
    avg_repaid = df.ix[df['TARGET'] == 0, var_name].median()
    avg_not_repaid = df.ix[df['TARGET'] == 1, var_name].median()
    
    plt.figure(figsize=(12, 6))
    
    sns.kdeplot(df.ix[df['TARGET'] == 0, var_name], label='TARGET == 0')
    sns.kdeplot(df.ix[df['TARGET'] == 1, var_name], label='TARGET == 1')
    
    plt.xlabel(var_name)
    plt.ylabel('Density')
    plt.title('%s Distribution' % var_name)
    plt.legend()
    
    print('The correlation between %s and the TARGET is %0.4f' % (var_name, corr))
    print('Median value for loan that was not repaid = %0.4f' % avg_not_repaid)
    print('Median value for loan that was repaid = %0.4f' % avg_repaid)
```

우리는 이 함수를 Home Credit에서 가장 중요하게 나온 변수 `EXT_SOURCE_3`에 적용해 볼 수 있을 것이다.

```python
kde_target('EXT_SOURCE_3', train)
```
<img src="/assets/images/typing/home-credit2/4.PNG" width="70%"><br>

이제 우리가 방금 만든 새로운 변수와 다른 기관의 이전 대출 건수를 적용해보자.

```python
kde_target('previous_loan_counts', train)
```
<img src="/assets/images/typing/home-credit2/5.PNG" width="70%"><br>

위의 분포는 상관관계 계수가 너무나 약하기 때문에 중요하다고 확신할 수 없다. ㄸ라서 데이터프레임에서 몇 가지 변수를 더 만들어보자.
<br><br>

## Aggregating Numeric Columns

`bureau` 데이터프레임에서 수치 정보를 설명하기 위해, 우리는 모든 숫자 컬럼에 대한 통계를 계산하였다. 마찬가지로, 고객의 id를 기준으로 그룹핑하고, 그룹핑된 데이터프레임을 집계하여, 결과를 훈련셋과 다시 병합한다.

`agg` 함수는 작업이 유요한 것으로 간주되는 숫자 컬럼의 값만 계산한다.

```python
bureau_agg = bureau.drop(columns = ['SK_ID_BUREAU']).groupby('SK_ID_CURR', as_index = False).agg(['count', 'mean', 'max', 'min', 'sum']).reset_index()
bureau_agg.head()
```
<img src="/assets/images/typing/home-credit2/6.PNG" width="100%"><br>

우리는 이러한 각각의 컬럼들에 새로운 이름을 만들어야 한다. 다음에 나오는 코드는 이름에 stat을 추가하여 새 이름을 만든다.

```python
columns = ['SK_ID_CURR']

for var in bureau_agg.columns.levels[0]:
    if var != 'SK_ID_CURR':
        for stat in bureau_agg.columns.levels[1][:-1]:

            columns.append('bureau_%s_%s' % (var, stat))
```

```python
bureau_agg.columns = columns
bureau_agg.head()
```
<img src="/assets/images/typing/home-credit2/7.PNG" width="100%"><br>

이제 훈련셋과 우리가 지금까지 만들어 놓은 데이터프레임을 병합하자.

```python
train = train.merge(bureau_agg, on='SK_ID_CURR', how='left')
train.head()
```
<img src="/assets/images/typing/home-credit2/8.PNG" width="100%"><br>

### 타겟과 함께 집계된 값들의 상관관계

```python
new_corrs = []

for col in columns:
    corr = train['TARGET'].corr(train[col])
    new_corrs.append((col, corr))
```

```python
# Sort the correlations by the absolute value
new_corrs = sorted(new_corrs, key=lambda x: abs(x[1]), reverse=True)
new_corrs[:15]
```
<img src="/assets/images/typing/home-credit2/9.PNG" width="70%"><br>

새로운 변수들 중 어느 것도 `TARGET`과 유의미한 상관관계를 가지고 있지 않다. KDE 그림으로 가장 높은 상관관계 변수를 확인해보자.

```python
kde_target('bureau_DAYS_CREDIT_mean', train)
```
<img src="/assets/images/typing/home-credit2/10.PNG" width="70%"><br>

이 컬럼은 Home Credit에서 대출을 위한 지원서를 작성하기 전에 대출금을 신청한 일수다. 따라서 더 큰 음수가 나타나면, 현재의 대출 신청하기 이전에 대출이 더 있었음을 나타낸다.

위의 그래프를 통해 과거에 대출을 더 많이 신청했던 고객들이 잠재적으로 Home Credit으로 대출금을 상환활 가능성이 높다는 것을 의미한다. 

### The Multiple Comparisons Problem

우리는 수백 개의 특성을 만들어 낼 수 있지만, 몇몇은 단순히 데이터의 랜덤한 이상치 때문에 타겟과 상관관계가 있다는 것을 판명할 수 있다.

### Functions for Numeric Aggregations

이전의 모든 함수들을 캡슐화한다. 이렇게하면 모든 데이터프레임에서 수치형 컬럼에 대한 통계집계를 계산할 수 있다. 우리는 다른 데이터프레임에도 동일한 작업을 적용하고자 할 때 이 함수를 다시 사용할 것이다.

```python
def agg_numeric(df, group_var, df_name):

    # 그룹핑된 변수 이외의 ID 변수 제거
    for col in df:
        if col != group_var and 'SK_ID' in col:
            df = df.drop(columns=col)
            
    group_ids = df[group_var]
    numeric_df = df.select_dtypes('number')
    numeric_df[group_var] = group_ids
    
    # 특정한 변수들을 그룹핑하고 통계치를 계산
    agg = numeric_df.groupby(group_var).agg(['count', 'mean', 'max', 'min', 'sum']).reset_index()
    
    # 새로운 컬럼명을 만들 필요가 있음
    columns = [group_var]
    
    for var in agg.columns.levels[0]:
        if var != group_var:
            for stat in agg.columns.levels[1][:-1]:
                columns.append('%s_%s_%s' % (df_name, var, stat))
                
    agg.columns = columns
    
    return agg
```

```python
bureau_agg_new = agg_numeric(bureau.drop(columns=['SK_ID_BUREAU']), group_var='SK_ID_CURR', df_name='bureau')
bureau_agg_new.head()
```
<img src="/assets/images/typing/home-credit2/11.PNG" width="100%"><br>

### Correlation Function

```python
def target_corrs(df):
    
    corrs = []
    
    for col in df.columns:
        print(col)
        
        if col != 'TARGET':
            corr = df['TARGET'].corr(df[col])
            corrs.append((col, corr))
            
    corrs = sorted(corrs, key=lambda x: abs(x[1]), reverse=True)
    
    return corrs
```
<br><br>

## 범주형 변수

이제 범주형 변수를 살펴보자. 이들은 문자열 변수로, mean, max와 같은 수치형 변수들에 사용되는 통계 계산에 사용할 수 없다. 대신에, 우리는 각 범주형 변수와 함께 있는 각각의 카테고리의 수를 계산하는 것에 의존할 수 있다.

예를 들어, 다음과 같은 테이블을 보자.

| SK_ID_CURR | Loan type |
|------------|-----------|
| 1          | home      |
| 1          | home      |
| 1          | home      |
| 1          | credit    |
| 2          | credit    |
| 3          | credit    |
| 3          | cash      |
| 3          | cash      |
| 4          | credit    |
| 4          | home      |
| 4          | home      |

<br>
우리는 각 고객마다 각각의 범주형 타입의 개수를 구한 정보를 사용할 것이다.

| SK_ID_CURR | credit count | cash count | home count | total count |
|------------|--------------|------------|------------|-------------|
| 1          | 1            | 0          | 3          | 4           |
| 2          | 1            | 0          | 0          | 1           |
| 3          | 1            | 2          | 0          | 3           |
| 4          | 1            | 0          | 2          | 3           |

<br>
그리고 우리는 이러한 각 범주의 전체 발생비율 대비 값들의 개수를 정규화할 수 있다.

| SK_ID_CURR | credit count | cash count | home count | total count | credit count norm | cash count norm | home count norm |
|------------|--------------|------------|------------|-------------|-------------------|-----------------|-----------------|
| 1          | 1            | 0          | 3          | 4           | 0.25              | 0               | 0.75            |
| 2          | 1            | 0          | 0          | 1           | 1.00              | 0               | 0               |
| 3          | 1            | 2          | 0          | 3           | 0.33              | 0.66            | 0               |
| 4          | 1            | 0          | 2          | 3           | 0.33              | 0               | 0.66            |

<br>

우리는 처음에 범주형 컬럼을 원-핫 인코딩을 사용할 것이다.

```python
categorical = pd.get_dummies(bureau.select_dtypes('object'))
categorical['SK_ID_CURR'] = bureau['SK_ID_CURR']
categorical.head()
```
<img src="/assets/images/typing/home-credit2/12.PNG" width="100%"><br>

```python
categorical_grouped = categorical.groupby('SK_ID_CURR').agg(['sum', 'mean'])
categorical_grouped.head()
```
<img src="/assets/images/typing/home-credit2/13.PNG" width="100%"><br>

합은 관련 고객에 대한 해당 범주의 개수를 나타내고, 평균은 개수의 정규화된 값을 나타낸다.

이전과 유사한 함수를 사용하여 컬럼의 이름을 바꿀 수 있다. 우리는 다시 한 번 컬럼의 다중 레벨 인덱스를 다뤄야 한다. 카테고리 변수와 함께 추가된 범주형 변수의 이름인 1단계(레벨 0)까지 반복한다. 그리고나서 각 고객에 대해 계산한 통계를 반복한다. 우리는 통계치와 함께 레벨 0이 추가된 열 이름을 사용하여 열의 이름을 바꿀 것이다. 예를 들어, `CREDIT_ACTIVE_Active`는 `level 0`로, `level 1`로 합친 컬럼은 `CREDIT_ACTIVE_Active_count`로 설정한다.

```python
categorical_grouped.columns.levels[0][:10]
```
<img src="/assets/images/typing/home-credit2/14.PNG" width="60%"><br>

```python
categorical_grouped.columns.levels[1]
```
<img src="/assets/images/typing/home-credit2/15.PNG" width="60%"><br>

```python
group_var = 'SK_ID_CURR'

columns = []

for var in categorical_grouped.columns.levels[0]:
    if var != group_var:
        for stat in ['count', 'count_norm']:
            columns.append('%s_%s' % (var, stat))
            
categorical_grouped.columns = columns
categorical_grouped.head()
```
<img src="/assets/images/typing/home-credit2/16.PNG" width="100%"><br>

위에서 만들어진 데이터프레임을 훈련셋과 병합하자.

```python
train = train.merge(categorical_grouped, left_on='SK_ID_CURR', right_index=True, how='left')
train.head()
```
<img src="/assets/images/typing/home-credit2/17.PNG" width="100%"><br>

```python
train.shape
```
<img src="/assets/images/typing/home-credit2/18.PNG" width="30%"><br>

```python
train.iloc[:10, 123:]
```
<img src="/assets/images/typing/home-credit2/19.PNG" width="100%"><br>

### Function to Handle Categorical Variables

범주형 변수를 다루기 위해서 코드를 효율적으로 작성된 함수를 만들 수 있다. 이것은 `agg_numeric` 함수와 같은 형태로, 데이터프레임과 그룹핑된 변수들을 수용한다. 그리고나서 데이터프레임의 모든 범주형 변수에 대한 각 범주의 개수와 정규화된 개수를 계산한다.

```python
def count_categorical(df, group_var, df_name):
    
    categorical = pd.get_dummies(df.select_dtypes('object'))
    categorical[group_var] = df[group_var]
    
    categorical = categorical.groupby(group_var).agg(['sum', 'mean'])
    
    column_names = []
    
    for var in categorical.columns.levels[0]:
        for stat in ['count', 'count_norm']:
            column_names.append('%s_%s_%s' % (df_name, var, stat))
            
    categorical.columns = column_names
    
    return categorical
```

```python
bureau_counts = count_categorical(bureau, group_var='SK_ID_CURR', df_name='bureau')
bureau_counts.head()
```
<img src="/assets/images/typing/home-credit2/20.PNG" width="100%"><br>
<br><br>

## Applying Operations to another dataframe

이번에는 bureau balance 데이터프레임을 확인해보자. 이 데이터프레임은 각 고객의 다른 금융기관에서 빌린 이전의 대출금 월간 정보를 가지고 있따. `SK_ID_CURR`로 이 데이터프레임을 그룹핑하는 대신, 우리는 이전의 대출금에 대한 id 변수인 `SK_ID_BUREAU`로 그룹핑할 것이다.

이것은 우리에게 각 행에 대해 각각의 대출금임을 알려주고, `SK_ID_CURR`로 그룹핑할 수 있고 고객마다의 집계된 대출금을 계산할 수 있다. 최종 결과는 각 고객에 대해 하나의 행에 통계적으로 계산된 그들의 대출금으로 이루어질 것이다.

```python
bureau_balance = pd.read_csv('../input/home-credit-default-risk/bureau_balance.csv')
bureau_balance.head()
```
<img src="/assets/images/typing/home-credit2/21.PNG" width="40%"><br>

```python
# 이전의 대출금에 대한 상태를 계산(위에서 구한 범주형 함수 사용)
bureau_balance_counts = count_categorical(bureau_balance, group_var='SK_ID_BUREAU', df_name='bureau_balance')
bureau_balance_counts.head()
```
<img src="/assets/images/typing/home-credit2/22.PNG" width="100%"><br>

이제 우리는 하나의 수치형 컬럼을 다룰 수 있다. `MONTHS_BALANCE` 컬럼은 "대출금 신청일 대비 월간 잔액"이다. 향후에는 이 변수를 시계열 변수로 고려할 수 있다. 지금은 우리가 단지 이전과 같은 집계된 통계들을 계산할 수 있을 뿐이다.

```python
bureau_balance_agg = agg_numeric(bureau_balance, group_var='SK_ID_BUREAU', df_name='bureau_balance')
bureau_balance_agg.head()
```
<img src="/assets/images/typing/home-credit2/23.PNG" width="100%"><br>

위의 데이터프레임은 각 대출에 대해 계산한 것이다. 이제 우리는 각 고객들을 위해 이것들을 통합해야 한다. 먼저, 데이터프레임을 병합하고 모든 변수가 수치형이기 때문에, 이번에는 `SK_ID_CURR`에 의해 그룹화된 통계를 다시 집계하면 된다.

```python
# Dataframe grouped by the loan
bureau_by_loan = bureau_balance_agg.merge(bureau_balance_counts, right_index=True, left_on='SK_ID_BUREAU', how='outer')

# Merge to include the SK_ID_CURR
bureau_by_loan = bureau_by_loan.merge(bureau[['SK_ID_BUREAU', 'SK_ID_CURR']], on='SK_ID_BUREAU', how='left')

bureau_by_loan.head()
```
<img src="/assets/images/typing/home-credit2/24.PNG" width="100%"><br>

```python
bureau_balance_by_client = agg_numeric(bureau_by_loan.drop(columns=['SK_ID_BUREAU']), group_var='SK_ID_CURR', df_name='client')
bureau_balance_by_client.head()
```
<img src="/assets/images/typing/home-credit2/25.PNG" width="100%"><br>

요약하자면, `bureau_balance` 데이터프레임은,  

1. 각 대출별로 그룹핑된 수치형 변수들의 계산값
2. 각 대출별로 그룹핑된 범주형 변수들의 계산값
3. 대출에 대한 통계와 가치 계수 병합
4. 고객의 id별로 결과 데이터프레임 그룹핑된 수치 통계값

최종 결과 데이터프레임은 각 고객에 대해 한 행씩 있으며, 월간 잔액 정보를 사용하여 모든 대출에 대한 통계를 계산한다.

몇몇의 이러한 변수들은 혼동을 일으킬 수 있기 때문에, 이를 설명하고자 하면:

+ `client_bureau_balance_MONTHS_BALANCE_mean_mean`: 각 대출에 대해 `MONTHS_BALANCE`의 평균값을 계산한다.
+ `client_bureau_balance_STATUS_X_count_norm_sum`: 각 대출에 대해 `STATUS == X`의 발생 횟수를 계산한다. 그런 다음 각 고객에 대해 각 대출에 대한 값을 추가하자.

모든 변수를 하나의 데이터프레임에 함께 채워질 때까지 상관관계를 연기하자!!
<br><br>

## Putting the Functions Together

우리는 이제 다른 귬융기관의 이전 대출과 이 대출에 대한 월별 지급정보를 수집하여 훈련셋에 적용할 수 있는 모든 부분을 확보했다. 모든 변수를 재설정하고 처음부터 다시 이러한 작업을 하기 위한 함수를 사용하자. 이는 반복 가능한 워크플로우에 함수를 사용하였을 경우에 이점을 보여준다.

```python
import gc
gc.enable()
del train, bureau, bureau_balance, bureau_agg, bureau_agg_new, bureau_balance_agg, bureau_balance_counts, bureau_by_loan, bureau_balance_by_client, bureau_counts
gc.collect()
```

```python
train = pd.read_csv('../input/home-credit-default-risk/application_train.csv')
bureau = pd.read_csv('../input/home-credit-default-risk/bureau.csv')
bureau_balance = pd.read_csv('../input/home-credit-default-risk/bureau_balance.csv')
```

#### Counts of Bureau Dataframe

```python
bureau_counts = count_categorical(bureau, group_var='SK_ID_CURR', df_name='bureau')
bureau_counts.head()
```
<img src="/assets/images/typing/home-credit2/26.PNG" width="100%"><br>

#### Aggreagated Stats of Bureau Dataframe

```python
bureau_agg = agg_numeric(bureau.drop(columns=['SK_ID_BUREAU']), group_var='SK_ID_CURR', df_name='bureau')
bureau_agg.head()
```
<img src="/assets/images/typing/home-credit2/27.PNG" width="100%"><br>

#### Value counts of Bureau Balance dataframe by loan

```python
bureau_balance_counts = count_categorical(bureau_balance, group_var='SK_ID_BUREAU', df_name='bureau_balance')
bureau_balance_counts.head()
```
<img src="/assets/images/typing/home-credit2/28.PNG" width="100%"><br>

#### Aggregated stats of Bureau Balance dataframe by loan

```python
bureau_balance_agg = agg_numeric(bureau_balance, group_var='SK_ID_BUREAU', df_name='bureau_balance')
bureau_balance_agg.head()
```
<img src="/assets/images/typing/home-credit2/29.PNG" width="100%"><br>

#### Aggregated Stats of Bureau Balance by Client

```python
bureau_by_loan = bureau_balance_agg.merge(bureau_balance_counts, right_index=True, left_on='SK_ID_BUREAU', how='outer')
bureau_by_loan = bureau[['SK_ID_BUREAU', 'SK_ID_CURR']].merge(bureau_by_loan, on='SK_ID_BUREAU', how='left')
bureau_balance_by_client = agg_numeric(bureau_by_loan.drop(columns=['SK_ID_BUREAU']), group_var='SK_ID_CURR', df_name='client')
```

#### Insert Computed Features into Training Data

```python
original_features = list(train.columns)
print('Original Number of Features: {}'.format(len(original_features)))
```
<img src="/assets/images/typing/home-credit2/30.PNG" width="60%"><br>

```python
train = train.merge(bureau_counts, on='SK_ID_CURR', how='left')
train = train.merge(bureau_agg, on='SK_ID_CURR', how='left')
train = train.merge(bureau_balance_by_client, on='SK_ID_CURR', how='left')
```

```python
new_features = list(train.columns)
print('Number of features using previous loans from other institutions data: {}'.format(len(new_features)))
```
<img src="/assets/images/typing/home-credit2/31.PNG" width="60%"><br>
<br><br>

## Features Engineering Outcomes

이러한 작업이 모두 끝나고, 우리가 만든 변수들을 살펴봐야 한다. 누락된 변수들, 타겟과의 상관관계가 있는 변수들, 그리고 다른 변수들과의 상관관계가 있는 변수들 등을 볼 수 잇따. 변수들간의 상관관계가 높은 것을 **다중공선성**이 있는 변수라고 하는데, 이는 중복되는 변수이기 때문에 제거해야할 필요가 있다. 또한, 누락된 값의 비율을 구하여 누락된 변수를 대체하거나 제거할 수 있다.

모델을 훈련시키거나 테스트셋에 더 일반화될 수 있도록 훈련시킬 수 있게 하기 위해 다수의 특성을 제거하는 **Feature Selection**은 중요하다고 할 수 있다. "차원의 저주"라고 불리는 이슈도 너무 많은 특성이 있기 때문이다. 변수의 개수가 증가함에 따라 이러한 변수와 타겟간의 관계를 파악하기 위해 필요한 datapoint의 개수는 기하급수적으로 증가한다.

Feature Selection에 사용할 수 있는 방식은 여러 가지가 있지만, 이 노트북에서는 누락된 값의 비율이 높은 컬럼과 서로 상관관계가 높은 변수를 제거하는 작업을 할 것이다. 이후에 우리는 Gradient Boosting Machine 또는 Random Forest와 같은 모델에서 Feature Selection을 사용할 수 있을 것이다.


### Missing Values

```python
def missing_values_table(df):
    
    mis_val = df.isnull().sum()
    mis_val_percent = 100 * df.isnull().sum() / len(df)
    
    mis_val_table = pd.concat([mis_val, mis_val_percent], axis=1)
    mis_val_table_ren_columns = mis_val_table.rename(columns={0: 'Missing Values', 1: '% of Total Values'})
    mis_val_table_ren_columns = mis_val_table_ren_columns[mis_val_table_ren_columns.iloc[:, 1] != 0].sort_values('% of Total Values', ascending=False).round(1)

    print("Your selected dataframe has " + str(df.shape[1]) + " columns.\n"
          "There are " + str(mis_val_table_ren_columns.shape[0]) + 
          " columns that have missing values.")
    
    return mis_val_table_ren_columns    
```

```python
missing_train = missing_values_table(train)
missing_train.head(10)
```
<img src="/assets/images/typing/home-credit2/32.PNG" width="70%"><br>

누락된 값을 제거하는 임계치는 정해져 있지 않고, 문제에 따라 달라진다. 우리는 90% 이상의 결측치를 가지는 훈련셋 또는 테스트셋의 컬럼을 제거할 것이다.

```python
missing_train_vars = list(missing_train.index[missing_train['% of Total Values'] > 90])
len(missing_train_vars)
```
<img src="/assets/images/typing/home-credit2/33.PNG" width="60%"><br>

### Calculate Information for Testing Data

```python
test = pd.read_csv('../input/home-credit-default-risk/application_test.csv')
test = test.merge(bureau_counts, on='SK_ID_CURR', how='left')
test = test.merge(bureau_agg, on='SK_ID_CURR', how='left')
test = test.merge(bureau_balance_by_client, on='SK_ID_CURR', how='left')
```

```python
print('Shape of Testing Data: {}'.format(test.shape))
```
<img src="/assets/images/typing/home-credit2/34.PNG" width="60%"><br>

```python
train_labels = train['TARGET']

train, test = train.align(test, join='inner', axis=1)
train['TARGET'] = train_labels
```

```python
print('Training Data Shape: {}'.format(train.shape))
print('Testing Data Shape: {}'.format(test.shape))
```
<img src="/assets/images/typing/home-credit2/35.PNG" width="60%"><br>

훈련셋과 테스트셋을 동일한 열로 맞췄기 때문에 누락된 데이터를 파악할 수 있다.

```python
missing_test = missing_values_table(test)
missing_test.head(10)
```
<img src="/assets/images/typing/home-credit2/36.PNG" width="70%"><br>

```python
missing_test_vars = list(missing_test.index[missing_test['% of Total Values'] > 90])
len(missing_test_vars)
```
<img src="/assets/images/typing/home-credit2/37.PNG" width="60%"><br>

```python
missing_columns = list(set(missing_test_vars + missing_train_vars))
print('There are %d columns with more than 90%% missing in either the training or testing data.' % len(missing_columns))
```
<img src="/assets/images/typing/home-credit2/38.PNG" width="60%"><br>

```python
# 결측치 제거
train = train.drop(columns=missing_columns)
test = test.drop(columns=missing_columns)
```

```python
train.to_csv('train_bureau_raw.csv', index=False)
test.to_csv('test_bureau_raw.csv', index=False)
```
<br><br>

## Correlations

먼저 변수들과 타겟간의 상관관계를 살펴보자. 우리가 생성한 변수 중 훈련셋에 이미 존재하는 변수보다 더 큰 상관관계가 있을 수 있다.

```python
corrs = train.corr()
```

```python
corrs = corrs.sort_values('TARGET', ascending=False)

pd.DataFrame(corrs['TARGET'].head(10))
```
<img src="/assets/images/typing/home-credit2/39.PNG" width="60%"><br>

```python
pd.DataFrame(corrs['TARGET'].dropna().tail(10))
```
<img src="/assets/images/typing/home-credit2/40.PNG" width="60%"><br>

상관관계를 보면 우리가 만든 변수들이 제일 높은 것을 알 수 있다. 하지만 이 변수들이 얼마나 유용할지는 모델에 적용해봐야 알 수 있기 때문에 섣불리 판단할 수는 없다.

```python
kde_target(var_name='client_bureau_balance_MONTHS_BALANCE_count_mean', df=train)
```
<img src="/assets/images/typing/home-credit2/41.PNG" width="70%"><br>

```python
kde_target(var_name='bureau_CREDIT_ACTIVE_Active_count_norm', df=train)
```
<img src="/assets/images/typing/home-credit2/42.PNG" width="70%"><br>

KDE 그림을 통해 명확한 결론을 얻으려 했지만, 너무 약하다고 생각한다.
<br>

### 다중공선성

```python
threshold = 0.8
above_threshold_vars = {}

# 각 변수마다 상관관계가 80% 이상인 변수들을 리스트로 해서 딕셔너리의 값으로 지정
for col in corrs:
    above_threshold_vars[col] = list(corrs.index[corrs[col] > threshold])
```

```python
cols_to_remove = []
cols_seen = []
cols_to_remove_pair = []

for key, value in above_threshold_vars.items():
    
    cols_seen.append(key)
    for x in value:
        if x == key:
            next
        else:
            if x not in cols_seen:
                cols_to_remove.append(x)
                cols_to_remove_pair.append(key)

cols_to_remove = list(set(cols_to_remove))
print("Number of columns to remove: {}".format(len(cols_to_remove)))
```
<img src="/assets/images/typing/home-credit2/43.PNG" width="60%"><br>

```python
train_corrs_removed = train.drop(columns=cols_to_remove)
test_corrs_removed = test.drop(columns=cols_to_remove)

print('Training Corrs Removed Shape: {}'.format(train_corrs_removed.shape))
print('Testing Corrs Removed Shape: {}'.format(test_corrs_removed.shape))
```
<img src="/assets/images/typing/home-credit2/44.PNG" width="60%"><br>

```python
train_corrs_removed.to_csv('train_bureau_corrs_removed.csv', index=False)
test_corrs_removed.to_csv('test_bureau_corrs_removed.csv', index=False)
```

훈련셋과 테스트셋 각각에서 중복된 변수를 제거한 후 이전에 만들었던 csv 파일과의 성능을 비교할 것이다.
<br><br>

## Modeling

우리는 3가지 종류의 데이터에 따른 실험을 할 것이다.
+ control: `application` 파일에 있는 데이터만 사용
+ test one: `bureau`와 `bureau_balance` 파일들로부터 기록된 모든 데이터와 관계된 `application` 파일의 데이터를 사용
+ test two: 가장 상관관계가 높은 변수를 제거한 `bureau`와 `bureau_balance` 파일들로부터 기록된 모든 데이터와 관계된 `application` 파일의 데이터를 사용

```python
import lightgbm as lgb
from sklearn.model_selection import KFold
from sklearn.metrics import roc_auc_score
from sklearn.preprocessing import LabelEncoder
import gc
import matplotlib.pyplot as plt
```

```python
def model(features, test_features, encoding = 'ohe', n_folds = 5):
    
    train_ids = features['SK_ID_CURR']
    test_ids = test_features['SK_ID_CURR']
    
    labels = features['TARGET']
    
    features = features.drop(columns = ['SK_ID_CURR', 'TARGET'])
    test_features = test_features.drop(columns = ['SK_ID_CURR'])
    
    # One Hot Encoding
    if encoding == 'ohe':
        
        features = pd.get_dummies(features)
        test_features = pd.get_dummies(test_features)
        
        features, test_features = features.align(test_features, join = 'inner', axis = 1)
        
        cat_indices = 'auto'
    
    # Integer label encoding
    elif encoding == 'le':
        
        label_encoder = LabelEncoder()
        cat_indices = []
        
        for i, col in enumerate(features):
            if features[col].dtype == 'object':
                
                features[col] = label_encoder.fit_transform(np.array(features[col].astype(str)).reshape((-1,)))
                test_features[col] = label_encoder.transform(np.array(test_features[col].astype(str)).reshape((-1,)))

                cat_indices.append(i)
    
    else:
        raise ValueError("Encoding must be either 'ohe' or 'le'")
        
    print('Training Data Shape: ', features.shape)
    print('Testing Data Shape: ', test_features.shape)
    
    feature_names = list(features.columns)
    
    features = np.array(features)
    test_features = np.array(test_features)
    
    k_fold = KFold(n_splits = n_folds, shuffle = False, random_state = 50)

    feature_importance_values = np.zeros(len(feature_names))
    test_predictions = np.zeros(test_features.shape[0])
    out_of_fold = np.zeros(features.shape[0])
    
    valid_scores = []
    train_scores = []
    
    # Iterate through each fold
    for train_indices, valid_indices in k_fold.split(features):
        
        train_features, train_labels = features[train_indices], labels[train_indices]
        valid_features, valid_labels = features[valid_indices], labels[valid_indices]
        
        # Create the model
        model = lgb.LGBMClassifier(n_estimators=10000, objective = 'binary', 
                                   class_weight = 'balanced', learning_rate = 0.05, 
                                   reg_alpha = 0.1, reg_lambda = 0.1, 
                                   subsample = 0.8, n_jobs = -1, random_state = 50)
        
        # Train the model
        model.fit(train_features, train_labels, eval_metric = 'auc',
                  eval_set = [(valid_features, valid_labels), (train_features, train_labels)],
                  eval_names = ['valid', 'train'], categorical_feature = cat_indices,
                  early_stopping_rounds = 100, verbose = 200)
        
        best_iteration = model.best_iteration_
        
        feature_importance_values += model.feature_importances_ / k_fold.n_splits
        
        test_predictions += model.predict_proba(test_features, num_iteration = best_iteration)[:, 1] / k_fold.n_splits
        
        out_of_fold[valid_indices] = model.predict_proba(valid_features, num_iteration = best_iteration)[:, 1]
        
        valid_score = model.best_score_['valid']['auc']
        train_score = model.best_score_['train']['auc']
        
        valid_scores.append(valid_score)
        train_scores.append(train_score)
        
        gc.enable()
        del model, train_features, valid_features
        gc.collect()
        
    submission = pd.DataFrame({'SK_ID_CURR': test_ids, 'TARGET': test_predictions})
    
    feature_importances = pd.DataFrame({'feature': feature_names, 'importance': feature_importance_values})
    
    valid_auc = roc_auc_score(labels, out_of_fold)
    valid_scores.append(valid_auc)
    train_scores.append(np.mean(train_scores))
    
    fold_names = list(range(n_folds))
    fold_names.append('overall')
    
    metrics = pd.DataFrame({'fold': fold_names,
                            'train': train_scores,
                            'valid': valid_scores}) 
    
    return submission, feature_importances, metrics
```

```python
def plot_feature_importances(df):
    
    df = df.sort_values('importance', ascending=False).reset_index()
    df['importance_normalized'] = df['importance'] / df['importance'].sum()
    
    plt.figure(figsize=(10, 6))
    ax = plt.subplot()
    
    ax.barh(list(reversed(list(df.index[:15]))),
            df['importance_normalized'].head(15),
            align='center', edgecolor='k')
    ax.set_yticks(list(reversed(list(df.index[:15]))))
    ax.set_yticklabels(df['feature'].head(15))
    
    plt.xlabel('Normalized Importance')
    plt.title('Feature Importances')
    plt.show()
    
    return df
```

### Control

```python
train_control = pd.read_csv('../input/home-credit-default-risk/application_train.csv')
test_control = pd.read_csv('../input/home-credit-default-risk/application_test.csv')
```

```python
submission, feature_importances, metrics = model(train_control, test_control)
```

```python
metrics
```
<img src="/assets/images/typing/home-credit2/45.PNG" width="40%"><br>

훈련셋의 점수가 검증셋의 점수보다 높은 것으로 보아 control 데이터는 과적합이 일어난 것을 볼 수 있다. 우리는 이것을 이후에 `reg_lambda`와 `reg_alpha`를 사용한 모델을 정규화를 통해 다룰 수 있다.

```python
feature_importances_sorted = plot_feature_importances(feature_importances)
```
<img src="/assets/images/typing/home-credit2/46.PNG" width="80%"><br>

```python
submission.to_csv('control.csv', index=False)
```

control 데이터셋의 점수는 0.754이다.
<br>

### Test One

```python
submission_raw, feature_importances_raw, metrics_raw = model(train, test)
```

```python
metrics_raw
```
<img src="/assets/images/typing/home-credit2/47.PNG" width="40%"><br>

결과를 보았을 때, control 데이터에 비해서 더 좋은 점수를 받은 것을 볼 수 있다. 그러나 우리는 이것이 테스트셋에 적용했을 경우 더 나은 검증 성능이 나올 수 있다고 말하기 전에 미리 예측결과를 리더보드에 제출해야 할 것이다.

```python
feature_importances_raw_sorted = plot_feature_importances(feature_importances_raw)
```
<img src="/assets/images/typing/home-credit2/48.PNG" width="80%"><br>

가장 중요한 100가지의 특성의 비율을 구해보자. 그러나 단순히 기존의 특성과 비교하기보다는 단열로 인코딩된 기존의 특성과 비교하는 것이 좋을 것 같다. 이것들은 이미 feature_importances에 기록되어 있다.

```python
top_100 = list(feature_importances_raw_sorted['feature'])[:100]
new_features = [x for x in top_100 if x not in list(feature_importances['feature'])]

print('%% of Top 100 Features created from the bureau data = %d.00' % len(new_features))
```
<img src="/assets/images/typing/home-credit2/49.PNG" width="60%"><br>

상위 100개의 특성이 우리가 만든 데이터임을 볼 수 있다. 앞서 해놓은 어려웠던 작업들이 가치가 있다는 것을 증명해준다!

```python
submission_raw.to_csv('test_one.csv', index=False)
```

Test One 데이터셋의 점수는 0.759이다.
<br>

### Test Two

```python
submission_corr, feature_importances_corr, metrics_corr = model(train_corrs_removed, test_corrs_removed)
```

```python
metrics_corr
```
<img src="/assets/images/typing/home-credit2/50.PNG" width="40%"><br>

```python
feature_importances_corr_sorted = plot_feature_importances(feature_importances_corr)
```
<img src="/assets/images/typing/home-credit2/51.PNG" width="80%"><br>

```python
submission_corr.to_csv('test_two.csv', index=False)
```

Test Two 데이터셋의 점수는 0.753이다.
<br><br>

## Results

우리는 위의 결과물들을 통해 추가 정보를 포함하면 모델의 성능이 향상된다는 것을 확인할 수 있었다. 모델은 데이터에 최적화되지 않았지만, 계산된 기능을 사용할 때 원본 데이터셋보다 눈에 띄게 개선되었다.  

| __Experiment__ | __Train AUC__ | __Validation AUC__ | __Test AUC__  |
|------------|-------|------------|-------|
| __Control__    | 0.815 | 0.760      | 0.745 |
| __Test One__   | 0.837 | 0.767      | 0.759 |
| __Test Two__   | 0.826 | 0.765      | 0.753 |
<br>

우리는 아직 4개의 다른 데이터가 남아있고, 다음 노트북에서는 이러한 다른 데이터 파일을 훈련셋에 통합할 것이다.
