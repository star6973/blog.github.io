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





























/*
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;
int main()
{
	Mat image = imread("../image/bit_test.jpg", IMREAD_COLOR);
	Mat logo = imread("../image/logo.jpg", IMREAD_COLOR);
	CV_Assert(image.data && logo.data);			// 예외처리
	Mat logo_th, masks[5], background, foreground, dst;		// 결과 행렬
	
	threshold(logo, logo_th, 70, 255, THRESH_BINARY); // 로고 영상 이진화
	split(logo_th, masks);						// 로고영상 채널 분리
	
//	imshow("image", image);
	// 로고를 3개의 컬러로 분리
//	imshow("masks[0]", masks[0]); // blue
//	imshow("masks[1]", masks[1]); // green
//	imshow("masks[2]", masks[2]); // red
//	imshow("logo", logo);
	
	// 흰색 글자는 RGB가 모두 가지고 있기 때문에 어느 영역에서든 포함
	bitwise_or(masks[0], masks[1], masks[3]);	// 전경통과 마스크(blue 컬러와 green 컬러의 합)
//	imshow("masks[3] - 1", masks[3]);
	
	bitwise_or(masks[2], masks[3], masks[3]);   // 마스크(blue 컬러와 green 컬러의 합에서 다시 한 번 더 red 컬러의 합)
	imshow("masks[3] - 2", masks[3]);

	bitwise_not(masks[3], masks[4]);			// 배경통과 마스크(모든 컬러의 반전)
	imshow("masks[4]", masks[4]);
	
	Point center1 = image.size() / 2;			// 영상 중심좌표
	Point center2 = logo.size() / 2;;			// 로고 중심좌표
	Point start = center1 - center2;
	
	Rect roi(start.x, start.y, logo.cols, logo.rows);
//	Rect roi(start, logo.size());

	//비트곱과 마스킹을 이용한 관심 영역의 복사
	bitwise_and(logo, logo, foreground, masks[3]); // 컬러가 True인 경우만 출력
	bitwise_and(image(roi), image(roi), background, masks[4]);

	imshow("foreground", foreground);
	imshow("background", background);

	// 로고 전경과 원본 배경의 합성
	bitwise_or(foreground, background, dst);
	imshow("dst", dst);

	// 합성 영상을 image 관심영역에 복사
	add(background, foreground, dst);
	dst.copyTo(image(roi));
	imshow("image", image);

	waitKey();
	return 0;
}
*/

/*

	심화 실습
	컬러 영상 파일을 입력 받아서 RGB의 3개 채널을 분리하고, 각 채널을 컬러영상을 윈도우에 표기
	
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;
int main()
{
	Mat image = imread("../image/logo.jpg", 1);
	// zero는 아무것도 없는 빈 채널
	Mat bgr[3], blue_img, red_img, green_img, zero(image.size(), CV_8U, Scalar(0));
	split(image, bgr);

	imshow("bgr[0]", bgr[0]);
	imshow("bgr[1]", bgr[1]);
	imshow("bgr[2]", bgr[2]);

	Mat b_ch[] = { bgr[0], zero, zero }; // blue 컬러만 1, 나머지는 0
	Mat g_ch[] = { zero, bgr[1], zero }; // green 컬러만 1, 나머지는 0
	Mat r_ch[] = { zero, zero, bgr[2] }; // red 컬러만 1, 나머지는 0

	merge(b_ch, 3,blue_img);
	merge(g_ch, 3, green_img);
	merge(r_ch, 3, red_img);

	imshow("image", image), imshow("blue_img", blue_img);
	imshow("green_img", green_img), imshow("red_img", red_img);
	waitKey();
}
*/


/*
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;
int main()
{
	Mat image1 = imread("../image/abs_test1.jpg", 0);
	Mat image2 = imread("../image/abs_test2.jpg", 0);
	CV_Assert(image1.data && image2.data);

	Mat dif_img, abs_dif1, abs_dif2;

	image1.convertTo(image1, CV_16S);
	image2.convertTo(image2, CV_16S);
	subtract(image1, image2, dif_img); // 감산

	Rect roi(10, 10, 7, 3);
	cout << "[dif_img] = " << endl << dif_img(roi) << endl;

	abs_dif1 = abs(dif_img);

	image1.convertTo(image1, CV_8U);
	image2.convertTo(image2, CV_8U);
	dif_img.convertTo(dif_img, CV_8U);
	abs_dif1.convertTo(abs_dif1, CV_8U);

	absdiff(image1, image2, abs_dif2);

	cout << "[dif_img] = " << endl << dif_img(roi) << endl << endl;
	cout << "[abs_dif1] = " << endl << abs_dif1(roi) << endl;
	cout << "[abs_dif2] = " << endl << abs_dif2(roi) << endl;

	imshow("image1", image1);
	imshow("image2", image2);
	imshow("dif_img", dif_img);
	imshow("abs_dif1", abs_dif1);
	imshow("abs_dif2", abs_dif2);
	waitKey();
	return 0;

}
*/

/*
	
	9.2. 원소의 최소값과 최대값

	void min() / max(): 두 입력 행렬을 원소 간 비교하여 작은값을 출력 행렬로 반환한다.
		- InputArray src1, InputArray src2: 두 개의 입력 배열
		- OutputArray dst: 계산 결과 출력 배열(행렬 및 벡터)
		- Mat& dst: 계산 결과 출력 행렬

	MatExpr min() / max(): 행렬의 원소와 스칼라를 비교하여 작은값을 출력 행렬로 반환한다.
		- Mat& a: 첫 번째 행렬
		- double s: 스칼라값

	void manMaxIdx(): 전체 배열에서 최솟값과 최댓값인 원소의 위치와 그 값을 반환한다.
		- InputArray src: 단일채널 입력 배열
		- double* minVal, double* maxVal: 최솟값과 최대값 원소의 값 반환
		- int* minIdx, int* maxIdx: 최솟값과 최댓값 원소의 위치를 배열로 반환
		- InputArray mask: 연산 마스크

	void minMaxLoc(): 전체 배열에서 최댓값과 최솟값을 갖는 원소의 위치와 그 값을 반환한다. 위치를 Point형으로 반환한다.
		- Point* minLoc: 최솟값인 원소의 위치를 Point 객체로 반환
		- Point* maxLoc: 최댓값인 원소의 위치를 Point 객체로 반환

#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;
int main()
{
	uchar data[] = {
		10, 200, 5, 7, 9,
		15, 35, 60, 80, 170,
		100, 2, 55, 37, 70
	};
	Mat  m1(3, 5, CV_8U, data);
	Mat  m2(3, 5, CV_8U, Scalar(50));
	Mat  m_min, m_max;						// 최소값 행렬, 최대값 행렬
	double minVal, maxVal;
	int    minIdx[2] = {}, maxIdx[2] = {};	// 최소값 좌표, 최대값 좌표
	Point  minLoc, maxLoc;

	min(m1, 30, m_min);						// 두 행렬 원소간 최소값 저장
	max(m1, m2, m_max);						// 두 행렬 최대값 계산
	minMaxIdx(m1, &minVal, &maxVal, minIdx, maxIdx);
	minMaxLoc(m1, 0, 0, &minLoc, &maxLoc);

	cout << "[m1] = " << endl << m1 << endl << endl;
	cout << "[m_min] = " << endl << m_min << endl;
	cout << "[m_max] = " << endl << m_max << endl << endl;

	cout << "m1 행렬 원소 최소값 : " << minVal << endl;
	cout << "    최소값 좌표 : " << minIdx[1] << ", " << minIdx[0] << endl;

	cout << "m1 행렬 원소 최대값 : " << maxVal << endl;
	cout << "    최대값 좌표 : " << maxIdx[1] << ", " << maxIdx[0] << endl << endl;

	cout << "m1 행렬 최소값 좌표: " << minLoc << endl;
	cout << "m1 행렬 최대값 좌표 " << maxLoc << endl;
	return 0;
}
*/

/*
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;
int main()
{
	Mat image = imread("../image/minMax.jpg", IMREAD_GRAYSCALE);

	double minVal, maxVal;
	minMaxIdx(image, &minVal, &maxVal);

	double ratio = (maxVal - minVal) / 255.0;
	Mat dst = (image - minVal) / ratio; // 영상에서 숫자를 적용해줌으로써 밝기값이 변해짐

	// (x - min) * 255 / (max - min)
	// x = min이면, 0 x 255 = 0
	// x = max이면, 1 x 255 = 255로 늘어남

	cout << "최소값  = " << minVal << endl;
	cout << "최대값  = " << maxVal << endl;

	imshow("image", image);
	imshow("dst", dst);
	waitKey();
	return 0;
}
*/


/*
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;
int main()
{
	Mat image = imread("../image/sum_test.jpg", 1);
	CV_Assert(image.data);

	Mat  mask(image.size(), CV_8U, Scalar(0));
	mask(Rect(20, 40, 70, 70)).setTo(255);

	Scalar sum_value = sum(image); // BGR 채널별로 합
	Scalar mean_value1 = mean(image); // BGR 채널별로 평균
	Scalar mean_value2 = mean(image, mask);// 마스크 1 영역만 평균 계산

	cout << "[sum_value] = " << sum_value << endl;
	cout << "[mean_value1] = " << mean_value1 << endl;
	cout << "[mean_value2] = " << mean_value2 << endl << endl;

	Mat mean, stddev;
	meanStdDev(image, mean, stddev);
	cout << "[mean] = " << mean << endl;
	cout << "[stddev] = " << stddev << endl << endl;

	meanStdDev(image, mean, stddev, mask);
	cout << "[mean] = " << mean << endl;
	cout << "[stddev] = " << stddev << endl;

	imshow("image", image), imshow("mask", mask);
	waitKey();
	return 0;
}
*/

/*
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;

int main()
{
	Mat_<uchar> m1(3, 5);
	m1 << 21, 15, 10, 9, 14,
		6, 10, 15, 9, 7,
		7, 12, 8, 14, 1;

	Mat  m_sort1, m_sort2, m_sort3;

	cv::sort(m1, m_sort1, SORT_EVERY_ROW); // 행으로 오름차순
	cv::sort(m1, m_sort2, SORT_EVERY_ROW + SORT_DESCENDING); // 행으로 내림차순
	cv::sort(m1, m_sort3, SORT_EVERY_COLUMN); // 열로 오름차순

	cout << "[m1] = " << endl << m1 << endl << endl;
	cout << "[m_sort1] = " << endl << m_sort1 << endl << endl;
	cout << "[m_sort2] = " << endl << m_sort2 << endl << endl;
	cout << "[m_sort3] = " << endl << m_sort3 << endl;
	return 0;
}
*/

/*
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;

int main()
{
	Mat_<uchar> m1(3, 5);
	m1 << 21, 15, 10, 9, 14,
		6, 10, 15, 9, 7,
		7, 12, 8, 14, 1;

	Mat  m_sort_idx1, m_sort_idx2, m_sort_idx3;

	sortIdx(m1, m_sort_idx1, SORT_EVERY_ROW);
	sortIdx(m1, m_sort_idx2, SORT_EVERY_COLUMN);

	cout << "[m1] = " << endl << m1 << endl << endl;
	cout << "[m_sort_idx1] = " << endl << m_sort_idx1 << endl << endl;
	cout << "[m_sort_idx2] = " << endl << m_sort_idx2 << endl << endl;

	return 0;
}
*/

/*
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;
int main()
{
	Matx<ushort, 5, 4>  pts; // 1, 2열은 앞점, 3, 4열은 뒷점
	Mat_<int> sizes, sort_idx;
	vector<Rect> rects;
	randn(pts, Scalar(200), Scalar(100)); // 평균 200, 표준편차 100인 랜덤값

	cout << "----------------------------------------" << endl;
	cout << "      랜덤 사각형 정보 " << endl;
	cout << "----------------------------------------" << endl;
	for (int i = 0; i < pts.rows; i++)
	{
		Point pt1(pts(i, 0), pts(i, 1));	// 사각형 시작좌표
		Point pt2(pts(i, 2), pts(i, 3));	// 사각형 종료좌표
		rects.push_back(Rect(pt1, pt2));	// 벡터 저장

		sizes.push_back(rects[i].area());	// 사각형 크기 저장
		cout << format("rects[%d] = ", i) << rects[i] << endl;
	}

	// 정렬 후, 정렬 원소의 원본 좌표 반환(열 단위의 오름차순 정렬 후 원본 좌표 반환)
	// sizes의 값을 열 방향으로 오름차순 정렬 후, sort_idx에 저장
	sortIdx(sizes, sort_idx, SORT_EVERY_COLUMN);

	cout << sort_idx << endl;

	cout << endl << " 크기순 정렬 사각형 정보 \t크기" << endl;
	cout << "----------------------------------------" << endl;
	for (int i = 0; i < rects.size(); i++) {
		int idx = sort_idx(i);

		cout << rects[idx] << "\t" << sizes(idx) << endl;
	}
	cout << "----------------------------------------" << endl;
	return 0;
}
*/

/*
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;

int main()
{
	Mat_<float> m1(3, 5);
	m1 << 11, 2, 3, 4, 10,
		6, 10, 15, 9, 7,
		7, 12, 8, 14, 1;

	Mat  m_reduce1, m_reduce2, m_reduce3, m_reduce4;

	reduce(m1, m_reduce1, 0, REDUCE_SUM);	// 열방향(0) 합
	reduce(m1, m_reduce2, 1, REDUCE_AVG);	// 행방향(1) 평균
	reduce(m1, m_reduce3, 0, REDUCE_MAX);	// 열방향(0) 최대값
	reduce(m1, m_reduce4, 1, REDUCE_MIN);	// 행방향(1) 최소값

	cout << "[m1] = " << endl << m1 << endl << endl;

	cout << "[m_reduce_sum] = " << m_reduce1 << endl;
	cout << "[m_reduce_avg] = " << m_reduce2.t() << endl << endl;
	cout << "[m_reduce_max] = " << m_reduce3 << endl;
	cout << "[m_reduce_min] = " << m_reduce4.t() << endl;
	return 0;
}
*/

/*
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;
int main()
{
	Matx23f src1(1, 2, 3, 4, 5, 1);
	Matx23f src2(5, 4, 2, 3, 2, 1);
	Matx32f src3(5, 4, 2, 3, 2, 1);
	Mat dst1, dst2, dst3;
	double alpha = 1.0, beta = 1.0;

	// 행렬 곱 수행
	gemm(src1, src2, alpha, Mat(), beta, dst1);
	gemm(src1, src2, alpha, noArray(), beta, dst2);
	gemm(src1, src3, alpha, noArray(), beta, dst3);

	// 콘솔창에 출력
	cout << "[src1] = " << endl << src1 << endl;
	cout << "[src2] = " << endl << src2 << endl;
	cout << "[src3] = " << endl << src3 << endl << endl;

	cout << "[dst1] = " << endl << dst1 << endl;
	cout << "[dst2] = " << endl << dst2 << endl;
	cout << "[dst3] = " << endl << dst3 << endl;
	return 0;
}
*/

/*

#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;

int main()
{
	vector<Point> rect_pt1, rect_pt2;
	rect_pt1.push_back(Point(200, 50));
	rect_pt1.push_back(Point(400, 50));
	rect_pt1.push_back(Point(400, 250));
	rect_pt1.push_back(Point(200, 250));

	float theta = 20 * CV_PI / 180;
	Matx22f m(cos(theta), -sin(theta), sin(theta), cos(theta));

	transform(rect_pt1, rect_pt2, m);

	Mat image(400, 500, CV_8UC3, Scalar(255, 255, 255));
	for (int i = 0; i < 4; i++)
	{
		line(image, rect_pt1[i], rect_pt1[(i + 1) % 4], Scalar(0, 0, 0), 1);
		line(image, rect_pt2[i], rect_pt2[(i + 1) % 4], Scalar(255, 0, 0), 2);
		cout << "rect_pt1[" + to_string(i) + "]=" << rect_pt1[i] << "\t";
		cout << "rect_pt2[" + to_string(i) + "]=" << rect_pt2[i] << "\t";
	}
	imshow("image", image);
	waitKey();
	return 0;
}
*/

/*
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;

int main()
{
	vector<Point3f> rect_pt1, rect_pt2;
	rect_pt1.push_back(Point3f(200, 50, 1)), rect_pt1.push_back(Point3f(400, 50, 1));
	rect_pt1.push_back(Point3f(400, 250, 1)), rect_pt1.push_back(Point3f(200, 250, 1));

	// 회전 변환 행렬 : 3x3 행렬(호모지어스 코디네이트)
	float theta = 45 * CV_PI / 180;
	Matx33f m;
	m << cos(theta), -sin(theta), 0,
		sin(theta), cos(theta), 0,
		0, 0, 1;

	// 평행이동 행렬
	Mat t1 = Mat::eye(3, 3, CV_32F);					// 평행이동
	Mat t2 = Mat::eye(3, 3, CV_32F);					// 역평행이동

	Point3f delta = (rect_pt1[2] - rect_pt1[0]) / 2.0f;
	Point3f center = rect_pt1[0] + delta;

	// 원점으로 기준을 보내기 위해서
	t1.at<float>(0, 2) = center.x;
	t1.at<float>(1, 2) = center.y;
	t2.at<float>(0, 2) = -center.x; // 원점으로
	t2.at<float>(1, 2) = -center.y; // 원점으로

	Mat m2 = t1 * (Mat)m * t2;								// 중심점 좌표 계산									

	transform(rect_pt1, rect_pt2, m2);

	Mat image(400, 500, CV_8UC3, Scalar(255, 255, 255));
	for (int i = 0; i < 4; i++)
	{
		Point pt1(rect_pt1[i].x, rect_pt1[i].y);
		Point pt2(rect_pt1[(i + 1) % 4].x, rect_pt1[(i + 1) % 4].y);
		Point pt3(rect_pt2[i].x, rect_pt2[i].y);
		Point pt4(rect_pt2[(i + 1) % 4].x, rect_pt2[(i + 1) % 4].y);

		line(image, pt1, pt2, Scalar(0, 0, 0), 2);
		line(image, pt3, pt4, Scalar(255, 0, 0), 2);
		cout << "rect_pt1[" + to_string(i) + "]=" << rect_pt1[i] << "\t";
		cout << "rect_pt2[" + to_string(i) + "]=" << rect_pt2[i] << endl;
	}
	imshow("image", image);
	waitKey();
	return 0;
}
*/
