---
layout: post
title: Computer Vision and OpenCV [Day 6]
date: 2020-06-22 10:00:00-19:00:00
categories: OpenCV
tag: OpenCV
use_math: true
---

# 7. 영역 기반 처리 이론 및 실습 1
## 7.1. 영역 기반 처리 이론
### 7.1.1. 영역 기반 처리
- 입력 화소와 그 주위 화소를 이용하여 출력 화소값을 결정
- 회선(convolution) 기법을 널리 이용
- 영역 기반 처리: 흐리게 하기, 선명하게 하기, 경계선 검출, 잡음 제거

### 7.1.2. 회선(Convolution) 기법
<center><img src="/assets/images/opencv6/1.PNG" width="50%"></center><br>

- 입력 픽셀과 그 주위 픽셀값에 회선 마스크의 값을 곱하여 합한 결과를 만든다.
- 마스크의 값을 어떻게 하느냐에 따라 영상이 흐려지고, 선명해지고, 경계선이 검출되는 등의 차이가 있다.
- 회선 마스크의 크기는 홀수를 사용해야 한다.
    + 주위 픽셀값을 각 방향에 대칭적으로 고려해야 하므로
    + 3x3, 5x5, 7x7 등의 크기
- 많은 회선 마스크들은 계수의 합이 1이 되어야 한다.
    + 회선된 영상은 원영상과 같은 평균 밝기값을 가져야 하므로
    + 회선의 합이 만약 1이 아닌 2가 되면, 2배로 밝아진다.

- 경계선 검출 등 일부 회선 마스크에서는 음수의 계수를 포함하고, 계수의 합이 0이 된다.
    + 음의 화소값들이 생성될 수 있으므로 전형적으로 생성된 화소값에 일정한 상수(최대밝기/2)가 더해진다.

### 7.1.3. 영상 부드럽게 하기(Blurring)
<center><img src="/assets/images/opencv6/2.PNG" width="50%"></center><br>

- 입력 픽셀값을 주위 픽셀값들과의 평균값으로 변환하는 회선 마스크를 사용한다.
- 영상을 더 흐리게 하고 싶으면, 영역의 크기를 높여서 하면됨(3x3보다 5x5가 평균적으로 더 뿌옇다)

    <center><img src="/assets/images/opencv6/3.PNG" width="50%"></center><br>

- 가우시안 스무딩(=블러링)
    + 모든 픽셀에 똑같이 가중치를 부여해서 블러링을 했던 것과 달리, 가우시안 스무딩은 중심에 있는 픽셀에 높은 가중치를 부여한다.
    + 가우시안 분포 함수를 근사하여 생성한 필터 마스크를 사용한다.
    + 가우시안 분포(Gaussian distribution)은 평균을 중심으로 좌우 대칭의 종모양을 갖는 확률분포를 말하며, 정규분포라고도 한다.
    + 가우시안 분포는 평균과 표준편차에 따라 형태가 결정된다.

        <center><img src="/assets/images/opencv6/4.PNG" width="50%"></center><br>

### 7.1.4. 경계선 검출
- 경계선
    + 입력 영상에 가장 중요한 특징을 추출한다.
    + 물체를 식별하고 물체의 위치, 모양, 크기 등을 인지하는 데 큰 역할을 한다.
    + 영상의 밝기가 낮은값에서 높은값으로 또는 높은값에서 낮은값으로 *변하는 지점*에 존재한다.
    + 컴퓨터는 경계를 x축/y축 방향의 밝기값 차이가 큰 픽셀로 판단할 수 있다.

        <center><img src="/assets/images/opencv6/5.PNG" width="50%"></center><br>
        <center><img src="/assets/images/opencv6/6.PNG" width="50%"></center><br>

    + x축 방향의 밝기값 차이 = x축 방향의 미분(미분은 데이터의 변화율이므로)
    + y축 방향의 밝기값 차이 = y축 방향의 미분
<br><br>

- 영상의 미분
    + 잘 알려진 다항함수나 수학함수 등의 미분은 공식을 통해 충분히 계산이 가능하다.  
    + 영상은 2차원 평면 위에 픽셀값이 정형화되지 않은 상태로 나열되어있기 때문에 미분 공식을 적용할 수 없다.  
    + 따라서 영상으로부터 미분을 계산하려면 2가지를 고려해야 한다.  
      1) 영상이 2차원 평면에서 정의된 함수이다.  
      2) 영상이 정수 단위 좌표에 픽셀이 나열되어있는 이산함수이다.  
      
    + 1차 미분의 근사화
        <center><img src="/assets/images/opencv6/9.PNG" width="50%"></center><br>
    
	+ I는 1차원 이산함수, h는 이산값의 간격
	+ 영상에서는 h는 픽셀의 간격이라 생각하며, 보통 픽셀 간격의 최소 단위인 1을 h값으로 사용
	+ 순방향 차이(=전진 차분)는 자기 자신 바로 앞에 있는 픽셀이 자기 자신 픽셀값을 뺀 형태
	+ 역방향 차이(=후진 차분)는 자기 자신 픽셀에서 바로 뒤에 있는 픽셀값을 뺀 형태
	+ 중간값 차이(=중앙 차분)는 자기 자신을 지외하고 바로 앞과 뒤에 있는 픽셀값을 이용하는 방법으로, 가장 오류가 적고 많이 사용됨

    + 영상은 2차원 평면이기 때문에 경계(엣지)를 찾기 위해서는 영상을 가로 방향과 세로 방향으로 각각 미분해야 한다.
        + 가로 방향으로 미분한다는 것은 y좌표는 고정한 상태에서 x축 방향으로만 미분 근사를 계산한다는 것 -> x축 방향으로의 편미분(partial derivative)
        + 세로 방향으로 미분한다는 것 -> y축 방향으로의 편미분
        
        <center><img src="/assets/images/opencv6/10.PNG" width="50%"></center><br>

    + 중앙 차분에 의한 미분 근사 마스크(x축 방향으로의 편미분, y축 방향으로의 편미분)
        <center><img src="/assets/images/opencv6/11.PNG" width="50%"></center><br>
        <center><img src="/assets/images/opencv6/12.PNG" width="50%"></center><br>

    + 2차원 공간에서 정의된 영상에서 경계를 검출하려면 x축 방향과 y축 방향의 편미분을 모두 사용해야 한다.
	+ 그래디언트(Gradient)를 이용한다.
          
	<center><img src="/assets/images/opencv6/7.PNG" width="50%"></center><br>

	+ 그래디언트는 벡터이기 때문에 크기(magnitude)와 방향(phase) 성분으로 표현할 수 있다.
        + 그래디언트 벡터의 방향은 변화율이 큰 방향을 나타내고, 벡터의 크기는 변화율의 세기를 나타내는 척도이다.

    + 2차원 영상에서 경계를 찾는 방법은 그래디언트 크기가 특정값보다 큰 위치를 찾는 것이다.
    + 엣지 여부를 판단하기 위해 기준이 되는 값을 임계값(threshold) 또는 문턱치라고 한다.
<br><br>

- 다양한 경계 검출 마스크
<center><img src="/assets/images/opencv6/13.PNG" width="50%"></center><br>

    1) 1차 미분 마스크 방식
       
        로버츠 마스크(Roberts Mask)
        : 3x3 마스크에 대각선 방향에 -1과 1을 배치해 x축 방향과 y축 방향의 편미분을 구하는 방식
        : 로버츠 마스크는 대각선 원소를 제외한 나머지 원소의 값이 모두 0이어서 다른 1차 미분 마스크에 비해서 수행속도가 빠르다.
        : 한 번만 차분을 계산하기 때문에 차분의 크기가 작고 이로 인해서 경계가 확실한 윤곽선만을 추출하지만 노이즈에 매우 민감하고 윤곽선의 강도가 약하다.
       
<center><img src="/assets/images/opencv6/14_1.PNG" width="50%"></center><br>            
<center><img src="/assets/images/opencv6/14.PNG" width="50%"></center><br>

        프레윗 마스크(Prewiit Mask)
        : 로버츠 마스크와 달리 3번의 차분을 윤관선의 강도가 강하며, 수직과 수평 엣지를 동등하게 찾는데 효과적이다.

<center><img src="/assets/images/opencv6/15_1.PNG" width="50%"></center><br>
<center><img src="/assets/images/opencv6/15.PNG" width="50%"></center><br>

        소벨 마스크(Sobel Mask)
        : 소벨 마스크는 프레윗 마스크의 중심화소의 차분을 2배 증폭시킨 방식이다.
        : 중심화소의 차분을 증폭시킨 마스크이기 때문에, 가로성분과 세로성분뿐만 아니라, 대각성분의 윤곽선도 잘 검출하는 특징이 있다.

<center><img src="/assets/images/opencv6/16_1.PNG" width="50%"></center><br>  
<center><img src="/assets/images/opencv6/16.PNG" width="50%"></center><br>
<center><img src="/assets/images/opencv6/17.PNG" width="50%"></center><br>

    2) 2차 미분 마스크 방식

        1차 미분 마스크는 밝기가 급격하게 변화하는 영역뿐만 아니라 점진적으로 변화하는 부분까지 민감하게 윤곽선을 검출하여 너무 많은 윤곽선이 나타날 수 있다.
        2차 미분 마스크는 이를 보완하는 방법으로, 변화하는 영역의 중심에 있는 윤곽선만을 검출하여, 밝기가 점진적으로 변화하는 영역은 반응하지 않는다.
        
        라플라시안 마스크(Laplacian Mask)
        : 라플라시안 마스크는 중심화소를 4배 증폭시키고, 상하좌우 화소는 중심화소의 반대부호를 가지도록 설정한다.
        : 4방향과 8방향법이 있으며, 모든 방향의 윤곽선을 검출하기 때문에 다른 마스크에 비해 날카로운 윤곽선을 검출한다.
        : 하나의 마스크로 연산을 수행하기 때문에, 연산 속도가 빠르다.

<center><img src="/assets/images/opencv6/18.PNG" width="50%"></center><br>

    3) 캐니 엣지 검출 방식
        
        좋은 경계 검출기의 조건은
          
          i) 정확한 검출(Good Detection): 경계가 아닌 점을 경계로 찾거나 또는 경계인 점을 검출하지 못하는 확률을 최소화해야 한다.
          ii) 정확한 위치(Good Localization): 실제 경계의 중심을 찾아야 한다.
          iii) 단일 엣지(Single Edge): 하나의 경계는 하나의 점으로 표현되어야 한다.
                
        캐니 엣지 디텍션은 다른 방식들보다 가장 우월한 성능을 가진 윤곽선 검출 방식이다.
        특히 윤곽을 가장 잘 찾으면서, 노이즈를 제거할 수 있다.
        
        케니 엣지 디텍션 기법은 다음과 같이 총 4단계로 구성된다.
        
          <1단계> 가우시안 필터링(=스무딩)
          
            노이즈 제거를 위한 블리링 작업이라고도 하는, 스무딩 작업을 수행한다.
            -> 영상을 부드럽게 만들기

<center><img src="/assets/images/opencv6/19.png" width="50%"></center><br>
       
          <2단계> 그래디언트 계산(크기 & 방향)
          
            소벨 마스크를 이용해 회선을 수행한다. 이미지의 강도가 급격하게 변하는 부분이 윤곽선이기 때문에, 미분을 통해 엣지를 검출한다.
            다만, 1차 미분 방식에서 사용되는 소벨 마스크와 달리 정확한 엣지를 찾기 위해 그래디언트 방향과 크기를 모두 고려한다.
            
          <3단계> 비최대 억제(Non-maximum suppresion)
            
            하나의 경계가 여러 개의 픽셀로 표현되는 현상을 없애기 위해 그래디언트 크기가 국지적 최대(local maximum)인 픽셀만을 경계 픽셀로 설정한다.
            즉, 딥러닝을 적용해서 물체를 인식하면 엄청나게 많이 경계가 잡힌다. 그 중에서 확률이 가장 높은 것을 제외한 나머지는 제거하는 방식이다.
          
          <4단계> 이중 임계값을 이용한 히스테리시스 경계 트래킹
            
            소벨 엣지 검출 방법에서는 그래디언트 크기가 특정 임계값보다 크면 엣지 픽셀로, 작으면 엣지가 아닌 픽셀로 판단했다.
            하지만 이 방법은 조명이나 임계값이 조금만 바뀌어도 판단 결과가 달라질 수 있기 때문에, 캐니 엣지 디텍션에서는 두 개의 임계값을 사용한다.

            가장 강한 경계만을 3단계를 통해 살려두었는데, 이 중에서도 임계값(threshold)을 통해 최종적으로 경계를 판별하는 작업이다.
            low threshold와 high threshold를 정의하고, low 이하의 세기를 가지는 윤곽선은 모두 삭제한다.
            low threshold와 high threshold 사이의 영역에 존재하는 윤곽선은 약한 윤곽선,
            high threshold 이상의 영역에 존재하는 윤곽선은 강한 윤곽선으로 설정하여 최종 엣지로 선정한다.
            
<center><img src="/assets/images/opencv6/20.PNG" width="50%"></center><br>
<center><img src="/assets/images/opencv6/21.PNG" width="50%"></center><br>
<center><img src="/assets/images/opencv6/22.PNG" width="50%"></center><br>

### 7.1.5. 경계 확률 모델
- 경계 확률 모델은 $$P_b(x, y, \Theta)$$
- 밝기 그래디언트(Brightness Gradient)
  + 각 반원의 밝기값 분포를 비교한다.
  + 각 반원의 히스토그램을 계산하고 비교한다.

<center><img src="/assets/images/opencv6/23.PNG" width="50%"></center><br>
<br><br>

- 컬러 그래디언트(Color Gradient)
  + 컬러 공간 CIE L*a*b
  + 2D 결합(a*b) - 연산 복잡도가 올라간다.
  + 1D 정보(a+b)
<br><br>

- 텍스처 그래디언트(Texture Gradient)
  + 각 픽셀에 대한 텍스처값은 13개의 필터 뱅크를 사용하여 결정한다.
  + 하지만 각 픽셀이 13개의 특징 벡터를 갖게 되어 두 반원의 특성을 계산하는 데 시간이 오래 소요된다.
  + 각 필터 뱅크에 대한 반응 벡터를 k-means 클러스터링으로 군집화한다.
  + 각 클러스터의 중심을 texture primitives로 명명한다.

<center><img src="/assets/images/opencv6/24.PNG" width="100%"></center><br>

### 7.1.6. 영상의 선명화(Sharpening)
- 블러링의 반대 개념으로 초점이 잘 맞은 사진처럼 객체의 윤곽이 뚜렷하게 구분되는 영상을 의미한다.
- 영상의 선명화(=경계가 강화됨)
    + 선형 마스크는 분해가 가능 (1,-1), (1, -1), ..., (1, -1), (1) 5개로 쪼개보자.  
    + 마지막의 (1)로 인해 자기 자신을 더하기 때문에 더욱 선명해진다.
<br><br>

- 언샤프 마스크 필터링
    + 샤프닝을 구현하기 위해 블러링된 영상을 사용한다. 블러링이 적용되어 부드러워진 영상을 활용하여 반대로 날카로운 영상을 생성한다는 것이다.
    + 블러링이 적용된 날카롭지 않은 영상을 언샤프(unsharp)라고 하며, 언샤프한 영상을 이용하여 역으로 날카로운 영상을 생성하는 필터를 언샤프 마스크 필터라고 한다.
    
        <center><img src="/assets/images/opencv6/25.PNG" width="50%"></center><br>
    
        + (a)는 영상의 엣지 부근의 픽셀값이 증가하는 모양
        + (b)는 f(x, y)에 블러링을 적용한 결과
        + (c)는 입력 영상에서 블러링된 영상을 뺀 결과, g(x, y) = 날카로운 성분만을 가지고 있는 함수
        + (d)는 입력 영상에서 샤프닝이 적용된 결과, h(x, y)
<br><br>

    + 위에서 구한 샤프닝을 만드는 함수에서 g(x, y)를 $$\alpha$$의 날카로운 정도를 조절할 수 있는 파라미터를 곱해준다.

        <center><img src="/assets/images/opencv6/26.PNG" width="50%"></center><br>
        <center><img src="/assets/images/opencv6/27.PNG" width="50%"></center><br>

### 7.1.7. 잡음 제거
- 원본 신호에 추가된 원치 않는 신호를 의미
- 잡음 모델

	1) 가우시안 잡음  
     	   : 정규분포를 갖는 잡음으로, 영상의 픽셀값으로부터 *불규칙적으로* 벗어나지만 뚜렷하게 벗어나지 않는 잡음
     	   : 표준 편차가 작은 가우시안 잡음 모델일수록 잡음에 의한 픽셀값 변화가 적다.

<center><img src="/assets/images/opencv6/29.PNG" width="50%"></center><br>
  
	2) 임펄스 잡음  
	   : 영상의 픽셀값과는 *뚜렷하게 다른* 픽셀값에 의한 잡음
     
<center><img src="/assets/images/opencv6/28.PNG" width="50%"></center><br><br><br>

## 7.2. 영역 기반 처리 실습
### 7.2.1. 블러링

    1) 블러링 현상
       - 디지털 카메라로 사진 찍을 때, 초점이 맞지 않으면 -> 사진이 흐려짐
       - 이러한 현상을 이용해서 영상의 디테일한 부분을 제거하는 아웃 포거싱 기법
    
    2) 블러링(blurring)
       : 영상에서 화소값이 급격하게 변하는 부분들을 감소시켜 점진적으로 변하게 함으로써 영상이 전체적으로 부드러운 느낌이 나게 하는 기술

```cython
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;

void filter(Mat img, Mat& dst, Mat mask)		// 회선 수행 함수
{
	dst = Mat(img.size(), CV_32F, Scalar(0));			// 회선 결과 저장 행렬
	Point h_m = mask.size() / 2;						// 마스크 중심 좌표(마스크의 사이즈를 2로 나누면)

	for (int i = h_m.y; i < img.rows - h_m.y; i++) {		// 입력 행렬 반복 순회
		for (int j = h_m.x; j < img.cols - h_m.x; j++)
		{
			float sum = 0;
			for (int u = 0; u < mask.rows; u++) {	// 마스크 원소 순회
				for (int v = 0; v < mask.cols; v++)
				{
					// 마스크의 위치가 계속 변하는 것만큼 이미지의 위치 역시 계속 바뀌어야 함
					int y = i + u - h_m.y;
					int x = j + v - h_m.x;
					sum += (img.at<uchar>(y, x) * mask.at<float>(u, v));	// 회선 수식
				}
			}
			dst.at<float>(i, j) = sum; // 회선 누적값 출력화소 저장
		}
	}
}

int main()
{
	Mat image = imread("../image/filter_blur.jpg", IMREAD_GRAYSCALE);
	CV_Assert(image.data);						// 영상파일 예외 처리

	// 마스크 필터
	float data[] = {							// 사프닝 마스크 원소 지정
		1 / 9.f, 1 / 9.f, 1 / 9.f,
		1 / 9.f, 1 / 9.f, 1 / 9.f,
		1 / 9.f, 1 / 9.f, 1 / 9.f,
	};

	Mat mask(3, 3, CV_32F, data);			// 마스크 행렬 선언
	Mat blur;
	filter(image, blur, mask);				// 회선 수행
	blur.convertTo(blur, CV_8U);		// 자료형 변환

	imshow("image", image);
	imshow("blur", blur);				// 결과 행렬 윈도우에 표시

	waitKey();
	return 0;
}
```
<center><img src="/assets/images/opencv6/30.PNG" width="50%"></center><br>

### 7.2.2. 실습 1
9 x 9 블러링 mask를 적용한 소스코드를 작성하시오.

```cython
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;

void filter(Mat img, Mat& dst, Mat mask)		// 회선 수행 함수
{
	dst = Mat(img.size(), CV_32F, Scalar(0));			// 회선 결과 저장 행렬
	Point h_m = mask.size() / 2;						// 마스크 중심 좌표(마스크의 사이즈를 2로 나누면)

	for (int i = h_m.y; i < img.rows - h_m.y; i++) {		// 입력 행렬 반복 순회
		for (int j = h_m.x; j < img.cols - h_m.x; j++)
		{
			float sum = 0;
			for (int u = 0; u < mask.rows; u++) {	// 마스크 원소 순회
				for (int v = 0; v < mask.cols; v++)
				{
					// 마스크의 위치가 계속 변하는 것만큼 이미지의 위치 역시 계속 바뀌어야 함
					int y = i + u - h_m.y;
					int x = j + v - h_m.x;
					sum += (img.at<uchar>(y, x) * mask.at<float>(u, v));	// 회선 수식
				}
			}
			dst.at<float>(i, j) = sum;				// 회선 누적값 출력화소 저장
		}
	}
}

int main()
{
	Mat image = imread("../image/filter_blur.jpg", IMREAD_GRAYSCALE);
	CV_Assert(image.data);						// 영상파일 예외 처리

	// 마스크 필터
	float data[] = {							// 사프닝 마스크 원소 지정
		1 / 81.f, 1 / 81.f, 1 / 81.f, 1 / 81.f, 1 / 81.f, 1 / 81.f, 1 / 81.f, 1 / 81.f, 1 / 81.f,
		1 / 81.f, 1 / 81.f, 1 / 81.f, 1 / 81.f, 1 / 81.f, 1 / 81.f, 1 / 81.f, 1 / 81.f, 1 / 81.f,
		1 / 81.f, 1 / 81.f, 1 / 81.f, 1 / 81.f, 1 / 81.f, 1 / 81.f, 1 / 81.f, 1 / 81.f, 1 / 81.f,
		1 / 81.f, 1 / 81.f, 1 / 81.f, 1 / 81.f, 1 / 81.f, 1 / 81.f, 1 / 81.f, 1 / 81.f, 1 / 81.f,
		1 / 81.f, 1 / 81.f, 1 / 81.f, 1 / 81.f, 1 / 81.f, 1 / 81.f, 1 / 81.f, 1 / 81.f, 1 / 81.f,
		1 / 81.f, 1 / 81.f, 1 / 81.f, 1 / 81.f, 1 / 81.f, 1 / 81.f, 1 / 81.f, 1 / 81.f, 1 / 81.f,
		1 / 81.f, 1 / 81.f, 1 / 81.f, 1 / 81.f, 1 / 81.f, 1 / 81.f, 1 / 81.f, 1 / 81.f, 1 / 81.f,
		1 / 81.f, 1 / 81.f, 1 / 81.f, 1 / 81.f, 1 / 81.f, 1 / 81.f, 1 / 81.f, 1 / 81.f, 1 / 81.f,
		1 / 81.f, 1 / 81.f, 1 / 81.f, 1 / 81.f, 1 / 81.f, 1 / 81.f, 1 / 81.f, 1 / 81.f, 1 / 81.f,
	};

	Mat mask(9, 9, CV_32F, data);			// 마스크 행렬 선언
	Mat blur;
	filter(image, blur, mask);				// 회선 수행
	blur.convertTo(blur, CV_8U);		// 자료형 변환

	imshow("image", image);
	imshow("blur", blur);				// 결과 행렬 윈도우에 표시

	waitKey();
	return 0;
}
```
<center><img src="/assets/images/opencv6/31.PNG" width="50%"></center><br>

### 7.2.3. 샤프닝

    1) 샤프닝(sharpening)
       - 출력화소에서 이웃 화소끼리 차이를 크게해서 날카로운 느낌이 나게 만드는 것
       - 영상의 세세한 부분을 강조할 수 있으며, 경계 부분에서 명암대비가 증가되는 효과
       
    2) 샤프닝 마스크
       - 마스크 원소들의 값 차이가 커지도록 구성
       - 마스크 원소 전체합이 1이 되어야 입력영상 밝기가 손실없이 출력영상 밝기로 유지
       - 만약 넘어가면, 배수가 되어 사이즈가 커지게 됨

```cython
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;

void filter(Mat img, Mat& dst, Mat mask)		// 회선 수행 함수
{
	dst = Mat(img.size(), CV_32F, Scalar(0));			// 회선 결과 저장 행렬
	Point h_m = mask.size() / 2;							// 마스크 중심 좌표	

	for (int i = h_m.y; i < img.rows - h_m.y; i++) {		// 입력 행렬 반복 순회
		for (int j = h_m.x; j < img.cols - h_m.x; j++)
		{
			float sum = 0;
			for (int u = 0; u < mask.rows; u++) {	// 마스크 원소 순회
				for (int v = 0; v < mask.cols; v++)
				{
					int y = i + u - h_m.y;
					int x = j + v - h_m.x;
					sum += mask.at<float>(u, v) * img.at<uchar>(y, x);	// 회선 수식
				}
			}
			dst.at<float>(i, j) = sum;				// 회선 누적값 출력화소 저장
		}
	}
}

int main()
{
	Mat image = imread("../image/filter_sharpen.jpg", CV_LOAD_IMAGE_GRAYSCALE);
	CV_Assert(image.data);

	// 상하좌우만 샤프닝
	float data1[] = {
		0, -1, 0,
		-1, 5, -1,
		0, -1, 0,
	};
	// 대각선까지 모두 샤프닝
	float data2[] = {
		-1, -1, -1,
		-1, 9, -1,
		-1, -1, -1,
	};

	Mat mask1(3, 3, CV_32F, data1);
	Mat mask2(3, 3, CV_32F, data2);
	Mat sharpen1, sharpen2;
	filter(image, sharpen1, mask1);
	filter(image, sharpen2, mask2);
	sharpen1.convertTo(sharpen1, CV_8U);
	sharpen2.convertTo(sharpen2, CV_8U);

	imshow("sharpen1", sharpen1);
	imshow("sharpen2", sharpen2);
	imshow("image", image);
	waitKey();
	return 0;
}
```
<center><img src="/assets/images/opencv6/32.PNG" width="50%"></center><br>

### 7.2.4. 차분 연산을 통해 엣지 검출

    유사 연산자 출력화소 = max(자신의 위치 - 마스크 위치)

```cython
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;

void homogenOp(Mat img, Mat& dst, int mask_size)
{
	dst = Mat(img.size(), CV_8U, Scalar(0));
	Point h_m(mask_size / 2, mask_size / 2);

	for (int i = h_m.y; i < img.rows - h_m.y; i++) {
		for (int j = h_m.x; j < img.cols - h_m.x; j++)
		{
			uchar max = 0;
			for (int u = 0; u < mask_size; u++) {
				for (int v = 0; v < mask_size; v++)
				{
					// 나를 나타내는 변수: i, j
					// 나를 기준으로 한 주변의 변수
					int y = i + u - h_m.y;
					int x = j + v - h_m.x;

					// 나와 주변의 값을 차분한 것의 절댓값 중 가장 큰 것을 찾기
					uchar difference = abs(img.at<uchar>(i, j) - img.at<uchar>(y, x));

					if (difference > max) max = difference;
				}
			}
			dst.at<uchar>(i, j) = max;
		}
	}
}

int main()
{
	Mat image = imread("../image/edge_test.jpg", IMREAD_GRAYSCALE);
	CV_Assert(image.data);

	Mat edge;
	homogenOp(image, edge, 3);

	imshow("image", image);
	imshow("edge-homogenOp", edge);
	waitKey();
	return 0;
}
```
<center><img src="/assets/images/opencv6/33.PNG" width="50%"></center><br>

### 7.2.5. 1차 미분 마스크

    1) 미분
       - 함수의 순간 변화율을 구하는 계산 과정을 의미
       - 엣지가 화소의 밝기가 급격히 변하는 부분이기 때문에 함수의 변화율을 취하는 미분 연산을 이용해서 엣지 검출 가능
    
    2) 밝기의 변화율을 검출하는 방법
       - 밝기에 대한 기울기(gradient)를 계산

    3) 로버츠(Roberts) 마스크
       - 대각선 방향으로 1과 -1을 배치하여 구성

<center><img src="/assets/images/opencv6/34_1.PNG" width="50%"></center><br>

```cython
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;

void filter(Mat img, Mat& dst, Mat mask)
{
	dst = Mat(img.size(), CV_32F, Scalar(0));
	Point h_m = mask.size() / 2;

	for (int i = h_m.y; i < img.rows - h_m.y; i++) {
		for (int j = h_m.x; j < img.cols - h_m.x; j++)
		{
			float sum = 0;
			for (int u = 0; u < mask.rows; u++) {
				for (int v = 0; v < mask.cols; v++)
				{
					int y = i + u - h_m.y;
					int x = j + v - h_m.x;
					sum += img.at<uchar>(y, x) * mask.at<float>(u, v);
				}
			}
			dst.at<float>(i, j) = sum;
		}
	}
}

void differential(Mat image, Mat& dst, float data1[], float data2[])
{
	Mat dst1, dst2;
	Mat mask1(3, 3, CV_32F, data1);			// 입력 인수로 마스크 행렬 초기화
	Mat mask2(3, 3, CV_32F, data2);

	filter(image, dst1, mask1);					// 사용자 정의 회선 함수
	filter(image, dst2, mask2);

	// 회선 결과 두 행렬의 크기 계산
	magnitude(dst1, dst2, dst);

	dst1 = abs(dst1);							// 회선 결과 음수 원소를 양수화	
	dst2 = abs(dst2);

	dst.convertTo(dst, CV_8U);
	dst1.convertTo(dst1, CV_8U);				// 윈도우 표시 위한 형변환
	dst2.convertTo(dst2, CV_8U);
	imshow("dst1", dst1);
	imshow("dst2", dst2);
}

int main()
{
	Mat image = imread("../image/edge_test1.jpg", IMREAD_GRAYSCALE);
	CV_Assert(image.data);

	float data1[] = {
		-1, 0, 0,
		0, 1, 0,
		0, 0, 0
	};
	float data2[] = {
		0, 0, -1,
		0, 1, 0,
		0, 0, 0
	};

	Mat dst;
	differential(image, dst, data1, data2); // 두 방향의 회선 수행 및 크기 계산

	imshow("image", image);
	imshow("로버츠 에지", dst);
	waitKey();
	return 0;
}
```  
<center><img src="/assets/images/opencv6/34.PNG" width="50%"></center><br>

    4) 프리윗(Prewitt) 마스크
       - 로버츠 마스크의 단점을 보완하기 위해 고안
       - 수직 마스크: 원소의 배치가 수직 방향으로 구성, 엣지의 방향도 수직
       - 수평 마스크: 원소의 배치가 수평 방향으로 구성, 엣지의 방향도 수평

<center><img src="/assets/images/opencv6/35_1.PNG" width="50%"></center><br>

```cython
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;

void differential(Mat image, Mat& dst, float data1[], float data2[])
{
	Mat dst1, mask1(3, 3, CV_32F, data1);
	Mat dst2, mask2(3, 3, CV_32F, data2);

	filter2D(image, dst1, CV_32F, mask1);		// OpenCV 제공 회선 함수
	filter2D(image, dst2, CV_32F, mask2);
	magnitude(dst1, dst2, dst);
	dst.convertTo(dst, CV_8U);

	convertScaleAbs(dst1, dst1);				// 절대값 및 형변환 동시 수행 함수
	convertScaleAbs(dst2, dst2);
	imshow("dst1 - 수직 마스크", dst1);						// 윈도우 행렬 표시
	imshow("dst2 - 수평 마스크", dst2);

}

int main()
{
	Mat image = imread("../image/edge_test1.jpg", IMREAD_GRAYSCALE);
	CV_Assert(image.data);

	float data1[] = {
		-1, 0, 1,
		-1, 0, 1,
		-1, 0, 1,

	};
	float data2[] = {
		-1, -1, -1,
		0, 0, 0, 
		1, 1, 1,
	};

	Mat dst;
	differential(image, dst, data1, data2);

	imshow("image", image);
	imshow("프리윗 에지", dst);
	waitKey();
	return 0;
}
```       
<center><img src="/assets/images/opencv6/35.PNG" width="50%"></center><br>

    5) 소벨(Sobel) 마스크
       - 프리윗 마스크와 유사, 중심화소의 차분에 대한 비중을 2배 키운 것
       - 수직, 수평 방향의 엣지를 검출한다.

<center><img src="/assets/images/opencv6/36_1.PNG" width="50%"></center><br>

```cython
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;

void differential(Mat image, Mat& dst, float data1[], float data2[])
{
	Mat dst1, mask1(3, 3, CV_32F, data1);
	Mat dst2, mask2(3, 3, CV_32F, data2);

	filter2D(image, dst1, CV_32F, mask1);		// OpenCV 제공 회선 함수
	filter2D(image, dst2, CV_32F, mask2);
	magnitude(dst1, dst2, dst);
	dst.convertTo(dst, CV_8U);

	convertScaleAbs(dst1, dst1);				// 절대값 및 형변환 동시 수행 함수
	convertScaleAbs(dst2, dst2);
	imshow("dst1 - 수직 마스크", dst1);						// 윈도우 행렬 표시
	imshow("dst2 - 수평 마스크", dst2);
}

int main()
{
	Mat image = imread("../image/edge_test1.jpg", IMREAD_GRAYSCALE);
	CV_Assert(image.data);

	float data1[] = {
		-1, 0, 1,
		-2, 0, 2,
		-1, 0, 1
	};
	float data2[] = {
		-1, -2, -1,
		0, 0, 0,
		1, 2, 1
	};

	Mat dst, dst3, dst4;
	differential(image, dst, data1, data2);	// 두 방향 소벨 회선 및 크기 계산

	// OpenCV 제공 소벨 에지 계산
	Sobel(image, dst3, CV_32F, 1, 0, 3);		// x방향 미분 - 수직 마스크
	Sobel(image, dst4, CV_32F, 0, 1, 3);		// y방향 미분 - 수평 마스크
	convertScaleAbs(dst3, dst3);				// 절대값 및 uchar 형변환
	convertScaleAbs(dst4, dst4);

	imshow("image", image), imshow("소벨에지", dst);
	imshow("dst3-수직_OpenCV", dst3), imshow("dst4-수평_OpenCV", dst4);

	waitKey();
	return 0;
}
```
<center><img src="/assets/images/opencv6/36.PNG" width="50%"></center><br>
<center><img src="/assets/images/opencv6/37.PNG" width="50%"></center><br>
<center><img src="/assets/images/opencv6/38.PNG" width="50%"></center><br>

 
### 7.2.6. 2차 미분 마스크

    1) 라플라시안 엣지 검출
       - 모든 방향의 엣지를 검출

<center><img src="/assets/images/opencv6/39_1.PNG" width="50%"></center><br>
       
```cython
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;

int main()
{
	Mat image = imread("../image/laplacian_test.jpg", IMREAD_GRAYSCALE);
	CV_Assert(image.data);

	short data1[] = {
		0, 1, 0,
		1, -4, 1,
		0, 1, 0
	};
	short data2[] = {
		-1, -1, -1,
		-1, 8, -1,
		-1, -1, -1
	};

	Mat dst1, dst2, dst3;
	Mat mask4(3, 3, CV_16S, data1);
	Mat mask8(3, 3, CV_16S, data2);

	// filter2D를 이용한 라플라시안 수행
	filter2D(image, dst1, CV_32F, mask4);
	filter2D(image, dst2, CV_32F, mask8);
	Laplacian(image, dst3, CV_16S, 1);

	// 절댓값 및 uchar형 변환
	// 음수이면 화면이 깨짐 -> 항상 양수를 해줘야 함
	convertScaleAbs(dst1, dst1);
	convertScaleAbs(dst2, dst2);
	convertScaleAbs(dst3, dst3);

	imshow("image", image);
	imshow("filter2D_4방향", dst1);
	imshow("filter2D_8방향", dst2);
	imshow("Laplacian_OpenCV", dst3);
	waitKey();
	return 0;
}
```
<center><img src="/assets/images/opencv6/39.PNG" width="50%"></center><br>

    2) LoG(Laplacian of Gaussian)
       - 라플라시안은 잡음에 민감하다는 단점이 있다.
       - 먼저 가우시안 잡음 필터링을 통해 잡음을 제거한 후 라플라시안을 수행하면, 잡음에 강한 엣지 검출이 가능해진다.

<center><img src="/assets/images/opencv6/40_1.PNG" width="50%"></center><br>

    3) DoG(Difference of Gaussian)
       - 단순한 방법의 2차 미분 계산 방식이다.
       - 가우시안 스무딩 필터링의 차이를 이용해서 엣지를 검출하는 방법이다.

<center><img src="/assets/images/opencv6/40_2.PNG" width="50%"></center><br>

```cython
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;

// LOG 마스크 생성 함수
Mat getLoGmask(Size size, double sigma)
{
	double ratio = 1 / (CV_PI * pow(sigma, 4.0));
	int center = size.height / 2;
	Mat dst(size, CV_64F);

	for (int i = 0; i < size.height; i++) {
		for (int j = 0; j < size.width; j++)
		{
			// 주의(마스크의 가운데 값을 빼고 제곱)
			int x2 = (j - center) * (j - center);
			int y2 = (i - center) * (i - center);

			double value = (x2 + y2) / (2 * sigma * sigma);
			dst.at<double>(i, j) = -ratio * (1 - value) * exp(-value);
		}
	}

	// 전체 계수의 합이 1이 되도록
	double scale = (center * 10 / ratio);
	return dst * scale;
}

int main()
{
	Mat image = imread("../image/laplacian_test.jpg", IMREAD_GRAYSCALE);
	CV_Assert(image.data);

	double sigma = 1.4;
	Mat LoG_mask = getLoGmask(Size(9, 9), sigma);

	cout << LoG_mask << endl;
	cout << sum(LoG_mask) << endl;

	Mat dst1, dst2, dst3, dst4, gaus_img;
	filter2D(image, dst1, -1, LoG_mask);
	GaussianBlur(image, gaus_img, Size(9, 9), sigma, sigma);
	Laplacian(gaus_img, dst2, -1, 5);

	GaussianBlur(image, dst3, Size(1, 1), 0.0);
	GaussianBlur(image, dst4, Size(9, 9), 1.6);
	Mat dst_DoG = dst3 - dst4; // 가우시안 스케일이 다른 필터 2개를 서로 빼면 DoG

	normalize(dst_DoG, dst_DoG, 0, 255, CV_MINMAX);

	imshow("image", image);
	imshow("dst1 - LoG_filter2D", dst1);
	imshow("dst2 - LOG_OpenCV", dst2);
	imshow("dst_DoG - DOG_OpenCV", dst_DoG);
	waitKey();
	return 0;
}
```
<center><img src="/assets/images/opencv6/40.PNG" width="50%"></center><br>

### 7.2.7. 캐니 엣지 검출

    캐니 엣지 검출
    
        - 잡음은 다른 부분과 경계를 이루는 경우가 많다.
        - 대부분의 엣지 검출 방법이 이 잡음들을 엣지로 검출한다.
        - 이러한 문제를 보완하는 방법 중 하나가 캐니 엣지 검출 기법이다.
        - 케니 엣지 검출 알고리즘 순서
          1단계. 블러링을 통한 노이즈 제거(가우시안 블러링)
          2단계. 화소 기울기(gradient)의 강도와 방향 검출(소벨 마스크)
          3단계. 비최대치 억제(non-maximum suppression)
          4단계. 이력 임계값(hysteresis threshold)으로 엣지 결정
          
    1) 블러링
       - 5x5 크기의 가우시안 필터 적용
       - 불필요한 잡음 제거
       - 필터 크기는 변경 가능
       
    2) 화소 기울기(gradient) 검출
       - 가로 방향과 세로 방향의 소벨 마스크로 회선 적용
       - 회선화된 행렬로 화소 기울기의 크기(magnitude)와 방향(direction) 계산
       - 기울기 방향은 4개의 방향(0, 45, 90, 135)으로 근사하여 단순화
       
    3) 비최대치 억제(non-maximum suppression)
       - 기울기 방향과 엣지의 방향은 수직
       - 기울기 방향에 따른 화소 선택

<center><img src="/assets/images/opencv6/41.PNG" width="50%"></center><br>
<center><img src="/assets/images/opencv6/42.PNG" width="50%"></center><br>       
       
    4) 이력 임계값 방법(hysteresis thresholding)
       - 두 개의 임계값을 사용해 엣지 이력 추적으로 엣지 결정
       - 각 화소에서 높은 임계값보다 크면 엣지 추적 시작
         -> 추적이 시작되면 추적하지 않은 이웃 화소로 낮은 임계값보다 큰 화소를 엣지로 결정한다.

<center><img src="/assets/images/opencv6/43.PNG" width="50%"></center><br>     

```cython
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;

// 기울기 방향(0도, 45도, 90도, 135도)
// 기울기 방향과 엣지의 방향은 수직
void calc_direct(Mat Gy, Mat Gx, Mat& direct)
{
	direct.create(Gy.size(), CV_8U);

	for (int i = 0; i < direct.rows; i++) {
		for (int j = 0; j < direct.cols; j++) {
			float gx = Gx.at<float>(i, j);
			float gy = Gy.at<float>(i, j);
			int theat = int(fastAtan2(gy, gx) / 45); // dy/dx 미분
			direct.at<uchar>(i, j) = theat % 4; // 0도: 0, 45도: 1, 90도: 2, 135도: 3
		}
	}
}

void supp_nonMax(Mat sobel, Mat  direct, Mat& dst)		// 비최대값 억제
{
	dst = Mat(sobel.size(), CV_32F, Scalar(0));

	// i가 행방향(세로) - y, j가 열방향(가로) - x
	for (int i = 1; i < sobel.rows - 1; i++) {
		for (int j = 1; j < sobel.cols - 1; j++)
		{
			int   dir = direct.at<uchar>(i, j);				// 기울기 값
			float v1, v2;
			if (dir == 0) {			// 기울기 방향 0도 방향
				v1 = sobel.at<float>(i, j-1); // 왼쪽 벡터
				v2 = sobel.at<float>(i, j+1); // 오른쪽 벡터
			}
			else if (dir == 1) {		// 기울기 방향 45도
				v1 = sobel.at<float>(i+1, j+1);
				v2 = sobel.at<float>(i-1, j-1);
			}
			else if (dir == 2) {		// 기울기 방향 90도
				v1 = sobel.at<float>(i-1, j);
				v2 = sobel.at<float>(i+1, j);
			}
			else if (dir == 3) {		// 기울기 방향 135도
				v1 = sobel.at<float>(i+1, j-1);
				v2 = sobel.at<float>(i-1, j+1);
			}

			// 현재 자신의 위치
			float center = sobel.at<float>(i, j);
			// 나의 소벨값이 주변보다 크면 나의 소벨을 출력, 주변보다 작다면 0으로 출력
			dst.at<float>(i, j) = (center > v1 && center > v2) ? center : 0;
			// non-maximum을 통과하면서 필요없는 sobel은 0으로 만들어주면서 dst로 4단계에서 사용
		}
	}
}

void trace(Mat max_so, Mat& pos_ck, Mat& hy_img, Point pt, int low)
{
	// 영상의 범위가 벗어나가면 재귀함수 탈출
	Rect rect(Point(0, 0), pos_ck.size());
	if (!rect.contains(pt)) return;			// 추적화소의 영상 범위 확인 

	if (pos_ck.at<uchar>(pt) == 0 && max_so.at<float>(pt) > low)
	{
		pos_ck.at<uchar>(pt) = 1;			// 추적 완료 좌표
		hy_img.at<uchar>(pt) = 255;			// 에지 지정

		// 추적 재귀 함수(주변 8방향을 다시 한 번 조사)
		trace(max_so, pos_ck, hy_img, pt + Point(-1, -1), low);
		trace(max_so, pos_ck, hy_img, pt + Point(0, -1), low);
		trace(max_so, pos_ck, hy_img, pt + Point(+1, -1), low);
		trace(max_so, pos_ck, hy_img, pt + Point(-1, 0), low);

		trace(max_so, pos_ck, hy_img, pt + Point(+1, 0), low);
		trace(max_so, pos_ck, hy_img, pt + Point(-1, +1), low);
		trace(max_so, pos_ck, hy_img, pt + Point(0, +1), low);
		trace(max_so, pos_ck, hy_img, pt + Point(+1, +1), low);
	}
}

void  hysteresis_th(Mat max_so, Mat& hy_img, int low, int high)
{
	Mat pos_ck(max_so.size(), CV_8U, Scalar(0));
	hy_img = Mat(max_so.size(), CV_8U, Scalar(0));

	for (int i = 0; i < max_so.rows; i++) {
		for (int j = 0; j < max_so.cols; j++)
		{
			// max sobel의 값이 높은 임계값(high)보다 크면 추적 시작
			if (max_so.at<float>(i, j) > high)
				// 추적하면 추적하지 않은 이웃 화소로 낮은 임계값보다 큰 화소를 에지로 결정
				trace(max_so, pos_ck, hy_img, Point(j, i), low);
		}
	}
}

int main()
{
	Mat image = imread("../image/cannay_tset.jpg", IMREAD_GRAYSCALE);
	CV_Assert(image.data);

	Mat gau_img, Gx, Gy, direct, sobel, max_sobel, hy_img, canny;

	GaussianBlur(image, gau_img, Size(5, 5), 0.3); // 1단계. 가우시안 블러링
	Sobel(gau_img, Gx, CV_32F, 1, 0, 3); // 2단계. 소벨 마스크 - X축
	Sobel(gau_img, Gy, CV_32F, 0, 1, 3); // 2단계. 소벨 마스크 - Y축
	sobel = abs(Gx) + abs(Gy);
	// magnitude(Gx, Gy, sobel);

	calc_direct(Gy, Gx, direct); // 3단계. 비최대치 억제 - 그래디언트 방향을 찾기
	supp_nonMax(sobel, direct, max_sobel); // 3단계. 비최대치 억제
	hysteresis_th(max_sobel, hy_img, 100, 150); // 4단계. 이력 임계값 방법(최소 100, 최대 150)

	Canny(image, canny, 100, 150);

	imshow("image", image);
	imshow("canny", hy_img);
	imshow("OpenCV_canny", canny);
	waitKey();
	return 0;
}
```
<center><img src="/assets/images/opencv6/44.PNG" width="50%"></center><br>

### 7.2.8. 최댓값/최솟값 필터링

    입력 영상의 해당 화소(중심화소)에서 마스크로 씌워진 영역의 입력화소들을 가져와서 그 중에 최댓값/최솟값을 출력화소로 결정하는 방법
    
<center><img src="/assets/images/opencv6/45.PNG" width="50%"></center><br>

```cython
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;

void minMaxFilter(Mat img, Mat& dst, int size, int flag = 1)
{
	dst = Mat(img.size(), CV_8U, Scalar(0));
	Size msize(size, size);
	Point h_m = msize / 2;

	// 마스크의 가장 자리를 빼면서 범위가 벗어나지 않도록
	for (int i = h_m.y; i < img.rows - h_m.y; i++) {			// 입력 영상 조회
		for (int j = h_m.x; j < img.cols - h_m.x; j++)
		{
			Point start = Point(j, i) - h_m; // 시작위치: 현재 방문한 점으로부터 마스크의 전체의 반만큼 빼준다.
			Rect roi(start, msize); // 마스크 사각형 영역
			Mat mask = img(roi); // 마스크 영역 참조

			double minVal, maxVal;
			minMaxLoc(mask, &minVal, &maxVal); // 마스크 영역의 최소값, 최대값
			dst.at<uchar>(i, j) = (flag) ? maxVal : minVal;
		}
	}
}

int main()
{
	Mat image = imread("../image/min_max.jpg", 0);
	CV_Assert(image.data);

	Mat min_img, max_img;
	minMaxFilter(image, min_img, 5, 0);		// 5x5 마스크 최소값 필터링
	minMaxFilter(image, max_img, 5, 1);		// 5x5 마스크 최대값 필터링

	imshow("image", image);
	imshow("minFilter_img", min_img);
	imshow("maxFilter_img", max_img);
	waitKey();
	return 0;
}
```

### 7.2.9. 평균값 필터링

```cython
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;
// 평균값 필터링 
void averageFilter(Mat img, Mat& dst, int size)
{
	dst = Mat(img.size(), CV_8U, Scalar(0));

	// 영상의 이미지를 처음부터 시작 -> 마스크의 범위가 벗어날 수 있음
	for (int i = 0; i < img.rows; i++) {
		for (int j = 0; j < img.cols; j++)
		{
			Point pt1 = Point(j - size / 2, i - size / 2); // 마스크의 시작위치
			Point pt2 = pt1 + (Point)Size(size, size); // 마스크의 종료위치

			// 좌표 예외처리
			if (pt1.x < 0) { pt1.x = 0; } // 0보다 작으면 0
			if (pt1.y < 0) { pt1.y = 0; } // 0보다 작으면 0
			if (pt2.x > img.cols) { pt2.x = img.cols; } // 너비보다 크면 너비값
			if (pt2.y > img.rows) { pt2.y = img.rows; } // 높이보다 크면 높이값

			// 시작위치와 종료위치로 마스크 사각형 선언
			Rect mask_rect(pt1, pt2); // pt1과 pt2의 넓이가 자동 계산
			Mat  mask = img(mask_rect);
			dst.at<uchar>(i, j) = (uchar)mean(mask)[0];
		}
	}
}

int main()
{
	Mat image = imread("../image/avg_filter.jpg", IMREAD_GRAYSCALE);
	CV_Assert(image.data);

	Mat avg_img, blur_img, box_img;
	averageFilter(image, avg_img, 5);
	blur(image, blur_img, Size(5, 5));
	boxFilter(image, box_img, -1, Size(5, 5));

	imshow("image", image);
	imshow("avg_Filter_img", avg_img);
	imshow("blur_img", blur_img);
	imshow("box_img", box_img);
	waitKey();
	return 0;
}
```

### 7.2.10. 미디언 필터링

```cython
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;

void medianFilter(Mat img, Mat& dst, int size)
{
	dst = Mat(img.size(), CV_8U, Scalar(0));
	Size msize(size, size);
	Point h_m = msize / 2;

	for (int i = h_m.y; i < img.rows - h_m.y; i++) {
		for (int j = h_m.x; j < img.cols - h_m.x; j++)
		{
			// 입력 영상 내 마스크 영역 시작 위치
			Point start = Point(j, i) - h_m;
			// 마스크 사각형
			Rect roi(start, msize);

			Mat mask, sort_m;
			// 마스크 영역 참조 및 복사
			img(roi).copyTo(mask);

			// 1행으로 행렬 변환
			Mat one_row = mask.reshape(1, 1);

			// 행단위 정렬 수행
			cv::sort(one_row, sort_m, CV_SORT_EVERY_ROW);

			// 중간 위치 찾기
			int medium = (int)(one_row.total() / 2);

			// 중간값 출력 화소 저장
			dst.at<uchar>(i, j) = sort_m.at<uchar>(medium);

		}
	}
}

int main()
{
	Mat image = imread("../image/median_test.jpg", IMREAD_COLOR);
	CV_Assert(image.data);

	Mat gray, med_img1, med_img2;
	cvtColor(image, gray, CV_BGR2GRAY);

	medianFilter(gray, med_img1, 5); // 사용자 정의 함수
	medianBlur(gray, med_img2, 5); // OpenCV 제공 함수

	imshow("gray", gray);
	imshow("median-User", med_img1);
	imshow("median-OpenCV", med_img2);

	waitKey();
	return 0;
}
```

### 7.2.11. 가우시안 스무딩 필터링

```cython
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;

#include <time.h>

Mat getGaussianMask(Size size, double sigmaX, double sigmaY)
{
	double ratio = 1 / (sigmaX * sigmaY * CV_PI);
	double sigmaX2 = 2 * sigmaX * sigmaX;
	double sigmaY2 = 2 * sigmaY * sigmaY;

	Point center = size / 2;
	Mat mask(size, CV_64F);

	for (int i = 0; i < size.height; i++) {
		for (int j = 0; j < size.width; j++)
		{
			mask.at<double>(i, j) = ratio * exp(-(pow(j - center.x, 2) / sigmaX2 + pow(i - center.y, 2) / sigmaY2));
		}
	}
	return (mask / sum(mask)[0]);
}

int main()
{
	Mat image = imread("../image/smoothing.jpg", 0);
	CV_Assert(image.data);

	// 가우시안 마스크
	Size  size(5, 29);
	double sigmaX = 0.3 * ((size.width - 1) * 0.5 - 1) + 0.8; // 가로방향 표준편차
	double sigmaY = 0.3 * ((size.height - 1) * 0.5 - 1) + 0.8; // 세로방향 표준편차

	Mat gauss_img1, gauss_img2, gauss_img3;
	Mat gaussian_2d = getGaussianMask(size, sigmaX, sigmaY); // 2차원 가우시안 마스크 생성
	Mat gaussian_1dX = getGaussianKernel(size.width, -1, CV_64F); // 1차원 가우시안 마스크 생성 - x방향
	Mat gaussian_1dY = getGaussianKernel(size.height, -1, CV_64F); // 1차원 가우시안 마스크 생성 - y방향

	GaussianBlur(image, gauss_img2, size, sigmaX, sigmaY); // 가우시안 블러링
	sepFilter2D(image, gauss_img3, -1, gaussian_1dX, gaussian_1dY); // 1차원 가우시안 마스크로 setFilter2D() 함수에 적용
	filter2D(image, gauss_img1, -1, gaussian_2d); // 2차원 가우시안 마스크로 filter2D() 함수에 적용

	imshow("image", image);
	imshow("사용자 생성 마스크 적용", gauss_img1);
	imshow("가우시안 블러링 적용 ", gauss_img2);
	imshow("가우시안 계수로 마스크 생성", gauss_img3);

	waitKey();
	return 0;
}
```

### 7.2.12. 심화 실습 - 블러링 & 캐니 엣지

```cython
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;

int th = 50;						// 캐니 에지 낮은 임계값
Mat image, gray, edge;

void onTrackbar(int, void*)	// 트렉바 콜백 함수
{
	GaussianBlur(gray, edge, Size(3, 3), 0.7);	// 가우시안 블러링
	Canny(edge, edge, th, 2*th, 3); // 캐니에지 수행(임계값 2개)

	Mat color_edge;
	image.copyTo(color_edge, edge);		// 에지 영역만 복사(원래 영상에서 에지만 가져옴)
	imshow("원본", image);
	imshow("컬러 에지", color_edge);
}

int main()
{
	image = imread("../image/FIJI_test_color.jpg", 1);		// 컬러 영상 로드
	CV_Assert(image.data);								// 예외 처리
	cvtColor(image, gray, COLOR_BGR2GRAY);				// 명암도 변환

	namedWindow("컬러 에지", 1);
	createTrackbar("Canny th", "컬러 에지", &th, 100, onTrackbar);
	onTrackbar(th, 0);	// 트랙바 함수 호출

	waitKey(0);
	return 0;
}
```
