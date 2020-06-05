---
layout: post
title:  Mobile Robotics and ROS [Day 2]
date:   2020-06-25 10:00:00-18:00:00
categories: DeepLearning
tag: DeepLearning
---

#### Contents
1. Locomotion
    - 환경과 물체간의 물리적 상호작용  
    - recognition에서 많이 연구됨  
    - 자연 현상에서 가져오는 운동을 모방함  
    - Mechanisms + Actuators(구동기)  
    - 가장 중요한 이슈  
      1) Stability(안정성)
          a) contact points의 수 -> 최소 3개 이상이라고 할 수 있지만, 조건이 필요함(무게중심을 벗어나지 않아야 한다는 것)
          b) center of gravity(무게중심)
          c) static: 움직임이 없는 안정성 / dynamic: 자연현상에 의한 안정성(관성, 중력 등..)
          d) 경사도
         
      2) Characteristics of contact
          a) point/area contact
          b) angle of contact
          c) friction(point인지, area인지에 따라 마찰이 달라짐)

      3) Type of environment
          a) structure
          b) medium(매질): 공기, 물, 부드러운/딱딱한 재질의 벽


2. Wheels
    - 대부분의 application의 적절한 solution
      * 2족 다리 형태의 로봇보다는 wheel 형태의 로봇이 좀 더 효율적이다!  
    - 3개의 wheels는 안정성을 보장해준다.
    - 3개 이상의 wheels는 suspension(스프링)이 필요할 수 있다(언덕 지형인 경우, 지면에서 붕 떠지면서 하나의 wheel이 제대로 기능을 못할 경우).
    - Four Basics Wheels Types
        1) Standard wheel
            + 일반적인 바퀴, 직진 방향만 가능
        2) Castor wheel
            + 보조 휠로 사용
        3) Swedish wheel
            + 옴니 디렉션, passive한 롤링
        4) Ball or Spherical wheel
    
    - Wheeled Robots의 특징
        1) Stability
        2) Bigger wheels allow to overcome higher obstacles
        3) nonholonomics
        4) 2개의 wheels + 1개의 sub wheels

    - Different Arrangements of Wheels
        1) ICC가 맞지 않으면 skidding이 생김
        2) ecomend sterring


3. Mobilie Robot Kinematics
    - Kinematics: 
    - forward kinematics
        + 로봇의 제어를 하는 공간(joint, link), 위치와 자세를 찾는 연구(해가 하나)
        + 
        
    - inverse kinematics
        + 위치와 자세 정보가 주어지면, 거꾸로 joint, link 공간을 연구(해가 여러 개)
        
        
    - Representing robot position
        + 기준이 되는 좌표계가 필요함
        + initial frame: {X1, Y1}
        + robot frame: {Xr, Yr}
        + robot pose: 기준점(x, y) + 앵글값(0), 어느 프레임으로부터 기준이 되느냐를 표시해야 함
        + mapping between the two frames: dot은 미분을 뜻함. 기준 위치에서 로봇이 얼마나 회전되어 있는지.
 
    - Holonomic systems
        + initial frame에서 diffrential equation(로봇의 움직임을 수학적으로 모델링 -> 미분방정식 형태)이 integrable(적분이 가능한) final position
        + 각 휠의 속도 -> differential equation이 구해짐
        + 위의 값을 적분하면 final position을 찾을 수 있음(휠의 회전량을 누적해서)
    
    - Non-holonomic systems
        + diffrential equation일 주어졌지만, not integrable인 final position을 찾을 수 없음
        + 휠의 속도를 적분해서 final position을 찾을 수 없음(이동량은 같지만, final position이 다를 수 있음)
        + 이동하는 함수를 시간에 따라 표현해야만 가능해짐
        
        
    - Kinematics of wheel motion
        + wheel motion model
        
    - Instantaneous Center of Rotation(IC/ICR/ICC, 순간적인 회전 중심)
        + IC가 있으면 IC를 중심으로 회전을 함
        + IC가 없으면 회전이 불가능함

    - Wheel Kinematic Constraints
        + 가정
            a) 
            b) 
            c) 
            d) 
            e) 
    
    - Kinematics Model
        + 목표: wheel speed, steering angles, steering speeds, ...의 값들의 함수를 만드는 것
        + Forward kinematics
            <img>
        + Inverse Kinematics
            <img>
    
    - Mobile Robot Locomotion
        + Differential drive robots
            - 두 개의 휠 축이 라인에 일치하도록 설계
            - 가장 만들기 쉽고, 간단함
            - 두 개의 휠의 상대적인 속도에 따라 IC의 값이 결정됨(두 휠의 상대속도가 일치하면, IC는 무한대 / 두 휠의 상대속도가 음수이면, IC가 결정)
            - 터틀봇
            - 
        
        + 

        + Kinematics model in the robot frame
        + Kinematics model in the initial frame

        + Omnidirectional mobile robots















