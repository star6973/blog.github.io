---
layout: post
title:  Deep Learning [Day 8]
date:   2020-08-12 10:00:00-19:00:00
categories: DeepLearning
tag: DeepLearning
use_math: true
---

# 딥러닝 개념 정리
## 목차
### 1. Parameter & Hyper Parameter
### 2. Loss Function
### 3. Optimizer
### 4. Learning rate & Batch size
### 4. Propagation
### 5. linearity vs non-linearity
### 6.

### 1. Parameter & Hyper Parameter
- 파라미터와 하이퍼 파라미터는 엄연히 차이가 있다.

- Parameter
    + Parameter는 매개변수로, 모델 내부에서 결정되는 변수이다.
    
    + Neural Network에서 Percㅉeptron 개념에서 등장하는 수식은 가중치(Weight)와 편향(bias)로 구성된다. 이때의 W와 b는 데이터를 통해 구해지며, 모델 내부적으로 결정되는 값으로 파라미터라고 한다.

    + Parameter의 특징
        1. 예측을 수행할 때, 모델에 의해 요구되어지는 값들이다.
        2. 모델의 능력을 결정한다.
        3. 측정되거나 데이터로부터 학습되어진다.
        4. 사용자에 의해 조정되지 않는다.
        5. 학습된 모델의 일부로 저장된다.

    + Parameter의 예
        1. 딥러닝에서의 Weight과 bias
        2. SVM에서의 서포트 벡터
        3. Linear Regression에서의 결정계수

- Hyper Parameter
    + 

> [파라미터(Parameter)와 하이퍼 파라미터(Hyper parameter)](https://bkshin.tistory.com/entry/%EB%A8%B8%EC%8B%A0%EB%9F%AC%EB%8B%9D-13-%ED%8C%8C%EB%9D%BC%EB%AF%B8%ED%84%B0Parameter%EC%99%80-%ED%95%98%EC%9D%B4%ED%8D%BC-%ED%8C%8C%EB%9D%BC%EB%AF%B8%ED%84%B0Hyper-parameter)
> [Parameter vs HyperParameter 둘의 차이점은 무엇일까?](http://blog.naver.com/PostView.nhn?blogId=tjdudwo93&logNo=221067763334&parentCategoryNo=&categoryNo=&viewDate=&isShowPopularPosts=false&from=postView)