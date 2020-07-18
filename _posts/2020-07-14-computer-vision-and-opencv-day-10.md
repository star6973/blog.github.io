---
layout: post
title: Computer Vision and OpenCV [Day 10]
date: 2020-07-14 10:00:00-19:00:00
categories: OpenCV
tag: OpenCV
use_math: true
---

> Visual C++ 영상 처리 프로그래밍

# 11. 영상 워핑과 모핑, 그리고 Feature 검출인식 및 실습
## 11.1. 영상 워핑과 모핑
### 11.1.1. 영상 워핑(Warping)
- 픽셀의 위치를 이동하는 기하학적 처리 중 한 기법이다.
- 회전, 이동, 확대/축소 등의 기하학적 처리는 모든 픽셀에 대해 일정한 규칙을 적용함으로써 균일한 반환 결과를 얻지만, 영상 워핑은 픽셀별로 이동 정도를 달리할 수 있어 고무판 위에 그려진 영상을 임의대로 구부리는 효과를 나타낼 수 있다.
- 인공위성이나 우주선으로부터 전공된 영상은 렌즈의 변형, 신호의 왜곡 등의 이유로 인해 일그러지는 경우가 많은데, 영상 워핑은 이러한 일그러짐을 복구하는데 사용한다.
<

- 입력 영상과 출력 영상의 대응관계 기술
    + 제어선, 제어점, 그물망, 다각형 등을 이용
    + 



### 11.1.2. 영상 모핑(Morphing)





### 11.2.1. Feature 검출인식 및 실습
#### 11.2.1.1. 허프 변환

```cython
// 수직 교차점이 제어선 내부에 있는 경우는 수직선에 따라 가중치를 줌
// 수직 교차점이 제어선 외부에 있는 경우는 거리에 따라 가중치를 줌
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;

void  hough_coord(Mat image, Mat& acc_mat, double rho, double theta)
{
	int  acc_h = int((image.rows + image.cols) * 2 / rho); // 피타고라스 정리로 구한 수직선보다 살짝 더 큰 범위인 width + height로 범위를 설정
	int  acc_w = int(CV_PI / theta);

	acc_mat = Mat(acc_h, acc_w, CV_32S, Scalar(0));

	for (int i = 0; i < image.rows; i++) {
		for (int j = 0; j < image.cols; j++)
		{
			Point pt(j, i); // 조회 좌표
			if (image.at<uchar>(pt) > 0) { // 직선 여부 검사

				for (int t = 0; t < acc_w; t++) // 행렬의 인덱스이므로 라디안이 필요
				{
					// pt(j, i)가 0보다 큰 값은 곧, 점을 뜻하므로, 점을 지나는 직선 방정식 구하기
					double radian = t * theta;
					double r = pt.x * cos(radian)+ pt.y * sin(radian);
					r = cvRound(r / rho + acc_mat.rows / 2.0); // rho를 나누는 이유는 theta와 같은 이유(허프 좌표계에서 y값 역시 그냥 상수이지만, 좌표로 봤을 때는 인덱스값이기 때문에)
					acc_mat.at<int>((int)r, t)++;
				}
			}
		}
	}
}

void acc_mask(Mat acc_mat, Mat& acc_dst, Size size, int thresh)
{
	acc_dst = Mat(acc_mat.size(), CV_32S, Scalar(0)); // 마스크 적용 후의 결과영상 구하기
	Point h_m = size / 2;	// 마스크 크기 절반

	for (int r = h_m.y; r < acc_mat.rows - h_m.y; r++) {
		for (int t = h_m.x; t < acc_mat.cols - h_m.x; t++)
		{
			Point center = Point(t, r) - h_m;
			int c_value = acc_mat.at<int>(center);	// 입력영상(아까 위에서 만들어준 누적 허프 변환 행렬)의 중심화소
			if (c_value >= thresh)
			{
				// 마스크만큼 정해준 영역에 대해서 순회하면서
				double maxVal = 0;
				for (int u = 0; u < size.height; u++) {
					for (int v = 0; v < size.width; v++)
					{
						Point start = center + Point(v, u);
						if (start != center && maxVal < acc_mat.at<int>(start))
							maxVal = acc_mat.at<int>(start);
					}
				}

				// 정해준 영역에서 가장 큰 값만 남기고, 나머지는 0으로 변환
				Rect rect(center, size);
				if (c_value >= maxVal)
				{
					acc_dst.at<int>(center) = c_value;
					acc_mat(rect).setTo(0);
				}
			}
		}
	}
}

// 아까와 반대의 메커니즘
// 아까는 실좌표계에서 허프 변환 좌표계로 변환하는 과정이었다면,
// 여기서부터는 다시 마스크를 씌운 허프 변환 좌표계에서 구한 제일 가능성 높은 직선들을 다시 실좌표계로 변환하는 과정
void thres_lines(Mat acc_dst, Mat& lines, double _rho, double theta, int thresh)
{
	for (int r = 0; r < acc_dst.rows; r++) {
		for (int t = 0; t < acc_dst.cols; t++)
		{
			float value = (float)acc_dst.at<int>(r, t);			// 누적값
			if (value >= thresh)								// 직선 길이 임계값
			{
				float rho = (float)((r - acc_dst.rows / 2) * _rho); // 수직거리 -> 위에 과정과 반대, 아까 acc_dst.rows / 2를 더했던 이유는 rho값이 음수가 될 수 있기 때문에
																	// 더했지만, 이번에는 반대로 실좌표계이므로 음수가 나와도 되기에 빼는 것. 또한, 다시 _rho를 아까는 곱했다면 이번에는 다시 나누기
				float radian = (float)(t * theta); // 각도

				Matx13f line(rho, radian, value); 				// 단일 직선
				lines.push_back((Mat)line);
			}
		}
	}
}

// lines에는 라디안(theat)값과 로우값들이 있음 -> 위에서 벡터 형태로 쌓았기 때문에
// sorting을 하는 이유는, 여러 개의 직선이 생기는 데, 그 중 몇 개만 추출하고 싶을 때를 위해서
void sort_lines(Mat lines, vector<Vec2f>& s_lines)
{
	Mat acc = lines.col(2), idx;
	sortIdx(acc, idx, SORT_EVERY_COLUMN + SORT_DESCENDING);

	for (int i = 0; i < idx.rows; i++)
	{
		int id = idx.at<int>(i);
		float rho = lines.at<float>(id, 0);		// 로우 값
		float radian = lines.at<float>(id, 1); // 라디안 값
		s_lines.push_back(Vec2f(rho, radian)); // 정렬된 직선 벡터 - 누적값 내림차순
	}
}

void houghLines(Mat src, vector<Vec2f>& s_lines, double rho, double theta, int thresh)
{
	Mat  acc_mat, acc_dst, lines;
	hough_coord(src, acc_mat, rho, theta);
	acc_mask(acc_mat, acc_dst, Size(3, 7), thresh);

	thres_lines(acc_dst, lines, rho, theta, thresh);
	sort_lines(lines, s_lines);
}

void draw_houghLines(Mat image, Mat& dst, vector<Vec2f> lines, int nline)
{
	if (image.channels() == 3)	image.copyTo(dst);
	else 						cvtColor(image, dst, COLOR_GRAY2BGR);

	for (int i = 0; i < min((int)lines.size(), nline); i++)
	{
		float rho = lines[i][0], theta = lines[i][1];
		double a = cos(theta), b = sin(theta);

		Point2d delta(1000 * -b, 1000 * a);
		Point2d pt(a * rho, b * rho);
		line(dst, pt + delta, pt - delta, Scalar(0, 255, 0), 1, LINE_AA);
	}
}

int main()
{
	Mat image = imread("../image/hough_test.jpg", 0);
	CV_Assert(image.data);

	Mat canny, dst1, dst2;
	GaussianBlur(image, canny, Size(5, 5), 2, 2);
	Canny(canny, canny, 100, 150, 3);

	double rho = 1, theta = CV_PI / 180;

	vector<Vec2f> lines1, lines2;
	houghLines(canny, lines1, rho, theta, 50);
//	HoughLines(canny, lines2, rho, theta, 50);
	draw_houghLines(canny, dst1, lines1, 10);
//	draw_houghLines(canny, dst2, lines2, 10);

	imshow("source", image);
	imshow("canny", canny);
	imshow("detected lines", dst1);
//	imshow("detected lines_OpenCV", dst2);
	waitKey();
}
```

#### 11.2.1.2. 허프 변환 심화 예제
멀티 하네스의 기울기 보정
```cython
// 심화 예제 - 멀티 하네스의 전처리
#include "hough.hpp"

// 가장 큰 객체 사각형 검색
void max_object(Mat img, Rect &rect)
{
	vector<vector<Point>> contours;
	findContours(img.clone(), contours, RETR_EXTERNAL, CHAIN_APPROX_SIMPLE);	// 외곽선 검출

	int max_area = 0 ;
	for (int i = 0; i < (int)contours.size(); i++)	// 가장 큰 영역 가져오기
	{
		Rect r = boundingRect(contours[i]);			// 외곽선 영역 포함 사각형

		// 최대 크기 사각형 찾기
		if (max_area < r.area()) {
			max_area = r.area();
			rect = r;
		}
	}
	rect = rect - Point(10, 10) + Size(20, 20);
}

void main()
{
	Rect  rect;
	Mat	gray, canny, morph, th_gray, canny_line, dst;
	double rho = 1, theta = CV_PI / 180;				// 허프변환 거리간격, 각도간격
	vector<Vec2f> lines;								// 허프 검출 라인들

	Mat  image = imread("../image/5.tif" , 1);
	CV_Assert(image.data);

	cvtColor(image, gray, CV_BGR2GRAY);						// 명암도 영상 변환
	threshold(gray, th_gray, 240, 255, THRESH_BINARY);		//이진 영상 변환
	erode(th_gray, morph, Mat(), Point(-1, -1), 2);			// 침식 연산
	imshow("morph", morph);

	max_object(morph, rect);								// 가장 큰 객체 검색
	rectangle(morph, rect, Scalar(100), 2);					// 검색 객체 표시

	// 커넥터 영역만 캐니 에지 수행
	Canny(th_gray(rect), canny, 40, 100);
	houghLines(canny, lines, rho, theta, 50);
	draw_houghLines(canny, canny_line, lines, 1);

	double angle = (CV_PI - lines[0][1]) * 180 / CV_PI; // lines[0][1]에는 theta값이 들어있음 -> 회전 각은 왜 이렇게 되나?
	Point  center = image.size() / 2;
	Mat rot_map = getRotationMatrix2D(center, -angle, 1);

	warpAffine(image, dst, rot_map, image.size(), INTER_LINEAR);
	
	imshow("morph", morph);
	imshow("image", image);
	imshow("line", canny_line);
	imshow("dst", dst);

	resizeWindow("line", 150, 150);
	waitKey();
}

//resize(gray, resize_gray, Size(), scale, scale);			// 영상 축소
//Canny(resize_gray, canny, 40, 100);						// 케니 에지 검출
//dilate(canny, morph, Mat(), Point(-1, -1), 2);			// 팽창 연산 2번 수행

//max_object(morph, rect);									// 가장 큰 객체 추출

//// 검출 하네스 사각형 원본 크기 복원
//Point pt1 = rect.tl() / scale;
//Point pt2 = rect.br() / scale;
//Rect	harness_rect(pt1, pt2);					// 하네스 사각형
//Mat	harness_object = gray(harness_rect);	// 영상 가져옴



/*
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;

void cornerharris(Mat image, Mat& corner, int bSize, int ksize, float k)
{
	Mat dx, dy, dxy, dx2, dy2;
	corner = Mat(image.size(), CV_32F, Scalar(0));

	vector<int> a;

	Sobel(image, dx, CV_32F, 1, 0, ksize);
	Sobel(image, dy, CV_32F, 0, 1, ksize);

	multiply(dx, dx, dx2);
	multiply(dy, dy, dy2);
	multiply(dx, dy, dxy);

	// 미분 행렬 제곱
	Size msize(5, 5);
	GaussianBlur(dxy, dxy, msize, 0);
	GaussianBlur(dx2, dx2, msize, 0);
	GaussianBlur(dy2, dy2, msize, 0);

	for (int i = 0; i < image.rows; i++) {
		for (int j = 0; j < image.cols; j++)
		{
			float  a = dx2.at<float>(i, j);
			float  b = dy2.at<float>(i, j);
			float  c = dxy.at<float>(i, j);
			corner.at<float>(i, j) = (a * b - c * c) - k * (a + b) * (a + b);
		}
	}
}

Mat draw_coner(Mat corner, Mat image, int thresh)
{
	int cnt = 0;
	normalize(corner, corner, 0, 100, NORM_MINMAX, CV_32FC1, Mat());

	for (int i = 1; i < corner.rows - 1; i++) {
		for (int j = 1; j < corner.cols - 1; j++)
		{
			float cur = (int)corner.at<float>(i, j);
			if (cur > thresh)
			{
				if (cur > corner.at<float>(i-1, j) &&
					cur > corner.at<float>(i, j-1) &&
					cur > corner.at<float>(i+1, j) &&
					cur > corner.at<float>(i, j+1))
				{
					circle(image, Point(j, i), 2, Scalar(255, 0, 0), -1);
					cnt++;
				}
			}
		}
	}
	cout << "코너수: " << cnt << endl;
	return image;
}

Mat image, corner1, corner2;

void cornerHarris_demo(int  thresh, void*)
{
	Mat img1 = draw_coner(corner1, image.clone(), thresh);
	Mat img2 = draw_coner(corner2, image.clone(), thresh);

	imshow("img1-User harris", img1);
	imshow("img2-OpenCV harris", img2);
}

int main()
{
	image = imread("../image/harris_test.jpg", 1);			// 컬러 영상입력
	CV_Assert(image.data);

	int blockSize = 4;
	int apertureSize = 3;
	double k = 0.04;
	int  thresh = 20;
	Mat gray;

	cvtColor(image, gray, CV_BGR2GRAY);
	cornerharris(gray, corner1, blockSize, apertureSize, k); 	// 직접 구현 함수
	cornerHarris(gray, corner2, blockSize, apertureSize, k);	// OpenCV 제공 함수

	cornerHarris_demo(0, 0);
	createTrackbar("Threshold: ", "img1-User harris", &thresh, 100, cornerHarris_demo);
	waitKey();
}
```

#### 11.2.1.3. 코너 검출






