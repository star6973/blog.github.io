---
layout: post
title:  Mobile Robotics and ROS [Day 8]
date:   2020-07-17 10:00:00-19:00:00
categories: MobileRobotics
tag: MobileRobotics
use_math: true
---

> [Kalman Filter](https://medium.com/@celinachild/kalman-filter-%EC%86%8C%EA%B0%9C-395c2016b4d6){:target="_blank"}

# SLAM(Simulation Localization and Mapping)
#### Contents
1. The SLAM problem
2. Issue in SLAM

### 1. The SLAM problem
- Navigation Problems
    - Map building problems
        + 주어진 measurements에서 map을 추정
    - Localization problems
        + 주어진 measurements에서 state를 추정
    - SLAM problems
        + 주어진 measurements에서 state와 map을 동시에 추정

- 로봇의 제어입력, observation의 정보가 주어지면서, static 환경에서 지도를 만들어야 함
- SLAM은 uncertainiy 분포가 구해지면, localization(pose를 구할 수 있음)이 이루어진 다음에 mapping이 된다. -> 맵을 만들 때 로봇의 pose 측정값이 연관되어 있기 때문에
- loop closure
    + 이전에 방문했던 위치를 다시 방문했을 때, 같은 위치임을 인식하고 현재 위치에 대한 uncertainty를 줄이는 방법이다.
    + 이전의 mapping과 현재의 mapping의 correspondence 구함
    + correnction 계산, uncertainty값 결정(correction이 맞으면 불확실성은 줄어듬)
    + SLAM의 가장 중요한 계산
    <center><img src="/assets/images/ros8/1.PNG" width="50%"><br></center>

- How to do SLAM
    [cycle 1]
    + Predict
    + Measure
    + Update - 로봇은 측정 모델과 관련된 불확실성과 매핑 된 특징을 관찰
    
    [cycle 2]
    + Predict - 로봇이 움직일 때 로봇의 모션 모델에 따라 예측하려는 위치에 대한 불확실성이 높아진다.
    + Measure - 새로운 feature를 관찰한다.
    + Update
        - 로봇은 측정 모델과 관련된 불확실성과 매핑 된 특징을 관찰한다.
        - 로봇의 현재 위치에 따른 불확실성은 로봇의 이전 위치에서 예측한 불확실성과 현재 측정된 값의 조합으로 구성되며, 이는 맵을 구성할 때 중요한 요소들이 된다.
    
    [cycle 3]
    + Predict - 로봇이 다시 움직이고 불확실성이 증가한다.
    + Measure - 로봇이 기존 feature를 다시 관찰한다.  loop closure 인지
    + Update 
        - 위치를 업데이트하고, 결과 포즈 추정값은 형상 위치 추정치와 상관 관계가 있다.
        - 로봇의 불확실성이 줄어들고 나머지 map의 불확실성도 줄어든다.

### 2. Issue in SLAM
- Basic SLAM Paradigms
    + 필터 기반 SLAM -> Online SLAM 이라고 불림.
    + 그래프 기반 SLAM -> Full SLAM 이라고 불림.

- Filtering Based Approches
    1) EKF SLAM  
        <center><img src="/assets/images/ros8/2.PNG" width="50%"><br></center>
        
    - 노란색: 로봇의 pose 공분산
    - 파란색: 맵에 대한 공분산
    - 초록색: pose와 map이 모두 함께 계산된(연관성) 공분산
    - correlation은 움직일 때마다, 랜드마크가 증가할 때마다 증가함
        + 차원의 저주 -> 변수가 증가함에 따라 계산량이 증가 -> 해결하고자 Particle Filter SLAM
    
    <center><img src="/assets/images/ros8/3.PNG" width="50%"><br></center>
    
    2) Particle Filter SLAM  
    - 특정한 distribution에서 뽑은 각각의 particle들은 자신의 pose가 절대 위치라고 생각함.
        + particle들의 state와 생성된 map은 독립적이라고 생각할 수 있음.
    
    - 다들 각자가 추정한 map이기 때문에, 각 파티클간의 correlation이 없음(독립적) -> 차원의 저주가 생기지 않음
        + EFK로 각각의 랜드마크를 추정, robot pose는 particle pose로 추정
        + 랜드마크의 mean과 covariance, robot의 pose로 구성
    
    - 모든 파티클마다 랜드마크 계산을 해야 하므로 확장된 Kalman Filter를 사용한다고 할 수 있다.
    
    - 파티클 resampling 문제점
        + resampling 과정에서 correction이 잘못된 경우, 제거하기 전에 복제를 함.
        + 이때, 파티클은 맵 정보를 가지고 있기 때문에, 제거하면서 맵정보도 사라짐.
        + 잘못된 경우가 지속될 경우, 맵 정보는 사라지면서, 특정 파티클만이 계속 복제되면서 파티클의 다양성이 줄어듬
        + 파티클의 다양성을 유지하기 해주는 것이 좋음
    
    3) Graph SLAM  
    <center><img src="/assets/images/ros8/4.PNG" width="50%"><br></center>
    <center><img src="/assets/images/ros8/5.PNG" width="50%"><br></center>

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

<br><br>

