---
layout: post
title: Computer Vision and OpenCV [Day 10]
date: 2020-07-14 10:00:00-19:00:00
categories: OpenCV
tag: OpenCV
use_math: true
---

> Visual C++ 영상 처리 프로그래밍
> [gaussian37님의 영상 워핑](https://m.blog.naver.com/PostView.nhn?blogId=infoefficien&logNo=220820421779&proxyReferer=https:%2F%2Fwww.google.com%2F){:target="_blank"}

# 11. 영상 워핑과 모핑, 그리고 Feature 검출인식 및 실습
## 11.1. 영상 워핑과 모핑
### 11.1.1. 영상 워핑(Warping)
- 픽셀의 위치를 이동하는 기하학적 처리 중 한 기법이다.
- 회전, 이동, 확대/축소 등의 기하학적 처리는 모든 픽셀에 대해 일정한 규칙을 적용함으로써 균일한 반환 결과를 얻지만, 영상 워핑은 픽셀별로 이동 정도를 달리할 수 있어 고무판 위에 그려진 영상을 임의대로 구부리는 효과를 나타낼 수 있다.
- 인공위성이나 우주선으로부터 전공된 영상은 렌즈의 변형, 신호의 왜곡 등의 이유로 인해 일그러지는 경우가 많은데, 영상 워핑은 이러한 일그러짐을 복구하는데 사용한다.
<center><img src="/assets/images/opencv10/1.PNG" width="70%"></center><br>

- 입력 영상과 출력 영상의 대응관계 기술
    + 제어선, 제어점, 그물망, 다각형 등을 이용
    <center><img src="/assets/images/opencv10/2.PNG" width="70%"></center><br>
    
    + 제어선을 이용하는 경우 아래의 그림과 같이 화소와 제어선 사이의 수직 교차점을 구한다. 그리고 화소와 수직 교차점 사이의 변위 정보와 제어선 내에서 수직 교차점의 위치 정보 두 가지를 활용하고, 역방향 사상을 이용하여 워핑을 수행한다.
    + 수직 교차점이 제어선 내부에 있는 경우로, 출력 영상에서 제어선 PQ가 입력 영상에서 제어선 P'Q'에 대응되고, 출력 영상의 픽셀 V가 입력 영상의 픽셀 V'에 대응된다. 역방향 사상에 의하여 픽셀 V에 대응되는 V'의 위치를 계산하기 위해서 PQ내에서의 C의 상대적 위치와 동일하게 P'Q'내에서 C'의 위치를 찾는다. 그 다음에 C와 V사이의 변위만큼 C'으로부터 떨어진 점 V'를 찾는다.
    <center><img src="/assets/images/opencv10/3.PNG" width="70%"></center><br>

    + 수직 교차점이 제어선 외부에 있는 경우로, 동일하게 역방향 사상으로 구한다.
    <center><img src="/assets/images/opencv10/4.PNG" width="70%"></center><br>

- 하나의 제어선에 대해서는 단순하게 픽셀의 이동 위치를 계산할 수 있지만, 여러 개의 제어선이 사용될 수 있다. 제어선이 여러 개일 경우에는 각 제어선은 영상의 모든 픽셀에 영향을 미치게 된다.
    + 한 픽셀이 여러 개의 제어선으로부터 영향받는 것을 반영하기 위해 가중치를 사용한다.
    + 제어선의 길이가 길수록 가중치가 커지고, 픽셀과 제어선 사이의 거리가 가까울수록 가중치가 커진다.
    <center><img src="/assets/images/opencv10/5.PNG" width="70%"></center><br>

    + b값이 커지면 픽셀들은 먼 거리에 있는 제어선들로부터 영향을 적게 받게 된다.
    + 픽셀과 제어선의 거리는 수직 교차점의 위치에 따라 다르게 정의된다. 수직 교차점이 제어선 내부에 있는 경우에는 픽셀과 수직 교차점 사이의 거리가 사용되지만, 제어선 외부에 있는 경우에는 제어선의 양 끝점 중에서 픽셀과 가까운 점과 픽셀 사이의 거리가 사용된다.
    <center><img src="/assets/images/opencv10/6.PNG" width="70%"></center><br>
    
    + 만약 픽셀이 제어선 외부에 있지만 제어선과 가깝다고 가중치를 많이 주게되면, 영향을 이상하게 받을 수 있기 때문이다.
    <center><img src="/assets/images/opencv10/7.PNG" width="70%"></center><br>

- 제어선이 여러 개인 경우의 출력 영상의 각 픽셀에 대한 입력 영상의 대응 픽셀을 구하는 warping() 함수의 알고리즘
    <center><img src="/assets/images/opencv10/8.PNG" width="70%"></center><br>

    + 워핑 영상 알고리즘 순서
        1) 수직 교차점의 위치 계산  
            - 각 픽셀 V(x, y)에 대하여 각각의 제어선 $$L_i$$와의 수직 교차점의 위치를 구한다.  
            - V(x, y)에서 제어선 $$L_i$$에 내린 수선의 점 $$C(x_c, y_c)$$와 $$P(x_i, y_i)$$ 사이의 거리를 u라 하면, u의 길이는 다음과 같다.  
                
            <center><img src="/assets/images/opencv10/9.PNG" width="70%"></center><br>

            - u의 값은 수직 교차점의 위치에 따라 다음과 같은 범위의 값을 가진다.  
            
            <center><img src="/assets/images/opencv10/10.PNG" width="70%"></center><br>
            
        2) 제어선으로부터의 수직 변휘 계산  
            - 출력 영상의 픽셀 V(x, y)에 대하여 각각의 제어선 $$L_i$$로부터의 수직 변위를 구한다.  
            
            <center><img src="/assets/images/opencv10/11.PNG" width="70%"></center><br>
            
            - 변위 h의 값은 다음과 같은 범위를 가진다.  
            
            <center><img src="/assets/images/opencv10/12.PNG" width="70%"></center><br>
            
            - 변위의 절대값은 수직 교차점과 픽셀 사이의 거리가 된다. 변위의 절대값이 아닌 제어선과 픽셀의 위치를 알기 위해서 변위를 구할 때, 절대값을 사용하지 않는다.  
            
            <center><img src="/assets/images/opencv10/13.PNG" width="70%"></center><br>

        3) 입력 영상에서의 대응 픽셀 위치 계산  

            - 출력 영상의 각 픽셀 V(x, y)에 대해 각각의 제어선 $$L_i$$와의 수직 교차점의 위치 u와 수직 변위 h를 구한 다음, u와 h값을 이용하여 출력 영상의 V(x, y)에 대응되는 입력 영상의 픽셀 V'(x', y')을 찾는다.  
            - 출력 영상의 제어선 $$L_i$$에 대응되는 입력 영상에서의 제어선 $$L_i'$$의 양 끝점 좌표를 $$(x_i', y_i')과 (x_(i+1)', y_(i+1)')$$이라고 하면 V'(x', y')은 다음 식과 같다.  

            <center><img src="/assets/images/opencv10/14.PNG" width="70%"></center><br>
        
        4) 픽셀과 제어선 사이의 거리 계산  
        
            <center><img src="/assets/images/opencv10/15.PNG" width="70%"></center><br>
        
        5) 제어선의 가중치 계산  
        
            - a, b, p의 상수값은 일반적으로 각각 0.001, 2.0, 0.75를 사용한다.  
            <center><img src="/assets/images/opencv10/16.PNG" width="70%"></center><br>
        
        6) 입력 영상의 대응 픽셀과의 변위 누적  
        
            - 각 제어선 $$L_i$$에 대해 출력 영상의 픽셀 V(x, y)에 대응되는 입력 영상의 픽셀 V'(x', y')을 구하고, 가중치 weight을 구한 다음에 다음 식과 같이 V와 V'사이의 변위와 가중치의 곱을 계산하여 t_x, t_y 변수에 누적한다.
            <center><img src="/assets/images/opencv10/17.PNG" width="70%"></center><br>
        
        7) 입력 영상의 대응 픽셀 위치 계산

            - 각 제어선에 대해 출력 영상의 픽셀 V(x, y)에 대응되는 입력 영상의 픽셀의 변위값을 구하여 이들의 합 $$(t_x, t_y)$$를 계산한 다음에는 다음 식에 의해 V(x, y)에 대응되는 입력 영상의 픽셀 V(X, Y)의 위치를 계산한다.
            <center><img src="/assets/images/opencv10/18.PNG" width="70%"></center><br>






