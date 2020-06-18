---
layout: post
title:  Deep Learning [Day 1]
date:   2020-06-17 10:00:00-19:00:00
categories: DeepLearning
tag: DeepLearning
use_math: true
---

## 고려대학교 김중헌 교수님
### Curriculum
- Basics
- Software Installation and Examples
- Linear Regression
- Binary Classification
- Softmax Classification
- Artificial Neural Networks(ANN)
- Convolutional Neural Networks(CNN)
- CNN for CIFAR-10
- Recurrent Neural Networks(RNN)
- Generative Adversarial Networks(GAN)

## 1. Basics
### 1.1. Machine Learning Overview
<img src="/assets/images/deeplearning/1.PNG" width="50%"><br>

- AI는 전 분야에 미칠 수 있는 기술이기에, 계속해서 열광할 것.
- 머신러닝은 크게 비지도 학습과 지도학습, 그리고 강화학습으로 나눌 수 있다.
- 강화학습은 지도학습/비지도학습의 단일 응답 구조와 다르게 끊임없이 응답하는 구조(바둑과 같은 게임에 있는 AI 알고리즘은 한 수를 두고 끝나는 것이 아니라 상대방의 수를 읽고서, 다음 수를 생각하면서 진행한다.)
<br><br>

### 1.2. Introduction
#### 딥러닝 프로세스 과정

1) 모델 만들기(껍데기) - MLP(Multi-Layer Perception), CNN, RNN, GAN, Customized
<img src="/assets/images/deeplearning/2.PNG" width="50%"><br>
 > input(5개의 유닛) -> 4개의 히든레이어 + 7개의 유닛 -> output(4개의 유닛)

2) 훈련하기 - 입력된 데이터를 레이블링할 수 있도록
<img src="/assets/images/deeplearning/3.PNG" width="50%"><br>

3) 테스팅/추론 - 현실의 row 데이터를 테스팅하여 결과물이 유의미하도록
<img src="/assets/images/deeplearning/4.PNG" width="50%"><br>

※ 딥러닝 과정에서 발생되는 문제

  **Overfitting**
  - 2단계에서 발생되는 문제로, 데이터가 충분하지 않은 경우 발생한다.  
  - 훈련 결과는 엄청 높게 나왔는데, 테스트 결과는 엄청 낮은 것을 과적합 문제라 한다.  
  - 예를 들어, 침대를 팔고 싶어서 데이터를 수집하고자 하는데, 나의 잠자리 유형에만 특화된 침대만을 학습시키면 훈련은 잘 나올지 몰라도, 다른 사람들의 잠자리 유형에 맞지 않는 침대가 나올 수 있다.
  - 더 많은 훈련 데이터가 필요하다!

#### 딥러닝에서 두 가지 주요 모델(CNN, RNN)

  **CNN(Convolutional Neural Network)**
  <img src="/assets/images/deeplearning/5.jpeg" width="50%"><br>
  
  - 기존의 딥러닝은 1차원 구조만 입력이 가능하지만 많은 응용 분야에서 입력은 다차원이 필요했다. CNN은 2차원 구조(이미지), 3차원 구조(영상)을 훈련시킬 수 있다.
  - 주로 시각 정보 학습에 사용한다.

  **RNN(Recurrent Neural Network)**
  <img src="/assets/images/deeplearning/6.PNG" width="50%"><br>
  
  - 기존의 신경망 아키텍처에는 시간의 개념을 사용할 방법이 없었다. 이러한 시계열 데이터를 학습시킬 수 있는 모델이 주로 LSTM 및 GRU이다.
  - 주로 시계열 정보 학습에 사용한다.
<br><br>

## 2. Linear Regression
### 2.1. Linear Regression Theory

- regression은 예측하는 것을 목표로 한다.
- linear model은 흔히 직선 방정식을 생각하면 된다. 이 linear model은 통계학에서는 가설(hypothesis)이라고 부른다.  

<img src="/assets/images/deeplearning/7.PNG" width="50%"><br>

- 위의 사진과 같이 3개의 직선 중 어느 직선이 가장 좋아보일까? 아마도, 예측 측면이나 분류 측면 모두 가운데 파란색 선이 가장 좋아보인다고 할 수 있을 것이다.
- 따라서 우리는 파란색 선과 같이 점과 직선 사이의 거리가 최소한이 되는 새로운 직선을 계속해서 만들어내야 한다.

<img src="/assets/images/deeplearning/8.PNG" width="50%"><br>

- 위의 사진과 같이 점과 직선 사이의 거리는 오차(cost, 비용)라고 할 수 있으며, 비용 또는 손실함수로 만들 수 있다.

<img src="/assets/images/deeplearning/9.PNG" width="50%"><br>

- 이러한 비용함수는 실제값과 예측값의 차이를 제곱(음수가 될 수 있기 때문에, 어차피 우리는 그 차이만을 보는 것이기 때문에 제곱을 해도 상관없다.)하여 평균을 내주면 만들 수 있다.
- 따라서 비용함수는 2차 방정식 형태의 곡선이 만들어진다.

<img src="/assets/images/deeplearning/10.PNG" width="50%"><br>

- 고등수학을 배웠다면 쉽게 이해할 수 있겠지만, 2차 방정식을 미분하게 되면 1차 방정식의 직선이 된다. 이 직선은 gradient(기울기)라고 하며, 오차값이 된다. 따라서 우리는 오차를 최대한 줄이기 위해서 기울기가 0이 되도록 만들어야 하는데, 위의 사진과 같이 점점 기울기의 크기가 작아지도록 만드는 것을 **Gradient Descent Method(경사하강법)** 라고 한다.
 
- 경사하강법 과정 중에서 기울기가 작아지도록 만들 때, 필요한 것이 learning rate이다. 한 번 학습할 때 얼마만큼 학습해야 하는지의 학습 양을 의미하며, learning rate를 적절하게 조정해줘야 모델의 학습이 잘 될 수 있다.
- learning rate가 크다는 것은 경사하강을 할 때 step이 크다는 것이다. step이 크면 왔다갔다 하거나, 위로 튕겨 올라가버릴 수 있다. 이는 학습이 이루어지지 않으며, 쓰레기값이 나올 수 있다. 또한, 이러한 현상을 overshooting이라고 한다.
- learning rate가 작다는 것은 경사하강을 할 때 step이 작다는 것이다. step이 작으면 너무 천천히 내려가기 때문에 시간이 다해 최저점이 아님에도 불구하고 멈추어 버린다.
- 이러한 현상들을 피하기 위해서는 cost함수를 출력해보고 작은값으로 변화하고 있다면 learning rate를 증가시켜보면서 관측해야 한다.

  **Multi-Variable Linear Regression**

<img src="/assets/images/deeplearning/11.PNG" width="50%"><br>

  - 다중 변수로 linear model을 만들기 위한 방정식과 비용함수를 만들 수 있는데, 방정식에서 더욱 깔끔하게 나타낼 수 있도록 선형대수인 행렬을 사용한다.
<br><br>

### 2.2. Linear Regression Implementation

시간 당 공부양, 족보 사용 여부 등등의 변수
각 변수마다 가중치(weight)가 있음

<br><br>

## 3. Binary Classification
### 3.1. Binary Classification Theory

- classification은 분류하는 것을 목표로 한다.
- Binary classification은 0 또는 1로만 나누어지는 것으로, 스팸 메일인지(1) 아닌지(0)와 같은 예시를 들 수 있다.
- Binary classification은 기본적으로 Linear regression의 H(x) = Wx + b와 같은 1차 방정식을 따르면서, 0과 1의 분류를 하는 기준이 있다. 이러한 기준을 bias라고 하며, 이는 문제에 따라 바뀌기 때문에 Logistic/sigmoid function을 사용한다.

<img src="/assets/images/deeplearning/12.PNG" width="50%"><br>

- 그렇다면, 왜 시그모이드 함수를 사용하는지를 알아보자. 0 또는 1과 같이 데이터를 분류하기 위해선 단순 직선의 방정식은 사용하기가 어렵다. 왜냐하면, 현재 데이터의 분포에 따라 직선으로 나누었다고 하더라도, 새로운 데이터의 값이 어디에 위치하는지에 따라 또 다시 새로운 모델이 필요하기 때문이다. 따라서 계속해서 모델의 변화를 만들어주는 것이 아닌, 시그모이드 함수를 만들어서 0과 1로 분류하게 한다.

- 변수가 1개인 선형 방정식은 목표가 실수값 예측이기 때문에 선형함수 y = Wx + b를 이용하여 예측한다(예측 변수의 수가 하나인 경우). 하지만 binary classification에서는 목표값이 0 또는 1이기 때문에 y = Wx + b를 이용해서 분류하는 것은 의미가 없다고 앞서 언급했다. 그래서 확률(Probability)을 이용하는데 다음과 같이 정의 된다. 

<img src="/assets/images/deeplearning/13.PNG" width="50%"><br>

- 확률 p의 범위가 (0, 1)이라면, Odds(p)의 범위는 (0, $$\infty$$)가 된다. 이를 로그함수를 취하면 범위가 ($$-\infty$$, $$\infty$$)가 된다. 즉, 범위가 실수 전체가 되어 분석을 하는 것이 의미가 있다.

<img src="/assets/images/deeplearning/14.PNG" width="50%"><br>

- 다시 위의 식을 p로 정리하면 다음과 같은 식을 얻을 수 있고, 이 식을 시그모이드라 한다.

<img src="/assets/images/deeplearning/15.PNG" width="50%"><br>


확률  
p
 가 주어져 있을 때

   -> 로지스틱/시그모이드 함수는 linear 모델과 다르게 제곱해서 평균해도 이쁜 곡선이 나올수가 없어서 다른 방안을 모색해야 함

  (43페이지) 로그 기반 함수 -> 엔트로피 함수
  -> 2줄은 귀찮으니까 1줄로 만들기 위해서 y, (1-y)를 곱해줌



소프트맥스 분류기(멀티 분류기)
-> 바이너리 클래시피케이션을 하되, 질문을 여러번
1) A인가 아닌가
2) B인가 아닌가
3) C인가 아닌가

B가 0.56, C가 0.99면, 시그모이드 값이 높은 것으로 결정(즉, prediction이 필요없음)
-> 시그모이드 함수에서 나온 값에서 A가 B와 C와 얼마나 같은지는 생각할 필요 없음. 걍 A인지 아닌지만

크로스엔트로피 함수 -> 크로스 엔트로피에서는 실제값과 예측값이 맞는 경우에는 0으로 수렴하고, 값이 틀릴경우에는 값이 커지기 때문에(무한대), 실제 값과 예측 값의 차이를 줄이기 위한 엔트로피라고 보시면 될 것 같습니다. -> 비트가 엇갈리는 순간 무한대값으로 변경됨


W: 4x3, b: 3 -> 15개의 변수를 학습





linear regression -> 일차식(Wx + b)이 기본 모델이며, 입력값에 대한 추정치에 따른 오차를 구하기 위해 최소차승법(제곱평균)을 이용하고, 구해진 오차를 경사하강법을 통해 줄여나간다.

binary classification -> 일차식(Wx + b)에서 단순히 직선그래프가 아니라, 기준이 바뀔 수 있다는 점을 감안하여, 시그모이드 함수로 모델을 만든다. 오차를 구하기 위해 엔트로피 함수를 만들고, 구해진 오차를 경사하강법을 통해 줄여나간다.

softmax classification -> binary classification과 다르게 2개 이상의 값을 분류해야 함. 소프트맥스 함수로 모델을 만들고, 오차를 구하기 위해 따로 크로스 엔트로피 함수를 만든다. 이 크로스 엔트로피 함수에는 소프트 맥스 함수가 포함 -> 텐서플로우




[1, 3]으로 들어오지만, 4개의 유닛이 있으므로, [3, 4]

H1: 	W[3, 4]
	b: [4]

[3, 4]로 들어오지만, 4개의 유닛이 있으므로 [4, 4]
H2: 	W: [4, 4]
	b: [4]

출력값을 [1, 1]로 만들어주기 위해 [4, 1]로
O: 	W: [4, 1]
	b: [1]


히든 레이어의 W -> 앞의 레이어와 자신의 레이어의 유닛 곱하기
히든 레이어의 b -> 자신의 유닛




히든 레이어의 유닛을 늘린다는 것은 기준을 세부적으로 더 좁혀주는 것(구체적으로)

조심! 들어오는 사이즈만큼은 히든레이어를 만들어야 함 -> 안그러면 나중에 output layer에서 더 적게 나오게 되어 압축 효과가 생김


너무 히든레이어를 만들면 -> 계속해서 시그모이드 함수를 사용 -> 0과 1의 사이로만 계속해서 출력 -> 값이 사라지는 문제
=> 활성화를 시키면 받은 만큼 보내는 activation(ReLU) 사용

adamoptimizer -> 이미지를 다룰 때 많이 사용되는 법
그 외에는 gradient


MNIST 데이터는 너무 잘되어있기 때문에 압축을 시켜도 성능에 영향 x
















