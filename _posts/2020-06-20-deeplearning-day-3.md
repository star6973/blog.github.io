---
layout: post
title:  Deep Learning [Day 3]
date:   2020-06-20 10:00:00-19:00:00
categories: DeepLearning
tag: DeepLearning
use_math: true
---

## 6. Convolution Neural Networks(CNN)
### 6.1. CNN Theory

> [CNN, Convolutional Neural Network 요약](http://taewan.kim/post/cnn/#1-5-pooling-%EB%A0%88%EC%9D%B4%EC%96%B4){: target="_blank"}

1. CNN이란?
- ANN은 Fully Connected Layer만으로 구성되어 있기 때문에, 입력 데이터는 1차원(배열) 형태로 입력되어야 한다(지난 ANN Implementation에서 reshape을 하는 이유 참조).
- 이미지나 영상 데이터는 흑백이 아닌 컬러 사진의 경우 3채널이기 때문에 3차원 데이터로 구성된다. 이를 ANN을 사용하려면 1차원으로 평면화시키는 과정에서 당연히 공간 정보가 손실될 수밖에 없다.
- 이러한 문제를 해결하는(이미지 공간 정보를 유지) 모델이 바로 CNN(Convolution Neural Network)이다.
- 특징
    + 객체, 얼굴, 장면을 인식하기 위한 패턴을 찾는 데 유용하다.
    + 2차원 이상의 데이터의 정보를 그대로 유지
    + 복수의 필터를 사용하여 정보의 feature를 직접 학습하기 때문에 수동으로 feature를 추출할 필요가 없음.
    + (convolution + pooling) 형태로 구성
    + 필터를 공유 파라미터로 사용하기 때문에, 학습 파라미터가 적다.

<center><img src="/assets/images/deeplearning/67.PNG" width="50%"></center><br>

- 이미지의 feature를 추출하는 부분과 class를 분류하는 부분으로 나뉜다.
    + feature 추출 영역: convolution layer, pooling layer
    + class 분류 영역: fully connected layer

- 이미지의 feature를 추출하기 위해 입력 데이터를 필터(mask)가 순회하며 합성곱(convolution)을 계산하고, 그 계산 결과를 이용해 feature map을 만든다.
    + convolution layer는 filter, stride, padding, pooling에 따라 출력 데이터의 shape이 달라진다.

<center><img src="/assets/images/deeplearning/68.gif" width="50%"></center><br>    

2. CNN 주요 용어

