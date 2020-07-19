---
layout: post
title:  Mobile Robotics and ROS [Day 7]
date:   2020-07-10 10:00:00-19:00:00
categories: MobileRobotics
tag: MobileRobotics
---

# Maps
## 1. Introduction of maps
- localization, path planning, obstacle avoidance
  -> map이 필요
  -> 동시에 map 생성과 localization이 가능하도록 -> SLAM
- 아직까진 불완전함
- type of map
  1) grid map - 환경을 격자로 나눔. 위치 정보로 표현. 직관적으로 확인이 가능하지만, cell이 모든 정보를 담지는 못하기 때문에 구체적이지는 않음.
  2) topological map - 노드와 엣지로 구성된 맵. graph 형태. 주변 환경들과 차이가 많이 나는 것을 노드로 표현. 에지를 이용해 연결성을 표현.
                       grid map에 비해 리소스가 적게 소요. graph searching 알고리즘을 사용하여 위치 정보를 파악.
                       정보들이 축약되어 있는 형태이기 때문에 정확하지는 않음.
  3) feature map - continuous 환경에서 얻어지는 특징들을 위치 정보로 표현. 문제는 확보된 특징이 얼마나 좋고, 강인한 특징일지 확신할 수 없음.
                   마찬가지로 grid map에 비해 리소스가 적게 소요.

## 2. Grid based representation
- occupancy grid representation
  -> 2D나 3D의 그리드 셀로 분할
  -> 각 셀은 0과 1사이의 값으로 표현하여 확률(probability)처럼 표현
  -> 1: occupied, 0: empty, 0.5: unknown
     => 점유 확률이 1에 가까워질수록 occupied 된다고 인식함
  -> 센서 모델을 확률적으로 표현하여 얻은 값을 bayesian 모델을 사용하여 각 셀의 확률을 구한다.

- uniform grids versus quadtree
  -> 장애물과 같은 물체들이 많은 셀의 경우 작게 설정하여 resolution을 적게 만들어줌.
  -> 점유되는 셀들만 표현하여 리소스를 줄일 수 있음
  -> 장점: generality
  -> 단점: 셀의 사이즈가 제한됨.

- uniform grids
  -> 동일한 크기로 환경을 나눈 것

- quadtree
  -> 
  
- Occupancy grid map using Sensor Model
  1) Gaussian Sensor model
     : 센서값이 정확하면 variance가 작아지지만, 정확하지 않으면 variance가 커진다.
     
  2) Occupancy Grid Map
     : corn 형태로 모델링하며, 셀들에 대해 점유 확률을 갱신할 수 있도록 만든다.
     : range가 d인 경우, 중간에 있는 셀들은 empty되어 있을 확률이 높다.(센서의 값이 도달하지 못하여)

- Occupancy Grid Map
  -> 베이지안 모델로 업데이트
  -> 장점: 최단 거리 계산 유용
     단점: 비슷한 환경이 많은 데, 동일한 셀들로 나누기 때문에 리소스가 많이 차지함.
           환경의 복잡성에는 영향을 받지 않음(그저 해당 셀이 점유가 되어 있는지 여부에 따라 결정).
           그리드 맵을 만들기 위해선 많은 메모리가 잡아먹는다(모든 위치 정보를 가져와서 맵을 구성하기 때문에)
           -> 최근에는 topological 맵 처럼 구역을 나누어서 리소스를 줄여나가고 있음.
  
## 3. Topological Representation
- discrrete 위치(node or vertex로 표현)에 대한 환경을 추상화시킴. 각 위치의 연결성을 설명해주는 에지와 함께
- 그래프 형태(노드와 에지) -> 인접행렬
- 에지: 노드와 노드 사이의 거리 정보는 없고, 단지 연결성만 가지고 있음
- SLAM에서 주로 사용하는 topological 방법
  : 상대적 위치를 계산. 영상에서 키 프레임을 노드로, 키 프레임과 키 프레임 사이를 에지로 구성.
  
<br><br>


# Localization
## 1. Introduction
- localization
    + global reference frame의 관점에서 로봇의 pos(위치와 방위)를 정의하는 것
      -> global reference frame: 지도의 기준 좌표
    
- 어려움
    + 불확실성
    + 단일 센서로 측정한 값들이 효율적이지 않음. -> 요즘은 Lidar를 이용해서 측정하고자 함.
    + 여러 가지 변수를 이용해서 pos를 구해야 한다!
 
- given
    + 환경에 대한 지도가 주어짐
    + 센서 정보들이 주어짐

- wanted
    + 로봇의 pos를 추정하고자 함

- 해결해야 할 이슈
    + 시간에 따라 pos가 어떻게 변하는지 추적(tracking)
    + 전역적으로 pos의 위치가 어딨는지
    + 로봇을 이상한 곳에 두어도, 자기 위치 정보를 정확히 파악할 수 있는지

- global localization
    + 초기 정보를 알려주지 않음(기준이 계속 바뀔 수 있음)
      -> 처음에 대략적인 가정을 가지고 시작함. 
    
- position tracking
    + 계속적으로 odometry로 에러를 줄여나가야 함


## 2. Map-based Localization
- 그림(cycle)
  ->불확실한 정보는 그 위치에 대해서만 자기 위치에 대한 센서 정보 및 local 정보를 비교하면서 불확실성을 줄여나감
 
- 필요 물품
  1) Position estimation (odometry)
     + 속도, 이동량 등의 값을 추정하는 단계
     + odometry로 계산을 계속하다 보면 에러가 누적될 수 있음
       -> 헤딩 센서가 있다면 누적 에러값을 줄여줄 수 있지만, 원천적으로는 힘들다
     + 입력되는 값: x, y, theta
       -> prediction

  2) Error (uncertainty) propagation
     + 오차를 어떻게 모델링할 것인지
       -> 이동량에 비례해서 왼쪽 바퀴와 오른쪽 바퀴의 에러값의 누적합
       -> 자코비안 메트릭스로 구할 수 있다
  
  3) Position representation
  
  4) Map representation

- 확률적 맵
  + 두 가지 방법
    1) Markov
    2) Kalman
    
  + 문제
    : 움직임에 따라서 불확실성이 발생한다. 그렇기 때문에 "observation"이 필요함.
    

- Description of the probabilistic localization problem
    + action(prediction) 업데이트



## 3. Kalman Filter Localization
- 에러 모델이 가우시안, 가정이 linear 시스템
- 2가지 가정이 만족되면 optimal이 보장됨
- 2개의 단계(prediction, correction)
  1) 모션 모델로 prediction -> 예측 상태, 예측 에러값
  2) measurement를 이용해서 correction

- drive robot system은 theta값이 들어있기 때문에 non-linear

- 가우시안 확률
- kalman filter는 무조건 localization에서만 쓰이는 모델이 아님.
- state model : x
- measurement model : z
- 오차가 큰(variance가 큰) 경우는 가중치를 작게, 오차가 작은(variance가 작은) 경우는 가중치를 크게 모델로 만들어줌

- Nonlinear Dynamic Systems
    + 특정 점의 위치한 근방 점들을 series expension을 하면서 방정식을 구하고(모델 생성) -> linear로 미분
    + 접선식을 구함
    + EKF vs UKF
    + EKF -> 자코비언 값 계산 필요

- correction: prediction과 자코비언으로 구한 값을 비교 -> 

- Fx, Fq: 각각 자코비언으로 구한 state와 control


- Application to the 2D Mobile Robot case
    + cos0, sin0, -sin0, cos0
    + 각각의 measurement가 동시에 업데이트되면 correlated가 실행
    
- 측정된 값과 예측된 값의 차이가 있을 경우, h(x)의 센서 모델과 x'(t)의 예측된 모델은 그대로지만
  ktVt의 위치가 correction에 의해 조정되면서 x(t)의 값이 조금씩 바뀐다. 그때의 신뢰도인 Pt 역시 또 바뀐다.
  -> 동기화 주기가 다르다(예측값과 실제값의 유입 주기).

## 4. Monte Carlo Localization
- 파티클 기반, 파티클 분산으로 오차 추정 -> 불확실성 추정
- (차이)ETK는 오차를 공분산으로 오차 추정 + 단일 모델 + 가우시안 분포 -> 불확실성 추정 
- 파티클이 처음에 퍼져있다가, 높은 확률의 state에 위치한 파티클이 resampling이 되면서 모이기 됨
- state(센서값)와 weight(신뢰도) 값을 같이 가지고 있는 모델
- 현실적으로 무한대의 샘플을 가지고 계산할 수 없기 때문에 고른 가우시안 분포를 이루지는 않음
- probability에 의해 랜덤하게 분포됨

- proposal distribution
  : 하나의 point에 대해서 uncertainty 분포가 구해지면, 그 분포가 proposal distribution
  : 다시 새로운 예측값이 생길 때, proposal distribution에서 방금 구해진 point가 포함되는 분포가 target distribution

  -> 별의 위치를 예측하여 target distribution이 정해졌지만, 실제로 별의 위치에 obstacle이 없다면 target distribution이 0으로 수렴하므로
     importance weight의 값 역시 0으로 수렴(작아짐)


* 룰렛에서 weight에 따라(한쪽으로 모아두는 경우) 1~1000까지를 나눠서 랜덤으로 뽑을 경우, 운이 나쁘게 weight가 적은 것만 계속 뽑힐 수 있음
-> 이를 방지하기 위해 1~1000까지를 고르게 나누고, weight값을 계산하여(각각 다르게) 그 부분을 resampling한다.
-> 373p

[하나의 주기]
particle sampling -> importance weight 계산 -> resampling




# SLAM(Simulation Localization and Mapping)
## Contents
### 1. The SLAM problem
- Navigation Problems
    - Map building problems
        + 주어진 measurements에서 map을 추정
    - Localization problems
        + 주어진 measurements에서 state를 추정
    - SLAM problems
        + 주어진 measurements에서 state와 map을 동시에 추정

- 로봇의 제어입력, observation의 정보가 주어지면서, static 환경에서 지도를 만들어야 함
- SLAM은 uncertainiy 분포가 구해지면, localization(pose를 구할 수 있음)이 이루어진 다음에 mapping이 됨
  -> 맵을 만들 때 로봇의 pose 측정값이 연관되어 있기 때문에
- loop closure: 로봇이 처음에 만든 map을 재방문해서 인식이 되는 것 -> 이전의 mapping과 현재의 mapping의 correspondece를 구함
  -> correnction 계산, uncertainty값 결정(correction이 맞으면 불확실성은 줄어듬)
  -> SLAM의 가장 중요한 계산
  
### 2. Issue in SLAM
- 필터 기반 SLAM -> Online SLAM / 그래프 기반 SLAM -> Full SLAM

1) EKF SLAM
    - 노란색: 로봇의 pose 공분산
    - 파란색: 맵에 대한 공분산
    - 초록색: pose와 map이 모두 함께 계산된(연관성) 공분산
    - correlation은 움직일 때마다, 랜드마크가 증가할 때마다 증가함
      -> 차원의 저주 -> 변수가 증가함에 따라 계산량이 증가 -> 해결하고자 Particle Filter SLAM

2) Particle Filter SLAM
    - 특정한 distribution에서 뽑은 각각의 particle들은 자신의 pose가 절대 위치라고 생각함
      -> particle들의 state와 생성된 map은 독립적이라고 생각할 수 있음
    - 다들 각자가 추정한 map이기 때문에, 각 파티클간의 correlation이 없음(독립적) -> 차원의 저주가 생기지 않음
      -> EFK로 각각의 랜드마크를 추정, robot pose는 particle pose로 추정
      -> 랜드마크의 mean과 covariance, robot의 pose로 구성
    - 모든 파티클마다 랜드마크 계산을 해야 하므로 확장된 Kalman Filter를 사용한다고 할 수 있다.
    - * 파티클 resampling 문제점
        : resampling 과정에서 correction이 잘못된 경우, 제거하기 전에 복제를 함.
          이때, 파티클은 맵 정보를 가지고 있기 때문에, 제거하면서 맵정보도 사라짐.
          -> 잘못된 경우가 지속될 경우, 맵 정보는 사라지면서, 특정 파티클만이 계속 복제되면서 파티클의 다양성이 줄어듬
          -> 파티클의 다양성을 유지하기 해주는 것이 좋음

3) Graph SLAM
    - 프론트엔드: 그래프 설정. 미니마이즈된 노드를 가지고 그래프 맵을 생성.
        + 데이터 전처리
        + 데이터 연관(병합)
        + 센서 데이터들을 가지고 특징을 뽑아서, correspondence를 계산
        + 그래프 생성
    - 백엔드: 전체 풀 path를 가지고(전체적인 에러를 가지고) 최적화하는 작업.
        + loop clousure를 하면서 생성되는 에러 등을 비교하면서 전체적으로 최적화
        
- 최근 이슈
    1) sementic SLAM: 의미적인 SLAM - 인식된 물체의 의미를 찾음
    2) 동적인 환경 문제 -> 3D로 인식 -> 움직이는 물체인지, 고정된 물체인지를 판단하여 연구
    3) loop closure -> 계절적인 환경에 따라 localization(눈이 오거나 비가 올 때 같은 자리로 인식 가능한가)
    4) robust perception
    5) sementic reasoning -> BOG

















