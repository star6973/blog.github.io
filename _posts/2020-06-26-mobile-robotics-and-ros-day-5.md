---
layout: post
title:  Mobile Robotics and ROS [Day 5]
date:   2020-06-26 10:00:00-19:00:00
categories: MobileRobotics
tag: MobileRobotics
---

# Perception with Uncertainty
#### Contents
1. From object recognition to scene/place recognition
2. Probabilistic Primer
3. Uncertainties
   - Representation + Propagation
   - Line extraction from a point cloud
        + Split-and-merge
        + Line-Regression
        + RANSAC
        + Hough Transform
<center><img src="/assets/images/ros5/1.PNG" width="100%"><br></center>
    
## 1. From object recognition to scene/place recognition

- scene/place recognition
  : 발견되었는지 안되었는지, 방문했었는지 안했었는지와 같은 고급기술 필요

- BoW(Back of Words)
  : loop closure
  : kidnapped robot - 자기 위치를 찾는 것

## 2. Probabilistic Primer
- Bayes Formula(베이즈 정리)

    p(x): 로봇의 추정위치
    p(y): 센서값이 읽히는 시점

## 3. Uncertainties
- 현실 세계의 센서는 항상 불확실하다(노이즈가 있다).

- 분산이 클수록 불확실성이 높음
- 

Jacobin matrix - 바퀴의 x축, y축, 회전각도의 uncertainties 함수(시스템의 모델)

wi = 1/시그마^2 : 분산의 크기가 크면 불확실성이 크다 -> 정확성이 작아진다 -> 가중치를 반대로 준다


- Line Extraction from a point cloud
    + point cloud로부터 라인을 어떻게 찾을 것인가?
        1) split - and - merge 방법
          : 라인을 유지하는 포인트를 찾아서 그룹핑
          : 포인트셋이 주어지면 split(가장 멀리 떨어진 포인트를 찾기, 그 라인을 기준으로 split)
            merge(연속된 두 세그먼트들을 그룹핑) -> split(threshold보다 크다 아니다를 따지기) -> merge
            계속 반복
           
        2) linear regression
        
        3) RANSAC
          : 이상치가 주어져도 강인한 알고리즘
          : 
        
        
        4) Hough-Transform
          : 이미지 스페이스 -> 파라미터 스페이스
          : 직선의 기울기와 절편이 파라미터
          : 포인트에서 라인으로 변하는 부분을 허프 변환
          : 이미지 스페이스(직선) -> 파라미터 스페이스(점)
          : 이미지 스페이스(한 점) -> 파라미터 스페이스(직선)
          : 이미지 스페이스(두 점) -> 파라미터 스페이스(두 직선의 교점)
          : 단점 - 노이즈에 영향을 많이 받음
          : 




