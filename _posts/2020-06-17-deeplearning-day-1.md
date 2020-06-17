---
layout: post
title:  Deep Learning [Day 1]
date:   2020-06-17 10:00:00-19:00:00
categories: DeepLearning
tag: DeepLearning
---

## 고려대학교 김중헌 교수님
### Curriculum
- Deep Learning Thoery and Software  

1. Machine Learning Overview
  1) AI는 전 분야에 미칠 수 있는 기술
  2) 강화학습은 지도학습/비지도학습의 단일 응답 구조와 다르게 끊임없이 응답하는 구조(바둑의 경우 한 수를 두고 끝나는 것이 아니라 계속해서 인공지능이 생각하면서 진행)
<img>

2. Introduction
  <img>
  
  1) How Deep Learning Works?

    - 딥러닝 프로세스 과정

      a) 모델 만들기(껍데기) - MLP, CNN, RNN, GAN, Customized
         > input -> 4개의 히든레이, 7개의 유닛(MLP) -> output

      b) 훈련하기 -> Input: data, Output: labels
      
      c) 테스팅/추론
  
    - 딥러닝 과정에서 발생되는 문제

      a) Overfitting: 2단계에서 발생되는 문제로, 데이터가 충분하지 않은 경우 발생한다.  
         -> 훈련 결과는 엄청 높게 나왔는데, 테스트 결과는 엄청 낮은 것
         -> 침대를 학습시키고(나의 잠자리 유형) 팔면 망함

  2) Two major Deep Learning Models(CNN, RNN)

    - CNN(Convolutional Neural Network)
      : 기존의 딥러닝은 1차원 구조만 입력이 가능했지만, CNN은 2차원 구조(이미지), 3차원 구조(영상)을 훈련시킬 수 있다.

    - RNN(Recurrent Neural Network)
      : 

  3) Deep Learning: Vision
    - Visual Learning 
      a) Object Recognition
      b) Style transfer
      c) Deblurring and Denosing
      d) Super-Resolution
      
  4) Deep Learning: GAN
    - 
  
  

[Linear Functions]
1) Linear Regression
2) Binary Classification
3) Softmax Classification

[Nonlinear Functions]
1) Neural Network
2) Convolutional NN
3) CNN for CIFAR-10
4) Recurrent NN




## Regression
linear model <=> hypothesis(통계학에서는 가설)
loss function -> 손실함수: 손실을 최대한 줄여야 함
cost: 실제값과 모델값의 차이(오차)
-> 처음에는 길게, 나중에는 작게(기울기를 점점 작게, 기울기=0이되면 오차가 최소값이 됨)
-> Gradient Descent Method(경사하강법)

어떻게 learning rate를 적용하는지? -> 계속 학습하면서 관측을 해야함



model
시간 당 공부양, 족보 사용 여부 등등의 변수
각 변수마다 가중치(weight)가 있음





이진 분류기
1) Linear regression with H(x) = Wx + b
2) Logistic/sigmoid function(sig(t)) 기준이 바뀔 수 있다는 점을 감안하는 함수
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

크로스엔트로피 함수 -> 크로스 엔트로피에서는 실제값과 예측값이 맞는 경우에는 0으로 수렴하고, 값이 틀릴경우에는 값이 커지기 때문에(무한대), 실제 값과 예측 값의 차이를 줄이기 위한 엔트로피라고 보시면 될 것 같습니다.


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




















