---
layout: post
title:  Mobile Robotics and ROS [Day 8]
date:   2020-07-17 10:00:00-19:00:00
categories: MobileRobotics
tag: MobileRobotics
---

[칼만 필터]
불확실성을 가지고 있는 모델의 상태 추정
추정값은 에러가 포함되어 있기 때문에, 정확한 값으로 나타낼 수 없음 -> 기댓값을 이용 -> 실제값과 비교하여 차이가 있다면, bias가 존재
이전 추정값 + 현재 들어오는 값 => 이후의 추정값 추정[linear combination을 사용해서 증명]

variance가 작아질 수록 다음 추정값의 정확도가 높아짐.
칼만 게인
    1) 칼만 게인값이 추정값에 근사해지면, zk만 영향을 줌.
    2) 이전의 추정값이 0에 근사해지면, -HK만 영향을 줌.

추정, 검증



이론책 기반
개념, 용어 


# Path and Motion Planning
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


















