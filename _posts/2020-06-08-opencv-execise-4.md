---
layout: post
title: OpenCV Exercise 4
date: 2020-06-08 15:00:00-19:00:00
categories: OpenCV
tag: OpenCV
---

## 9.1. 기본 배열 처리 함수

    컬러 영상
    - 파란색(B), 녹색(G), 빨간색(R)의 각기 독립적인 2차원 정보
    - 2차원 정보 3개를 갖는 컬러 영상을 표현 -> 3차원 배열 사용

```angular2
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;
int main()
{
	Mat image = imread("../image/flip_test.jpg", IMREAD_COLOR); // 컬러 읽기 모드
	CV_Assert(image.data);

	Mat x_axis, y_axis, xy_axis, rep_img, trans_img;
	flip(image, x_axis, 0);		// 상하 반전
	flip(image, y_axis, 1);		// 좌우 반전
	flip(image, xy_axis, -1);	// 상하좌우 반전

	// Mat repeat(InputArray src, int ny, int nx);
	// src: 반복할 행렬
	// ny: y방향 반복 횟수
	// nx: x방향 반복 횟수
	// return: 반복 복사된 행렬
	repeat(image, 1, 2, rep_img); // 가로 방향 2번 반복 복사
	transpose(image, trans_img); // 행렬 전치

	imshow("image", image);
	imshow("x_axis", x_axis);
	imshow("y_axis", y_axis);
	imshow("xy_axis", xy_axis);
	imshow("rep_img", rep_img);
	imshow("trans_img", trans_img);

	// 응용 실습
	repeat(image, 2, 2, rep_img);
	int i = 0, j = 0;
	// 하나의 행에 대해
	for (j = 0; j < image.rows; j++)
		// 컬러는 열로만 이루어짐
		// 컬러는 3개의 채널(B, G, R)이 있기 때문에 3칸씩 움직임
		for (i = 0; i < image.cols * 3; i+=3) {
		    // image.cols * 3: 원래 영상
			// 우측 상단은 좌측 기준부터 image.cols * 3만큼 떨어짐
			rep_img.at<uchar>(j, i + image.cols * 3) = x_axis.at<uchar>(j, i);
			rep_img.at<uchar>(j, i + image.cols * 3 + 1) = x_axis.at<uchar>(j, i + 1);
			rep_img.at<uchar>(j, i + image.cols * 3 + 2) = x_axis.at<uchar>(j, i + 2);
		}
	
	for (j = 0; j < image.rows; j++)
		for (i = 0; i < image.cols * 3; i += 3) {
			// 좌측 하단은 좌측 기준부터 image.rows만큼 떨어짐
			rep_img.at<uchar>(j + image.rows, i) = y_axis.at<uchar>(j, i);
			rep_img.at<uchar>(j + image.rows, i + 1) = y_axis.at<uchar>(j, i + 1);
			rep_img.at<uchar>(j + image.rows, i + 2) = y_axis.at<uchar>(j, i + 2);
		}

	for (j = 0; j < image.rows; j++)
		for (i = 0; i < image.cols * 3; i += 3) {
			// 우측 하단은 좌측 기준부터 image.rows, image.cols * 3만큼 떨어짐
			rep_img.at<uchar>(j + image.rows, i + image.cols * 3) = xy_axis.at<uchar>(j, i);
			rep_img.at<uchar>(j + image.rows, i + image.cols * 3 + 1) = xy_axis.at<uchar>(j, i + 1);
			rep_img.at<uchar>(j + image.rows, i + image.cols * 3 + 2) = xy_axis.at<uchar>(j, i + 2);
		}
		

//	imshow("image", image);
	imshow("rep_img", rep_img);
	
	waitKey();
	return 0;
}
```
<img src="/assets/images/opencv4/1.PNG" width="50%"><br>

## 9.2. 채널 처리 함수

    void merge(): 여러 개의 단일채널 배열로 다중 채널의 배열을 합성한다.
        - Mat* mv: 합성될 입력 배열 혹은 벡터, 합성될 단일채널 배열들의 크기와 깊이가 동일해야 함
        - size_t count: 합성될 배열의 개수, 0보다 커야 함
        - OutputArray dst: 입력 배열과 같은 크기와 같은 깊이의 출력 배열

    void split(): 다중 채널 배열을 여러 개의 단일채널 배열로 분리한다.
        - Mat& src: 입력되는 다중 채널 행렬
        - Mat* mvbegin: 분리되어 반환되는 단일채널 행렬을 원소로 하는 배열
        - InputArray m: 입력되는 다중 채널 배열
        - InputArrayOfArrays mv: 분리되어 반환되는 단일채널 배열들의 벡터 혹은 베열
    
    void mixChannels(): 명시된 채널의 순서쌍에 의해 입력 배열들(src)로부터 출력 배열들(dst)의 복사한다.
        - Mat* src: 입력 배열 혹은 행렬 벡터
        - size_t nsrcs: 입력 배열(src)의 행렬 개수
        - Mat* dst: 입력 배열 혹은 행렬 벡터
        - size_t ndsts: 출력 배열(dst)의 행렬 개수
        - int* fromTo: 입력과 출력의 순서쌍 배열
        - size_t npairs: 순서쌍의 개수

```angular2
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;
int main()
{
	Mat ch0(3, 4, CV_8U, Scalar(10));
	Mat ch1(3, 4, CV_8U, Scalar(20));
	Mat ch2(3, 4, CV_8U, Scalar(30));

	Mat bgr_arr[] = { ch0, ch1, ch2 };
	Mat bgr;
	merge(bgr_arr, 3, bgr);
	vector<Mat> bgr_vec;
	split(bgr, bgr_vec);

	cout << "[ch0] = " << endl << ch0 << endl;
	cout << "[ch1] = " << endl << ch1 << endl;
	cout << "[ch2] = " << endl << ch2 << endl << endl;

	cout << "[bgr] = " << endl << bgr << endl << endl;
	cout << "[bgr_vec[0]] = " << endl << bgr_vec[0] << endl;
	cout << "[bgr_vec[1]] = " << endl << bgr_vec[1] << endl;
	cout << "[bgr_vec[2]] = " << endl << bgr_vec[2] << endl;

	return 0;
}
```

## 실습
컬러 영상 파일에서 채널 분리하기

```angular2
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;
int main()
{
	Mat image = imread("../image/color.jpg", IMREAD_COLOR);
	CV_Assert(image.data);

	Mat bgr[3];
	split(image, bgr);

	imshow("image", image);
	imshow("blue 채널", bgr[0]);
	imshow("green 채널", bgr[1]);
	imshow("red 채널", bgr[2]);

	waitKey(0);
	return 0;
}
```
<img src="/assets/images/opencv4/2.PNG" width="50%"><br>

```angular2
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;
int main()
{
	Mat ch0(3, 4, CV_8U, Scalar(10));
	Mat ch1(3, 4, CV_8U, Scalar(20));
	Mat ch2(3, 4, CV_8U, Scalar(30));
	Mat ch_012;

	vector<Mat> vec_012;
	vec_012.push_back(ch0);
	vec_012.push_back(ch1);
	vec_012.push_back(ch2);
	merge(vec_012, ch_012); // ch_012: CV_8UC3
	
	Mat ch_13(ch_012.size(), CV_8UC2);
	Mat  ch_2(ch_012.size(), CV_8UC1);

	Mat out[] = { ch_13, ch_2 };
	int from_to[] = { 0,0,  2,1,  1,2 };
	

	Mat ch_13(ch_012.size(), CV_8UC3);
	Mat  ch_2(ch_012.size(), CV_8UC1);
	Mat out[] = { ch_13, ch_2 };
	int from_to[] = { 0,0,  2,1,  1,2,  0,1 };

	// ch_012를 ch_13과 ch_2로 나누기
	mixChannels(&ch_012, 1, out, 2, from_to, 3); // ch_012 행렬은 1개, out의 행렬의 개수는 2개, 0번->0번/2번->1번/1번->2번의 3가지
	
	cout << "[ch_123] = " << endl << ch_012 << endl << endl;
	cout << "[ch_13] = " << endl << ch_13 << endl;
	cout << "[ch_2] = " << endl << ch_2 << endl;
	return 0;
}
```

## 10.1. 사칙연산

    void add(): 두 개의 배열이나 배열과 스칼라의 각 원소 간 합을 계산한다.
                입력 인수 src1, src2 중 하나는 스칼라값일 수 있다.

        - InputArray src1: 첫 번째 입력 배열 혹은 스칼라
        - InputArray src2: 두 번째 입력 배열 혹은 스칼라
        - OutputArray dst: 계산된 결과의 출력 배열
        - InputArray mask: 연산 마스크 - 마스크가 0이 아닌 좌표만 연산 수행(8비트 단일채널)
        - int dtype: 출력 배열의 깊이
        
    void subtract(): 두 개의 배열이나 배열과 스칼라의 각 원소 간 차분을 계산한다.
                     add() 함수와 인수 동일
    
    void multiply(): 두 배열의 각 원소 간 곱을 계산한다.
        - double scale: 원소 간의 곱할 때 추가로 곱해지는 배율
        
    void divide(): 두 배열의 각 원소 간 나눗셈을 수행한다.

    void scaleAdd(): 스케일된 배열과 다른 배열의 합을 계산한다.
        - double alpha: 첫 번째 배열의 모든 원소들에 대한 배율
        
    void addweighted(): 두 배열의 가중된 합을 계산한다.
        - double alpha: 첫 번째 배열의 원소들에 대한 가중치
        - double beta: 두 번째 배열의 원소들에 대한 가중치
        - double gamma: 두 배열의 합에 추가로 더해지는 스칼라
        
```angular2
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;
int main()
{
	Mat m1(3, 6, CV_8UC1, Scalar(10));
	Mat m2(3, 6, CV_8UC1, Scalar(50));
	Mat m_add1, m_add2, m_sub, m_div1, m_div2;
	Mat mask(m1.size(), CV_8UC1);			// 마스크 행렬 - 8비트 단일채널
	mask.setTo(0);
	// Point(3, 0)은 보통 행렬과 다르게
	// (3, 0) 값은 좌측 기준에서 오른쪽으로 3만큼 떨어짐
	// Size(3, 3)은 3 x 3만큼
	Rect rect(Point(3, 0), Size(3, 3));			// 관심영역 지정
	// rect 사각형 영역만큼 mask 행렬에서 참조하여 원소값을 1로 지정
	mask(rect).setTo(1);
	
	add(m1, m2, m_add1);
	// mask 1인 영역만 덧셈 수행
	add(m1, m2, m_add2, mask);

	divide(m1, m2, m_div1);
	// 형변환 - 소수부분 보존
	m1.convertTo(m1, CV_32F);
	m2.convertTo(m2, CV_32F);
	divide(m1, m2, m_div2);

	cout << "[m1] = " << endl << m1 << endl;
	cout << "[m2] = " << endl << m2 << endl;
	cout << "[mask] = " << endl << mask << endl << endl;

	cout << "[m_add1] = " << endl << m_add1 << endl;
	cout << "[m_add2] = " << endl << m_add2 << endl;
	cout << "[m_div1] = " << endl << m_div1 << endl;
	cout << "[m_div2] = " << endl << m_div2 << endl;
	return 0;
}
```

## 10.2. 지수 로그 루트 관련 함수

    void exp(): 모든 배열 원소의 지수를 계산한다.
        - InputArray src: 입력 배열
        - OutputArray dst: 입력 배열과 같은 크기의 타입의 출력 배열
        
    void log(): 모든 배열 원소의 절댓값에 대한 자연 로그를 계산한다.
    
    void sqrt(): 모든 배열 원소에 대해 제곱근을 계산한다.
    
    void pow(): 모든 배열 원소에 대해서 power 승수를 계산한다.
        - double power: 제곱 승수
        
    void magnitude(): 2차원 벡터들의 크기를 계산한다.
        - InputArray x: 벡터의 x좌표들의 배열
        - InputArray y: 벡터의 y좌표들의 배열
        - OutputArray magnitude: 입력 배열과 같은 크기의 출력 배열
        
    void phase(): 2차원 벡터의 회전 각도를 계산한다.
        - InputArray x: 벡터의 x좌표들의 배열
        - InputArray y: 벡터의 y좌표들의 배열
        - OutputArray angle: 벡터 각도들의 출력 배열
        - bool angleInDegrees: true - 각을 도(degree)로 측정, false - 각을 라디안(radian)으로 측정

    void cartToPolar(): 2차원 벡터들의 크기와 각도를 계산한다.
    
    void polarToCart(): 각도와 크기로부터 2차원 벡터들의 좌표를 계산한다.

```angular2
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;
int main()
{
	vector<float> v1, v_exp, v_log;
	Matx <float, 1, 5> m1(1, 2, 3, 5, 10);
	Mat m_exp, m_sqrt, m_pow;
	v1.push_back(1);
	v1.push_back(2);
	v1.push_back(3);

	exp(v1, v_exp);			// 벡터에 대한 지수 계산
	exp(m1, m_exp);			// 행렬에 대한 지수 계산
	log(m1, v_log);			// 입력은 행렬, 출력은 벡터
	sqrt(m1, m_sqrt);
	pow(m1, 3, m_pow);

	cout << "[m1] = " << m1 << endl << endl;
	cout << "[v_exp] = " << ((Mat)v_exp).reshape(1, 1) << endl << endl; // 벡터를 행렬로 변환 후, 1채널 1행으로 변환
	cout << "[m_exp] = " << m_exp << endl << endl;
	cout << "[v_log] = " << ((Mat)v_log).reshape(1, 1) << endl << endl;

	cout << "[m_sqrt] = " << m_sqrt << endl << endl;
	cout << "[m_pow] = " << m_pow << endl << endl;
	return 0;
}
```

## 10.3. 논리(비트) 연산
	
	NOT: 각 자릿수의 값을 반대로 바꾸는 연산
	OR: 두 값의 각 자릿수를 비교해, 둘 중 하나라도 1이 있다면 1, 아니면 0
	XOR: 두 값의 각 자릿수를 비교해, 값이 0으로 같으면 0, 값이 1로 같으면 0, 다르면 1
	AND: 두 값의 각 자릿수를 비교해, 두 값 모두에 1이 있을 때만 1, 나머지는 0

    void bitwise_and(): 두 배열의 원소 간 혹은 배열 원소와 스칼라 간에 비트 간 논리곱(AND) 연산을 수행한다.
        - InputArray src1: 첫 번째 입력 배열 혹은 스칼라값
        - InputArray src2: 두 번째 입력 배열 혹은 스칼라값
        - OutputArray dst: 입력 배열과 같은 크기의 출력 배열
        - InputArray mask: 마스크 연산 - 마스크의 원소가 0이 아닌 좌표만 계산 수행

    void bitwise_or(): 두 배열의 원소 간 혹은 배열 원소와 스칼라 간에 비트 간 논리합(OR) 연산을 수행한다.
    
    void bitwise_xor(): 두 개의 배열 원소 간 혹은 배열 원소와 스칼라 간에 비트 간 배타적 논리합(OR) 연산을 수행한다.
    
    void bitwise_not(): 입력 배열의 모든 원소에 대해서 각 비트의 역을 계산한다.

```angular2
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;
int main()
{
	Mat image1(250, 250, CV_8U, Scalar(0));
	Mat image2(250, 250, CV_8U, Scalar(0));
	Mat image3, image4, image5, image6;

	Point center = image1.size() / 2;
	circle(image1, center, 80, Scalar(255), -1); // 중심에 원 그리기(마지막 인자를 -1로 주면, 내부가 채워짐)
	rectangle(image2, Point(0, 0), Point(125, 250), Scalar(255), -1);

	// 비트 연산
	bitwise_or(image1, image2, image3);
	bitwise_xor(image1, image2, image4);
	bitwise_and(image1, image2, image5);
	bitwise_not(image1, image6);

	imshow("image1", image1);
	imshow("image2", image2);
	imshow("bitwise_or", image3);
	imshow("bitwise_xor", image4);
	imshow("bitwise_and", image5);
	imshow("bitwise_not", image6);
	waitKey();
	return 0;
}
```
<img src="/assets/images/opencv4/3.PNG" width="50%"><br>
