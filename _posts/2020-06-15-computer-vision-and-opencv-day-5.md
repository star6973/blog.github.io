---
layout: post
title: Computer Vision and OpenCV [Day 5]
date: 2020-06-15 10:00:00-19:00:00
categories: OpenCV
tag: OpenCV
---

## 6. 픽셀 기반 처리 이론 및 실습
- 영상처리
    + 2차원 데이터에 대한 행렬 연산
    + 2차원 데이터의 원소의 값을 개발자가 원하는 방향으로 변경하는 것
    + 영상의 화소 접근 및 그 값의 수정 필수

- OpenCV API
    + Mat 클래스 제공
      : 영상 파일을 데이터로 저장 및 처리 가능
    + Mat 클래스의 내부 메소드들 제공
      : 영상 데이터에 대한 다양한 연산 가능
    
### 6.1. Mat::at() 함수

    행렬의 지정된 원소에 접근하는 템플릿 함수
    템플릿 함수 - 함수 오버로딩과 비슷한 형태로 모든 자료형 사용 가능
    반환되는 자료형으로 템플릿 구성도 가능

```cython
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;
int main()
{
	Mat  m1(3, 5, CV_32SC1);
	Mat  m2(3, 5, CV_32FC1);
	Mat  m3(3, 5, CV_8UC2);  // 2채널 uchar 행렬
	Mat  m4(3, 5, CV_32SC3); // 3채널 int 행렬

	for (int i = 0, k = 0; i < m1.rows; i++) {
		for (int j = 0; j < m1.cols; j++, k++)
		{
			// 방법 1(i행, j열에 접근, 반환자료형을 int로 지정)
			m1.at<int>(i, j) = k;

			// 방법 2
			Point pt(j, i);
			m2.at<float>(pt) = (float)j;

			// 방법 3(2채널 행렬이기에 반환자료형은 Vec2b)
			int idx[2] = { i, j };
			m3.at<Vec2b>(idx) = Vec2b(0, 1);

			// 방법 4(3채널 행렬로 반환자료형은 Vec3i, 각 채널 원소 접근은 []로)
			m4.at<Vec3i>(i, j)[0] = 0;
			m4.at<Vec3i>(i, j)[1] = 1;
			m4.at<Vec3i>(i, j)[2] = 2;
		}
	}
	cout << "[m1] = " << endl << m1 << endl;
	cout << "[m2] = " << endl << m2 << endl;
	cout << "[m3] = " << endl << m3 << endl;
	cout << "[m4] = " << endl << m4 << endl;
	return 0;
}
```

### 6.2. Mat::ptr() 함수

```cython
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;
int main()
{
	Mat  m1(3, 5, CV_8UC1);
	Mat  m2(m1.size(), CV_32FC1);

	for (int i = 0, k = 0; i < m1.rows; i++)
	{
		uchar* ptr_m1 = m1.ptr(i); // m1의 행의 시작을 가리킴
		float* ptr_m2 = m2.ptr<float>(i);
		for (int j = 0; j < m1.cols; j++)
		{
			ptr_m1[j] = j;
			*(ptr_m2 + j) = (float)j;
		}
	}
	cout << "m1 = " << endl << m1 << endl << endl;
	cout << "m2 = " << endl << m2 << endl;
	return 0;
}
```

### 6.3. 반복자를 통한 조회

    - 반복자 사용  
      : 컬렉션의 요소 조회 위한 전문 클래스  
      : 컬렉션의 타입이나 내부 구조와 관계없이 동일하게 사용 가능  
    
    - OpenCV의 반복자  
      : C++ STL의 표준 반복자와 호환되는 Mat 반복자 클래스 제공
      : 템플릿 형태로 대부분의 자료형 선언 가능

```cython
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;
int main()
{
	uchar data[] = {
		1, 2, 3,
		1, 2, 3,
		1, 2, 3,
	};

	Mat  m1(3, 3, CV_8UC1, data);
	Mat  m2(m1.size(), m1.type());
	Mat  m3(m1.size(), CV_32FC3);

	MatConstIterator_<uchar> it_m1 = m1.begin<uchar>();
	MatIterator_<uchar>      it_m2 = m2.begin<uchar>();
	Mat_<Vec3f>::iterator    it_m3 = m3.begin<Vec3f>();

	for (; it_m1 != m1.end<uchar>(); ++it_m1, ++it_m2, ++it_m3)
	{
		*it_m2 = *it_m1;

		(*it_m3)[0] = *it_m1 * 0.5f;
		(*it_m3)[1] = *it_m1 * 0.3f;
		(*it_m3)[2] = *it_m1 * 0.2f;
	}

	cout << "m1 = " << endl << m1 << endl;
	cout << "m2 = " << endl << m2 << endl;
	cout << "m3 = " << endl << m3 << endl;
	return 0;
}
```

### 6.4. 그레이 스케일 영상

    - 단일 채널 영상을 그레이 스케일 영상이라 부름  
      : 일반적으로 지칭하는 흑백 영상 - 의미에 맞지 않음
      : 그레이 스케일(gray-scale)
        -> 0은 검은색을, 255는 흰색, 그 사이의 값들은 진한 회색에서 연한 회색까지 표현

```cython
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;
int main()
{
	Mat image1(50, 512, CV_8UC1, Scalar(0));
	Mat image2(50, 512, CV_8UC1, Scalar(0));
    
    // 행렬 원소 전체 조회
	for (int i = 0; i < image1.rows; i++) // i < 50
	{
		for (int j = 0; j < image1.cols; j++) // j < 512
		{
			image1.at<uchar>(i, j) = j / 2;
			image2.at<uchar>(i, j) = (j / 20) * 10;
		}
		
	}
	imshow("image1", image1);
	imshow("image2", image2);
	waitKey();
}
```

<center><img src="/assets/images/opencv5/1.PNG" width="50%"></center><br>

### 6.5. 영상의 화소 표현

```cython
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;
int main()
{
	Mat image = imread("../image/pixel_test.jpg", IMREAD_GRAYSCALE);
	if (image.empty())
	{
		cout << "영상을 읽지 못 했습니다." << endl;
		exit(1);
	}

	Rect roi(135, 95, 20, 15);
	Mat roi_img = image(roi); // 입력 영상의 관심영역 참조
	cout << "[roi_img] =" << endl;

	for (int i = 0; i < roi_img.rows; i++) {
		for (int j = 0; j < roi_img.cols; j++)
		{
			cout.width(5);
			cout << (int)roi_img.at<uchar>(i, j) << endl;;
		}
		cout << endl;
	}
	//cout << roi_img << endl << endl;

	rectangle(roi_img, roi, Scalar(0));
	imshow("image", image);
	waitKey();
	return 0;
}
```
<center><img src="/assets/images/opencv5/2.PNG" width="50%"></center><br>

### 6.6. 영상 밝기의 가감 연산

```cython
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;
int main()
{
	Mat image = imread("../image/bright.jpg", IMREAD_GRAYSCALE);
	CV_Assert(!image.empty());

	Mat dst1 = image + 100; // opencv가 자체적으로 255가 넘어가면 자동으로 잘라주어 제일 깔끔하게 나옴
	Mat dst2 = image - 100; 
	Mat dst3 = 255 - image; // 영상 반전

	Mat dst4(image.size(), image.type());
	Mat dst5(image.size(), image.type());

	for (int i = 0; i < image.rows; i++) {
		for (int j = 0; j < image.cols; j++)
		{
			dst4.at<uchar>(i, j) = image.at<uchar>(i, j) + 100; // 깨지는 이유는, 255가 넘어가면 자동으로 잘라주지 못하기 때문에
			//		dst4.at<uchar>(i, j) = saturate_cast<uchar>(image.at<uchar>(i, j) + 100);
			dst5.at<uchar>(i, j) = 255 - image.at<uchar>(i, j);
		}
	}

	imshow("원 영상", image);
	imshow("dst1 - 밝게", dst1);
	imshow("dst2 - 어둡게", dst2);
	imshow("dst3 - 반전", dst3);
	imshow("dst4 - 밝게", dst4);
	imshow("dst5 - 반전", dst5);
	waitKey();
	return 0;
}
```
<center><img src="/assets/images/opencv5/3.PNG" width="50%"></center><br>


### 6.7. 행렬 덧셈 및 곱셈을 이용한 영상 합성

    합성하는 다양한 예시
    1) dst(y, x) = image1(y, x) * 0.5 + image2(y, x) * 0.5;
    2) dst(y, x) = image1(y, x) * alpha + image2(y, x) * (1-alpha)
    3) dst(y, x) = image1(y, x) * alpha + image2(y, x) * beta

    void addWeighted()
        - InputArray src1: 첫 번째 입력 행렬
        - double alpha: src1 행렬의 가중치
        - InputArray src2: 두 번째 입력 행렬. src1과 크기와 채널 수가 같아야 한다.
        - double beta: src2 행렬의 가중치
        - double gamma: 가중합 결과에 추각적으로 더할 값
        - OutputArray dst: 출력 행렬. 입력 행렬과 같은 크기. 같은 채널 수의 행렬이 생성된다.
        - int dtype: 출력 행렬의 깊이. src1과 src2의 깊이가 같은 경우에는 dtype에 -1을 지정할 수 있다.
                     이 경우 dst의 깊이는 src1, src2와 같은 깊이로 설정된다.
                     src1과 src2의 깊이가 서로 다른 경우에는 dtype을 반드시 지정해야 한다. 

```cython
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;
int main()
{
	Mat image1 = imread("../image/add2.jpg", IMREAD_GRAYSCALE);
	Mat image2 = imread("../image/add1.jpg", IMREAD_GRAYSCALE);
	CV_Assert(!(image1.empty() || image2.empty()));

	double alpha = 0.6, beta = 0.7;

	Mat add_img1 = image1 + image2; // 너무 커져서 밝아지게 되어 합성 효과가 나오지 않음
	Mat add_img2 = image1 * 0.5 + image2 * 0.5; // 반씩 섞기 때문에 합성 효과가 나타남
	Mat add_img3 = image1 * alpha + image2 * (1 - alpha);

	Mat add_img4, sub_img1, sub_img2;;
	addWeighted(image1, alpha, image2, beta, 0, add_img4); // opencv 함수

	imshow("image1", image1), imshow("image2", image2);
	imshow("add_img1", add_img1), imshow("add_img2", add_img2);
	imshow("add_img3", add_img3), imshow("add_img4", add_img4);
	waitKey(0);
	return 0;
}
```
<center><img src="/assets/images/opencv5/4.PNG" width="50%"></center><br>
<center><img src="/assets/images/opencv5/4_1.PNG" width="50%"></center><br>
<center><img src="/assets/images/opencv5/4_2.PNG" width="50%"></center><br>

### 6.8. 명암 대비

```cython
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;
int main()
{
	Mat image = imread("../image/contrast_test.jpg", 0);	// 명암도 타입 읽기
	CV_Assert(image.data);								// 예외처리

	Scalar avg = mean(image) / 2.0;					// 원본 영상 화소 평균의 절반
	Mat dst1 = image * 0.5;							// 명암대비 감소(나누면 감소)
	Mat dst2 = image * 2.0;							// 명암대비 증가(곱하면 증가)
	Mat dst3 = image * 0.5 + avg[0];				// 영상 평균 이용 대비 감소
	Mat dst4 = image * 2.0 - avg[0];				// 영상 평균 이용 대비 증가

	imshow("image", image);
	imshow("dst1-대비감소", dst1), imshow("dst2-대비증가", dst2);
	imshow("dst3-평균이용 대비감소", dst3), imshow("dst4-평균이용 대비증가", dst4);

	waitKey();
	return 0;
}
```
<center><img src="/assets/images/opencv5/5.PNG" width="50%"></center><br>
<center><img src="/assets/images/opencv5/5_1.PNG" width="50%"></center><br>
<center><img src="/assets/images/opencv5/5_2.PNG" width="50%"></center><br>

### 6.9. 히스토그램 계산

    - 히스토그램
      : "관측 값의 개수를 겹치지 않는 다양한 계급으로 표시하는 것"
      : 그림(그래프)이기 때문에 데이터의 분포 상태 쉽게 이해
      : 화소 분포 나타내는 지표 - 영상 특성 판단 도구로 사용 가능

```cython
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;

// bins가 256이면 0 ~ 255
// bins가 128이면 (0, 1), (2, 3) ... (254, 255)와 같음
void calc_histo(Mat image, Mat& hist, int bins, int range_max = 256)
{
	// 히스토그램 누적 행렬
	hist = Mat(bins, 1, CV_32F, Scalar(0));
	// 계급 간격
	float gap = range_max / (float)bins;
}

int main()
{
	Mat image = imread("../image/pixel_test.jpg", IMREAD_GRAYSCALE);
	CV_Assert(!image.empty());

	Mat hist;
	calc_histo(image, hist, 256);		// 히스토그램 계산
	cout << hist.t() << endl;

	imshow("image", image);
	waitKey();
	return 0;
}
```
<center><img src="/assets/images/opencv5/6.PNG" width="50%"></center><br>

### 6.10. OpenCV 함수로 히스토그램 계산

    void calHist()
        - Mat* images: 원본 영상배열들. CV_8U 혹은 CV_32F 형으로 크기가 같아야 함
        - int nimages: 원본 영상의 개수
        - int* channels: 히스토그램 계산에 사용되는 차원 목록
        - InputArray mask: 특정 영역에만 계산하기 위한 마스크 행렬. 입력 영상과 같은 크기의 8비트 배열
        - OutputArray hist: 계산된 히스토그램이 출력되는 배열
        - int dims: 히스토그램의 차원 수
        - int* histSize: 각 차원의 히스토그램 배열 크기. 계급(bin)의 개수
        - float** ranges: 각 차원의 히스토그램의 범위
        - bool uniform: 히스토그램이 균일(uniform)한지를 나타내는 플래그
        - bool accumulate: 누적 플래그. 여러 배열에서 단일 히스토그램을 구할 때 사용

```cython
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;

void calc_histo(Mat image, Mat& hist, int bins, int range_max = 256)
{
	hist = Mat(bins, 1, CV_32F, Scalar(0));
	float gap = range_max / (float)bins;

	for (int i = 0; i < image.rows; i++) {
		for (int j = 0; j < image.cols; j++)
		{
			int idx = int(image.at<uchar>(i, j) / gap);
			hist.at<float>(idx)++;
		}
	}
}

void  calc_Histo(const Mat& image, Mat& hist, int bins, int range_max = 256)
{
	int		histSize[] = { bins };					// 히스토그램 계급개수
	float   range[] = { 0, (float)range_max };		// 채널 화소값 범위
	int		channels[] = { 0 };						// 채널 목록
	const float* ranges[] = { range };

	calcHist(&image, 1, channels, Mat(), hist, 1, histSize, ranges);
}

int main()
{
	Mat image = imread("../image/pixel_test.jpg", IMREAD_GRAYSCALE);
	CV_Assert(!image.empty());

	Mat hist, hist_img;
	calc_Histo(image, hist, 256);

	cout << hist.t() << endl;

	imshow("image", image);
	waitKey();
	return 0;
}
```
<center><img src="/assets/images/opencv5/7.PNG" width="50%"></center><br>

```cython
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;

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
	normalize(hist, hist, 0, size.height, NORM_MINMAX); // 히스토그램의 최소/최대 사이즈를 정해주기

	for (int i = 0; i < hist.rows; i++)
	{
		float  start_x = (i * bin);
		float  end_x = (i + 1) * bin;
		Point2f pt1(start_x, 0);
		Point2f pt2(end_x, hist.at <float>(i));

		if (pt2.y > 0)
			rectangle(hist_img, pt1, pt2, Scalar(i), -1);
	}
	flip(hist_img, hist_img, 0);
}

int main()
{
	Mat image = imread("../image/pixel_test.jpg", IMREAD_GRAYSCALE);
	CV_Assert(!image.empty());

	Mat hist, hist_img;
	calc_Histo(image, hist, 256);
	draw_histo(hist, hist_img);

	imshow("hist_img", hist_img);
	imshow("image", image);
	waitKey();
	return 0;
}
```
<center><img src="/assets/images/opencv5/8.PNG" width="50%"></center><br>


### 6.11. 히스토그램 스트레칭

    히스토그램의 분포가 좁아서 영상의 대비가 좋지 않은 영상의 화질을 개선할 수 있는 알고리즘

    새 화소값 = (화소값 - low) x 255 / (hight - low)

```cython
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;

void calc_Histo(const Mat& image, Mat& hist, int bins, int range_max = 256)
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
		float idx1 = i * bin;
		float idx2 = (i + 1) * bin;
		Point2f pt1 = Point2f(idx1, 0);
		Point2f pt2 = Point2f(idx2, hist.at <float>(i));

		if (pt2.y > 0)
			rectangle(hist_img, pt1, pt2, Scalar(0), -1);
	}
	flip(hist_img, hist_img, 0);
}

int search_valueIdx(Mat hist, int bias = 0)
{
	for (int i = 0; i < hist.rows; i++)
	{
		int idx = abs(bias - i);
		if (hist.at<float>(idx) > 0)
			return idx;
	}
	return -1;
}

int main()
{
	Mat image = imread("../image/histo_test.jpg", 0);
	CV_Assert(!image.empty());

	Mat hist, hist_dst, hist_img, hist_dst_img;
	int  histsize = 64, ranges = 256;
	
	calc_Histo(image, hist, histsize, ranges);
	
	float bin_width = (float)ranges / histsize;

	int low_value = (int)(search_valueIdx(hist, 0) * bin_width);
	int high_value = (int)(search_valueIdx(hist, hist.rows-1) * bin_width);
	cout << "high_value = " << high_value << endl;
	cout << "low_value = " << low_value << endl;

	
	int d_value = high_value - low_value;
	Mat  dst = (image - low_value) * (255.0) / (float)d_value;

	calc_Histo(dst, hist_dst, histsize, ranges);
	draw_histo(hist, hist_img);
	draw_histo(hist_dst, hist_dst_img);

	imshow("image", image);
	imshow("dst-stretching", dst);
	imshow("img_hist", hist_img);
	imshow("dst_hist", hist_dst_img);
	
	waitKey();
	return 0;
}
```
<center><img src="/assets/images/opencv5/9.PNG" width="50%"></center><br>

### 6.12. 히스토그램 평활화

    한쪽으로 치우친 명암 분포를 가진 영상을 히스토그램의 재분배 과정을 거쳐서 균등한 히스토그램 분포를 갖게하는 알고리즘

    1) 영상의 히스토그램을 계산한다.
       - 명암값 j의 빈도수 hist[j]를 계산

    2) 히스토그램 빈도값에서 누적 빈도수(누적합)를 계산한다.
       - sum[i] = sum(hist[j])
    
    3) 누적 빈도수를 정규화(정규화 누적합)한다.
       - n[i] = sum[i] x 1/n x 255

    4) 결과 화소값 = 정규화 누적합 * 최대 화소값

    * CLAHE(Contrast Limited Adaptive Histogram Equalization)
    
      지금까지의 처리는 이미지의 전체적인 부분에 균일화를 적용하였지만, 일반적인 이미지는 밝은 부분과 어두운 부분이 섞여 있기 때문에 전체에 적용하는 것은 그렇게 유용하지 않다. 
      주변의 어두운 부분은 균일화가 적용되어 밝아졌지만, 밝은 부분은 오히려 너무 밝아져 경계선을 알아볼 수 없게 된다. 
      이 문제를 해결하기 위해서 adaptive histogram equalization을 적용하게 되는데, 이미지를 작은 title형태로 나누어 그 title안에서 Equalization을 적용하는 방식이다. 
      그런데 작은 영역이다 보니 작은 노이즈(극단적으로 어둡거나, 밝은 영역)가 있으면 이것이 반영이 되어 원하는 결과를 얻을 수 없게 된다. 
      이 문제를 피하기 위해서 contrast limit라는 값을 적용하여 이 값을 넘어가는 경우는 그 영역은 다른 영역에 균일하게 배분하여 적용한다.

```cython
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;

void calc_Histo(const Mat& image, Mat& hist, int bins, int range_max = 256)
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
		float idx1 = i * bin;
		float idx2 = (i + 1) * bin;
		Point2f pt1 = Point2f(idx1, 0);
		Point2f pt2 = Point2f(idx2, hist.at <float>(i));

		if (pt2.y > 0)
			rectangle(hist_img, pt1, pt2, Scalar(0), -1);
	}
	flip(hist_img, hist_img, 0);
}


void create_hist(Mat img, Mat& hist, Mat& hist_img) // 히스토그램 계산과 그래프 그리기 통합
{
	int  histsize = 256, range = 256;
	calc_Histo(img, hist, histsize, range);			// 히스토그램 계산
	draw_histo(hist, hist_img);							// 히스토그램 그래프 그리기
}

int main()
{
	Mat image = imread("../image/equalize_test.jpg", 0);		// 명암도 영상 읽기
	CV_Assert(!image.empty());									// 영상파일 예외처리

	Mat hist, dst1, dst2, hist_img, hist_img1, hist_img2;

	// 1단계. 히스토그램 생성
	create_hist(image, hist, hist_img);				// 히스토그램 및 그래프 그리기

	// 2단계. 각 명암값 i에 대해 0부터 i까지의 빈도수의 누적합
	Mat accum_hist = Mat(hist.size(), hist.type(), Scalar(0));
	accum_hist.at<float>(0) = hist.at<float>(0);
	for (int i = 1; i < hist.rows; i++) {
		accum_hist.at<float>(i) = accum_hist.at<float>(i - 1) + hist.at<float>(i);
	}

	
	// 3단계. 단계 2에서 구한 누적값을 정규화
	accum_hist /= sum(hist)[0];
	accum_hist *= 255;

	// 4단계. 입력 영상에서 픽셀값 i를 정규화된 값 n[i]로 변환하여 결과 영상 생성
	dst1 = Mat(image.size(), CV_8U);
	for (int i = 0; i < image.rows; i++) {
		for (int j = 0; j < image.cols; j++) {
			int idx = image.at<uchar>(i, j);
			dst1.at<uchar>(i, j) = (uchar)accum_hist.at<float>(idx);
		}
	}
	
	equalizeHist(image, dst2);	// 히스토그램 평활화
	create_hist(dst1, hist, hist_img1);			// 히스토그램 및 그래프 그리기
	create_hist(dst2, hist, hist_img2);

	imshow("image", image), imshow("img_hist", hist_img);	// 원본 히스토그램
	imshow("dst1-User", dst1), imshow("User_hist", hist_img1);		// 사용자 평활화 
	imshow("dst2-OpenCV", dst2), imshow("OpenCV_hist", hist_img2);// OpenCV 평활화
	
	waitKey();
	return 0;
}
```
<center><img src="/assets/images/opencv5/10.PNG" width="50%"></center><br>
<center><img src="/assets/images/opencv5/11.PNG" width="50%"></center><br>
<center><img src="/assets/images/opencv5/12.PNG" width="50%"></center><br>
