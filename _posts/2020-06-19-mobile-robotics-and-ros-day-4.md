---
layout: post
title:  Mobile Robotics and ROS [Day 4]
date:   2020-06-19 10:00:00-19:00:00
categories: MobileRobotics
tag: MobileRobotics
---

# Perception ~ Visual Perception
#### Contents
1. Vision for Robotics
2. Camera Model
3. Perspective Projection
4. Camera Calibration
5. Distance Measure with Camera
6. Image Processing

## 1. Vision for Robotics
- 카메라: 캡처된 상을 디지털 이미지로 변환
- 다양한 분야에서 사용가능한 어플리케이션

## 2. Camera Model
- Pinhole Camera Model: geometry 측정을 위한 카메라  
    : 핀홀에 있는 구멍에만 특정 물체의 빛이 투과됨  
    : 역상으로 맺힘  
    : 필름에 투과되는 중심점을 Optical Center, 상이 맺혀지는 뒷면을 Image Plane  
    : 핀홀이 작을수록 투과되는 빛의 양이 줄어들 수밖에 없어서 흐릿해짐  
    : >> 렌즈를 사용하면 빛을 모아서 한 점에 맺히도록(볼록렌즈) -> 밝고 선명한 영상을 얻을 수 있음  
    : 초점이 모아지는 부분을 Focal Point  
    : 렌즈와 Focal Point 사이의 거리를 Focal Length  
    : 렌즈와 물체간의 평행한 축을 Optical Axis  

## 3. Perspective Projection
- 레이를 따라서 한 점들이 모여서 만들어짐
- 3차원의 공간의 점들이 2차원 점들에 닿으면서 COP로 도달하는데, 이 때 2차원들의 집합이 Projection Plane임
- Perspective Camera
    : COP를 기준으로 3차원 좌표계가 형성
<img>
    : CCD, CMOS 센서가 놓여있는 위치가 Image Plane(무한대가 아닌 한정된 영역)
    : 가장 좌측 위쪽을 Image Plane의 COP라 한다.
    : Optical axis와 Image Plane과 만나는 점이 Principal point라 한다.
    : 카메라는 거리를 측정할 수 없지만, 앵글은 측정 가능(bearing sensor)
      -> 3차원에서 2차원으로 줄어들기 때문에 거리 계산은 불가능하지만, ray들과 COP가 주어지므로, 각도를 구할 수 있다.

- World to Pixel coordinates
    : frame 간에 상관관계를 알면, world point와 기준 포인트 간의 거리를 알 수 있음
    : Rotation / Translation
    : P_w -> P_c(점이 바뀌는 것이 아니라(점은 그대로 있음), 표현하는 방식이 카메라 프레임으로 바뀌는 것)

...

    : P_w가 Image plane의 (u, v)로 픽셀 점으로 변환이 가능
    
    : Homogeneous Coordinates: 람다를 사용해서 점을 변경(3D -> 2D)
    : 각 필요한 변수를 구하는 과정을 Camera Calibration
    : 호모지니어스 matrix - 1을 넣어줘서 하나의 매트릭스로 표현 가능
    
    - Radial distortion
    : 렌즈를 사용하다보면 왜곡이 발생 가능(가장 자리로 갈 수록 곡률에 의해 영상이 왜곡)
    : 


## 4. Camera Calibration
- Intrinsic Parameters
    : 각 포인트를 가져옴


## 5. Distance Measure with Camera
- 거리 정보를 측정
    : 하나의 점을 가지고는 거리 측정이 어려움
      => 카메라를 2대 이상 사용
      => 카메라 사이의 거리차를 이용(Stereo Vision)

- Stereo Vision
    : 각 특정한 포인트가 왼쪽과 오른쪽에 각각 어디에 맺히는 지에 따라 거리를 측정
    : 
    
    : Disparity
        + 똑같은 점이 맺혔을 때 uv에서 얼마나 차이가 나는지
        + 얼마나 멀리 있느냐, 가까이 있느냐를 알 수 있음
        + 물체가 가까워지면, disparity가 커짐
    
    : Disparity Map
        + 어두울 수록 disparity값이 작음, 밝을 수록 disparity값은 커짐(카메라로부터 거리가 가깝다)
            
    : Correspondence 
        + 왼쪽과 오른쪽 이미지가 어떻게 같은가가 중요
          -> Normalized Cross-Correlation(NCC)
          -> Sum of Squared Differences(SSD)
    
    : 이전의 영상 포인트와 현재 영상 포인트를 비교하면서 얼마나 차이가 많이 나는지를 비교하면서 depths값을 구함
        

## 6. Image Processing
- 이미지/영상 처리: 이미지/영상를 개선시키기 위해(enhance)
- intensities: 에너지를 contance해서 얻은 값들(각각의 화소가 가지는 값들)
    -> 이미지: intensities의 매트릭스 형태로 나타난다.
    -> 가로 세로 사이즈가 이미지의 해상도
    -> 픽셀: 하나의 intensities를 담고 있는 것
    -> 픽셀의 개수가 resolution
    -> 1바이트로 표현함(0~255)

- 영상에서는 어떤 정보가 USEFUL, REDUNDANT 하는가?
    : 값이 바뀌는(변화율이 큰) -> USEFUL
    : intensities가 급격하게 바뀌는 지점이 고주파 성분이 바뀌는 지점
      -> 고주파 성분이 영상에서 중요한 정보

- Image filtering(특정한 성분을 제거하거나 추출하는 작업)
    1) Low pass: low frequency 성분만을 필터링
        -> 노이즈가 있는 경우(intensities에 너무 많은 노이즈가 껴있는 경우) 사용
        a) 픽셀에 대한 인텐시티브 값을 평균으로 만들어서 필터로 사용(스무딩) -> 영상이 blur해질 수 있음
        b) 인텐시티브 값들을 순서대로 만들어서 중간값을 취하는 방법
        -> 스무딩은 average를 추출
            
    2) High pass: high frequency 성분만을 필터링
        -> 정보를 사용하기 위해서 사용
        -> 차이가 많이 나는 두드러진 부분만을 추출
        -> 엣지 성분이 주로 추출
    
- spatial filter(공간상의 필터링)
    : 점 (x, y)를 중심으로 이미지의 이웃 픽셀의 영역
    : averaging filter: 마스크의 모든 필터를 합하여 평균을 내는 것

- linear filter
- shift-invarient
- correlation: 
    : 특정한 x 값을 필터링한다 -> 1x3 mask인경우 -> 1/3, 1/3, 1/3 -> 주변의 픽셀과 함께 평균을 내기 때문에
      -> 매칭되는 점(필터에 있는 값)과 곱해서 합쳐서 평균을 냄
      -> -1, 0, 1과 같이 형태는 high pass
      -> 해당 위치에 새로운 인텐시브 값을 만드는 것이 correlation
      -> 해상도가 줄어들 수 있음(첫 번째값의 경우는 왼쪽이 없기 때문에)
      -> 바운더리를 무시할 수 있음 -> 맨끝 좌우가 사라짐
      -> 기존에는 마스크를 어떻게 설정하느냐에 따라 이미지 처리의 향상이 바뀜
      -> CNN은 필터 자체를 학습시키는 것(가중치 값들)
         : 특정한 필터를 정해놓고 하면 결과가 좋을 때도 있고, 아닐 때도 있지만
         : CNN은 다수의 필터를 만들어서 학습시키는 것이므로, 평균적으로 더 높게 나타남
      -> 바운더리 처리: 무시하거나 0을 사용하거나, 첫 번째/마지막 이미지 값을 넣어주거나  
      -> 

- convolution: 

- Gaussian
    : 스무딩 필터에서 많이 사용됨
    : 가중치의 합이 1이 되도록

- derivative
    : 1차 미분: 변화율을 찾는다
    : 2차 미분: 라플라시안  

- correlation이 어디에서 쓰이나?
    : 매칭(내가 원하는 샘플과 이미지에서 어디 부분이 일치하는지)
    : 얼마만큼 비슷한지
    : 큰 값일 수록 매칭이 높음
    : SSD(Sum of Squared Differences)라고 하는 에러를 제곱하여 최소화가 되도록
    : 가장 작은 값이 가장 닮아있는 위치
    : 차이가 너무 크면 안좋기 때문에 normalize할 수도 있다
      -> Filter 값에 I를 곱하는 것(Correlation) : 가장 정확히 매칭된 포인트들 끼리 합을 하는 것이 가장 큰 값을 가질 것임
      -> 절대값이 커지는 것이 correlation이 좋다고 할 수 있지만, 어느 경우에는 차이가 많이 나는 경우에도 절대값이 커지게 되어
         일치하는 값보다 더 매칭이라고 할 수 있다(착각) -> 이를 해결
      => 지금 있는 템플릿과 현재 템플릿의 인텐시브 값을 노멀라이즈해서(0~1값) -> 코렐레이션 -> 베스트 매칭 -> 크기값을 노멀라이즈

- convolution은 correlation과 다르게 -로 되서 뒤집힘

- Edge Detection
    : 목표 - high pass filter 사용
    : 한 방향으로 인텐시티 값이 불연속적인 값을 엣지라 함
    : 엣지는 인텐시티가 급격하게 변하는 부분에 일치해서 만들어짐(검정이었다가 흰색으로 바뀌는 경계)
    : 엣지는 인텐시티 차이값을 구해서 만들 수 있음    
    : 노이즈가 있는 경우에는 인텐시티의 차이가 너무 높아서 이걸 미분하게 되면 변화율이 너무 심한 상태가 되어버림
      -> 노이즈 필터링이 필요함(가우시안)
      -> 노이즈가 스무딩되면서 제거되고, 다시 필터를 적용
      -> 이러한 것을 동시에 할 수 있는 것이 가우시안 자체를 미분(gradient gaussian)
      -> 비대칭적인 성분인 경우에만 사용 가능
      
      -> LoG(라플라시안 of 가우시안)은 대칭적인 성분에서 사용 가능
    
    : 엣지가 정말 필요하다면 엣지 부분을 바이너라이제이션 -> 논 맥시멀 서프레션을 해서 엣지 성분만 추출
      1) LOG를 통해 굵은 면처럼 생긴 선이 생김
      2) 스레싱홀딩을 통해 스레싱홀드 값보다 작으면 삭제
      3) 논 맥시멀 서프레션을 통해(finning 알고리즘) 엣지를 추출

- 이미지로부터 특징을 찾기
    1) 엣지
    2) 포인트(코너) - SIFT, SURF, ORB(포인트에 대해서 구분이 가능하도록 해주는 구분자(디스크립터)가 필요
    주변의 인텐시티브 값을 비교하면 포인트에 대해 정확히 구분이 가능할 수 있기 때문에)
    3) 직선, 평면, 등

- 피처는 어디에 쓰이나?
    1) 로봇 네비게이션
    2) 오브젝트 인식 - 포인트 피처를 찾아서, 포이트 디스크립터로 또 다른 이미지의 디스크립터와 매칭
                     -> 유사도를 찾아서 두 이미지 간의 correspondence를 계산
    3) 3D reconstruction - 매칭을 통해 오버랩되는 포인트와 뎁스를 얻어 3차원 좌표계로 나타냄
    4) Motion tracking - 이동량을 계산

- 파라노마
    1) 포인트 피처를 찾고
    2) corresponding 쌍을 찾고
    3) 각 페어를 이미지를 겹침

    * 문제
    1) 한 영상에서는 추출이 되지만, 다른 영상에서는 추출이 안됨
       -> 어느 시점에서 어떻게 뽑히더라도 특징이 추출이 되어야만을 전제로!
    
    2) 두 피처간의 코레스폰더스(같은지, 다른지)를 찾기 위해선 구분이 가능해야 함
       -> 점만으로는 구분할 수 없기 때문에

- 해리스 코너의 가장 큰 단점
    : 스케일이 달라지면, 코너가 달라짐
     -> 이를 해결하기 위해 스케일 인바이런트 피처 특징 찾기
 
 
- SIFT Feature
    : 피처 포인트의 중심이 원의 중심, 선이 피처의 오리엔테이션을 나타냄
    : 1. 키 포인트(코너같은) + 스케일
      2. 8개의 방위에 대한 히스토그램을 오리엔테이션으로 잡음
      3. 키 포인트 디스크립터를 제작
    : 이미지 그래디언트의 값을 히스토그램 형태로 바꾸면 키 포인트 디스크립터가 됨
    : 8개의 방위(45도 각도로)로 히스토그램을 만듦
    : 히스토그램이 가장 큰 값이 피처의 오리엔테이션이 되도록
    : 성능은 좋지만 속도가 느린 단점
        
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
