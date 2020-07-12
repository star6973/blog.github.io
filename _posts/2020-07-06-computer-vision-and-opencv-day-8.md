---
layout: post
title: Computer Vision and OpenCV [Day 8]
date: 2020-07-06 10:00:00-19:00:00
categories: OpenCV
tag: OpenCV
use_math: true
---

# 9. 2D/3D 기하학 처리 이론 및 실습
## 9.1. 2D/3D 기하학 처리 이론
### 9.1.1. 기하학적 처리란?
- 확대
- 역방향 사상과 양선형 보간법
- 보간법
- 축소
- 회전
- 대칭



/*

roi pooling - overlap 되게 쪼개기(몇 칸씩 잡을건지-window size, 몇 칸씩 뛸건지-stride)

classification -> invariance(물체의 위치 반영 x)
segmentation -> equivariance(물체의 위치 반영 o)

roi pool은 그냥 4개의 원색
roi align은 공간을 나눠서 가져온


[이론]
1. 확대 -> 할수록 화질 저하(초점 낮아짐)(계단현상)
-> 해결: 출력 영상 크기를 정하고, 거꾸로 어디서부터 오는지를 역방향으로 처리 -> 확대되었다고 가정하고, 거꾸로 원본에서 참조해서 사상(역방향 사상)
=> 보간법

2. 축소 -> 축소 배율만큼 건너뛰기 -> 할수록 얇은 경계가 소실 가능(세부 항목 상실) -> 해결: 서브 샘플링 전에 blurring -> 공간주파수(단위 공간당 변화율)가 높을수록 샘플링 주파수도 높아야 소실이 되지 않음. 즉, blurring을 하게 되면 공간주파수가 낮기 때문에 서브샘플링을 해도 덜 소실됨.
-> 해결: 평균값으로 대치

3. 회전 -> 전방향 사상을 이용한 회전을 사용하면 -> 출력영상에서 픽셀값을 할당받지 못한 빈 곳이 발생  -> 해결: 역방향 사상 -> cos, sin, -sin, cos -> 해결: 영상의 중심점을 기준으로 한 회전, 그래도 좀 잘리긴함 => 모든 점들을 중심으로 보내고, 다시 원상복구
고려사항3(출력영상 크기를 미리 잡아야함)


*/

## 9.2. 2D/3D 기하학 처리 실습
### 9.2.1. 사상

### 9.2.2. 크기 변경(확대/축소)
```angular2
// 순방향 사상
/*

	확대는 300*400 - 240*300의 차이만큼 누락됨


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

### 9.2.3. 최근접 이웃 보간법
```angular2

	목적영상을 만드는 과정에서 홀이 되어 할당받지 못하는 화소들의 값을 찾을 때,
	목적영상의 화소에 가장 가깝게 이웃한 입력영상의 화소값을 가져오는 방법

	최근방 이웃 보간법Nearest neighbor interpolation은 가장 가까운 위치에 있는 픽셀의 값을 참조하는 방법이다. 
	예를 들어 역방향 매핑 시 원본 영상의 참조 좌표가 (50.2, 32.8)로 계산되었다면, 
	이 위치에서 가장 가까운 정수 좌표인 (50, 33)의 픽셀 값을 그대로 사용하는 방식이다. 
	최근방 이웃 보간법의 장점은 구현하기가 쉽고 동작이 빠르다는 점이다. 
	그러나 이 방법을 이용하여 확대된 결과 영상은 픽셀 값이 급격하게 변화하는 것 같은 계단 현상(또는 블록 현상)이 나타날 수 있다는 단점이 있다.

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

### 9.2.5. 평행이동

```angular2
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;

void translation(Mat img, Mat& dst, Point pt)				// 평행이동 함수
{
	Rect rect(Point(0, 0), img.size());						// 입력영상 범위 사각형
	dst = Mat(img.size(), img.type(), Scalar(0));

	// 역방향 사상

		x = x' - dx
		y = y' - dy


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
	return  saturate_cast<uchar>(P);
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
