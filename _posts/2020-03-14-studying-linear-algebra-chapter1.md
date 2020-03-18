---
title: "Studying linear algebra [Chapter1]"
date: 2020-03-14 18:00:30
categories: Studying linear algebra
tag: LinearAlgebra
use_math: true
---

# 선형대수학 공부하기
## 챕터1: 벡터, 행렬, 행렬식
### 1.1. 벡터와 공간
#### 1.1.1. 벡터<br>
&nbsp;&nbsp;&nbsp;&nbsp;수치의 조합을 정리하여 나타내는 기법이다. 수를 나열한 것으로, 성분수를 명시하고 싶을 때는 $$\begin{pmatrix} 3 \\ 0 \end{pmatrix}$$ $$\begin{pmatrix} 1 \\ 2 \\ 3 \end{pmatrix}$$ 각각 2차원 벡터, 3차원 벡터로 불린다.
<br><br>

#### 1.1.2. 공간<br>
&nbsp;&nbsp;&nbsp;&nbsp;2차원 벡터는 x, y 축으로 이루어지는 좌표에 점으로 찍을 수 있으며, 3차원 벡터는 x, y, z축으로 이루어진 좌표에 점을 찍을 수 있다. 이런 식으로 벡터들은 공간에 대응시킬 수 있다.
<br><br>

#### 1.1.3. span<br>
&nbsp;&nbsp;&nbsp;&nbsp;동사로 '포괄하다', '걸치다', '가로지르다'의 의미를 가지며, 일반적으로 **"span a space"** 라고 하는데 "어떤 공간을 포괄하다"라는 의미이다.<br>  
+ 두 개의 벡터 $$v_1$$ = [2, 3], $$v_2$$ = [3, 2]가 있다고 가정하자. $$v_1$$과 $$v_2$$가 어떤 공간을 포괄한다고 할 때, 이것이 의미하는 것은?  
&nbsp;&nbsp;&nbsp;&nbsp;span이라는 것은 이 벡터들로 형성할 수 있는 공간을 의미한다. 즉, 선형 조합을 통해 만들어지는 공간이다.  
<br><br>

#### 1.1.4. 기저(bias)<br>
&nbsp;&nbsp;&nbsp;&nbsp;특정 벡터 $$\overrightarrow{v}$$를 지정하는데, '여기'라고 손가락으로 가리키는 것 보다 말로도 위치를 전달할 수 있도록 '번지(좌표)'를 매겨주자. 아래의 그림과 같은 기준이 되는 벡터 $$\overrightarrow{e_1}$$, $$\overrightarrow{e_2}$$를 정한다.<br>

<img src="/assets/images/studying/chapter1/3.JPG">

<br>

&nbsp;&nbsp;&nbsp;&nbsp;기준을 정하면 **"$$\overrightarrow{e_1}$$ 을 3칸, $$\overrightarrow{e_2}$$ 를 2칸"** 처럼 말하여 벡터 $$\overrightarrow{v}$$의 위치를 지정할 수 있다.

&nbsp;&nbsp;&nbsp;&nbsp;기저는 어떤 공간을 span하면서 독립인 벡터들이라 할 수 있다.<br>
+ 기저는 2가지의 성질을 가지고 있다.  
&nbsp;&nbsp;&nbsp;&nbsp;1) 기저 벡터들은 서로 독립이다.  
&nbsp;&nbsp;&nbsp;&nbsp;2) 기저 벡터들은 공간을 span 한다.  
<br>
+ 3차원 공간 $$R^3$$에 대한 기저는 무엇일까? 즉, 독립이면서 3차원 공간을 span하는 벡터는 무엇일까?  
&nbsp;&nbsp;&nbsp;&nbsp;가장 쉽게 떠올릴 수 있는 벡터는 바로, **$$e_1$$ = [1, 0, 0], $$e_2$$ = [0, 1, 0], $$e_3$$ = [0, 0, 1]** 이다.  
&nbsp;&nbsp;&nbsp;&nbsp;이들 벡터는 서로 독립이면서, 직관적으로 생각해보면 각각 x축, y축, z축을 나타낸다.  
&nbsp;&nbsp;&nbsp;&nbsp;위의 형태를 표준 기저라 하며, 기저는 무수히 많다.  
