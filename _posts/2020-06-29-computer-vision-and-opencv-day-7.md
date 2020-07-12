---
layout: post
title: Computer Vision and OpenCV [Day 7]
date: 2020-06-29 10:00:00-19:00:00
categories: OpenCV
tag: OpenCV
use_math: true
---

# 8. 영역 기반 처리 이론 및 실습 2
## 8.1. 영역 기반 처리 실습
### 8.1.1. 모폴로지(morphology)
- 모폴로지 연산은 영상 내부 객체의 형태와 구조를 분석하고 처리하는 기법
- 그레이스케일 영상과 이진 영상에 모두 적용이 가능하지만, 주로 이진화된 영상에서 객체의 모양을 변형하는 용도로 사용
- 주로 이진 영상에서 객체의 모양을 단순화시키거나 잡음을 제거하는 용도
- 종류
    + 침식
    	+ 객체 영역의 외곽을 골고루 깎아 내는 연산으로 전체적으로 객체 영역은 축소되고 배경은 확대됨
        + 구조 요소를 영상 전체에 대해 스캔하면서, 구조 요소가 객체 영역 내부에 완전히 포함될 경우 고정점 위치 픽셀을 255로 설정
	+ 원래 영상에서 껍데기를 떼어내는 것, 아주 얇은 정보들(잡음일 경우가 많음)이 사라짐 -> 잡음 제거 효과
	+ 입력은 0과 1만, 하나라도 0이면 0을 출력
	+ 내부 잡음이 더 커질 수도 있는 단점이 있다.
	+ 4방향보다 8방향 마스크가 더 침식이 됨
    + 팽창 - 객체 외곽을 확대하는 연산으로 객체 영역은 확대되고, 배경은 축소됨
        + 구조 요소를 영상 전체에 대해 이동하면서, 구조 요소와 객체 영역이 한 픽셀이라도 만날 경우 고정점 위치 픽셀을 255로 설정
	+ 내부 잡음이 채워지면서 사라지지만, 외부의 얇은 잡음은 더 확대가 됨
- 구조 요소는 원소값이 0 또는 1로 구성된 CV_8UC1 타입의 Mat 행렬로 표현된다.

### 8.1.2. 침식 연산
```cython
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;

//마스크 원소와 마스크 범위 입력화소 간의 일치 여부 체크
bool check_match(Mat img, Point start, Mat mask, int mode = 0)
{
	for (int u = 0; u < mask.rows; u++) {
		for (int v = 0; v < mask.cols; v++) {
			Point pt(v, u);
			int m = mask.at<uchar>(pt); // 마스크 계수 	
			int p = img.at<uchar>(start + pt); // 해당 위치 입력화소 

			bool ch = (p == 255); // 일치 여부 비교(0이면 0, 255이면 1 -> 바이너리(0, 1)로 만들어주기 위해서)
			// mode = 0, 하나라도 불일치, false
			// mode = 1, 하나라도 일치, flase
			if (m == 1 && ch == mode) return false;
		}
	}
	return true;
}

void erosion(Mat img, Mat& dst, Mat mask)
{
	dst = Mat(img.size(), CV_8U, Scalar(0));
	if (mask.empty())	mask = Mat(3, 3, CV_8UC1, Scalar(0));

	Point h_m = mask.size() / 2;

	for (int i = h_m.y; i < img.rows - h_m.y; i++) {
		for (int j = h_m.x; j < img.cols - h_m.x; j++)
		{
			Point start = Point(j, i) - h_m;
			// mode = 0 -> 모두 일치하면 true, 하나라도 불일치하면 false
			bool  check = check_match(img, start, mask, 0);
			// 모두 일치시(check값이 true인 경우) 255, 하나라도 불일치할 시(check값이 false인 경우) 0
			dst.at<uchar>(i, j) = (check) ? 255 : 0;
		}
	}
}

int main()
{
	Mat image = imread("../image/morph_test1.jpg", 0);
	CV_Assert(image.data);

	Mat th_img, dst1, dst2;
	threshold(image, th_img, 128, 255, THRESH_BINARY); // 128보다 크면 255로, 128보다 작으면 0으로 보낸다

	Matx < uchar, 3, 3> mask;
	mask << 0, 1, 0,
		1, 1, 1,
		0, 1, 0;

	erosion(th_img, dst1, (Mat)mask);
	morphologyEx(th_img, dst2, MORPH_ERODE, mask);

	imshow("image", image), imshow("이진 영상", th_img);
	imshow("User_erosion", dst1);
	imshow("OpenCV_erosion", dst2);

	waitKey();
	return 0;
}
```
<center><img src = "/assets/images/opencv7/1.PNG" width="50%"></center><br>

### 8.1.3. 팽창 연산

```cython
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;

// 팽창: mode = 1
bool check_match(Mat img, Point start, Mat mask, int mode)
{
	for (int u = 0; u < mask.rows; u++) {
		for (int v = 0; v < mask.cols; v++) {
			int m = mask.at<uchar>(Point(v, u));				// 마스크 계수 	
			int p = img.at<uchar>(start + Point(v, u));		// 해당 위치 입력화소 

			bool ch = (p == 255);				// 일치 여부 비교
			if (m == 1 && ch == mode) return false;
		}
	}
	return true;
}

void dilation(Mat img, Mat& dst, Mat mask)
{
	dst = Mat(img.size(), CV_8U, Scalar(0));
	if (mask.empty())	mask = Mat(3, 3, CV_8UC1, Scalar(0));

	Point h_m = mask.size() / 2;
	for (int i = h_m.y; i < img.rows - h_m.y; i++) {
		for (int j = h_m.x; j < img.cols - h_m.x; j++)
		{
			Point start = Point(j, i) - h_m;
			// mode = 1 -> 모두 불일치하면 true, 하나라도 일치하면 false
			bool  check = check_match(img, start, mask, 1);	// 원소 일치여부 비교
			// 모두 불일치시(check값이 true인 경우) 0, 하나라도 일치할 시(check값이 false인 경우) 255
			dst.at<uchar>(i, j) = (check) ? 0 : 255;
		}
	}
}

int main()
{
	Mat image = imread("../image/morph_test1.jpg", 0);
	CV_Assert(image.data);

	Mat th_img, dst1, dst2;
	threshold(image, th_img, 128, 255, CV_THRESH_BINARY);


	Matx < uchar, 3, 3> mask;
	mask << 0, 1, 0,
		1, 1, 1,
		0, 1, 1;

	dilation(th_img, dst1, (Mat)mask);
	morphologyEx(th_img, dst2, MORPH_DILATE, mask);

	imshow("image", image);	imshow("User_dilation", dst1);
	imshow("OpenCV_dilation", dst2);
	waitKey();
	return 0;
}
```
<center><img src = "/assets/images/opencv7/2.PNG" width="50%"></center><br>

### 8.1.4. 열림 연산과 닫힘 연산

```cython
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;

bool check_match(Mat img, Point start, Mat mask, int mode = 0)
{
	for (int u = 0; u < mask.rows; u++) {
		for (int v = 0; v < mask.cols; v++) {
			Point pt(v, u);
			int m = mask.at<uchar>(pt);					// 마스크 계수 	
			int p = img.at<uchar>(start + pt);			// 해당 위치 입력화소 

			bool ch = (p == 255);				// 일치 여부 비교
			if (m == 1 && ch == mode)	return  false;
		}
	}
	return true;
}

void erosion(Mat img, Mat& dst, Mat mask)
{
	dst = Mat(img.size(), CV_8U, Scalar(0));
	if (mask.empty())	mask = Mat(3, 3, CV_8UC1, Scalar(0));

	Point h_m = mask.size() / 2;
	for (int i = h_m.y; i < img.rows - h_m.y; i++) {
		for (int j = h_m.x; j < img.cols - h_m.x; j++)
		{
			Point start = Point(j, i) - h_m;
			bool  check = check_match(img, start, mask, 0);
			dst.at<uchar>(i, j) = (check) ? 255 : 0;
		}
	}
}

void dilation(Mat img, Mat& dst, Mat mask)
{
	dst = Mat(img.size(), CV_8U, Scalar(0));
	if (mask.empty())	mask = Mat(3, 3, CV_8UC1, Scalar(0));

	Point h_m = mask.size() / 2;
	for (int i = h_m.y; i < img.rows - h_m.y; i++) {
		for (int j = h_m.x; j < img.cols - h_m.x; j++)
		{
			Point start = Point(j, i) - h_m;
			bool  check = check_match(img, start, mask, 1);
			dst.at<uchar>(i, j) = (check) ? 0 : 255;
		}
	}
}

// 열림 연산: 침식 -> 팽창
void opening(Mat img, Mat& dst, Mat mask)
{
	Mat tmp; // 임시변수
	erosion(img, tmp, mask);
	dilation(tmp, dst, mask);
}

// 닫힘 연산: 팽창 -> 침식
void closing(Mat img, Mat& dst, Mat mask)
{
	Mat tmp;
	dilation(img, tmp, mask);
	erosion(tmp, dst, mask);
}

int main()
{
	Mat image = imread("../image/morph_test1.jpg", 0);
	CV_Assert(image.data);
	Mat th_img, dst1, dst2, dst3, dst4;
	threshold(image, th_img, 128, 255, THRESH_BINARY);

	Matx < uchar, 3, 3> mask;
	mask << 0, 1, 0,
		1, 1, 1,
		0, 1, 0;

	opening(th_img, dst1, (Mat)mask);
	closing(th_img, dst2, (Mat)mask);
	morphologyEx(th_img, dst3, MORPH_OPEN, mask);
	morphologyEx(th_img, dst4, MORPH_CLOSE, mask);

	imshow("User_opening", dst1), imshow("User_closing", dst2);
	imshow("OpenCV_opening", dst3), imshow("OpenCV_closing", dst4);
	waitKey();
	return 0;
}
```
<center><img src = "/assets/images/opencv7/3.PNG" width="50%"></center><br>

### 8.1.5. 모폴로지(morphology) 심화 예제

```cython
// 팽창 후 침식
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;

bool check_match(Mat img, Point start, Mat mask, int mode = 0)
{
	for (int u = 0; u < mask.rows; u++) {
		for (int v = 0; v < mask.cols; v++) {
			Point pt(v, u);
			int m = mask.at<uchar>(pt);					// 마스크 계수 	
			int p = img.at<uchar>(start + pt);			// 해당 위치 입력화소 

			bool ch = (p == 255);				// 일치 여부 비교
			if (m == 1 && ch == mode)	return  false;
		}
	}
	return true;
}

void erosion(Mat img, Mat& dst, Mat mask)
{
	dst = Mat(img.size(), CV_8U, Scalar(0));
	if (mask.empty())	mask = Mat(3, 3, CV_8UC1, Scalar(0));

	Point h_m = mask.size() / 2;
	for (int i = h_m.y; i < img.rows - h_m.y; i++) {
		for (int j = h_m.x; j < img.cols - h_m.x; j++)
		{
			Point start = Point(j, i) - h_m;
			bool  check = check_match(img, start, mask, 0);
			dst.at<uchar>(i, j) = (check) ? 255 : 0;
		}
	}
}

void dilation(Mat img, Mat& dst, Mat mask)
{
	dst = Mat(img.size(), CV_8U, Scalar(0));
	if (mask.empty())	mask = Mat(3, 3, CV_8UC1, Scalar(0));

	Point h_m = mask.size() / 2;
	for (int i = h_m.y; i < img.rows - h_m.y; i++) {
		for (int j = h_m.x; j < img.cols - h_m.x; j++)
		{
			Point start = Point(j, i) - h_m;
			bool  check = check_match(img, start, mask, 1);
			dst.at<uchar>(i, j) = (check) ? 0 : 255;
		}
	}
}

int main()
{
	while (1)
	{
		int no;
		cout << "차량 영상 번호( 0:종료 ) : ";
		cin >> no;
		if (no == 0) break;

		string fname = format("../test_car/%02d.jpg", no);
		Mat image = imread(fname, 1);
		if (image.empty()) {
			cout << to_string(no) + "번 영상 파일이 없습니다. " << endl;
			continue;
		}

		Mat gray, sobel, th_img, morph;
		Mat kernel(5, 31, CV_8UC1, Scalar(1));		// 열림 연산 마스크
		cvtColor(image, gray, CV_BGR2GRAY);		// 명암도 영상 변환

		blur(gray, gray, Size(5, 5));					// 블러링
		Sobel(gray, gray, CV_8U, 1, 0, 3);			// 소벨 에지 검출

		threshold(gray, th_img, 120, 255, THRESH_BINARY);	// 이진화 수행
//		morphologyEx(th_img, morph, MORPH_CLOSE, kernel);	// 닫힘 연산 수행

		Mat tmp;
		morphologyEx(th_img, tmp, MORPH_DILATE, kernel);
		morphologyEx(tmp, morph, MORPH_ERODE, kernel);

		imshow("image", image);
		imshow("중간 영상", tmp);
		imshow("이진 영상", th_img), imshow("닫힘 연산", morph);
		waitKey();
	}
	return 0;
}
```
<center><img src = "/assets/images/opencv7/4.PNG" width="50%"></center><br>

### 8.1.6. 연습문제: Connected Component Labeling

```cython
// 흑백으로 바꾸기, 가우시안 블러, threshold로 쳐내기, 열림 연산으로 노이즈 제거
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;

int main() {

	// 1. 이미지 불러오기
	Mat image = imread("../image/coins.jpg", 1);
	CV_Assert(image.data);

	Mat kernel(5, 5, CV_8UC1, Scalar(1));

	// 2. 흑백 영상
	Mat gray;
	cvtColor(image, gray, CV_BGR2GRAY);

	// 3. 가우시안 블러 처리
	Mat blur;
	GaussianBlur(gray, blur, Size(9, 9), 3.5);

	// 4. threshold 처리
	Mat thresh;
	threshold(blur, thresh, 70, 255, THRESH_BINARY);
	
	// 5. 열림 연산
	Mat morph;
	morphologyEx(thresh, morph, MORPH_OPEN, kernel);

	imshow("image", image);
	imshow("gray", gray);
	imshow("blur", blur);
	imshow("thresh", thresh);
	imshow("morph", morph);

	// 5. connectedComponents를 이용해 동전 개수 세기
	Mat labelImage(morph.size(), CV_32S);
	int nLabels = connectedComponents(morph, labelImage, 8);
	cout << nLabels << endl;

	// 6. 라벨링된 이미지를 각기 다른 컬러로 보여주기
	labelImage.convertTo(labelImage, CV_8U);
	labelImage = labelImage * 10; // 1부터 11까지 라벨링된 동전들은 각각 밝기값이 1부터 11까지이지만, 밝기가 너무 어두워서 곱해준다.
	imshow("labelImage", labelImage);

	waitKey();
	return 0;

}
```
<center><img src = "/assets/images/opencv7/5.PNG" width="100%"></center><br>

### 8.1.7. Otsu's Thresholding

```cython
// 임계값 이상은 물체, 미만은 배경으로
// 모든 밝기값에 대해서 forground, background로 나누서 분산을 각각 구해서 제일 작은 값의 T값을 threshold로 출력한다.
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;

String	title = "Coins";

void  calc_Histo(const Mat& image, Mat& hist, int bins, int range_max = 256)
{
	int		histSize[] = { bins };			// 히스토그램 계급개수
	float   range[] = { 0, (float)range_max };		// 히스토그램 범위
	int		channels[] = { 0 };				// 채널 목록
	const float* ranges[] = { range };

	calcHist(&image, 1, channels, Mat(), hist, 1, histSize, ranges);
}

void draw_histo(Mat hist, Mat& hist_img, Size size = Size(256, 200))
{
	hist_img = Mat(size, CV_8U, Scalar(255));
	float  bin = (float)hist_img.cols / hist.rows;
	normalize(hist, hist, 0, size.height, NORM_MINMAX);

	for (int i = 0; i < hist.rows; i++)
	{
		float  start_x = (i * bin);
		float  end_x = (i + 1) * bin;
		Point2f pt1(start_x, 0);
		Point2f pt2(end_x, hist.at <float>(i));

		if (pt2.y > 0)
			rectangle(hist_img, pt1, pt2, Scalar(0), -1);
	}
	flip(hist_img, hist_img, 0);
}

int main()
{
	Mat image = imread("../image/coins.jpg", 1);
	CV_Assert(image.data);

	Mat gray, th_img;
	cvtColor(image, gray, CV_BGR2GRAY);				// 명암도 변환 
	GaussianBlur(gray, gray, Size(5, 5), 2, 2);				// 블러링

	// 433draw_histogram.cpp
	Mat hist, hist_img;
	calc_Histo(gray, hist, 256);
	draw_histo(hist, hist_img);

	imshow("hist_img", hist_img);
	imshow("gray_image", gray);

	// otsu's thresholding
	int iTH, i, j, iMinTH = -1;
	float ave, sum, varBG, varOB, Wb, Wf, fMin = 10000;
	for (iTH = 0; iTH <= 255; iTH++)
	{
		// background < iTH
		sum = 0;		ave = 0;	varBG = 0;
		for (i = 0; i < iTH; i++)
		{
			ave += i * hist.at<float>(i);
			sum += hist.at<float>(i);
		}
		ave = ave / (float)sum;
		Wb = sum / (float)(gray.rows * gray.cols);
		for (i = 0; i < iTH; i++)
			varBG += (i - ave) * (i - ave) * hist.at<float>(i);
		varBG = varBG / (float)sum;

		// object >= iTH
		sum = 0;		ave = 0;	varOB = 0;
		for (i = iTH; i <= 255; i++)
		{
			ave += i * hist.at<float>(i);
			sum += hist.at<float>(i);
		}
		ave = ave / (float)sum;
		Wf = sum / (float)(gray.rows * gray.cols);
		for (i = iTH; i <= 255; i++)
			varOB += (i - ave) * (i - ave) * hist.at<float>(i);
		varOB = varOB / (float)sum;

		//		cout << varBG << '\t' << Wb << '\t' << varOB << '\t' << Wf << '\n';

		if (fMin > (Wf * varOB + Wb * varBG))
		{
			fMin = Wf * varOB + Wb * varBG;
			iMinTH = iTH;
		}
	}

	cout << iMinTH << '\n';

	threshold(gray, th_img, iMinTH, 255, THRESH_BINARY); // 이진화
	// threshold(gray, th_img, 130, 255, THRESH_BINARY | THRESH_OTSU); // 이진화
	morphologyEx(th_img, th_img, MORPH_OPEN, Mat());// , Point(-1, -1), 1);  // 열림연산
	Mat labels = Mat(th_img.size(), CV_32S);
	int iCC = connectedComponents(th_img, labels, 8, CV_32S);
	cout << iCC - 1 << '\n';
	labels.convertTo(labels, CV_8U);
	labels = labels * 10;
	imshow("image", image);
	imshow(title, th_img);
	imshow("labels", labels);
	waitKey();
	return 0;
}
```
<center><img src = "/assets/images/opencv7/6.PNG" width="70%"></center><br>
