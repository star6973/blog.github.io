---
layout: post
title:  Deep Learning [Day 2]
date:   2020-06-18 10:00:00-19:00:00
categories: DeepLearning
tag: DeepLearning
use_math: true
---


## 5. Artificial Neural Networks(ANN)
### 5.1. ANN Theory

1. 퍼셉트론(Perceptron)
    - 퍼셉트론은 초기 형태의 인공신경망으로 다수의 입력으로부터 하나의 결과를 내보내는 알고리즘이다.
    - 퍼셉트론은 실제 뇌를 구성하는 신경 세포 뉴런의 동작과 유사핟.

    <center><img src="/assets/images/deeplearning/31.PNG" width="25%"></center><br>
    
    - 다수의 입력을 받는 퍼셉트론은 다음과 같이 입력값과 출력값이 있다. x는 입력값을, W는 가중치를, y는 출력값을 의미하며, b는 bias이다.
    
    <center><img src="/assets/images/deeplearning/32.PNG" width="25%"></center><br>
    
    - 각각의 입력값에는 각각의 가중치가 존재하는데, 가중치의 값이 클수록 해당 입력값이 중요하다는 것을 의미한다.
    - 이러한 뉴런에서 출력값을 변경시키는 함수를 활성화 함수(Actication Function)이라고 한다.
        + 초기 인공신경망 모델은 활성화 함수를 계단함수를 사용하였지만, 그 외에도 다양한 함수를 사용하기 시작했다(시그모이드, 소프트맥스).

2. 단층 퍼셉트론(Singl-Layer Perception)
    - 퍼셉트론은 단층 퍼셉트론과 다층 퍼셉트론으로 나누어지는데, 단층 퍼셉트론은 값을 보내는 단계와 값을 받아서 출력하는 2개의 단계로만 이루어진다.
    - 각 단계를 층(layer)이라고 부르며, 입력층(input layer)과 출력층(output layer)이 있다.
 
    <center><img src="/assets/images/deeplearning/33.PNG" width="25%"></center><br>

    - 단층 퍼셉트론을 이용한 논리 게이트 연산자
    
        1) AND 게이트
           : 두 개의 입력값이 모두 1인 경우에만 출력값이 1이 나오는 구조
           
            ```python
            def AND_gate(x1, x2):
                w1=0.5
                w2=0.5
                b=-0.7
                result = x1*w1 + x2*w2 + b
                if result <= 0:
                    return 0
                else:
                    return 1
            ```

        2) OR 게이트
           : 두 개의 입력이 모두 0인 경우에 출력값이 0이고, 나머지 경우에는 모두 출력값이 1인 구조

            ```python
            def OR_gate(x1, x2):
                w1=0.6
                w2=0.6
                b=-0.5
                result = x1*w1 + x2*w2 + b
                if result <= 0:
                    return 0
                else:
                    return 1
            ```
    
            <center><img src="/assets/images/deeplearning/34.PNG" width="25%"></center><br>
            <center><img src="/assets/images/deeplearning/35.PNG" width="25%"></center><br>
    
        + 이외에도 논리 게이트를 충족시키는 다양한 가중치와 편향의 값이 있다.
        + 하지만 단층 퍼셉트론으로는 XOR 게이트를 구현할 수 없다.
        + XOR 게이트는 입력값 두 개가 서로 다른값을 가지고 있을 때만 출력값이 1이 되고, 입력값 두 개가 서로 같은 값을 가지면 출력값이 0이 되는 구조이다.
        + 즉, 단층 퍼셉트론은 직선 하나로 두 영역을 나눌 수 있는 문제에 대해서만 구현이 가능하지만, XOR 게이트는 두 개의 직선이 필요하다.

            <center><img src="/assets/images/deeplearning/36.PNG" width="25%"></center><br>
            <center><img src="/assets/images/deeplearning/37.PNG" width="25%"></center><br>

            > XOR 게이트는 직선이 아닌 곡선, 비선형 영역으로 분리하면 구현이 가능하다.

3. 다층 퍼셉트론(Multilayer Perceptron, MLP)
    - 입력층과 출력층 사이에 하나 이상의 중간층이 존재하는 신경망으로 다음 그림에 나타낸 것과 같은 계층구조를 갖는다.

        <center><img src="/assets/images/deeplearning/38.PNG" width="25%"></center><br>  
  
    - 이 때, 입력층과 출력층 사이의 중간층을 은닉층(hidden layer) 이라 부른다.
    - Multilayer perceptron은 단층 perceptron과 유사한 구조를 가지고 있지만 중간층과 각 unit의 입출력 특성을 비선형으로 함으로써 네트워크의 능력을 향상시켜 단층 퍼셉트론의 여러 가지 단점들을 극복했다. 
    - Multilayer perceptron은 층의 갯수가 증가할수록 perceptron이 형성하는 결정 구역의 특성은 더욱 고급화된다. 
    - 이와 같이 은닉층이 2개 이상인 신경망을 심층 신경망(Deep Neural Network, DNN)이라고 한다.

4. 순방향 신경망(Feed-Forward Neural Network, FFNN)
    - 다층 퍼셉트론(MLP)과 같이 입력층에서 출력층 방향으로 연산이 전개되는 신경망을 FFNN이라 한다.
    - 별도로 정의되는 이유는 은닉층의 출력값이 다시 은닉층의 입력으로 사용되는 재귀적인 구조를 가진 RNN이 있기 때문이다.

        <center><img src="/assets/images/deeplearning/39.PNG" width="25%"></center><br>
        <center><img src="/assets/images/deeplearning/40.PNG" width="25%"></center><br>

5. 전결합층(Fully-connected layer, FC, Dense layer)
    - 다층 퍼셉트론의 은닉층과 출력층에 있는 모든 뉴런은 이전 층의 모든 뉴련과 연결되어 있다.
    - 이와 같이 어떤 층의 모든 뉴런이 이전 층의 모든 뉴런과 연결되어 있는 층을 전결합층이라 하며, 모든 은닉층과 출력층이 전결합층이다.
    - 밀집층(Dense layer)이라고도 한다.

6. 활성화 함수(Activation Function)

       1) 선형/비선형 함수
          : 선형 함수는 출력이 입력의 상수배만큼 변하는 직선을 그리는 함수이고, 비선형 함수는 직선 1개로는 그릴 수 없는 함수이다.
          : 인공신경망의 성능을 높이기 위해서는 은닉층을 추가해야 하는데, 활성화 함수를 선형 함수를 사용하게 되면 은닉층을 쌓을 수 없게 된다.
            예를 들어 f(x) = Wx라 할 때, 은닉층을 2개 추가한다고 하면 출력층을 포함해서 y(x) = f(f(f(x)))가 되며 이는 선형적인 구조임을 알 수 있다.
            즉, 선형 함수로 은닉층을 추가하더라도, 1회 추가한 것과 차이를 줄 수 없다.
          : 그렇다고 선형 함수를 사용한 층이 의미가 없다는 것은 아니다. 학습 가능한 가중치가 추가로 생긴다는 점에서 분명히 의미가 있다.
          : 활성화 함수를 사용하는 일반적인 은닉층을 선형층과 대비되는 표현을 사용하면 비선형층이다.

       2) 시그모이드 함수
          : 일반적인 인공신경망의 학습 과정은, 우선 입력에 대해서 순전파(forward propagaion) 연산을 하고, 그리고 순전파 연산을 하고 나온
            예측값과 실제값의 오차를 손실 함수(loss function)을 통해 계산하고, 그리고 이 손실(loss)을 미분을 통해서 기울기(gradient)를 구하고,
            이를 통해 역전파(back propagation)를 수행한다.
          : 시그모이드 함수의 문제점은 미분을 하며 기울기를 구할 때 발생한다. 시그모이드 함수의 출력값이 0 또는 1에 가까워지면, 그래프의
            기울기가 완만해지는 모습을 보여준다.
          : 역전파 과정에서 0에 가까운 기울기가 곱해지면, 기울기 소실(Vanishing Gradient) 문제가 발생한다. 즉, 시그모이드 함수를 사용하는 
            은닉층의 개수가 다수가 될 경우에는 0에 가까운 기울기가 계속 곱해지면 앞단에서는 거의 기울기를 전파받을 수가 없게 되어 가중치가 업데이트가 되지 않아 학습되지 않는다.

    <center><img src="/assets/images/deeplearning/41.PNG" width="25%"></center><br>          
    <center><img src="/assets/images/deeplearning/42.PNG" width="25%"></center><br>

       3) 하이퍼볼릭탄젠트 함수(Hyperbolic tangent function)
          : 하이퍼볼릭탄젠트 함수는 입력값을 -1과 1 사이의 값으로 변환한다.
          : 이 함수 역시 시그모이드 함수와 같은 문제가 발생하지만, 시그모이드 함수와는 달리 0을 중심으로 하고 있기 때문에 반환값의 변환폭이 더 크다.
          : 따라서 기울기 소실 현상이 적은 편이다.

    <center><img src="/assets/images/deeplearning/43.PNG" width="25%"></center><br>

       4) 렐루 함수(ReLU)
          : 가장 많이 사용되고 있는 함수
          : f(x) = max(0, x)로 간단하다.
          : 렐루 함수는 음수를 입력하면 0을 출력하고, 양수를 입력하면 입력값을 그대로 반환한다. 렐루 함수는 특정 양수값에 수렴하지 않으므로
            깊은 신경망에서 시그모이드 함수보다 훨씬 더 잘 작동한다. 뿐만 아니라, 다른 연산보다 속도가 빠르다.
          : 문제는 입력값이 음수이면 기울기가 0이 되기 때문에, 이 뉴런은 다시 회생이 불가능하다. 이 문제를 죽은 렐루(dying ReLU)라고 한다.

    <center><img src="/assets/images/deeplearning/44.PNG" width="25%"></center><br>

       5) 리키 렐루(Leaky ReLU)
          : 죽은 렐루를 보완하기 위한 함수
          : 입력값이 음수일 경우에 0이 아니라 0.0001과 같은 매우 작은 수를 반환한다.
          : f(x) = max(ax, x)로 간단하다. a는 하이퍼파라미터로 Leaky 정도를 결정하며 일반적으로 0.01의 값을 가진다.

    <center><img src="/assets/images/deeplearning/45.PNG" width="25%"></center><br>
    
       6) 소프트맥스 함수(Softmax function)
          : 분류 문제에서 자주 사용되는 함수
          : 시그모이드 함수처럼 출력층의 뉴런에서 주로 사용되는데, 시그모이드 함수가 두 가지 선택지 중 하나를 고르는 이진 분류(Binary Classification)
            문제에 사용된다면, 소프트맥스 함수는 다중 클래스 분류(Multiclass Classification) 문제에서 주로 사용된다.
    
    <center><img src="/assets/images/deeplearning/46.PNG" width="25%"></center><br>

7. 손실 함수(Loss function)
    - 손실 함수는 실제값과 예측값의 차이를 수치화해주는 함수이다.
    - 오차가 클수록 손실 함수의 값은 크고, 오차가 작을수록 손실 함수의 값은 작아진다.
    - 회귀에서는 평균 제곱 오차(MSE), 분류에서는 크로스 엔트로피(Cross-Entropy)를 주로 사용한다.
    
       1) MSE(Mean Squared Error)
          : 오차 제곱 평균을 의미하며, 연속형 변수를 예측할 때 사용한다.

       2) 크로스 엔트로피(Cross-Entropy)
          : 낮은 확률로 예측해서 맞추거나, 높은 확률로 예측해서 틀리는 경우 손실이 더 크다.
          : 이진 분류의 경우 binary_crossentropy를 사용하며, 다중 클래스 분류의 경우 categorical_crossentropy를 사용한다.

8. 옵티마이저(Optimizer)
