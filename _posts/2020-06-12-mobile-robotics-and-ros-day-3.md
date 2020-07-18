---
layout: post
title:  Mobile Robotics and ROS [Day 3]
date:   2020-06-12 10:00:00-18:00:00
categories: MobileRobotics
tag: MobileRobotics
---

# Perception ~ Sensors
#### Contents
1. Introduction
    - 센서: 모바일 로봇의 움직임에 중요한 역할, 다양한 정보 감지를 해서 정보 전달
        + 환경을 인식하는 가장 중요한 컴포넌트
        + 물리적 원리를 이해해야 적절히 사용 가능
        + 가공되지 않은 데이터(노이즈)

    - 기존의 모바일 로봇에서 사용된 센서들
        + Wheel encoder: 휠의 변이값, 속도값을 측정하는 센서
        + Bump detector: 충돌 감지 센서
        + Sonar range finder: 거리 측정을 위한 센서
        + Ring of sonar sensor: 전방위 환경 인식 센서
        + IMU(Inertial Measurement Unit): 자가 인식 센서
        + Pressure sensor: 압력 센서
        + Omnidirectional camera: 전방위 영상 측정 센서
        + Differential GPS system 
        + Optical Gyro
        + Odometry

            => 주변 환경 인식 센서 및 자기 위치 인식 센서는 지금까지 지속적으로 필요

2. Classification of Sensors
    - What(무엇을 측정하는가)
        1) Proprioceptive sensor
            - 내부 환경 측정
            - 모터의 속도, 휠의 로드, 로봇의 헤딩앵글

        2) Exteroceptive sensors
            - 외부 환경 측정
            - 장애물까지의 거리 측정, 빛 감지
    
    - How(어떻게 측정하는가)
        1) Passive sensors
            - emit(방출)하지 않고 주변에서 들어오는 물리량을 측정
        
        2) Active sensors
            - emit 하면서 주변의 에너지를 재측정

    - General Classification
        1) Tactile sensors(접촉 형태의 센서)
           
           ex) Conatact switches, bumpers, Optical barriers(화장실에서 사람이 지나갈 때마다 소리가 나는 센서), 
               Noncontact proximity sensors(IR센서와 같이 빛의 양에 따라서)

        2) Wheel/motor sensors(속도, 위치 측정 센서)
           
           ex) Brush encoders, Potentiometers, Synchros, resolvers, Optical encoders
        
        3) Heading sensors(전방의 정보, 외부 환경을 측정하는 센서)
           
           ex) Compass, Gyroscopes(자세 정보), Inclinometers(기울기 정보)

        4) Ground-based beacons(절대 위치값을 제공해주는 위치 인식 센서)
           
           ex) GPS, Active optical or RF beacons, Active ultrasonic beacons, Reflective beacons
        
        5) Active ranging(외부 에너지를 emit해서 돌아오는 값을 측정, 외부 환경의 거리를 측정)

           ex) Reflectivity sensors, Ultrasonic sensor, Laser rangefinder,
               Optical triangulation(1D), Structured light(2D)

        6) Motion/speed sensors
            
           ex) Doppler radar/sound(공기와 다른 매질에서 사용)

        7) Vision-based sensors
           
           ex) CCD(들어오는 에너지를 축적해서 2D Array 형태의 디지털값으로 변환)
               CMOS(반도체의 형태에서 측정)
               Visual ranging packages, Object tracking packages

3. Characterizing Sensor Performance
    - Basic sensor response ratings(기본적인 센서 반응 지표)  
        1) Dynamic range: 물리량들의 변화를 측정 가능  
        2) Range: upper limit ~ lower limit  
        3) Resolution(분해능): 값들 사이의 가장 최소한의 차이  
        4) Linearity: 인풋 대비 아웃풋이 선형적으로 변하는지 비선형적으로 변하는지  
        5) Bandwidth or Frequency: 센서의 스피드 측정  
        6) Sensitivity: 민감도 측정  
        7) Error / Accuracy(실제값과 얼마나 차이가 있는지, 목표물에 맞았는지 틀렸는지)  
           - systemic error(deterministic): 시스템이 가지고 있는 에러 -> 충분히 보정 가능  
           - random error(non-deterministic): 예측이 안되는 노이즈와 같은 에러 -> 확률과 통계로 모델링  

        8) Precision(얼마나 정밀하게 측정, 목표물에 몇 번이나 맞았는지)   
   
4. Sensors  
    1) Encoders  
    
        [개념]  
        
            - 휠이나 스티어링의 속도나 위치를 측정하기 위해 사용되는 센서  
            - 위치 운동량 측정을 얻기 위해 휠 움직임을 통합(odometry를 측정)  
            - 광학 엔코더는 고유 센서  
            
        [원리]  
        
            - pair로 구성된 감지 센서에서 디스크가 돌 때마다, 빛의 끊김으로 몇 번 돌았는지 카운팅하는 방식  
            - regular : 트랜지션 수를 세지만 모션 방향을 알 수 없음(단일 channel)  
            - quadrature(구적법) : 구적 위상 편이에서 두 개의 센서를 사용하여, 어떤 웨이브가 상승 에지를 생성하는 지를 알려줌. 또한, resolution(해상도)이 4배 더 크다.  
                    -> index channel: 1바퀴가 돌았는지 확인  
                    -> a channel, b channel: 엇갈리게 채널을 구성, 기존의 encoder에 비해 4배(00, 01, 10, 11)를 구분할 수 있음  
                    -> 목표: 회전의 방향 측정 및 resolution을 4배까지 올리기 위해  
            - 외부 트랙의 단일 슬롯은 회 전당 기준 펄스를 생성
                    
        [종류]
        
            - Absolute Encoder  
              : 디스크마다 주어진 값(절대값)이 있어서, 절대 위치를 측정 가능  
                -> 채널이 1개이면 어떤 방향으로 회전하는지 모르는 문제가 있음  
                -> 따라서 보통 채널 수가 3개  
            - Incremental Encoder  
              : 절대 위치보다는 휠의 스피드를 측정  
        
    2) Heading sensors  
    
        [개념]  
        
            - 로봇의 방향과 기울기를 결정하는 센서  
            - proprioceptive(gyroscope, accelerometer) 또는 exteroceptive(compass, inclinometer)  
            - 적절한 속도 정보와 함께 움직임을 위치 추정치에 통합 가능 -> 이 절차를 dead reckoning(선박 항법)  
            - dead reckoning vs odometry  
    
        [종류]  
        
            [1] Gyroscopes(proprioceptive)  
                - 비틀리는 각도에 따라 자세를 측정  
                - 고정 참조 프레임과 관련하여 방향을 유지하는 헤딩 센서  
                - angular acceleration(각 가속도)를 측정  
                - 모바일 시스템의 헤딩에 대한 절대값을 제공  
                - 종류  
                
                    a) Mechanical Gyroscopes  
                    
                        기계식 자이로 스코프는 본질적으로 축을 중심으로 회전하는 회전 질량으로 구성된다.  
                        특히 질량이 축에서 회전 할 때 질량은 자신과 평행을 유지하고 방향을 변경하려는 시도에 반대하는 경향이 있다.  
                              
                            standard gyro - angle 오리엔테이션 대신 각속도 측정  
                            rate gyro - 센서 요소의 코리올리 효과를 사용하여 회전 속도를 감지  
                              
                    b) Optical Gyroscopes  
                    
                        옵티컬 센서를 이용해 시간차로 발생하는 빛의 양을 측정  
                        두 조명 간의 이동 거리 차이를 활용한다. 비교적 높은 정확도.  
                        
            [2] Accelerometer(가속도계)
            
                - 진동 또는 구조물의 운동 가속을 측정하는 장치
                - 중력을 포함하여 그들에 작용하는 모든 외부 힘을 측정
                - 진동 또는 운동(가속)의 변화에 의한 힘은 질량이 가해지는 힘에 비례하는 전하를 생성하는 압전재료에 압력을 가하게끔 한다. 
                  전하는 힘에 비례하고 질량은 일정하기 때문에 전하 또한 가속에 비례
            
            [3] IMU    
                - gyro나 accelerometers를 사용해 자세, 상대적 속도, 위치 등을 추정해주는 통합적인 센서
    
      <center><img src="/assets/images/ros3/1.png" width="120%"><br></center>
    
    3) Absolute Position Sensors  
    - Ground-based Active and Passive Beacons
                
            비콘: 표식
            정확히 아는 위치에 비콘이 장착되고, 정보가 입력
            triangulation - 정확히 알고있는 비콘까지의 거리와 각도를 알면 사용자의 위치를 알 수 있음
            trilateration - 거리만으로 위치를 추정(하나의 비콘은 같은 거리에 대해 원으로, 다른 비콘이 같은 거리에 대해 원으로 -> 교점 발생)

            QR code
            UWB(Ultra Wide Bandwith): 넓은 대역폭, 아주 초단파 신호를 쏘고 받는 데까지의 시간으로 거리 측정
            RFID: 태그를 읽으면서 거리 측정
            Laser Reflector: 산업용 로봇, 반사판을 부착시켜서 반사되어 측정되는 거리를 추정

    - Motion Capture System
        
            아바타 모션 캡처 장비
            
    - GPS(Global Positioning System)
    
    4) Range sensors  
    - 보낸 신호를 다시 받을 때까지를 측정하여 거리 추정(round-trip)  
    - localization, environment modeling  
    - Ultrasonic sensors: sound를 기반으로, 강한 음파를 발생시킴  
        <center><img src="/assets/images/ros3/2.PNG" width="50%"><br></center>
    - ToF(Time of Flight Camera) 범위 센서의 품질은 다음 요소들에 의해 달라진다.  
        * 여기서 ToF란, 공장 자동화, 로봇 공학 및 물류 등의 분야에서는 3D 이미지 데이터가 2D 데이터를 효과적으로 보완할 수 있는 애플리케이션이 사용되는 경우가 많다. TOF(Time-of-Flight) 카메라는 2D 데이터뿐 아니라 필요한 깊이 데이터도 제공한다.  
        * 3D 카메라의 한 종류로, 적외선이 물체를 향해 비상(flight)하는 시간(프로젝션 + 반사시간)을 측정하여 거리로 환산한다.  
        1) 반사 된 신호의 정확한 도착 시간에 대한 불확실성  
        2) 카메라 측정시 부정확 함 (레이저 범위 센서)  
        3) 투과빔의 개방 각도 (초음파 범위 센서)  
        4) 대상과의 상호 작용 (표면, 정반사)  
        5) 전파 속도의 변화  
        6) 모바일 로봇 및 대상의 속도 (정지 상태가 아닌 경우)  
    - 종류  
        a) Ultrasonic Sensor(time of flight, sound)  
            - Sonar  
            - Time-of-Flight(ToF)  
            <center><img src="/assets/images/ros3/3.PNG" width="100%"><br></center>
            - Problem  
                a) 넓은 원뿔 모양의 빔 (각도의 불확실성)  
                b) 사운드 에너지의 대부분을 흡수하는 부드러운 표면  
                c) 소리의 방향에 직각이 되는 표면에 정반사 
            <center><img src="/assets/images/ros3/4.PNG" width="50%"><br></center>
        b) Laser Range Sensor(time of flight, electromagnetic)  
            - Lidar(Light Detection And Ranging)  
            <center><img src="/assets/images/ros3/5.PNG" width="50%"><br></center>
                
              LiDAR, 종종 LADAR, ToF, 레이저 스캐너, 레이저 레이더 등으로 불리기도 하는 이것은 물체를 감지해 거리를 맵핑하는 센싱 방식이다.
              광학 펄스로 목표물을 비춘 후 반사된 반송 신호의 특징을 측정한다. 광학-펄스의 폭은 몇 나노초부터 몇 마이크로초까지 변동할 수 있다.

              라이다의 기본 원리는 특정 패턴으로 빛을 쏘아 수신쪽의 반사광들을 바탕으로 정보를 추출해준다. 정보를 추출하는데 펄스 전력, 왕복 시간, 위상 변이, 펄스폭 등이 사용된다.

              자율 주행에서 핵심이 되는 센서들은 저마다의 고유의 장단점이 있다.

                - 초음파 센서는 몇 미터만 벗어나도 대기 중에서 크게 감쇠하게 된다. 따라서 주로 단거리 물체 감지에 쓰인다.
                - 카메라는 비용 효율적이고 쉽게 구할 수 있는 센서이지만, 유용한 정보를 추출하려면 상당한 프로세싱이 필요하며 주변의 광 조건에 크게 좌우된다. 카메라는 컬러를 인지하는 유일한 기술이다.
                - 라이다와 레이더는 주변을 맵핑하고 물체 속도를 측정할 수 있는 다양한 공통 기술들과 보완 기술들을 공유하고 있다.

              [라이다 vs 레이더]

                  a) 거리

                     라이다와 레이더 시스템은 수 m부터 200m 이상까지 떨어져있는 물체를 감지할 수 있다. 다만 라이다는 근거리 물체는 감지하기 어렵고, 레이더는 근거리, 장거리 모두 감지할 수 있다.

                  b) 공간 분해능

                     레이저 광을 조준할 수 있는 능력과 905~1550nm의 짧은 파장 때문에 라이다의 적외선 공간 분해능은 0.1도 단위까지 나눌 수 있다. 이는 백-엔드 프로세싱 없이도 물체들의 특징을 한 장면으로 3D 묘사할 수 있다.
                     반면 레이더의 파장은 거리가 늘어날수록 작은 특징들을 분석하는데 애를 먹는다.

                  c) FOV(Field of View)

                     고정형 라이다와 레이더는 둘 다 뛰어난 수평 FOV(방위각)을 가지고 있지만, 360도 회전을 하는 기계식 라이다 시스템은 모든 ADAS(Advanced Driver Assistance System) 기술 중에서도 가장 넓은 FOV를 가지고 있다.
                     라이다는 수직 FOV(고도)에서 레이더보다 뛰어나며, 고도 분해능에서도 레이더보다 우위에 있다. 이것은 물체 분류를 개선하는데 핵심 기능이다.

                  d) 날씨 조건

                     레이더 시스템의 가장 큰 장점 중 하나가 비, 안개, 눈에 강하다는 것이다. 라이다는 대개 이러한 날씨 조건에서 성능이 하락한다.

                  e) 기타 요소들

                     라이다와 카메라는 둘 다 주변 광 조건에 영향을 받기 쉽다. 그렇지만 야간의 경우, 라이다 시스템은 매우 높은 성능을 낼 수 있다. 레이더와 변조된 라이다 기술들은 다른 센서들의 간섭에 강하다.


              [라이다의 종류]

                  a) 기계식 라이다

                     고급 광학이나 회전 어셈블리를 이용해 와이드(360도) FOV를 만들어낸다. 기계식 요소는 와이드 FOV 이상으로 SNR(Signal-to-Noise Ratio)이 높지만, 몸집은 커진다.

                  b) 고정형 라이다

                     회전하는 기계식 컴포넌트들이 없고 FOV는 줄어들기 때문에, 더욱 저렴하다. 차량의 전방과 후방, 측면의 여러 채널들을 이용해 그 데이터들을 융합하면 기계식 라이다에 필적하는 FOV가 만들어진다.


              [라이다의 구현방식]

                  a) MEMS 라이다

                     전압같은 자극이 적용되면 기울기 각이 달라지는 작은 미러들을 사용한다. 사실상, 기계식 스캐닝 하드웨어를 전자기계식으로 대체한 것이다.
                     MEMS 라이다 시스템의 경우, 수신 SNR을 결정하는 리시버 집광 조리개가 몇 mm로 일반적으로 매우 작다. 여러가지 차원으로 레이저 빔을 이동시키려면 복수의 미러들을 연속으로 늘어놔야 한다.
                     이러한 정렬 프로세스는 평범하지 않지만 일단 설치하고 나면, 움직이는 차량에서 쉽게 접하게 되는 충격과 진동에 취약하다.

                  b) 플래시 라이다

                     플래시 라이다의 작동은 광학 플래시를 사용하는 표준 디지털 카메라와 매우 유사하다. 플래시 라이다, 한 개의 대면적 레이저 펄스가 전방 환경에 빛을 비추고 그 레이저에 가까이 위치한 광검출기의 초점면 어레이가 후방산란 광을 포착한다.
                     이 검출기는 이미지의 거리, 위치, 반사 강도를 포착한다. 이 방식은 기계식 레이저 스캐닝 방식에 비해 단 하나의 이미지로 전체 장면을 포착하고, 데이터 포착 속도가 훨씬 빠르다. 또한, 전체 이미지가 단 하나의 플래시에 포착되기 때문에, 이 방식은 이미지를 왜곡시킬 수 있는 진동 효과에 영향을 덜 받는다.
                     이 방식의 단점은 실생활 속에 존재하는 역반사체에 있다. 역반사체는 빛의 대부분을 반사시키고 아주 조금 후방 산란시켜, 사실상 전체 센서를 블라인드시켜 이를 무용지물로 만든다. 또 다른 단점은 전체 장면에 빛을 비추어 충분히 먼 곳까지 보려면 매우 높은 피크 레이저 파워가 필요하다.

                  c) OPA(Optical Phase Array)

                     OPA의 원리는 위상 어레이 레이더와 유사하다. OPA 시스템에서는 광학 위상 모듈레이터가 렌즈를 통과하는 빛의 속도를 제어한다. 빛의 속도를 제어하면, 광학 파면 형상을 제어할 수 있다.
                     상단의 빔은 지체되지 않지만, 중간과 아래의 빔은 양을 늘리면서 지체된다. 이 현상은 사실상 레이저 빔을 여러 방향으로 겨누도록 '조향'하는 것과 같다. 비슷한 방식들 역시 후방 산란된 빛을 그 센서로 조향함으로써 기계식 운동 부품들을 제거할 수 있다.

                  d) FMCW(Frequency-Modulated Continous Wave)

                     지금까지의 방식들은 협광 펄스를 이용한 ToF에 기반한 것들이지만, FMCW는 가간섭성 방식을 이용해 짧은 처프의 주파수 변조 레이저 광을 만들어낸다. 되돌아온 처프의 위상과 주파수를 측정하면 시스템이 거리와 속도를 둘 다 측정할 수 있다.
                     FMCW에서는 계산량과 광학이 훨씬 단순하다. 그렇지만 처프 생성으로 복잡함은 가중된다.
                                            
        - 2D Lidar
        <center><img src="/assets/images/ros3/6.PNG" width="50%"><br></center>

        - 3D Lidar
        <center><img src="/assets/images/ros3/7.PNG" width="50%"><br></center>
    
        3) Triangluation Ranging  
        - 원리
                    
            삼각측량법은 측량 구역을 삼각형으로 분할하여 각 지점의 수평위치를 결정하는 측량법의 하나이다. 공공측량표준작업규정에서는 "1등, 2등, 3등, 4등 기본 삼각점 또는 기설 기준 삼각점을 기준으로 공공측량표준작업규정에서 정한 방법이나 정도로써 그 점의 위치를 결정하는 작업"이라고 정의한다.
            그 점과 두 기준점이 주어졌으면, 그 점과 두 기준점이 이루는 삼각형에서 밑변과 다른 두 변이 이루는 각을 각각 측정하고, 그 변의 길이를 측정한 뒤, 사인 법칙 등을 이용하여 일련의 계산을 수행함으로써, 그 점에 대해 좌표와 거리를 알아내는 방법이다.
            삼각측량법은 측량, 항해, 측정, 천체측량학, 로켓 공학 등에 쓰이며, 무기(대포 등)의 방향 설정에도 쓰인다.
            삼각측량은, 기준이 되는 한 변만 거리를 측정하고 나머지는 각만 측정하여 측점들의 위치를 계산하기 때문에, 멀리 떨어져 있는 점이라도 망원경으로 시준만 가능하다면 지형이나 거리에 제약에 관계 없이 측량을 할 수 있다는 장점이 있다.
            
      <center><img src="/assets/images/ros3/8.png" width="50%"><br></center>

        - Laser Triangulation(1D)
      <center><img src="/assets/images/ros3/9.PNG" width="50%"><br></center>

        - Structued Light(vision, 2/3D)
      <center><img src="/assets/images/ros3/10.PNG" width="50%"><br></center>
