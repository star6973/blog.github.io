---
layout: post
title: OpenCV Exercise 5
date: 2020-06-15 10:00:00-19:00:00
categories: OpenCV
tag: OpenCV
---

/*
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;
int main()
{
	Mat  m1(3, 5, CV_32SC1);
	Mat  m2(3, 5, CV_32FC1);
	Mat  m3(3, 5, CV_8UC2);
	Mat  m4(3, 5, CV_32SC3);

	for (int i = 0, k = 0; i < m1.rows; i++) {
		for (int j = 0; j < m1.cols; j++, k++)
		{
			// 방법 1
			m1.at<int>(i, j) = k;

			// 방법 2
			Point pt(j, i);
			m2.at<float>(pt) = (float)j;

			// 방법 3
			int idx[2] = { i, j };
			m3.at<Vec2b>(idx) = Vec2b(0, 1);

			// 방법 4
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
*/


/*
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
*/


/*
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
*/

/*
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;
int main()
{
	Mat image1(50, 512, CV_8UC1, Scalar(0));
	Mat image2(50, 512, CV_8UC1, Scalar(0));

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
*/


/*
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
	Mat roi_img = image(roi);
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
*/


/*
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;
int main()
{
	Mat image = imread("../image/bright.jpg", IMREAD_GRAYSCALE);
	CV_Assert(!image.empty());

	Mat dst1 = image + 100; // opencv가 자체적으로 255가 넘어가면 자동으로 잘라주어 제일 깔끔하게 나옴
	Mat dst2 = image - 100; 
	Mat dst3 = 255 - image;

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
*/

/*
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
*/

/*
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
*/

/*
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

void  calc_Histo(const Mat & image, Mat & hist, int bins, int range_max = 256)
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
	calc_Histo();

	cout << hist.t() << endl;

	imshow("image", image);
	waitKey();
	return 0;
}
*/


/*
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

*/



/*
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
		if (hist.at<float>(idx) > 0)	return idx;
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
	int high_value = (int)(search_valueIdx(hist, hist.rows) * bin_width);
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
*/



// 클라이
// 히스토그램의 평활화를 하면 거의 좋아지지만, 어떤 경우 더 안좋아지는 경우가 있다.
// 이를 해결해줄 수 있는 방법인 클라이(CLAHE)
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

	dst1 = Mat(image.size(), CV_8U);
	
	for (int i = 0; i < image.rows; i++) {
		for (int j = 0; j < image.cols; j++) {
			int idx = image.at<uchar>(i, j);
			dst1.at<float>(i, j) = (uchar)(accum_hist.at<float>(idx)); // 교체
		}
	}
	
	equalizeHist(image, dst2);	// 히스토그램 평활화

	// 4단계. 입력 영상에서 픽셀값 i를 정규화된 값 n[i]로 변환하여 결과 영상 생성
	create_hist(dst1, hist, hist_img1);			// 히스토그램 및 그래프 그리기
	create_hist(dst2, hist, hist_img2);

	imshow("image", image), imshow("img_hist", hist_img);	// 원본 히스토그램
	imshow("dst1-User", dst1), imshow("User_hist", hist_img1);		// 사용자 평활화 
	imshow("dst2-OpenCV", dst2), imshow("OpenCV_hist", hist_img2);// OpenCV 평활화
	waitKey();
	return 0;
}
