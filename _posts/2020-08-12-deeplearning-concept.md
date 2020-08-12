---
layout: post
title:  Deep Learning Concept Theorem
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
    + Neural Network에서 Perceptron 개념에서 등장하는 수식은 가중치(Weight)와 편향(bias)로 구성된다. 이때의 W와 b는 데이터를 통해 구해지며, 모델 내부적으로 결정되는 값으로 파라미터라고 한다.

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
    + Hyper Parameter는 모델링을 할 때, 사용자가 직접 설정해주는 값을 뜻한다.
    + Neural Network에서 모델 훈련을 할 때 등장하는 다양한 개념인 learning rate가 있다.
    + Hyper Parameter는 정해진 최적의 값이 없고, heuristics 방법이나 empirical하게 결정하는 경우가 많다.

    + Hyper Parameter의 특징
        1. 모델의 parameter값을 측정하기 위해 알고리즘 구현 과정에서 사용된다.
        2. Empirical하게 측정되기도 하며, 알고리즘을 여러 번 수행하면서 최적의 값을 구해나간다.
        3. 사용자에 의해 조정된다.
        4. 예측 알고리즘 모델링의 문제점을 위해 조절된다.

    + Hyper Parameter의 예
        1. 딥러닝에서의 learning rate, batch size, loss function 등
        2. SVM에서의 C

    + Hyper Parameter의 최적화 방법
        1. Manual Search
            + 사용자의 직감 또는 경험에 의해

        2. Grid Search
            + 처음부터 시작하여 모든 조합을 시행

        3. Random Search
            + 범위 내에서 무작위값을 반복적으로 추출

            <center><img src="/assets/images/deeplearning/87.jpg" width="50%"></center><br>

        4. Bayesian Optimization
            + 기존에 추출되어 평가된 결과를 바탕으로 추출 범위를 좁혀서 효율적으로 시행

            <center><img src="/assets/images/deeplearning/88.png" width="50%"></center><br>


> [파라미터(Parameter)와 하이퍼 파라미터(Hyper parameter)](https://bkshin.tistory.com/entry/%EB%A8%B8%EC%8B%A0%EB%9F%AC%EB%8B%9D-13-%ED%8C%8C%EB%9D%BC%EB%AF%B8%ED%84%B0Parameter%EC%99%80-%ED%95%98%EC%9D%B4%ED%8D%BC-%ED%8C%8C%EB%9D%BC%EB%AF%B8%ED%84%B0Hyper-parameter){:target="_blank"}
> [Parameter vs HyperParameter 둘의 차이점은 무엇일까?](http://blog.naver.com/PostView.nhn?blogId=tjdudwo93&logNo=221067763334&parentCategoryNo=&categoryNo=&viewDate=&isShowPopularPosts=false&from=postView){:target="_blank"}
> [Hyperparameter optimization](http://blog.naver.com/PostView.nhn?blogId=cjh226&logNo=221408767297&categoryNo=16&parentCategoryNo=0&viewDate=&currentPage=1&postListTopCurrentPage=1&from=section){:target="_blank"}