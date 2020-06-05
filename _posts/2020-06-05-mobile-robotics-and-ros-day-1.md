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
    - 3개 이상의 wheels는 suspension이 필요할 수 있다(지형에 따라 하나의 wheel이 제대로 기능을 못할 경우).
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
  

3. Mobilie Robot Kinematics









