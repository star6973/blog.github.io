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
    + CNN은 결과값이 원하는 방향으로 설정(특징 a만 모으기, 특징 b만 모으기, ...)할 수 있기 때문에 성능이 정말 좋다.

<center><img src="/assets/images/deeplearning/67.PNG" width="50%"></center><br>

- 이미지의 feature를 추출하는 부분과 class를 분류하는 부분으로 나뉜다.
    + feature 추출 영역: convolution layer, pooling layer
    + class 분류 영역: fully connected layer

- 이미지의 feature를 추출하기 위해 입력 데이터를 필터(filter)가 순회하며 합성곱(convolution)을 계산하고, 그 계산 결과를 이용해 feature map을 만든다.
    + convolution layer는 filter, stride, padding, pooling에 따라 출력 데이터의 shape이 달라진다.

2. CNN 주요 용어
    1) 합성곱(Convolution)
       - 합성곱 처리는 아래의 사진과 같으며, 그 결과로 Feature Map을 만든다.
       - convolution layer에는 항상 Relu 함수가 함께 계산된다.
       
    <center><img src="/assets/images/deeplearning/68.gif" width="50%"></center><br>    

    2) 채널(Channel)
       - 이미지의 픽셀의 값은 모두 실수이다. 각 픽셀은 RGB 3개의 실수로 표현한 3차원 데이터이다.
       - 컬러 이미지는 3개의 채널로 구성도지만, 흑백 이미지는 1개의 채널로 구성된다.
       - 합성곱 레이어에 유입되는 입력 데이터에는 한 개 이상의 필터가 적용된다. 만약, n개의 필터가 적용된다면 출력 데이터는 n개의 채널을 갖는다.

    <center><img src="/assets/images/deeplearning/69.jpg" width="50%"></center><br>
    
    3) 필터(Filter) & 스트라이드(Stride)
       - 필터는 이미지의 특징을 찾아내기 위한 공용 파라미터이다.
       - CNN에서는 filter를 kernel과 같은 의미이다(OpenCV에서는 mask)
       - 필터는 일반적으로 정사각 행렬로 정의되며, 입력 데이터를 지정된 간격으로 순회하며 채널별로 합성곱을 하고 모든 채널의 합성곱의 합을 feature map으로 만든다.
       - 지정된 간격으로 필터를 순회하는 간격을 *Stride*라고 한다. Stride를 사용하면 정보는 조금 잃더라도 빠르게 학습이 가능하기 때문에, 대용량 데이터의 경우 자주 사용된다.

    <center><img src="/assets/images/deeplearning/70.png" width="50%"></center><br>
    <center><img src="/assets/images/deeplearning/71.jpg" width="50%"></center><br>
    
       - 입력 데이터가 여러 채널을 갖을 경우(컬러 이미지, 영상) 필터는 각각의 채널을 순회하며 합성곱을 계산하고, 채널별 feature map을 만든다.
       - 그리고 각 채널의 feature map을 합산하여 최종 feature map을 반환한다.

    <center><img src="/assets/images/deeplearning/72.jpg" width="50%"></center><br>
    
       - 계산된 결과로 만들어진 *feature map*은 *activation map*이라고도 하며, convolution layer의 최종 출력 결과물이다(다음 단계는 pooling).

    4) 패딩(Padding)
       - OpenCV에서 mask 필터를 적용하면 이전의 이미지보다 압축된다는 것을 알 수 있다.
       - 마찬가지로 convolution layer에서 filter와 stride를 적용하면서 feature map의 크기는 입력 데이터보다 작다.
       - 이렇게 줄어드는 것을 방지하기 위해 사용하는 방법이 패딩(padding)이다.
       - 패딩은 입력 데이터의 외각에 지정된 픽셀만큼 특정값으로 채워 넣는 것을 의미하며, 보통 0으로 채운다.
       - 예를 들어 6x6 이미지의 1만큼의 패딩을 준다는 것은 6x6 이미지의 외곽을 1 픽셀만큼 더 크게 만드는 것으로, 이미지의 사이즈는 8x8이 된다.
         이를 3x3 필터로 합성곱을 해주어도 그대로 6x6 이미지의 크기를 유지할 수 있다.
       
    <center><img src="/assets/images/deeplearning/73.png" width="50%"></center><br>
    
    5) 풀링(Pooling)
       - 풀링은 CNN의 구성 중 2단계에 해당하는 layer로, convolution layer의 출력 데이터(feature map, activation map)를 입력으로 받아서 크기를 줄이거나 특정 데이터를 강조하는 용도로 사용한다.
       - 풀링의 방법은 Max Pooling, Average Pooling, Min Pooling이 있다.
       - 정사각 행렬의 특정 영역 안에 값의 최댓값/평균값/최솟값을 구하는 방식으로 동작한다.
       - pooling layer는 convolution layer에 비해 학습 파라미터가 없으며, 행렬의 크기가 감소한다는 특징이 있다.
       
    <center><img src="/assets/images/deeplearning/74.png" width="50%"></center><br>       

3. CNN 구성

<center><img src="/assets/images/deeplearning/75.PNG" width="50%"></center><br>

### 6.2. CNN Implementation
