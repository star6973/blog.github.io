---
layout: post
title: Computer Vision and OpenCV [Day 8]
date: 2020-07-06 10:00:00-19:00:00
categories: OpenCV
tag: OpenCV
use_math: true
---

> Visual C++ 영상 처리 프로그래밍

# 9. 2D/3D 기하학 처리 이론 및 실습
## 9.1. 2D/3D 기하학 처리 이론
### 9.1.1. 기하학적 처리란?
- 임의의 기하학적 변환에 의하여 화소들의 위치를 변경하는 처리
- 크기 변환(확대/축소), 회전, 이동, 대칭
1. 크기 변환(확대/축소)
    - 영상 인식 시스템은 정해진 크기의 영상만을 입력받기 때문에 영상을 해당 크기에 맞게 변경하여 입력으로 전달해야 함.
    - 복잡한 알고리즘을 수행하기에 앞서 연산 시간을 단축하기 위하여 입력 영상의 크기를 줄여서 사용하는 경우
    - 확대의 단점: 확대된 영상에 계단 현상이 발생하여, 영상이 매끄럽지 않을 수 있음. -> 역방향 사상과 양선형 보간법을 통해 극복 가능.
    - 보간법
        + 영상을 기하학적 변환을 할 때, 원본 영상의 아무런 정보를 받지 못하는 픽셀이 생길 수 있다.  
          예를 들어, 다음 사진과 같이 확대를 하게 될 때, 확대 후 출력 픽셀에 빈 곳이 많이 생김을 알 수 있다.
          
        <center><img src="/assets/images/opencv8/1.jpg" width="70%"></center><br>
          
        + 이러한 방법을 순방향 매핑(forward mapping)이라고 하며, 이 방법은 생성한 결과 영상에 빈 공간이 생기는 문제점이 있다.
        + 따라서 순방향 매핑의 문제점을 해결하기 위해 일반적으로 역방향 매핑(backward mapping) 방법을 사용한다.
        + 역방향 매핑에서는 목적영상의 각각의 픽셀에 대하여 대응되는 입력영상의 픽셀 위치를 찾아 그 위치에서의 픽셀값을 참조한다.
          역방향 매핑을 구현할 때에는 목적영상의 전체 크기만큼의 for 루프를 반복한다. 즉, 역방향 매핑으로 크기 변환을 수행하려면 목적영상의 픽셀좌표가 입력영상의 어느 위치에 해당하는 지를 계산해야 한다.

        <center><img src="/assets/images/opencv8/1_1.jpg" width="30%"></center><br>

        + 위 수식에서 X, Y, X', Y'은 픽셀의 좌표이기 때문에 정수이다. 하지만, $$X'/S_x$$와 $$Y'/S_y$$의 계산 결과는 실수형이기 때문에, 원본 영상의 어느 좌표의 픽셀값을 참조할 것인지에 대한 결정이 필요하다.
          예를 들어, 다음 사진과 같이 목적영상의 (1, 1) 위치의 픽셀은 입력영상의 (0.5, 0.5) 위치에 해당하기 때문에, 이를 (0, 0)으로 간주해야 할지, (1, 1)로 간주해야 할지를 결정해야 한다.
          
        <center><img src="/assets/images/opencv8/1_2.jpg" width="70%"></center><br>
          
        + 이와 같이 영상의 기하학적 변환을 수행하는 경우에는 실수 좌표 상에서의 픽셀값을 결정해야 하는 경우가 발생하며, 이때 주변 픽셀값들을 이용해 원하는 위치의 값을 추정하는 방법을 보간법(interpolation)이라고 한다.
        + 보간법에는 최근접 이웃 보간법(nearest neighbor interpolation), 양선형 보간법(bilinear interpolation), 3차 회선 보간법(cubic convolution interpolation) 등이 있다.
          1) 최근접 이웃 보간법
            + 목적 영상의 화소에 가장 가깝게 이웃한 입력영상의 화소값을 가져오는 방법
            + 목적화소의 좌표를 반올림하는 알고리즘으로 비어있는 픽셀들을 채울 수 있어 쉽고 빠르게 해결이 가능하다.
              예를 들어, 역방향 매핑 시 입력영상의 참조 좌표가 (50.2, 32.8)로 계산되었다면, 이 위치에서 가장 가까운 정수 좌표인 (50, 33)의 픽셀값이 적용된다.
            + 하지만, 확대의 비율이 커지면 영상 내에서 경계선이나 모서리 부분에서 계단현상이 나타날 수 있다.

            <center><img src="/assets/images/opencv8/2.PNG" width="100%"></center><br>
            <center><img src="/assets/images/opencv8/2_1.PNG" width="50%"></center><br>

          2) 양선형 보간법
            + 최근접 이웃 보간법의 계단 현상을 보완할 수 있는 방법으로 중첩의 원리(superposition principle)이 적용되었다.
            + 두 개의 화소값을 알고 있을 때 그 값으로 직선을 그리면, 직선의 선상에 위치한 중간화소값은 수식을 통해 계산이 가능하다.
            + 즉, 인접한 네 개의 픽셀값을 이용해 실수 좌표 상의 픽셀값을 계산하는 방법이다.
            
            <center><img src="/assets/images/opencv8/3.PNG" width="100%"></center><br>
            
            + 예를 들어, 역방향 매핑에 의해 참조해야 할 원본 영상의 좌표가 (i+p, j+q)로 결정되었다고 가정하자. 여기서 i와 j는 정수이고, p와 q는 0부터 1사이의 실수이다.
              이 실수 좌표를 둘러싼 네 개의 픽셀 좌표는 (i, j), (i+1, j), (i, j+1), (i+1, j+1)이며, 이들 좌표에서의 픽셀값을 각각 a, b, c, d라고 하자.
              양선형 보간법에서는 총 3번의 선형 보간을 수행한다. 먼저, (i, j)와 (i+1, j) 사이에서의 픽셀값 x를 구한다.
              그리고 (i, j+1)와 (i+1, j+1) 사이에서의 픽셀값 y를 구한다. 마지막으로 앞에서 구한 x와 y를 이용하여 최종 픽셀값 z를 구하며, z값을 목적영상의 픽셀값으로 사용한다.
              
            <center><img src="/assets/images/opencv8/4.jpg" width="100%"></center><br>
            <center><img src="/assets/images/opencv8/5.jpg" width="100%"></center><br>
            <center><img src="/assets/images/opencv8/6.jpg" width="100%"></center><br>
            <center><img src="/assets/images/opencv8/7.jpg" width="100%"></center><br>
            
            + z는 다음과 같은 비례법칙을 이용해 계산이 가능하다.
            
            <center><img src="/assets/images/opencv8/8.PNG" width="50%"></center><br>
            <center><img src="/assets/images/opencv8/8_1.PNG" width="50%"></center><br>
            <center><img src="/assets/images/opencv8/8_2.PNG" width="50%"></center><br>

    - 축소: 서브 샘플링 방식(축소 배율만큼 건너뛰면서 픽셀값을 취하는 방법)
    - 축소의 단점: 영상의 상세한 세부항목을 상실 -> 블러링을 통해 극복 가능.
    
    <center><img src="/assets/images/opencv8/9.PNG" width="100%"></center><br>
    <center><img src="/assets/images/opencv8/9_1.PNG" width="100%"></center><br>

2. 회전
    - 원점을 중심으로 점 $$(X_source, Y_source)$$를 반시계 방향으로 $$\theta$$만큼 회전한 점 $$(X_dest, Y_dest)$$

    <center><img src="/assets/images/opencv8/10.PNG" width="100%"></center><br>

    - 고려사항 1
        + 순방향 사상을 이용한 회전 -> 출력 영상에서 픽셀값을 할당받지 못한 빈 곳이 발생할 수 있음.

        <center><img src="/assets/images/opencv8/10_1.PNG" width="100%"></center><br>
    
        + 역방향 사상을 이용하여 극복 가능.
        + 원점을 중심으로 $$(X_dest, Y_dest)$$를 시계 방향으로 $$\theta$$만큼 회전한 점이 $$(X_source, Y_source)$$가 됨.

        <center><img src="/assets/images/opencv8/10_2.PNG" width="100%"></center><br>

    - 고려사항 2
        + 회전의 중심이 원점을 기준으로 하면 목적영상에서 잘리는 부분이 발생할 수 있음.

        <center><img src="/assets/images/opencv8/11.PNG" width="100%"></center><br>

        + 영상의 중심점$$(C_x, C_y)$$을 기준으로 한 회전으로 극복 가능.

        <center><img src="/assets/images/opencv8/11_1.png" width="100%"></center><br>

    - 고려사항 3
        + 입력 영상과 출력 영상의 크기를 같게하면 출력 영상에서 잘려나가는 부분이 발생할 수 있음.
        + 출력 영상의 크기를 미리 계산하여 역방향 사상을 적용해야 함.
        + 출력 영상의 크기는 회전 각도에 따라 다르다.

        <center><img src="/assets/images/opencv8/11_2.PNG" width="50%"></center><br>
        <center><img src="/assets/images/opencv8/12.png" width="100%"></center><br>

3. 이동
    - 이동 변환된 결과 영상은 원본 영상의 크기의 바깥으로 빠져나가는 픽셀들은 보이지 않게 될 것이기 때문에, 새로 생겨난 빈 공간들은 그레이스케일 값을 0으로 설정해준다.    

    <center><img src="/assets/images/opencv8/15.jpg" width="30%"></center><br>
    <center><img src="/assets/images/opencv8/15_1.jpg" width="30%"></center><br>
    <center><img src="/assets/images/opencv8/15_2.jpg" width="30%"></center><br>

4. 대칭
    - 좌우 대칭 변환의 결과 영상은 그 크기가 입력 영상의 크기와 동일하다.

    <center><img src="/assets/images/opencv8/13.jpg" width="50%"></center><br>
    <center><img src="/assets/images/opencv8/13_1.PNG" width="50%"></center><br>

    - 상하 대칭 변환의 결과 영상 역시 그 크기가 입력 영상의 크기와 동일하다.

    <center><img src="/assets/images/opencv8/14.jpg" width="50%"></center><br>
    <center><img src="/assets/images/opencv8/14_1.PNG" width="50%"></center><br>

5. 정리  
    1) 확대  
        - 확대는 할수록 화질이 저하되는 현상이 나탐남(초점이 낮아짐, 계단현상).  
        - 해결방법: 출력 영상 크기를 정하고, 거꾸로 어디서부터 오는지를 역방향으로 처리 -> 확대됨을 가정하고, 거꾸로 원본 픽셀을 참조해서 사상(역방향 사상)  

    2) 축소  
        - 축소는 축소 배율만큼 픽셀이 건너뛰어지는 것이므로 할수록 얇은 경계가 소실됨(세부 항목 상실).  
        - 해결방법: 서브 샘플링 전에 blurring 처리 후 평균값으로 소실 부분을 대치 -> 공간주파수(단위 공간당 변화율)가 높을수록 샘플링 주파수도 높아야 소실이 되지 않는다. 즉, blurring을 통해 공간주파수가 낮아지면서 소실이 덜 된다.  

    3) 회전  
        - 순방향 사상을 이용한 회전을 사용하면 출력영상에서 픽셀값을 할당받지 못한 빈 곳이 발생함.  
        - 해결방법: 영상의 중심점을 기준으로 회전을 한다. 모든 점들을 중심으로 보내고, 다시 원상 복구를 해주는 역방향 사상이 필요.  
<br><br>

## 9.2. 2D/3D 기하학 처리 실습
### 9.2.1. 사상
### 9.2.2. 크기 변경(확대/축소)
```angular2
// 순방향 사상
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;

void scaling(Mat img, Mat& dst, Size size)			// 크기 변경 함수
{
	dst = Mat(size, img.type(), Scalar(0));	// 목적영상 생성
	double ratioY = (double)size.height / img.rows;	// 세로 변경 비율(입력의 row -> 세로)
	double ratioX = (double)size.width / img.cols;    // 가로 변경 비율(입력의 col -> 가로)

	for (int i = 0; i < img.rows; i++) { // 입력영상 순회 – 순방향 사상
		for (int j = 0; j < img.cols; j++)
		{
			int x = (int)(j*ratioX);
			int y = (int)(i*ratioY);
			
			dst.at<uchar>(y, x) = img.at<uchar>(i, j);
		}
	}
}

int main()
{
	Mat image = imread("../image/scaling_test.jpg", 0);
	CV_Assert(image.data);

	Mat dst1, dst2;
	scaling(image, dst1, Size(150, 200));		// 크기변경 수행 - 축소
	scaling(image, dst2, Size(300, 400));		// 크기변경 수행 - 확대

	imshow("image", image),
	imshow("dst1-축소", dst1);
	imshow("dst2-확대", dst2),
	resizeWindow("dst1-축소", 200, 200);		// 윈도우 크기 확장
	waitKey();
	return 0;
}
```
<center><img src="/assets/images/opencv8/15.PNG" width="100%"></center><br>

### 9.2.3. 최근접 이웃 보간법
```angular2
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;

void scaling(Mat  img, Mat& dst, Size size)			// 크기 변경 함수
{
	dst = Mat(size, img.type(), Scalar(0));				// 목적영상 생성
	double ratioY = (double)size.height / img.rows;	// 세로 변경 비율 
	double ratioX = (double)size.width / img.cols;	// 가로 변경 비율 

	for (int i = 0; i < img.rows; i++) {				// 입력영상 순회 – 순방향 사상
		for (int j = 0; j < img.cols; j++)
		{
			int x = (int)(j * ratioX);
			int y = (int)(i * ratioY);
			dst.at<uchar>(y, x) = img.at<uchar>(i, j);
		}
	}
}

void scaling_nearest(Mat  img, Mat& dst, Size size)		// 최근접 이웃 보간 
{
	dst = Mat(size, CV_8U, Scalar(0));
	double ratioY = (double)size.height / img.rows;
	double ratioX = (double)size.width / img.cols;

	// 역방향 사상(dst를 돌면서, x와 y가 원본 어디에서 나온건지 확인)
	for (int i = 0; i < dst.rows; i++) {				// 목적영상 순회 – 역방향 사상
		for (int j = 0; j < dst.cols; j++)
		{
			int x = (int)cvRound(j / ratioY);
			int y = (int)cvRound(i / ratioY);
			dst.at<uchar>(i, j) = img.at<uchar>(y, x);
		}
	}
}

int main()
{
	Mat image = imread("../image/interpolation_test.jpg", 0);
	CV_Assert(image.data);

	Mat dst1, dst2;
	scaling(image, dst1, Size(300, 300));				// 크기변경 - 기본
	scaling_nearest(image, dst2, Size(300, 300));		// 크기변경 – 최근접 이웃

	imshow("image", image);
	imshow("dst1-순방향사상", dst1);
	imshow("dst2-최근접 이웃보간", dst2);

	waitKey();
	return 0;
}
```

### 9.2.4. 양선형 보간법

```angular2
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;

void scaling_nearest(Mat  img, Mat& dst, Size size)		// 최근접 이웃 보간 
{
	dst = Mat(size, CV_8U, Scalar(0));
	double ratioY = (double)size.height / img.rows;
	double ratioX = (double)size.width / img.cols;

	for (int i = 0; i < dst.rows; i++) {				// 목적영상 순회 – 역방향 사상
		for (int j = 0; j < dst.cols; j++)
		{
			int x = (int)cvRound(j / ratioY);
			int y = (int)cvRound(i / ratioY);
			dst.at<uchar>(i, j) = img.at<uchar>(y, x);
		}
	}
}

uchar bilinear_value(Mat img, double x, double y)	// 단일 화소 양선형 보간
{
	// 입력 영상 범위가 벗어나지 않도록
	if (x >= img.cols - 1) x--;
	if (y >= img.rows - 1) y--;

	// 4개 화소 가져옴
	Point pt((int)x, (int)y);
	int A = img.at<uchar>(pt);						// 왼쪽상단 화소
	int B = img.at<uchar>(pt + Point(0, 1));		// 왼쪽하단 화소
	int C = img.at<uchar>(pt + Point(1, 0));		// 오른쪽상단 화소
	int D = img.at<uchar>(pt + Point(1, 1));		// 오른쪽하단 화소

	double alpha = y - pt.y; // 원래 값(실수)에서 pt.y(정수)의 차이
	double beta = x - pt.x;
	int M1 = A + (int)cvRound(alpha * (B - A));		// 1차 보간
	int M2 = C + (int)cvRound(alpha * (D - C));
	int P = M1 + (int)cvRound(beta * (M2 - M1));		// 2차 보간
	return  saturate_cast<uchar>(P);
}

void scaling_bilinear(Mat img, Mat& dst, Size size)		// 크기변경 – 양선형 보간
{
	dst = Mat(size, img.type(), Scalar(0));
	double ratio_Y = (double)size.height / img.rows;
	double ratio_X = (double)size.width / img.cols;

	for (int i = 0; i < dst.rows; i++) {		// 목적영상 순회 – 역방향 사상
		for (int j = 0; j < dst.cols; j++) {
			double y = i / ratio_Y;
			double x = j / ratio_X;
			dst.at<uchar>(i, j) = bilinear_value(img, x, y);		// 화소 양선형 보간
		}
	}
}

int main()
{
	Mat image = imread("../image/interpolation_test.jpg", 0);		// 명암도 영상 로드
	CV_Assert(image.data);

	Mat dst1, dst2, dst3, dst4;
	scaling_bilinear(image, dst1, Size(300, 300));		// 크기변경 – 양선형 보간
	scaling_nearest(image, dst2, Size(300, 300));		// 크기변경 – 최근접 보간
	resize(image, dst3, Size(300, 300), 0, 0, INTER_LINEAR); // OpenCV 함수 적용
	resize(image, dst4, Size(300, 300), 0, 0, INTER_NEAREST);

	imshow("image", image);
	imshow("dst1-양선형", dst1), imshow("dst2-최근접이웃", dst2);
	imshow("OpenCV-양선형", dst3), imshow("OpenCV-최근접이웃", dst4);
	waitKey();
	return 0;
}
```
<center><img src="/assets/images/opencv8/16.PNG" width="100%"></center><br>

### 9.2.5. 평행이동
```angular2
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;

void translation(Mat img, Mat& dst, Point pt)				// 평행이동 함수
{
	Rect rect(Point(0, 0), img.size());						// 입력영상 범위 사각형
	dst = Mat(img.size(), img.type(), Scalar(0));

	for (int i = 0; i < dst.rows; i++) {
		for (int j = 0; j < dst.cols; j++) {		
			int y = i - pt.y;
			int x = j - pt.x;

			// 현재 이미지가 가지고 있는 영역에서 위치값이 포함되어 있으면 이동, 넘어가면 이동 x
			if (rect.contains(Point(y, x)))
				dst.at<uchar>(i, j) = img.at<uchar>(y, x);
		}
	}

}

int main()
{
	Mat image = imread("../image/translation_test.jpg", 0);
	CV_Assert(image.data);

	Mat dst1, dst2;
	translation(image, dst1, Point(30, 80));		// 평행이동 수행
	translation(image, dst2, Point(-80, -50));

	imshow("image", image);
	imshow("dst1 - ( 30, 80) 이동", dst1);
	imshow("dst2 - (-80, -50) 이동", dst2);
	waitKey();
	return 0;
}
```
<center><img src="/assets/images/opencv8/17.PNG" width="100%"></center><br>

### 9.2.6. 회전

```angular2
// 원점 중심
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;

uchar bilinear_value(Mat img, double x, double y)
{
	if (x >= img.cols - 1)  x--;
	if (y >= img.rows - 1)  y--;

	// 4개 화소 가져옴
	Point pt((int)x, (int)y);
	int A = img.at<uchar>(pt);
	int B = img.at<uchar>(pt + Point(0, 1));
	int C = img.at<uchar>(pt + Point(1, 0));
	int D = img.at<uchar>(pt + Point(1, 1));

	//1차 보간
	double alpha = y - pt.y;
	double beta = x - pt.x;
	int M1 = A + (int)cvRound(alpha * (B - A));
	int M2 = C + (int)cvRound(alpha * (D - C));

	//2차 보간
	int P = M1 + (int)cvRound(beta * (M2 - M1));
	return  saturate_cast<uchar>(P);
}

void rotation(Mat img, Mat& dst, double dgree)			// 회전 변환 함수
{
	double radian = dgree / 180 * CV_PI;				// 회전각도 - 라디안
	double sin_value = sin(radian);							// 사인 코사인 값 미리 계산
	double cos_value = cos(radian);

	Rect rect(Point(0, 0), img.size());						// 입력 영상 범위 사각형
	dst = Mat(img.size(), img.type(), Scalar(0));			// 목적 영상 

	for (int i = 0; i < dst.rows; i++) {						// 목적영상 순회 – 역방향 사상
		for (int j = 0; j < dst.cols; j++)
		{
			double center_x = 0;
			double center_y = 0;
			double x = (j - center_x)*cos_value + (i - center_y)*sin_value + center_x;	// 회전 변환 수식
			double y = -(j - center_x)*sin_value + (i - center_y)*cos_value + center_y;

			if (rect.contains(Point2d(x, y)))
				dst.at<uchar>(i, j) = bilinear_value(img, x, y);
		}
	}
}

// pt 좌표 기준 회전 변환
void rotation(Mat img, Mat& dst, double dgree, Point pt)
{
	double radian = dgree / 180 * CV_PI;
	double sin_value = sin(radian);
	double cos_value = cos(radian);

	Rect rect(Point(0, 0), img.size());
	dst = Mat(img.size(), img.type(), Scalar(0));

	for (int i = 0; i < dst.rows; i++) {
		for (int j = 0; j < dst.cols; j++)
		{
			double jj = j - pt.x;
			double ii = i - pt.y;
			double x = jj * cos_value + ii * sin_value + pt.x;	// 회전 변환 수식
			double y = -jj * sin_value + ii * cos_value + pt.y;

			if (rect.contains(Point2d(x, y)))
				dst.at<uchar>(i, j) = bilinear_value(img, x, y);
		}
	}
}

int main()
{
	Mat image = imread("../image/rotate_test.jpg", 0);
	CV_Assert(image.data);

	Mat dst1, dst2, dst3, dst4;
	Point center = image.size() / 2;					// 영상 중심 좌표 계산
	rotation(image, dst1, 20);							// 원점 기준 회전변환
	rotation(image, dst2, 20, center);					// 영상 중심 기준 회전변환

	imshow("image", image);
	imshow("dst1-20도 회전(원점)", dst1);
	imshow("dst1-20도 회전(기준점)", dst2);
	waitKey();
	return 0;
}
```
<center><img src="/assets/images/opencv8/18.PNG" width="100%"></center><br>

### 9.2.7. 회전 심화 예제
```cython
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;
Mat image;

uchar bilinear_value(Mat img, double x, double y)
{
	if (x >= img.cols - 1)  x--;
	if (y >= img.rows - 1)  y--;

	// 4개 화소 가져옴
	Point pt((int)x, (int)y);
	int A = img.at<uchar>(pt);
	int B = img.at<uchar>(pt + Point(0, 1));
	int C = img.at<uchar>(pt + Point(1, 0));
	int D = img.at<uchar>(pt + Point(1, 1));

	//1차 보간
	double alpha = y - pt.y;
	double beta = x - pt.x;
	int M1 = A + (int)cvRound(alpha * (B - A));
	int M2 = C + (int)cvRound(alpha * (D - C));

	//2차 보간
	int P = M1 + (int)cvRound(beta * (M2 - M1));
	return saturate_cast<uchar>(P);
}

void rotation(Mat img, Mat& dst, double dgree, Point pt) // pt 좌표 기준 회전 변환
{
	double radian = dgree / 180 * CV_PI;
	double sin_value = sin(radian);
	double cos_value = cos(radian);

	Rect rect(Point(0, 0), img.size());
	dst = Mat(img.size(), img.type(), Scalar(0));

	for (int i = 0; i < dst.rows; i++) {
		for (int j = 0; j < dst.cols; j++)
		{
			int jj = j - pt.x; // pt 좌표만큼 평행이동
			int ii = i - pt.y;
			double x = jj * cos_value + ii * sin_value + pt.x;
			double y = -jj * sin_value + ii * cos_value + pt.y;

			if (rect.contains(Point2d(x, y)))
				dst.at<uchar>(i, j) = bilinear_value(img, x, y);
		}
	}
}

float calc_angle(Point pt[3]) // 중심점과 두 점으로 회전각 구하기
{
	Point2f d1 = pt[1] - pt[0];
	Point2f d2 = pt[2] - pt[0];
	float  angle1 = fastAtan2(d1.y, d1.x);
	float  angle2 = fastAtan2(d2.y, d2.x);
	return (angle2 - angle1);
}

void  onMouse(int event, int x, int y, int flags, void*)
{
	Point curr_pt(x, y);
	static Point pt[3] = {};
	static Mat tmp;
	// 회전 중심 선택
	if (flags == (EVENT_FLAG_LBUTTON | EVENT_FLAG_CTRLKEY))
	{
		pt[0] = curr_pt;
		tmp = image.clone();
		circle(tmp, pt[0], 2, Scalar(255), 2);
		imshow("image", tmp);
		cout << "회전 중심 : " << pt[0] << endl;
	}

	// 회전 각도 지정
	else if (event == EVENT_LBUTTONDOWN && pt[0].x > 0)
	{
		pt[1] = curr_pt;
		circle(tmp, pt[1], 2, Scalar(255), 2);
		imshow("image", tmp);

	}
	else if (event == EVENT_LBUTTONUP && pt[1].x > 0)
	{
		pt[2] = curr_pt;
		circle(tmp, pt[2], 2, Scalar(255), 2);
		imshow("image", tmp);
	}
	
	// 회전 수행
	if (pt[2].x > 0) {
		float angle = calc_angle(pt);
		cout << "회전각 : " << angle << endl;
		Mat dst;
		rotation(image, dst, angle, pt[0]);
		imshow("image", dst);
		pt[0] = pt[1] = pt[2] = Point(0, 0);
	}
}

int main()
{
	image = imread("../image/rotate_test.jpg", 0);
	CV_Assert(image.data);

	imshow("image", image);
	setMouseCallback("image", onMouse, 0);
	waitKey();
	return 0;
}
```
<center><img src="/assets/images/opencv8/19.PNG" width="100%"></center><br>

### 9.2.8. 행렬 연산을 통한 기하학 변환 - 어파인 변환 1
- 이동과 같은 기하학 변환은 덧셈 연산이기 때문에 알고리즘 처리가 느리다.
- 이를 행렬의 곱으로 표현하여 더욱 빠른 연산이 가능하도록 할 수 있는데, 어파인 변환 수식은 각 변환 행렬을 곱으로 구성 가능하다.

<center><img src="/assets/images/opencv8/20.PNG" width="100%"></center><br>

```cython
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;

uchar bilinear_value(Mat img, double x, double y)
{
	if (x >= img.cols - 1)  x--;
	if (y >= img.rows - 1)  y--;

	// 4개 화소 가져옴
	Point pt((int)x, (int)y);
	int A = img.at<uchar>(pt);
	int B = img.at<uchar>(pt + Point(0, 1));
	int C = img.at<uchar>(pt + Point(1, 0));
	int D = img.at<uchar>(pt + Point(1, 1));

	//1차 보간
	double alpha = y - pt.y;
	double beta = x - pt.x;
	int M1 = A + (int)cvRound(alpha * (B - A));
	int M2 = C + (int)cvRound(alpha * (D - C));

	//2차 보간
	int P = M1 + (int)cvRound(beta * (M2 - M1));
	return saturate_cast<uchar>(P);
}

void affine_transform(Mat img, Mat& dst, Mat map, Size size)
{
	dst = Mat(img.size(), img.type(), Scalar(0));
	Rect rect(Point(0, 0), img.size()); // 역사상을 위해, 원위치에 있는지 확인

	Mat inv_map;
	invertAffineTransform(map, inv_map); // 역방향 사상을 위해, 어파인 변환의 역행렬 계산

	// 역방향 사상
	for (int i = 0; i < dst.rows; i++) {
		for (int j = 0; j < dst.cols; j++)
		{
			/*
				우리는 검은색 값이 파란색쪽으로 변환되길 원하지만,
				~~ 이유로 역방향 사상을 통해 구하는 것이 더 좋다.
				따라서 파란색 입장에서 보았을 때, 자신이 변환 전, 검은색이 변환 후이므로
				(x, y, 1)가 변환전, (a, a2, a3)는 변환 행렬, (x', y')은 변환 후 값이 된다.				
			*/

			// 호모지니어스 코디네이트
			Point3d ji(j, i, 1); // 어파인 변환 후 행렬
			Mat xy = inv_map * (Mat)ji; // xy는 원영상의 행렬
			Point2d pt = (Point2d)xy;
			// pt가 xy(원영상)에 포함되는 지 확인
			if (rect.contains(pt)) {
				dst.at<uchar>(i, j) = bilinear_value(img, pt.x, pt.y);
			}
		}
	}
}

int main()
{
	Mat image = imread("../image/affine_test3.jpg", 0);
	CV_Assert(!image.empty());

	Point2f pt1[3] = { Point2f(10, 200), Point2f(200, 150), Point2f(280, 280) };
	Point2f pt2[3] = { Point2f(10, 10) , Point2f(250, 10) , Point2f(280, 280) };

	Point center(200, 100);
	double  angle = 30.0;
	double  scale = 1;

	Mat aff_map = getAffineTransform(pt1, pt2); // 어파인 변환 행렬 반환
	Mat rot_map = getRotationMatrix2D(center, angle, scale); // 회전 변환과 크기 변경을 수행할 수 있는 어파인 행렬 반환

	Mat dst1, dst2, dst3, dst4;
    // 사용자 정의 함수
	affine_transform(image, dst1, aff_map, image.size());
	affine_transform(image, dst2, rot_map, image.size());
    
    // opencv 제공함수
	warpAffine(image, dst3, aff_map, image.size(), INTER_LINEAR);
	warpAffine(image, dst4, rot_map, image.size(), INTER_LINEAR);

	cvtColor(image, image, CV_GRAY2BGR);
	cvtColor(dst1, dst1, CV_GRAY2BGR);

	for (int i = 0; i < 3; i++) {
		circle(image, pt1[i], 3, Scalar(0, 0, 255), 2);
		circle(dst1, pt2[i], 3, Scalar(0, 0, 255), 2);
	}

	imshow("image", image);
	imshow("dst1-어파인", dst1), imshow("dst2-어파인 회전", dst2);
	imshow("dst3-어파인_OpenCV", dst3), imshow("dst4-어파인 -회전_OpenCV", dst4);

	waitKey();
	return 0;
}
```
<center><img src="/assets/images/opencv8/21.PNG" width="100%"></center><br>

### 9.2.9. 행렬 연산을 통한 기하학 변환 - 어파인 변환 2
```cython
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;

uchar bilinear_value(Mat img, double x, double y)
{
	if (x >= img.cols - 1)  x--;
	if (y >= img.rows - 1)  y--;

	// 4개 화소 가져옴
	Point pt((int)x, (int)y);
	int A = img.at<uchar>(pt);
	int B = img.at<uchar>(pt + Point(0, 1));
	int C = img.at<uchar>(pt + Point(1, 0));
	int D = img.at<uchar>(pt + Point(1, 1));

	//1차 보간
	double alpha = y - pt.y;
	double beta = x - pt.x;
	int M1 = A + (int)cvRound(alpha * (B - A));
	int M2 = C + (int)cvRound(alpha * (D - C));

	//2차 보간
	int P = M1 + (int)cvRound(beta * (M2 - M1));
	return saturate_cast<uchar>(P);
}


void affine_transform(Mat img, Mat& dst, Mat map, Size size)
{
	dst = Mat(img.size(), img.type(), Scalar(0));
	Rect rect(Point(0, 0), img.size());

	Mat  inv_map;
	invertAffineTransform(map, inv_map);

	for (int i = 0; i < size.height; i++) {
		for (int j = 0; j < size.width; j++)
		{
			Point3d ji(j, i, 1);
			Mat xy = inv_map * (Mat)ji;
			Point2d pt = (Point2d)xy;

			if (rect.contains(pt))
				dst.at<uchar>(i, j) = bilinear_value(img, pt.x, pt.y);
		}
	}
}

// 복합 변환(회전, 평행이동, 크기변경)
Mat getAffineMap(Point2d center, double dgree, double fx = 1, double fy = 1, Point2d translate = Point(0, 0))
{
	Mat rot_map = Mat::eye(3, 3, CV_64F);
	Mat center_trans = Mat::eye(3, 3, CV_64F);
	Mat org_trans = Mat::eye(3, 3, CV_64F);
	Mat scale_map = Mat::eye(3, 3, CV_64F); // 크기 변경 행렬
	Mat trans_map = Mat::eye(3, 3, CV_64F); // 평행이동 행렬

	double radian = dgree / 180 * CV_PI;
	rot_map.at<double>(0, 0) = cos(radian);
	rot_map.at<double>(0, 1) = sin(radian);
	rot_map.at<double>(1, 0) = -sin(radian);
	rot_map.at<double>(1, 1) = cos(radian);

	center_trans.at<double>(0, 2) = center.x; // 회전 중심으로 이동
	center_trans.at<double>(1, 2) = center.y;
	org_trans.at<double>(0, 2) = -center.x; // 원점으로 이동
	org_trans.at<double>(1, 2) = -center.y;

	scale_map.at<double>(0, 0) = fx;
	scale_map.at<double>(1, 1) = fy;
	trans_map.at<double>(0, 2) = translate.x;
	trans_map.at<double>(1, 2) = translate.y;

	Mat ret_map = center_trans * rot_map * trans_map * scale_map * org_trans;
//	Mat ret_map = center_trans * rot_map * scale_map * trans_map * org_trans ;

	ret_map.resize(2);
	return ret_map;
}

int main()
{
	Mat image = imread("../image/affine_test5.jpg", 0);
	CV_Assert(!image.empty());

	Point center = image.size() / 2, tr(100, 0);
	double  angle = 30.0;
	Mat dst1, dst2, dst3, dst4;

	Mat aff_map1 = getAffineMap(center, angle);
	Mat aff_map2 = getAffineMap(Point2d(0, 0), 0, 2.0, 1.5);
	Mat aff_map3 = getAffineMap(center, angle, 0.7, 0.7);
	Mat aff_map4 = getAffineMap(center, angle, 0.7, 0.7, tr);

	warpAffine(image, dst1, aff_map1, image.size());
	warpAffine(image, dst2, aff_map2, image.size());
	affine_transform(image, dst3, aff_map3, image.size());
	affine_transform(image, dst4, aff_map4, image.size());

	imshow("image", image);
	imshow("dst1-회전만", dst1);
	imshow("dst2-크기변경만", dst2);
	imshow("dst3-회전+크기변경", dst3);
	imshow("dst4-회전+크기변경+평행이동", dst4);

	waitKey();
	return 0;
}
```
<center><img src="/assets/images/opencv8/22.PNG" width="100%"></center><br>

### 9.2.10. 실습
OpenCV에서 제공하는 함수 중에 영상을 좌우 혹은 상하로 뒤집는 cv::flip() 함수가 있다. cv::warpAffine() 함수를 이용하여 동일하게 뒤집기를 수행하도록 하는 어파인 변환 행렬을 구성하여 전체 프로그램을 완성하시오.
```cython
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;

int main()
{
	Mat image = imread("../image/filp_test.jpg", 0);
	CV_Assert(image.data);

	// 상하 뒤집기
	Matx23d flip_map1;
	flip_map1 << 1, 0, 0,
		  		 0, -1, image.rows;

	// 좌우 뒤집기
	Matx23d flip_map2;
	flip_map2 << -1, 0, image.cols,
				 0, 1, 0;

	// 상하좌우 뒤집기
	Matx23d flip_map3;
	flip_map3 << -1, 0, image.cols,
				 0, -1, image.rows;

	Mat dst1, dst2, dst3;
	warpAffine(image, dst1, flip_map1, image.size());
	warpAffine(image, dst2, flip_map2, image.size());
	warpAffine(image, dst3, flip_map3, image.size());

	imshow("실습", image), imshow("dst1 - 상하뒤집기", dst1);
	imshow("dst2 - 좌우뒤집기", dst2), imshow("dst3 - 상하좌우뒤집기", dst3);
	waitKey();
	return 0;
}
```
<center><img src="/assets/images/opencv8/23.PNG" width="100%"></center><br>

### 9.2.11. 원근 투시(투영) 변환
- 원근 투시 변환(perspective projection transformation): 원근법을 영상 좌표계에서 표현하는 것
- 3차원 실세계 좌표를 투영 스크린상의 2차원 좌표로 표현할 수 있도록 변환해주는 것
- 동차 좌표계(homogeneous coordinates)
    + 모든 항의 차수가 동일하기 때문에 붙여진 이름으로서 n차원의 투영공간을 n+1개의 좌표로 나타내는 좌표계이다.
    + 직교 좌표인 (x, y)를 (x, y, 1)로 표현한다. 일반화하면 0이 아닌 상수 w에 대해 (x, y)를 (wx, wy, w)로 표현한다.
    + 상수 w가 무한히 많기 때문에 (x, y)에 대한 동차 좌표 표현은 무한히 많이 존재한다.
    + 동차 좌표계에서 한 점(wx, wy, w)을 직교 좌표로 나타내면, 각 원소를 w로 나누어서 (x/w, y/w)가 된다.

> cv::getPerspectiveTransform() -> 입력영상에서 4개의 좌표와 이 점들이 이동한 결과영상의 좌표 네 개를 입력으로 받아 3x3 투시 변환 행렬을 반환
> cv::warpPerspactive() -> 3x3 투시 변환 행렬을 가지고 있을 때, 영상을 투시 변환한 결과영상을 생성
> cv::transform() -> 입력영상의 4개 좌표와 원근 행렬을 인수로 입력하면 원근 변환된 좌표를 반환

```cython
// 트럼프 카드 영상에서 사용자가 마우스로 카드 모서리 좌표를 선택하면(시계 방향) 해당 카드를 반듯한 직사각형 형태로 투시 변환하여 영상 출력하기
#include <opencv2/opencv.hpp>
using namespace std;
using namespace cv;

Mat src;
Point2f srcQuad[4], dstQuad[4];

void on_mouse(int event, int x, int y, int flags, void* userdata) {
	static int cnt = 0;

	if (event == EVENT_LBUTTONDOWN) {
		if (cnt < 4) {
			srcQuad[cnt++] = Point2f(x, y);
			circle(src, Point(x, y), 5, Scalar(0, 0, 255), -1);
			imshow("src", src);

			if (cnt == 4) {
				int w = 200, h = 300;

				dstQuad[0] = Point2f(0, 0);
				dstQuad[1] = Point2f(w - 1, 0);
				dstQuad[2] = Point2f(w - 1, h - 1);
				dstQuad[3] = Point2f(0, h - 1);

				Mat pers = getPerspectiveTransform(srcQuad, dstQuad);

				Mat dst;
				warpPerspective(src, dst, pers, Size(w, h));

				imshow("dst", dst);
			}
		}
	}
}

int main(void) {
	src = imread("../image/card.bmp");
	CV_Assert(src.data);

	namedWindow("src");
	setMouseCallback("src", on_mouse);

	imshow("src", src);
	waitKey();
	return 0;
}
```
<center><img src="/assets/images/opencv8/24.PNG" width="100%"></center><br>

```cython
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;
int main()
{
	Mat image = imread("../image/perspective_test.jpg", IMREAD_COLOR);
	CV_Assert(image.data);

	// 원본 영상 좌표 4개
	Point2f pts1[4] = {
		Point2f(90, 170) , Point2f(300, 120),
		Point2f(90, 285), Point2f(300, 320)
	};
	// 목적 영상 좌표 4개
	Point2f pts2[4] = {
		Point2f(60, 120), Point2f(340, 110),
		Point2f(60, 280), Point2f(340, 280)
	};

	Mat dst(image.size(), CV_8UC1);
	// 4개 좌표쌍을 이용해 원근 변환 행렬 계산
	Mat perspect_map = getPerspectiveTransform(pts1, pts2);
	// 원근 변환 행렬로 원근 변환 수행
	warpPerspective(image, dst, perspect_map, image.size(), INTER_CUBIC);

	// 3차원 좌표로 동차좌표 표현
	vector<Point3f> pts3, pts4;
	for (int i = 0; i < 4; i++) {
		pts3.push_back(Point3f(pts1[i].x, pts1[i].y, 1)); // 원본좌표 -> 동차좌표 저장
	}

	transform(pts3, pts4, perspect_map);
	cout << "[perspect_map] = " << endl << perspect_map << endl << endl;

	imshow("image ", image);
	imshow("dst - 왜곡 보정", dst);
	waitKey();
	return 0;
}
```
<center><img src="/assets/images/opencv8/25.PNG" width="100%"></center><br>

### 9.2.12. 원근 투시(투영) 변환 심화 예제
마우스 드래그로 선택된 영역에 원근 변환 제거하기
```cython
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;
// 전역 변수 설정
Point2f  pts[4], small(10, 10);							// 4개 좌표 및 좌표 사각형 크기
Mat image;												// 입력 영상 

void draw_rect(Mat image)								// 4개 좌표 잇는 사각형 그리기
{
	Rect img_rect(Point(0, 0), image.size());			// 입력영상 크기 사각형
	for (int i = 0; i < 4; i++)
	{
		Rect rect(pts[i] - small, pts[i] + small);		// 좌표 사각형
		rect &= img_rect;								// 교차 영역 계산
		image(rect) += Scalar(70, 70, 70);				// 사각형 영역 밝게 하기
		line(image, pts[i], pts[(i + 1) % 4], Scalar(255, 0, 255), 1);
		rectangle(image, rect, Scalar(255, 255, 0), 1);	// 좌표 사각형 그리기
	}
	imshow("select rect", image);
}

void warp(Mat image)									// 원근 변환 수행 함수
{
	Point2f dst_pts[4] = {								// 목적 영상 4개 좌표
		Point2f(0, 0), Point2f(350, 0),
		Point2f(350, 350), Point2f(0, 350)
	};
	Mat dst;
	Mat perspect_mat = getPerspectiveTransform(pts, dst_pts);		// 원근변환 행렬 계산
	warpPerspective(image, dst, perspect_mat, Size(350, 350), INTER_CUBIC);
	imshow("왜곡보정", dst);
}

void  onMouse(int event, int x, int y, int flags, void*)	// 마우스 이벤트 제어
{
	Point curr_pt(x, y);									// 현재 클릭 좌표
	static int check = -1;								// 마우스 선택 좌표번호

	if (event == EVENT_LBUTTONDOWN) {		// 마우스 좌 버튼 
		for (int i = 0; i < 4; i++)
		{
			// 위에 있는 사각형은 선까지 연결해서 있는 사각형, 이건 따로 선언
			Rect rect(pts[i] - small, pts[i] + small);
			if (rect.contains(curr_pt)) check = i;
		}
	}
	if (event == EVENT_LBUTTONUP)
		check = -1;									// 선택 좌표번호 초기화

	if (check >= 0) {									// 좌표 사각형 선택시	
		pts[check] = curr_pt;							// 클릭 좌표를 선택 좌표에 저장
		draw_rect(image.clone());						// 4개 좌표 연결 사각형 그리기
		warp(image.clone());							// 원근 변환 수행
	}
}

int main()
{
	image = imread("../image/perspective_test.jpg", 1);
	CV_Assert(image.data);

	pts[0] = Point2f(100, 100), pts[1] = Point2f(300, 100);	// 4개 좌표 초기화
	pts[2] = Point2f(300, 300), pts[3] = Point2f(100, 300);
	draw_rect(image.clone());									// 좌표 사각형 그리기
	setMouseCallback("select rect", onMouse, 0);				// 콜백 함수 등록
	waitKey(0);
	return 0;
}
```
<center><img src="/assets/images/opencv8/26.PNG" width="100%"></center><br>
