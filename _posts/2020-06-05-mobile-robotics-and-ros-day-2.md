---
layout: post
title:  Mobile Robotics and ROS [Day 2]
date:   2020-06-05 10:00:00-18:00:00
categories: MobileRobotics
tag: MobileRobotics
---

#### Contents
1. Locomotion
    - 환경과 물체간의 물리적 상호작용  
    - recognition에서 많이 연구됨  
    - 자연 현상에서 가져오는 운동을 모방함(crawling, sliding, running, jumping, walking, rolling)  
    - Mechanisms + Actuators(구동기)  
    - 가장 중요한 이슈  
        1) Stability(안정성)  
            a) contact points의 수 -> 최소 3개 이상이라고 할 수 있지만, 조건이 필요함(무게중심을 벗어나지 않아야 한다는 것)  
            b) center of gravity(무게중심)  
            c) static: 움직임이 없는 안정성 / dynamic: 자연현상에 의한 안정성(관성, 중력 등..)  
            d) inclination of terrain(경사도)  
         
        2) Characteristics of contact(접촉 특징)  
            a) point/area contact  
            b) angle of contact  
            c) friction(point인지, area인지에 따라 마찰이 달라짐)  
        
        3) Type of environment(환경의 종류)  
            a) structure  
            b) medium(매질): 공기, 물, 부드러운/딱딱한 재질의 벽  

2. Wheels
    - 대부분의 application의 적절한 solution
        * 2족 다리 형태의 로봇보다는 wheel 형태의 로봇이 좀 더 효율적이다!  
    - 3개의 wheels는 안정성을 보장해준다.
    - 3개 이상의 wheels는 suspension(스프링)이 필요할 수 있다(언덕 지형인 경우, 지면에서 붕 떠지면서 하나의 wheel이 제대로 기능을 못할 경우).
    
    - Four Basics Wheels Types  
        1) Standard wheels  
            + 일반적인 바퀴, 직진 방향만 가능  
            + 이 휠은 2 자유도를 가지며 Front 또는 Reverse를 통과할 수 있다.   
            + 휠의 중심은 로봇 섀시에 고정되어 있으며, 로봇 섀시와 휠 평면 사이의 각도는 일정하다.  
            + 고정 휠은 일반적으로 휠이 모터에 부착되어 로봇을 운전하고 조종하는 데 사용되는 대부분의 WMR에서 볼 수 있다.  


        2) Castor wheels  
            + 보조 휠로 사용  
            + 이 휠은 3 자유도를 가지며 휠을 제자리에 고정시키는 포크에 장착된다.  
            + 캐스터 휠은 일반적으로 로봇의 균형을 잡는 데 사용되며 로봇을 구동하는 데는 거의 사용되지 않는다.   
            + 2가지 종류  
                a) centered : 휠의 수직 차축은 휠의 중심을 통과한다. 디자인은 간단하며 휠을 로봇베이스에 연결하기 위해 몇 개의 나사만 있으면 된다.  
                b) off-centered : 수직 축은 휠 중심을 통과하지 않지만 약간 중심을 벗어난다. 일부 디자인에는 휠과 포크 사이에 스위블 조인트가 포함되어있어 360° 자유롭게 회전 할 수 있다. 이 스위블 휠의 가장 큰 단점 중 하나는 _flutter_ 이다. flutter은 휠이 지면에 닿지 않고 어떤 방향으로든 자유롭게 회전 할 때 발생한다.  


        3) Swedish wheels(=Omni wheels)  
            + 옴니 디렉션, passive한 롤링  
            + 이 휠은 3 자유도를 가지며 다 방향 이동이 필요한 로봇에 가장 적합하다.  
            + 이 휠은 중앙 휠 둘레에 패시브 휠(롤러)이 부착된 일반 휠이다. 옴니 휠은 어떤 방향으로든 움직일 수 있으며, 어떤 방향으로 움직일 때 저항이 낮다.   
            + 작은 바퀴는 작은 바퀴의 축이 큰 중심 바퀴의 축에 직각이되도록 부착되어 바퀴가 자체 축과 평행하게 회전하도록 한다.   
            + Mecanum Wheel은 롤러가 또 다른 더 큰 휠의 원주 주위에 45° 각도로 부착되는 것을 제외하고 옴니 휠의 한 유형이다.  
            
        4) Ball or Spherical wheels  
            + 이 볼 휠에는 구형 금속 또는 나일론 볼(또는 단단한 구형 물질)이 홀더 안에 있다.   
            + 공의 자유도는 360°이며 일반적으로 로봇의 균형을 잡는 데 사용된다.   
            + 단점은 이러한 캐스터는 일반적으로 견인력이 높고 구동 휠을 밀고 지지하는 데 더 많은 힘이 필요하다는 것이고, 또한 고르지 않고 먼지가 많고 기름기가 많은 표면에는 적합하지 않다.  
    
    
    - Wheeled Robots의 특징  
        1) Stability(안정성)  
        2) Bigger wheels allow to overcome higher obstacles(높은 장애물 극복)  
        3) nonholonomics  
            + 모바일 로봇에서 Holonomics은 어느 방향으로도 움직일 수 있는 로봇을 의미하기 때문에 모션 제어가 쉽다는 장점이 있다.  
            + 이에 반해서 대부분의 모바일 로봇은 Nonholonomics인데, 순간적으로 어떤 방향으로 움직일 수 없기 때문에 어떤 위치로 이동을 하기 위해서는 자동차를 사이드 주차하기 위해서 핸들을 여러 번 돌리는 것과 같은 과정이 필요하다.   
            + Holonomics 시스템으로 대표적인 것은 메카넘휠과 같은 특수한 바퀴를 사용하여 어느 방향이든 이동할 수 있도록 설계된 Omnidirectional Robot을 들 수 있다.  
            + 위치 변수와 속도 변수가 포함  
            
        4) 2개의 wheels + 1개의 sub wheels로 오류를 낮출 수 있음  

3. Mobilie Robot Kinematics
    - Kinematics: 신체의 움직임을 다루는 역학의 서브 필드  
        1) forward kinematics  
            + 로봇의 제어를 하는 공간(joint, link)  
            + 위치와 자세를 찾는 연구(해가 하나)  
            
        2) inverse kinematics  
            + 위치와 자세 정보가 주어지면, 거꾸로 joint, link 공간을 연구(해가 여러 개)

    - Representing robot position  
        + 기준이 되는 좌표계가 필요함  
        
        <img src="/assets/images/ros2/1.PNG" width="50%"><br>
        
        <img src="/assets/images/ros2/2.PNG" width="50%"><br>

        + robot pose: 기준점(x, y) + 앵글값(0), 어느 프레임으로부터 기준이 되느냐를 표시해야 함  
        + mapping between the two frames: dot은 미분을 뜻함. 기준 위치에서 로봇이 얼마나 회전되어 있는지.  
        
        <img src="/assets/images/ros2/3.PNG" width="50%"><br>
        
        <img src="/assets/images/ros2/4.PNG" width="50%"><br>
     
     
    - Holonomic systems  
        + initial frame에서 diffrential equation(로봇의 움직임을 수학적으로 모델링 -> 미분방정식 형태)이 integrable(적분이 가능한) final position  
        + 각 휠의 속도 -> differential equation이 구해짐  
        + 위의 값을 적분하면 final position을 찾을 수 있음(휠의 회전량을 누적해서)  
        + 각 휠의 이동 거리 측정은 로봇의 최종 위치를 계산  
    
    
    - Non-holonomic systems  
        + diffrential equation이 주어졌지만, 적분이 불가능하여 final position을 찾을 수 없음  
        + 휠의 속도를 적분해서 final position을 찾을 수 없음(why? 이동량은 같지만, final position이 다를 수 있기 때문에)   
        + 이동하는 함수를 시간에 따라 표현해야만 가능해짐  
        
        <img src="/assets/images/ros2/5.PNG" width="50%"><br>  
        
        
    - Kinematics of wheel motion  
        + wheel motion model  
            + lateral slip  
            + (자동차)차량 동역학에서 슬립은 타이어와 이동하는 노면 간의 상대 운동이다. 이 미끄러짐은 타이어의 회전 속도가 프리 롤링 속도보다 크거나 작거나(보통 미끄러짐 비율로 표시) 타이어의 회전면이 운동 방향과 비스듬하게 되었을 때(슬립이라고 함) 발생한다.  
            + lateral slip(타이어 측면의 미끄러짐)이란, 움직이는 방향과 가리키는 방향 사이의 각도이다. 예를 들어 이것은 코너링에서 발생할 수 있으며, 타이어 및 트레드의 변형에 의해 가능하다.  
            
            <img src="/assets/images/ros2/6.PNG" width="50%"><br>
            
            <img src="/assets/images/ros2/7.PNG" width="50%"><br>
        
        
    - Instantaneous Center of Rotation(IC/ICR/ICC, 순간적인 회전 중심)  
        1) Case 1: IC가 존재  
        
            <img src="/assets/images/ros2/8.PNG" width="50%"><br> 
        
            + 차량의 각 바퀴는 IC를 중심으로 회전을 한다.  
            + IC는 각 휠의 롤 축의 교차점에 있다.  
            + 각 바퀴의 속도는 차량의 회전과 일치한다.  
        
            > 𝑣𝑣1=𝑅𝑅1𝜔𝜔,𝑣𝑣2=𝑅𝑅2𝜔𝜔, 𝑣𝑣3=𝑅𝑅3𝜔𝜔
        
        2) Case 2: IC가 없음  
        
            <img src="/assets/images/ros2/9.PNG" width="50%"><br>
    
            + IC가 없으면 회전이 불가능함

        3) Case 3: IC가 존재하면서, 각 휠의 거리와 속도가 비례할 때  
        
            <img src="/assets/images/ros2/10.PNG" width="50%"><br>
    
            > 𝑣𝑣1=𝑅𝑅1𝜔𝜔,𝑣𝑣2=𝑅𝑅2𝜔𝜔


    - Wheel Kinematic Constraints  
        + 가정  
            a) Movement on a horizontal plane(수평면에서의 움직임)  
            b) Point contact of the wheels(바퀴의 점 접촉)  
            c) Wheels not deformable(변형할 수 없는 바퀴)  
            d) Pure rolling(순수한 롤링)  
            e) No slipping, skidding or sliding(미끄러짐 없음)  
            f) No friction for rotation around contact point(접점 주변의 회전 마찰 없음)  
            g) Steering axes orthogonal to the surface(표면에 직교하는 스티어링 축)  
            h) Wheels connected by rigid frame(chassis)(견고한 프레임으로 연결된 휠)  
    
        + Fixed Standard Wheel  
            + 표준 휠은 속도의 방향 제약을 제공  
            
            <img src="/assets/images/ros2/11.PNG" width="50%"><br>
            
        + Steered Standard Wheel  
            + 스티어링 작동으로 스티어링 가능한 표준 휠 정렬 가능  
            
            <img src="/assets/images/ros2/12.PNG" width="50%"><br>
            
        + Castor Wheel 
            + 오프셋 캐스터 휠은 연결 지점에서 두 개의 직교 선형 속도를 허용  
            
            <img src="/assets/images/ros2/13.PNG" width="50%"><br>
            
        + Swedish Wheel
            + 표준 휠에서 1개의 DOF(Degrees of Freedom, 자유도)가 추가  
            
            <img src="/assets/images/ros2/14.PNG" width="50%"><br>
            
        + Spherical Wheel
            + 모션에 직접적인 제약이 없는 전 방향 가능  

            <img src="/assets/images/ros2/15.PNG" width="50%"><br>

    - Kinematics Model
        + 목표: 휠 속도, 조향 각도, 조향 속도 및 로봇의 기하학적 파라미터 (구성 좌표)의 함수로 로봇 속도 설정
        + Forward kinematics
            <img src="/assets/images/ros2/16.PNG" width="50%"><br>
            
        + Inverse Kinematics
            <img src="/assets/images/ros2/17.PNG" width="50%"><br>
    
    
    - Mobile Robot의 Locomotion  
        1) Differential drive robots  
            - 두 개의 바퀴가 공통 축에 장착되어 라인이 일치되고, 별도의 모터로 제어  
            - 가장 단순하지만 가장 인기있는 드라이브 메커니즘  
            - 각 휠이 회전 운동을 나타내려면 로봇이 공통 축에있는 IC를 중심으로 회전해야 함.  
            - IC는 두 바퀴의 상대 속도에 따라 달라짐.  
            - 두 개의 휠의 상대적인 속도에 따라 IC의 값이 결정됨(두 휠의 상대속도가 일치하면, IC는 무한대 / 두 휠의 상대속도가 음수이면, IC가 결정)  
            - 대표적으로 터틀봇  


        2) Kinematics model in the robot frame  
        
            <img src="/assets/images/ros2/18.PNG" width="50%"><br>
        
            <img src="/assets/images/ros2/19.PNG" width="50%"><br>
        
        
        3) Synchronous drive mobile robots  
            - 각 휠은 구동 및 스티어링(조향)이 가능  
            - 일반적인 구성: 3개의 스티어링 휠이 정삼각형의 정점에 배치  
            - 모든 바퀴가 함께 조향되고 운전  
            - 하나의 모터가 모든 바퀴를 같은 속도로 회전  
            - 다른 모터는 모든 휠을 조향하여 항상 같은 방향을 가리키도록 함.  
            - IC는 항상 무한대로, 로봇의 방향은 변경할 수 없음.  
            - 터렛과 함께 사용되는 경우가 많음.  
            - 기계식 체인으로 인해 휠이 잘못 정렬될 수 있음.  


        4) Omnidirectional mobile robots  
            - 3 DOF 모션 가능  
            - inverse kinematics is significant  
            - 설계 문제는 Nonholonomic 제약 조건 해결과 밀접한 관련이 있음.  
            - 수동 롤러로 둘러싸인 원형 허브로 구성  
            - 허브가 구동되고 롤러가 유휴 상태(수동)  


        5) Kinematics of roller wheels  
            - 허브 회전: 롤러가 여전히 남아있는 상태에서 허브 축을 중심으로 회전(또는 롤)  
            - 롤러 회전: 롤러가 지면 회전과 접촉하고 허브가 고정된 상태에서 허브 축 방향으로 이동  
            - 다른 방향으로의 움직임에는 허브 회전과 롤러 회전이 조합  
            
            <img src="/assets/images/ros2/20.PNG" width="50%"><br>
             
             
        6) Three-wheeled omnidirectional mobile robot with universal wheels  
            - 3개의 바퀴 힘으로부터의 결과 힘 벡터는 로봇의 움직임을 결정  
            - 동작은 로봇 중심의 변환 및 로봇 중심에 대한 회전으로 분해  
            
            <img src="/assets/images/ros2/21.PNG" width="50%"><br>
            
            
        7) Four-wheeled mobile robot with Swedish wheels  
            - 결점    
                a) 불연속 접촉으로 인한 수직 진동  
                b) 신뢰성 문제  
                c) 복잡한 디자인  
                
            <img src="/assets/images/ros2/22.PNG" width="50%"><br>
