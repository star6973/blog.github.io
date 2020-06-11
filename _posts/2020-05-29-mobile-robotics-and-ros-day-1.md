---
layout: post
title:  Mobile Robotics and ROS [Day 1]
date:   2020-05-29 10:00:00-18:00:00
categories: MobileRobotics
tag: MobileRobotics
---

## 충북대학교 김곤우 교수님
### Curriculum
- ROS(Robot Operating System)
- 터틀봇3 로봇

#### Contents
1. 모바일 로봇
    - Autonomous Navigation(자율 주행)
    - Localization and Mapping(위치 인식)
        + 포지셔닝을 하기엔 오차가 너무 크고, 제약이 많이 큼
        + GPS(절대 위치) -> 오차가 큼 -> 모바일 로봇에서 제어를 위해선, localization 기술을 높여 자기 위치 인식이 필요
        + VINS: 외부 환경 인식, 자기 위치 인식
        + SLAM(Simultaneous Localization and Mapping)
            : 동시에 위치를 추정하고, 매핑도 하는 기술 -> 화성 탐사 로봇에서 쓰이는(위성이 없기 때문에 자기가 위치를 찾고, 지도를 만들면서)
            : 로봇공학 등에서 사용하는 개념으로, 임의 공간에서 이동하면서 주변을 탐색할 수 있는 로봇에 대해, 그 공간의 지도 및 현재 위치를 추정하는 문제
        + 캘리브레이션: 하나의 센서로만은 위치 인식이 불가능, 다중 센서로 파악

2. 모바일 로봇의 인공지능을 위한 필수 4가지
    - perception(인식)
    - navigation(지도 작성 - 자율주행)
    - recognition(판단)
    - execution(실행 - 컨트롤)

3. 2D LiDAR Odometry
    - Odometry란, 자기 자신의 거리를 추정하는 방법(자이로 센서). 사람으로 가정하면, 눈으로 봤을 때 몇 걸음을 걸었는지 추정하는 것
    - Line feature extraction 및 association 알고리즘
    - 파이프라인
        1) data preprocessing
        2) data association
        3) pose estimation
 
    <img src="/assets/images/ros/1.PNG" width="50%"><br>
    
4. 3D LiDAR based GraphSLAM
    - Tree/Graph Algorithm

5. Sensor Fusion based SLAM/Localizatino
    - LiDAR, GPS, IMU 센서 융합
    - Robust LiDAR feature detection: Corner, blob feature 인식 및 매핑
        * Robust: 이상치/에러값으로 부터 영향을 크게 받지 않는 (건장한) 통계량

6. LiDAR based MODT(Multiple Objects Detection and Tracking)
    - 지도를 작성할 때, static하게 작성해야됨 -> 위치 추정을 할 때 에러를 낮추기 위해서
    - 주변의 나무, 가로수, 차량을 detection / 추정을 하면서 tracking
    - camera와 융합해서 사용 -> YOLO 딥러닝 알고리즘 사용 -> 카메라에선 데이터로 변환할 때, 픽셀로 변환되기 때문에 거리정보가 없음. 따라서 3D LiDAR 정보로 거리를 같이 받아옴.

7. Stereo Visual Odometry
    - Visual Odometry: 이전 프레임 영상과 현재 프레임 영상을 합쳐서 추정

8. Multi-Sensor Fusion based SLAM
    - 자이로 센서, IMU 센서
    - 자신의 위치를 추정

9. Symantic SLAM
    - 딥러닝 기반, perception, MODT

10. 모바일 로봇의 핵심적인 이슈
    - 어디에 있는가?
    - 어디로 갈 것인지?
    - 어떻게 갈 것인가?
        
            주변의 환경에 대한 모델(지도) -> 인식/분석(perception) -> 인지/판단(recognition) -> 실행/제어(execution)
      
11. 일반적인 모바일 로봇 시스템
    
    <img src="/assets/images/ros/2.PNG" width="50%"><br>

    1) Localization(자기 위치에서 만들어진 지도)
    2) path : 경로 - 거리(움직이는 길)
    3) trajectory : 궤적 - 시간(시간에 따라서 어느 위치) : 현실적인 제약을 제어하기 위해(신호등, 속도 준수 등)

12. Control Architectures / Strategies
    - static object -> semi-static object(언젠가는 옮겨질 수 있다)
    - dynamic object -> semi-dynamic object(allow the client to benefit from the functionalities of more than one service and to combine them at run-time and at compile-time to get novel services which provide novel functionalities)
    - 확률이 중요함(얻어진 센서 정보에서도 불확실한 것들이 많기 때문에 이상치로 제거해야 될 지를 결정)
    - 에러: perception에서 발생하는 에러(축적 안됨) / 모션에서 발생하는 에러(축적 가능) 
    
    - 컨트롤 2가지 접근법
        1) Classical AI - 기본적인 모델링 방식(perception, localization, cognition, motion control)을 따르는 function based, horizontal decomposition -> 예측하지 못한 환경에서는 제약이 많음
        2) New AI, AL - 모델링 방식을 따르지 않은 behavior based, vertical decomposition

13. Environment Representation and Modeling
    - 환경을 표현하는 방법(geometric 환경)
        1) continuous metric(x, y, 세타와 같은 position)
        2) discrete metric(metric grid, 포인트 정보 (1, 1), (1, 2)와 같은.., 거리정보, position 정보)
        3) discrete topological(topological grid, 최적의 경로, 가장 적은 비용이 드는, 노드와 노드 간의 연결성(경로와 거리 값들이 들어가는 것은 아님))
    
    - 환경 모델링
        1) Raw sensor data
            : 센서 정보 자체를 활용(레이저를 기준 좌표계로 하는 포인트 정보)
            : 정보량과 데이터량이 많음
        2) Low level feature
            : 구분 가능한 포인트(corner, arc와 같은 기하학적인 정보)
            : 정보량이 많지만, 데이터량은 줄어듦 -> 유용한 정보를 얻을 수 있음
        3) High level feature
            : 시맨틱한 정보들(의미있는)

14. From Perception to Understanding  

        raw data      ->      features        ->      objects(주방 기구)          ->      places/situations(주방이구나) 
                                                물체를 인식할 수 있으면 소통 가능   => 상황 부여 가능(서비스)
                                                
                < Navigation >         < Interaction >                  < Servicing/Reasoning >
                  perception             recognition
