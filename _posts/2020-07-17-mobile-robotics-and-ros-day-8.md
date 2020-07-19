---
layout: post
title:  Mobile Robotics and ROS [Day 8]
date:   2020-07-17 10:00:00-19:00:00
categories: MobileRobotics
tag: MobileRobotics
---

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

<br><br>

# Path and Motion Planning
#### Contents
1. Introduction
2. Motion Planning

## 1. Motion Planning
- 목표: 충돌없이 도달하는 경로를 찾기, 가장 빠르게 목적지에 도달하는 경로를 찾기
- path: 물리적 경로(서울~부산까지의 경로), trajectory: 경로의 시간까지 포함(1분 뒤에 어디 위치에 있는지까지를 생각)
- 방법
    1) DWA(Dynamic Window Approaches)
    2) 그리드맵
    3) Nearness-Diagram-Navigation
    4) 벡터 필드 히스토그램
    5) A*: 그래프 서치 알고리즘 / D*: 동적으로 변화되는 환경에 적용 / D* Lite: 연산의 속도를 증가
    6) 로드맵

- 도전과제
    1) 잠재적 불확실성까지 고려한 최적 경로
    2) 예측할 수 없는 물체의 행동을 일반화


1. DWA
    - 충돌을 피하는 것이 목표
    - 모바일 환경에서 목표까지 어떻게 갈 수 있을지(허용 가능성 / 도달 가능성)의 명령들 + 목표까지 움직일 수 있는 추정값까지 포함
    - (v, w)는 충돌가능성의 벡터값
    - 현재 속도(Vs) + 거리(Va) + 불확실성(Vd) -> dynamic window(바운더리)를 생성 -> 도달할 수 있는 공간이다를 결정함
    - 그림은 그냥 충돌이 되는 부분에 대한 영역을 표현한 것(Vs, Va, Vd를 바탕으로)
    - (v, w)를 정하기 위해 navigation function을 설정
        + 가장 빠른 속도와 올바른 길로 가기 위한 방향
        + NF = alpha * (Maximize velocity) + Beta * (Cost to reach the goal) + Gamma * (Follows grid based path computed by A*) + delta * (Goal nearness, 목적지까지와의 거리)

    - local minima 문제 - 더이상 (v, w)를 줄 수 없는 경우 -> 딥러닝에서 learning rate가 작은 경우랑 비슷하다고 보면 됨 -> gradient minimize를 하면서 국부 최솟값을 발견하고 계속 최솟값을 찾다가 빠져나오질 못함
    
    
2. Motion Planning의 문제
 
3. Configuration Space
    - 좌표 공간의 x, y, 0 -> 자세 공간으로 일반화
    - motion planning에서는 충돌 가능성 있는 공간으로 일반화(v, w, 0) // DWA는 좌표 공간을 (v, w, 0)로 옮긴 것이고, Configuration Space는 (v, w, 0)를 일반 자세 공간으로 옮긴 것이다.
    - C_free : obstacle이 차지하는 공간과 로봇의 자세공간이 교집합이 없는 경우
    - C_obs : 전체 공간에서 C_free를 뺀 여집합
    - C_free에서 C_obs 공간을 피하면서 목적지로 가는 길이 로봇의 목적지로 가는 path
    
    - Combinatorial 방법: C_free 공간에서 그래프를 그리면서 목적지까지 찾음
    - Sampling 방법: 랜덤으로 노드에서 트리 구조로 목적지까지 찾음 -> 이후에 최적 경로, 최단 경로를 찾음, 문제는 랜덤을 기반하기 때문에 path를 찾을 때마다 path의 노드가 달라짐    

    - Search
        1) Uninformed search: blind 서치 - BFS, DFS, Uniform-cost
        2) Informed search: 어느정도 길은 앎(어느 방향으로 가야될 지에 대한 정보[휴리스틱 정보]가 주어짐) - 그리디 알고리즘, A*, D*
            i) informed search A*
                : f(n) = g(n) + h(n) // g(n): 노드에서 노드로 가는 에지값, h(n): 휴리스틱값(주어진 정보)
                : 


















