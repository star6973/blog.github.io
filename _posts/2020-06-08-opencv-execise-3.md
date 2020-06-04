---
layout: post
title:  OpenCV Exercise 3
date:   2020-06-08 09:00:00-18:00:00
categories: OpenCV
tag: OpenCV
---

// 4.1. 이미지 파일 처리
/*
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;

void  print_matInfo(string name, Mat img)
{
  string str;
  int depth = img.depth();

  if (depth == CV_8U)	 str = "CV_8U";
  else if (depth == CV_8S)	 str = "CV_8S";
  else if (depth == CV_16U) str = "CV_16U";
  else if (depth == CV_16S)	 str = "CV_16S";
  else if (depth == CV_32S) str = "CV_32S";
  else if (depth == CV_32F) str = "CV_32F";
  else if (depth == CV_64F) str = "CV_64F";

  cout << name;
  cout << format(": depth(%d) channels(%d) -> 자료형: ", depth, img.channels());
  cout << str << "C" << img.channels() << endl;
}

int main()
{
  string filename1 = "../image/read_gray.jpg";
  Mat gray2gray = imread(filename1, IMREAD_GRAYSCALE); // 채널이 1개
  Mat gray2color = imread(filename1, IMREAD_COLOR); // RGB가 들어가서, 채널이 3개
  CV_Assert(gray2gray.data && gray2color.data); // 데이터를 읽어서 파일이 있는지 없는지 Assert 에러 확인

  Rect roi(100, 100, 5, 5); // 5 x 5
  cout << "행렬 좌표 (100,100) 화소값 " << endl;
  cout << "gray2gray " << gray2gray(roi) << endl;
  cout << "gray2color " << gray2color(roi) << endl << endl;

  print_matInfo("gray2gray", gray2gray);
  print_matInfo("gray2color", gray2color);
  imshow("gray2gray", gray2gray);
  imshow("gray2color", gray2color);
  waitKey(0);
  return 0;
}
*/

#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;

void  print_matInfo(string name, Mat img)
{
  string str;
  int depth = img.depth();

  if (depth == CV_8U)	 str = "CV_8U";
  else if (depth == CV_8S)	 str = "CV_8S";
  else if (depth == CV_16U) str = "CV_16U";
  else if (depth == CV_16S)	 str = "CV_16S";
  else if (depth == CV_32S) str = "CV_32S";
  else if (depth == CV_32F) str = "CV_32F";
  else if (depth == CV_64F) str = "CV_64F";

  cout << name;
  cout << format(": depth(%d) channels(%d) -> 자료형: ", depth, img.channels());
  cout << str << "C" << img.channels() << endl;
}

int main()
{
  string filename = "../image/read_color.jpg";
  Mat color2gray = imread(filename, IMREAD_GRAYSCALE);
  Mat color2color = imread(filename, IMREAD_COLOR);
  CV_Assert(color2gray.data && color2color.data);

  Rect roi(100, 100, 5, 5); // (100, 100)위치에서 5x5의 사각형을 추출(추출된 화소)
  cout << "행렬 좌표 (100,100) 화소값 " << endl;
  cout << "color2gray " << color2gray(roi) << endl; // 한 화소값 표시(부분 행렬 원소 출력)
  cout << "color2color " << color2color(roi) << endl;

  print_matInfo("color2gray", color2gray);
  print_matInfo("color2color", color2color);
  imshow("color2gray", color2gray);
  imshow("color2color", color2color);
  waitKey(0);
  return 0;
}