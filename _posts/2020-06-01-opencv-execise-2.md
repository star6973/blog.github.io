---
layout: post
title:  OpenCV Exercise 2
date:   2020-06-01 09:00:00-18:00:00
categories: OpenCV
tag: OpenCV
---

## 3. OpenCV의 기본 자료 구조 및 실습[2]
### 3.8. Mat 클래스
- 단일 채널 혹은 다중 채널의 값
- 실수나 복소수로 구성된 벡터
- 명암도 영상이나 컬러 영상 데이터
- 점의 집합
- 히스토그램 데이터

```angular2
#include <opencv2/highgui.hpp>
#include <iostream>
using namespace cv;
using namespace std;

void main() {


  CV_8U  : 8-bit unsigned integer: uchar ( 0..255 )
  CV_8S  : 8-bit signed integer: schar ( -128..127 )
  CV_16U : 16-bit unsigned integer: ushort ( 0..65535 )
  CV_16S : 16-bit signed integer: short ( -32768..32767 )
  CV_32S : 32-bit signed integer: int ( -2147483648..2147483647 )
  CV_32F : 32-bit floating-point number: float ( -FLT_MAX..FLT_MAX, INF, NAN )
  CV_64F : 64-bit floating-point number: double ( -DBL_MAX..DBL_MAX, INF, NAN )


  // Mat 객체 선언 방법
  Mat m1(2, 3, CV_8U, Scalar(0));
  cout << "[m1] = \n" << m1 << endl;
  cout << endl;

  Mat m2(2, 3, CV_8U, Scalar(300)); // 자료형을 8bit unsigned char형으로 선언했기 때문에, 최대가 255로 나옴
  cout << "[m2] = \n" << m2 << endl;
  cout << endl;

  Mat m3(2, 3, CV_16S, Scalar(300)); // 자료형을 16bit short형으로 선언했기 때문에, 최대가 65535이므로 300까지 제대로 나옴
  cout << "[m3] = \n" << m3 << endl;
  cout << endl;

  float data[] = {
    1.2f, 2.3f, 3.2f,
    4.5f, 5.f, 6.5f,
  };

  Mat m4(2, 3, CV_32F, data);
  cout << "[m4] = \n" << m4 << endl;
  cout << endl;

  // Size_ 객체로 Mat 객체 선언 방법
  Size sz(2, 3); // Size는 기본적으로 (width, height)이므로, 행렬로 보면 3 X 2 행렬이 됨
  cout << "size = " << sz << endl;
  cout << endl;

  Mat m5(Size(2, 3), CV_64F);
  Mat m6(sz, CV_32F, data);

  cout << "[m5] = \n" << m5 << endl;
  cout << endl;

  cout << "[m6] = \n" << m6 << endl;
  cout << endl;


  // 단위행렬이나 1로 구성된 행렬 등의 특수한 행렬을 생성하는 함수들
  1. ones()	: 행렬의 모든 원소 1인 행렬을 반환
  2. eye()	: 지정된 크기와 타입의 단위 행렬을 반환
  3. zeros()	: 행렬의 원소를 0으로 초기화


  Mat m7 = Mat::ones(3, 5, CV_8UC1);
  cout << "[m7] = \n" << m7 << endl;
  cout << endl;

  Mat m8 = Mat::zeros(Size(5, 3), CV_8UC1);
  cout << "[m8] = \n" << m8 << endl;
  cout << endl;

  Mat m9 = Mat::eye(3, 3, CV_8UC1);
  cout << "[m9] = \n" << m9 << endl;
  cout << endl;

}
```

## 3.9. Mat_ 클래스

```angular2
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;
int main()
{
  double a = 32.12345678, b = 2.7;
  short  c = 400;
  float  d = 10.f, e = 11.f, f = 13.f;

  Mat_<int> 	m1(2, 4);
  Mat_<uchar> 	m2(3, 4, 100);
  Mat_<short>   	m3(4, 5, c);

  m1 << 1, 2, 3, 4, 5, 6;
  Mat m4 = (Mat_<double>(2, 3) << 1, 2, 3, 4, 5, 6);
  Mat m5 = (Mat_<float>(2, 3) << a, b, c, d, e, f);

  cout << "[m1]=" << endl << m1 << endl;
  cout << "[m2]=" << endl << m2 << endl << endl;
  cout << "[m3]=" << endl << m3 << endl << endl;
  cout << "[m4]=" << endl << m4 << endl;
  cout << "[m5]=" << endl << m5 << endl;
  return 0;
}
```

## 3.10. Matx 클래스
```angular2
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;
int  main()
{
  // 기본 선언 방법
  Matx<int, 3, 3> 	 m1(1, 2, 3, 4, 5, 6, 7, 8, 9);
  Matx<float, 2, 3>	 m2(1, 2, 3, 4, 5, 6);
  Matx<double, 3, 5>	 m3(3, 4, 5, 7);

  cout << "[m1] = " << endl << m1 << endl;
  cout << endl;

  cout << "[m2] = " << endl << m1 << endl;
  cout << endl;

  cout << "[m3] = " << endl << m1 << endl;
  cout << endl;

  // 간편 선언 방법
  Matx23d  m4(3, 4, 5, 6, 7, 8);
  m4 << 1, 2, 3, 4, 5, 6;
  cout << "[m4] = " << endl << m4 << endl;
  cout << endl;

  Matx34d  m5(1, 2, 3, 10, 11, 12, 13, 14, 15); // 초기화되지 않은 값은 자동으로 0으로 할당
  Matx66d m6(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);

  // 행렬 원소 접근
  cout << "m5(0, 0) =" << m5(0, 0) << " m5(1, 0) =" << m5(1, 0) << endl;
  cout << endl;

  cout << "m6(0, 1) =" << m6(0, 1) << " m6(1, 3) =" << m6(1, 3) << endl << endl;
  cout << endl;


  cout << "[m5] = " << endl << m5 << endl;
  cout << endl;
  cout << "[m6]= " << endl << m6 << endl;
  cout << endl;

  return 0;
}
```

## 3.11. Mat 클래스의 다양한 속성

    Mat::dims			// 차원 수
    Mat::rows			// 행의 개수
    Mat::cols			// 열의 개수
    Mat::data			// 행렬 원소 데이터에 대한 포인터
    Mat::step			// 행렬의 한 행이 차지하는 바이트 수
    Mat::channels()		// 행렬의 채널 수 반환
    Mat::depth()		// 행렬의 깊이값 반환
    Mat::elemSize()		// 행렬의 한 원소에 대한 바이트 크기 반환
    Mat::elemSize1()	// 행렬의 한 원소의 한 채널에 대한 바이트 크기 반환
    Mat::empty()		// 행렬 원소가 비어있는지 여부 반환
    Mat::isSubmatrix()	// 참조 행렬인지 여부 반환
    Mat::size()			// 행렬의 크기를 Size형으로 반환
    Mat::step1()		// step을 elemSize1()로 나누어서 정규화된 step 반환
    Mat::total()		// 행렬 원소의 전체 개수 반환
    Mat::type()			// 행렬의 데이터 타입(짜료형 + 채널 수) 반환

```angular2
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;
int main()
{
  Mat m1(4, 3, CV_32FC3);
  cout << "차원 수 = " << m1.dims << endl;
  cout << "행 개수 = " << m1.rows << endl;
  cout << "열 개수 = " << m1.cols << endl;
  cout << "행렬 크기 = " << m1.size() << endl << endl;

  // depth는 자료형을 구분하기 위한 상수

  CV_8U 		depth 0	unsigned char
  CV_8S 		depth 1	char
  CV_16U		depth 2	unsigned short int
  CV_16S		depth 3	short int
  CV_32S		depth 4	int
  CV_32F		depth 5	float
  CV_64F		depth 6	double


  cout << "depth(자료형을 구분) = " << m1.depth() << endl; // CV_32FC3를 반환
  cout << "전체 원소 개수 = " << m1.total() << endl;
  cout << "한 원소의 바이트 크기 = " << m1.elemSize() << endl;
  cout << "채널당 한 원소의 바이트 크기 = " << m1.elemSize1() << endl << endl;

  // 1 채널의 크기 = 4byte

  return 0;
}
```

## 3.12. Mat 클래스의 할당 연산자(=)

    m1 = m2
    
    m2 행렬이 m1 행렬에 복사되는 것이 아니라 m2 행렬을 m1 행렬이 공유한다.
    따라서 m2 행렬의 원소가 변경되면, m1 행렬의 원소도 같이 변경된다.

```angular2
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;
int main()
{
  Mat m1(2, 3, CV_8U, 2);
  Mat m2(2, 3, CV_8U, 10);

  // 행렬 연산
  Mat m3 = m1 + m2;
  Mat m4 = m1 * 2;
  Mat m5 = m1;

  cout << "[m2] =" << endl << m2 << endl;
  cout << "[m3] =" << endl << m3 << endl;
  cout << "[m4] =" << endl << m4 << endl << endl;

  // 공유 행렬 처리
  cout << "[m1] =" << endl << m1 << endl;
  cout << "[m5] =" << endl << m5 << endl << endl;

  // 포인터처럼 행렬의 값을 참조(복사 x)
  m1 = m2 * 10;

  cout << "[m1] =" << endl << m1 << endl;
  cout << "[m5] =" << endl << m5 << endl;
  return 0;
}
```

## 3.13. Mat 클래스의 크기 및 형태 변경 - I

    resize(): 행의 개수를 기준으로 기존 행렬의 크기를 변경한다.
              기존 행렬의 개수보다 size가 작으면 하단 행을 제거하고, 크면 하단 행을 추가한다.
    
    reshape(): 행렬 전체 원소 개수는 바뀌지 않으면서, 행렬의 모양을 변경하여 새 행렬을 반환한다.
               기존 행렬과 변경된 행렬의 전체 원소 개수(채널 수 x 행 수 x 열 수)가 일치하지 않으면 에러가 발생한다.
    
    create(): 기존에 존재하는 행렬의 차원, 행, 열, 자료형을 변경하여 다시 생성한다.
              기존 행렬과 크기와 자료형이 다르면 기존 메모리를 해제하고 새로운 데이터를 생성한다.

```angular2
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;
int main()
{
  Mat m = (Mat_<uchar>(2, 4) << 1, 2, 3, 4, 5, 6, 7, 8);
  cout << "[m] = " << endl << m << endl << endl;

  m.resize(1); // 1행으로 변경
  cout << "m.resize(1) = " << endl << m << endl;

  m.resize(3); // 3행으로 변경, 추가되는 값을 지정하지 않아 임의의 값으로 지정됨
  cout << "m.resize(3) = " << endl << m << endl << endl;

  m.resize(5, Scalar(50)); // 5행으로 변경, 추가된 값들을 50으로 지정
  cout << "m.resize(5) = " << endl << m << endl;

  return  0;
}
```


## 3.14. Mat 클래스의 크기 및 형태 변경 - II
```angular2
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;

void print_matInfo(string m_name, Mat m)
{
  cout << "[ " << m_name << " 행렬]" << endl;
  cout << "    채널수  = " << m.channels() << endl;
  cout << "    행 x 열 = " << m.rows << " x " << m.cols << endl << endl;
}

int main()
{

  // reshape(채널, 행, 열)

  Mat m1(2, 6, CV_8U);	   // 채널: 1, 행: 2, 열: 6
  Mat m2 = m1.reshape(2);    // 채널: 2로 변환, 행: 2, 열: 3로 변환(원소의 개수가 유지되어야 하므로)
  Mat m3 = m1.reshape(3, 2); // 채널: 3로 변환, 행: 2, 열: 2로 변환(원소의 개수가 유지되어야 하므로)

  print_matInfo("m1(2, 6)", m1);
  print_matInfo("m2 = m1_reshape(2)", m2); 
  print_matInfo("m3 = m1_reshape(3, 2)", m3);

  m1.create(3, 5, CV_16S);   // 완전히 새로운 값을 생성
  print_matInfo("m1.create(3, 5)", m1);
  return  0;
}
```

## 3.15. Mat 복사 및 자료형 변환

    영상처리, 컴퓨터 비전 응용 시 원본 행렬 복사가 빈번히 발생한다.
    
    Mat clone()		:	행렬 데이터와 같은 값을 복사해서 새로운 행렬을 반환한다.
    void copyTo()	:	행렬 데이터를 인자로 입력된 mat 행렬에 복사한다.
    
        - copyTo(Mat &mat, Mat mask)
        - mat: 복사될 목적 행렬
        - mask: 연산 마스크(mask 행렬의 원소가 0이 아닌 위치만 복사가 수행됨)
    
    void converTo()	:	행렬 원소의 데이터 타입을 변경하여 인수로 입력된 mat 행렬에 반환한다.
    
        - converTo(Mat &mat, int type, double alpha=1, double beta=0)
        - mat: 데이터 타입이 변경될 목적 행렬
        - type: 변경하고자 하는 데이터 타입(CV_8U, CV_16S, ...)
        - alpha: 원본 행렬의 원소값의 배율 지정
        - beta: 원본 행렬의 원소값에 대한 이동값

```angular2
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;
int main()
{
  double data[] = {
    1.1, 2.2, 3.3, 4.4,
    5.5, 6.6, 7.7, 8.9,
    9.9, 10, 11, 12
  };
  Mat m1(3, 4, CV_64F, data);
  Mat m2 = m1.clone();	 // m1 행렬을 복사한 후, m2에 저장

  Mat m3, m4;
  m1.copyTo(m3);			 // m1 행렬을 복사한 후, m3에 저장
  m1.convertTo(m4, CV_8U); // m1 행렬의 데이터 타입을 uchar 행렬로 형변환 후, m4에 저장

  cout << "[m1] =\n" << m1 << endl;
  cout << "[m2] =\n" << m2 << endl;
  cout << "[m3] =\n" << m3 << endl;
  cout << "[m4] =\n" << m4 << endl;
  return 0;
}
```

## 3.16. vector 클래스
```angular2
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;

int main() {

  vector<Point> v1;
  v1.push_back(Point(10, 20));
  v1.push_back(Point(20, 30));
  v1.push_back(Point(50, 60));

  cout << "[v1] = \n" << (Mat)v1 << endl << endl; // 벡터는 Mat 형변환을 해야 출력이 가능하다

  vector<float> v2(3, 9.25);
  Size arr_size[] = { Size(2, 2), Size(3, 3), Size(4, 4) }; // 3행, 2열
  int arr_int[] = { 10, 20, 30, 40, 50 };

  cout << "[v2] = \n" << (Mat)v2 << endl << endl;
  cout << "[arr_size] = \n" << arr_size << endl << endl;
  cout << "[arr_int] = \n" << arr_int << endl << endl;

  // 배열 원소로 벡터 초기화
  vector<Size> v3(arr_size, arr_size + sizeof(arr_size) / sizeof(Size));
  vector<int> v4(arr_int + 2, arr_int + sizeof(arr_int) / sizeof(int));

  cout << "[v3] = \n" << (Mat)v3 << endl << endl; // 현재 3행 2열로
  cout << "reshape [v3] = \n" << ((Mat)v3).reshape(1, 1) << endl << endl; // reshape으로 (1채널, 1행, 6열)으로 변환

  cout << "[v4] = \n" << (Mat)v4 << endl << endl;
  cout << "reshape [v4] = \n" << ((Mat)v4).reshape(1, 1) << endl << endl;

}
```

## 3.17. vector 클래스의 사용 - I

    1. 벡터 원소 접근 - 배열처럼 첨자 연산자([]) 이용
    2. 벡터 할당된 용량 확인 - vector::capacity()
    3. 벡터 메모리 확보 - vector::reserve()

```angular2
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;

void print_vectorInfo(string v_name, vector<int> v)
{
  cout << "[ " << v_name << " ] = ";
  if (v.empty()) 	cout << "벡터가 비어있습니다." << endl;
  else			cout << ((Mat)v).reshape(1, 1) << endl;

  cout << ".size() = " << v.size() << endl;
}

int main()
{
  int  arr[] = { 10, 20, 30, 40, 50 };
  vector<int>  v1(arr, arr + sizeof(arr) / sizeof(int)); // 포인터 형태와 비슷(arr는 arr배열의 초기값, sizeof(arr)/sizeof(int)는 배열의 크기)

  print_vectorInfo("v1", v1);
  cout << ".capacity() = " << v1.capacity() << endl << endl; // 할당된 용량 확인(처음에는 5)

  // 함수가 호출될 때마다 벡터의 용량이 재할당됨 -> 용량을 미리 설정하여 재할당이 안일어나도록 할 수 있음(reserve)
  v1.insert(v1.begin() + 2, 100); // 2 번째 원소의 앞에다 insert
  print_vectorInfo("v1, insert(2) ", v1);
  cout << ".capacity() = " << v1.capacity() << endl << endl; // 할당된 용량 확인(재할당 발생으로 용량 2 증가하여 7)

  v1.erase(v1.begin() + 3);
  print_vectorInfo("v1, erase(3) ", v1); // 앞에서부터 3 번째 원소(배열은 0부터 시작하므로, 4 번째)를 삭제
  cout << ".capacity() = " << v1.capacity() << endl << endl;

  v1.erase(v1.begin() + 3);
  print_vectorInfo("v1, erase(3) ", v1);
  cout << ".capacity() = " << v1.capacity() << endl << endl;

  v1.clear(); // 전체 삭제
  print_vectorInfo("v1, clear() ", v1);

  return 0;
}
```

## 3.18. vector 클래스의 사용 - II
```angular2
#include <opencv2/opencv.hpp>
#include <time.h>
using namespace cv;
using namespace std;
int main()
{
  vector<double> v1, v2;
  v1.reserve(10000000); // 벡터 메모리 할당

  double start_time = clock();
  for (int i = 0; i < v1.capacity(); i++) {
    v1.push_back(i); // 할당된 용량만큼 원소값을 삽입
  }
  printf("reserve() 사용 %5.2f ms\n", (clock() - start_time));

  start_time = clock();
  for (int i = 0; i < v1.capacity(); i++) {
    v2.push_back(i); // v2는 할당되지 않았기에, 할당된 용량만큼 원소값을 삽입하면 남는 값이 생김(double형은 최대가 10^127이므로)
  }
  printf("reserve() 미사용 %5.2f ms\n", (clock() - start_time));
  return 0;
}
```

## 3.19. Mat의 원소 추가 및 삭제
```angular2
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;

void  print_matInfo(string name, Mat m)   	// 행렬의 정보와 원소 출력 함수
{
  string mat_type;
  if (m.depth() == CV_8U)	 		mat_type = "CV_8U";
  else if (m.depth() == CV_8S)	mat_type = "CV_8S";
  else if (m.depth() == CV_16U) 	mat_type = "CV_16U";
  else if (m.depth() == CV_16S)	mat_type = "CV_16S";
  else if (m.depth() == CV_32S) 	mat_type = "CV_32S";
  else if (m.depth() == CV_32F)	mat_type = "CV_32F";
  else if (m.depth() == CV_64F)	mat_type = "CV_64F";

  cout << name << " 크기 " << m.size() << ", ";
  cout << " 자료형 " << mat_type << "C" << m.channels() << endl;
  cout << m << endl << endl;
}

int main()
{
  Mat  m1, m2, m3, m4(2, 6, CV_8UC1);       // m1, m2, m3는 모두 1채널, 0행, 0열(초기화를 안했기에)
                        // m4는 uchar 1채널, 2행, 6열
  Mat  add1(2, 3, CV_8UC1, Scalar(100));    // uchar 1채널, 2행, 3열
  Mat  add2 = (Mat)Mat::eye(4, 3, CV_8UC1); // uchar 1채널, 4행, 3열
                        // 정방행렬이 아닌 대각행렬의 경우, 정방행렬인 곳은 대각행렬로, 나머지는 0으로 
  cout << "[add1] = " << add1 << endl << endl;
  cout << "[add2] = " << add2 << endl << endl;
  cout << "[m1] = " << m1.channels() << ", " << m1.rows << ", " << m1.cols << endl << endl;
  cout << "[m2] = " << m2.channels() << ", " << m2.rows << ", " << m2.cols << endl << endl;
  cout << "[m3] = " << m3.channels() << ", " << m3.rows << ", " << m3.cols << endl << endl;
  cout << "[m4] = " << m4.channels() << ", " << m4.rows << ", " << m4.cols << endl << endl;

  m1.push_back(100), m1.push_back(200);
  m2.push_back(100.5), m2.push_back(200.6);

  m3.push_back(add1);
  m3.push_back(add2);

  //m4.push_back(add1); // 행렬의 행과 열이 다르면 행렬에 행렬을 넣을 수 없다 -> reshape를 이용해 변환해서 사용 가능
  //m4.push_back(100);  // m4는 2행 6열이므로, add1은 2행 3열이기 때문에

  m4.push_back(add1.reshape(1, 1)); // 원래 add1(1채널, 2행, 3열) -> reshape add1(1채널, 1행, 6열)
  m4.push_back(add2.reshape(1, 2)); // 원래 add2(1채널, 4행, 3열) -> reshape add2(1채널, 2행, 6열)

  print_matInfo("m1", m1), print_matInfo("m2", m2);
  print_matInfo("m3", m3), print_matInfo("m4", m4);

  m1.pop_back(1); // pop하기 전에 m1은 [1x2], 마지막 원소(뒤에서부터) 1개 제거
  m2.pop_back(2); // pop하기 전에 m2는 [1x2], 마지막 원소(뒤에서부터) 2개 제거
  m3.pop_back(3); // pop하기 전에 m3는 [3x6], 마지막 원소(뒤에터부터) 3개 제거

  cout << "m1" << endl << m1 << endl;
  cout << "m2" << endl << m2 << endl;
  cout << "m3" << endl << m3 << endl;

  return 0;
}
```

## 3.20. Mat 행렬의 메모리 해제
```angular2
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;
void  print_matInfo(string name, Mat m)   	// 행렬의 정보와 원소 출력 함수
{
  string mat_type;
  if (m.depth() == CV_8U)	 		mat_type = "CV_8U";
  else if (m.depth() == CV_8S)	mat_type = "CV_8S";
  else if (m.depth() == CV_16U) 	mat_type = "CV_16U";
  else if (m.depth() == CV_16S)	mat_type = "CV_16S";
  else if (m.depth() == CV_32S) 	mat_type = "CV_32S";
  else if (m.depth() == CV_32F)	mat_type = "CV_32F";
  else if (m.depth() == CV_64F)	mat_type = "CV_64F";

  cout << name << " 크기 " << m.size() << ", ";
  cout << " 자료형 " << mat_type << "C" << m.channels() << endl;
  cout << m << endl << endl;
}

int main()
{
  Mat m1(2, 6, CV_8UC1, Scalar(100)); // 행렬 선언
  Mat m2(3, 3, CV_32S);
  Range  r1(0, 2), r2(0, 2); // 관심영역 Range r1(0, 2) -> start는 포함, end는 포함 x -> 0, 1만
  Mat m3 = m1(r1, r2); // 관심 영역 참조

  print_matInfo("m1", m1); // 행렬 정보 출력
  print_matInfo("m3", m3);

  m2.release(); // 행렬 해제
  m3.release(); // 행렬 해제(부모 행렬 m1에는 영향이 없음)

  print_matInfo("m2", m2); // 행렬 정보 출력
  print_matInfo("m3", m3);
  print_matInfo("m1", m1);

  m1.release();
  print_matInfo("m1", m1);

  return 0;
}
```

## 3.21. 행렬 연산 함수

    Mat cross(): 두 개의 3-원소 벡터들의 외적
    double dot(): 두 벡터의 내적
    MatExpr inv(): 역행렬
    
        - MatExpr inv(int method)
        - method 1) DECOMP_LU : 가우시안 소거법(LU분해)
                 2) DECOMP_SVD : 특이치 분해 방법(역행렬이 존재하지 않아도 가능)
                 3) DECOMP_CHOLESKY : 숄레스키 분해 방법(역행렬이 존재하는 정방행렬, 대칭행렬)
        
    MatExpr mul(): 두 행렬의 각 원소 간 곱셈 수행
    MatExpr t(): 전치행렬

```angular2
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;

int main() {

  float data[] = {
    1, 0, 2,
    -3, 4, 6,
    -1, -2, 3
  };

  float data2[] = { 6, 30, 8 };

  // m1 * x = m2
  // x = m1'(역행렬) * m2
  Mat m1(3, 3, CV_32F, data);
  Mat m2(1, 3, CV_32F, data2); // Matx13f m2(6, 30, 8);

  cout << "[m1] = " << endl << m1 << endl << endl;
  cout << "[m1의 역행렬] = " << endl << m1.inv(DECOMP_LU) << endl << endl;
  cout << "[m2의 전치행렬] = " << endl << m2.t() << endl << endl;

  Mat x = m1.inv(DECOMP_LU) * m2.t();
  cout << "연립방정식의 해 x1, x2, x3 = " << x.t() << endl << endl;

  return 0;

}
```

## 3.22. saturate_cast <_Tp>

    표와 산술(saturation arithmetics) 연산
    
    - saturate_cast() 메소드
    - I(x, y) = min(max(round(r), 0), 255)
    
```angular2
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;
int main()
{
  Matx<uchar, 2, 2> m1; // 2행 2열 행렬 선언
  Matx<ushort, 2, 2> m2;
    
  // uchar 행렬에 음수 -50 및 300 저장
  // 50의 2진수: 00110010
  // -50 = 50의 2의 보수: 11001110
  // 50의 2의 보수 -> 10진수: 206
  m1(0, 0) = -50;
  m1(0, 1) = 300;
  m1(1, 0) = saturate_cast<uchar>(-50);
  m1(1, 1) = saturate_cast<uchar>(300);

  m2(0, 0) = -50;
  m2(0, 1) = 80000;
  m2(1, 0) = saturate_cast<unsigned short>(-50);
  m2(1, 1) = saturate_cast<unsigned short>(80000);

  cout << "[m1] = " << endl << m1 << endl;
  cout << "[m2] = " << endl << m2 << endl;
  return 0;
}
```

## 3.23. 예외처리 매크로

    CV_Assert()	: 실행시간에 조건을 체크하는 매크로, 조건이 false가 되면 예외발생
        - condition: 체크하려는 조건
        
    CV_Error()	: 해당 에러 코드 발생 시, msg 문자열 출력
        - code: 에러 코드 상수. 음수값을 가짐.
            1) StsOk: 오류 아님
            2) StsBadArg: 함수의 인수 오류
            3) StsNullPtr: 널 포인터 오류
            4) StsVecLengthErr: 벡터 길이 오류
            5) StsBadSize: 자료형의 크기 오류
            6) StsDivByZero: 0으로 나누기 오류
            7) StsOutOfRange: 인수가 범위를 벗어난 오류
            8) StsAssert: CV_Assert()에서 조건이 거짓일 때
            
        - msg: 해당 에러 코드 발생시, args로 포맷 매칭하여 문자열 출력
        
    CV_Error_()	: 해당 에러 코드 발생 시, args로 포맷 매칭하여 문자열 출력
        - code: 에러 코드 상수.
        - args: printf()와 비슷하게 포맷 매칭 사용

```angular2
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;

int main()
{
  string msg1 = "a is one.";
  string msg2 = "a is two.";
  string msg3 = "a is three.";
  int a;

  while (1) {
    cout << "input a : ";
    cin >> a;

    try {
      if (a == 0)
        CV_Error(Error::StsDivByZero, "a is zero.");

      if (a == 1)
        CV_Error(Error::StsBadSize, msg1);

      if (a == 2)
        CV_Error_(Error::StsOutOfRange, ("%s : %d", msg2.c_str(), a));

      if (a == 3)
        CV_Error_(Error::StsBadArg, ("%s : %d", msg3.c_str(), a));

      CV_Assert(a != 4);
    }
    catch (cv::Exception& e)
    {
      cout << "Exception code (" << e.code << "): " << e.what();
      cout << endl;
      if (e.code == Error::StsAssert)
        break;
    }
  }

  return 0;

}
```

## 3.24. 윈도우 창 제어

    - namedWindow(): 윈도우의 이름을 설정하고, 해당 이름으로 윈도우를 생성한다.
      * flag
        1) WINDOW_NORMAL: 윈도우 크기의 재조정 가능
        2) WINDOW_AUTOSIZE: 표시된 행렬의 크기에 맞춰 자동 설정
        3) WINDOW_OPENGL: OpenGL을 지원하는 윈도우 생성
    
    - imshow(): winname 이름의 윈도우에 mat 행렬을 영상으로 표시한다.
    - destroyWindow(): 인수로 지정된 타이틀의 윈도우를 파괴한다.
    - destroyAllWindows(): HighGUI로 생성된 모든 윈도우를 파괴한다.
    - moveWindow(): winname 이름의 윈도우를 지정된 위치로 (x, y)로 이동시킨다.
                    이동되는 윈도우의 기준 위치는 좌측 상단이다.
    - resizeWindow(): 윈도우의 크기를 재설정한다.

```angular2
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;
int main()
{
  Mat image1(300, 400, CV_8U, Scalar(255));
  Mat image2(300, 400, CV_8U, Scalar(100));
  string  title1 = "white창 제어";
  string  title2 = "gray 창 제어";

  namedWindow(title1, WINDOW_AUTOSIZE);
  namedWindow(title2, WINDOW_NORMAL);
  moveWindow(title1, 100, 200);
  moveWindow(title2, 300, 200);

  imshow(title1, image1);
  imshow(title2, image2);
  waitKey();
  destroyAllWindows();

  return 0;
}

#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;
int main()
{
  Mat image(300, 400, CV_8U, Scalar(255)); // 1채널 uchar 행렬 ,255로 초기화
  string  title1 = "창 크기변경1 - AUTOSIZE";
  string  title2 = "창 크기변경2 - NORMAL";

  namedWindow(title1, WINDOW_AUTOSIZE); // 창 크기 변경 불가
  namedWindow(title2, WINDOW_NORMAL);	  // 창 크기 변경 가능
  resizeWindow(title1, 500, 200);
  resizeWindow(title2, 500, 200);

  imshow(title1, image);
  imshow(title2, image);
  waitKey(); // delay(ms) 시간만큼 키 입력 대기하고, 키 이벤트 발생하면 해당 키 값을 반환
  return 0;
}
```

## 3.25. 키보드 이벤트 제어

    waitKey(): delay(ms) 시간만큼 키 입력 대기하고, 키 이벤트 발생하면 해당 키 값을 반환한다.

```angular
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;

int main() {

  Mat image(200, 300, CV_8UC1, Scalar(255));
  namedWindow("키보드 이벤트", WINDOW_AUTOSIZE);
  imshow("키보드 이벤트", image);

  while (1) {

    int key = waitKey(100);

    if (key == 27) break;
    switch (key) {

      case 97: cout << "a키 입력" << endl; break;
      case 98: cout << "b키 입력" << endl; break;
      case 65: cout << "A키 입력" << endl; break;
      case 66: cout << "B키 입력" << endl; break;

      case 0x250000: cout << "왼쪽 화살표 키 입력" << endl; break;
      case 0x260000: cout << "왼쪽 화살표 키 입력" << endl; break;
      case 0x270000: cout << "왼쪽 화살표 키 입력" << endl; break;
      case 0x280000: cout << "왼쪽 화살표 키 입력" << endl; break;

    }

  }

  return 0;

}
```

## 3.26. 마우스 이벤트 제어

    콜백 함수 - 개발자가 함수를 호출하는 것이 아니라, 
              어떤 이벤트가 발생하거나 특정 시점에 도달했을 때 시스템에서 개발자가 등록한 함수를 호출하는 방식
    
    마우스 이벤트 등록: cv::setMouseCallback()
    
    1. setMouseCallback(): 사용자가 정의한 마우스 콜백함수를 시스템에 등록하는 함수
    2. (*MouseCallback)(): 발생한 마우스 이벤트에 대해서 처리 및 제어를 구현하는 콜백 함수
    
    [event 옵션]
    1) EVENT_FLAG_LBUTTON	: 왼쪽 버튼 누름
    2) EVENT_FLAG_RBUTTON	: 오른쪽 버튼 누름
    3) EVENT_FLAG_MBUTTON	: 중간 버튼 누름
    4) EVENT_FLAG_CTRLKEY	: [Ctrl]키 누름
    5) EVENT_FLAG_SHIFTKEY	: [Shift]키 누름
    6) EVENT_FLAG_ALTKEY	: [Alt]키 누름
    
    7) EVENT_MOUSEMOVE		: 마우스 움직임
    8) EVENT_LBUTTONDOWN	: 왼쪽 버튼 누름
    9) EVENT_RBUTTONDOWN	: 오른쪽 버튼 누름
    10) EVENT_MBUTTONDOWN	: 중간 버튼 누름
    11) EVENT_LBUTTONUP		: 왼쪽 버튼 떼기
    12) EVENT_RBUTTONUP		: 오른쪽 버튼 떼기
    13) EVENT_MBUTTONUP		: 중간 버튼 떼기
    14) EVENT_LBUTTONDBLCLK	: 왼쪽 버튼 더블클릭
    15) EVENT_RBUTTONDBLCLK	: 오른쪽 버튼 더블클릭
    16) EVENT_MBUTTONDBLCLK	: 중간 버튼 더블클릭
    17) EVENT_MOUSEWHEEL	: 마우스 휠
    18) EVENT_MOUSEWHEEL	: 마우스 가로 휠

```angular2
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;

void onMouse(int event, int x, int y, int flags, void* param) {

  switch (event) {

    case EVENT_LBUTTONDOWN:
      cout << "마우스 왼쪽 버튼 누르기" << endl;
      break;
    case EVENT_RBUTTONDOWN:
      cout << "마우스 오른쪽 버튼 누르기" << endl;
      break;
    case EVENT_RBUTTONUP:
      cout << "마우스 오른쪽 버튼 떼기" << endl;
      break;
    case EVENT_LBUTTONDBLCLK:
      cout << "마우스 왼쪽 버튼 더블클릭" << endl;
      break;
  }

}

// 응용 실습: 두 상황의 감지 기능 추가
void onMouse2(int event, int x, int y, int flags, void* param) {

  if (flags == (EVENT_FLAG_SHIFTKEY + EVENT_FLAG_RBUTTON)) {
    cout << "shift키 + 오른쪽 버튼 누르기" << endl;
  }
  else if (flags == (EVENT_FLAG_CTRLKEY + EVENT_FLAG_LBUTTON)) {
    cout << "ctrl키 + 왼쪽 버튼 누르기" << endl;
  }

}

int main() {

  Mat image(200, 300, CV_8U);
  image.setTo(255);
  imshow("마우스 이벤트1", image);
  imshow("마우스 이벤트2", image);

  setMouseCallback("마우스 이벤트1", onMouse, 0);
  setMouseCallback("마우스 이벤트2", onMouse2, 0);
  waitKey(0);

  return 0;

}
```
<img src="/assets/images/opencv2/1.PNG" width="50%"><br>

## 3.27. 트랙바 이벤트 제어 - I

    - 트랙바: 일정한 범위 내에서 특정한 값을 선택하고자 할 때, 사용하는 일종의 스크롤 바
    - createTrackbar(): 트랙바를 생성하고, 그것을 지정된 윈도우 창에 추가하는 함수
    - (*TrackbarCallback)(): 트랙바의 위치가 변경될 때마다 호출되는 콜백 함수

```angular2
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;

string  title = "트랙바 이벤트";
Mat  image;

void onChange(int  value, void* userdata)
{
  int  add_value = value - 130;
  cout << "추가 화소값 " << add_value << endl;

  Mat tmp = image + add_value;
  imshow(title, tmp);
}

int main()
{
  int  value = 130;
  image = Mat(300, 400, CV_8UC1, Scalar(130));

  namedWindow(title, WINDOW_AUTOSIZE);
  createTrackbar("밝기값", title, &value, 255, onChange);

  imshow(title, image);
  waitKey(0);
  return 0;
}
```
<img src="/assets/images/opencv2/2-1.PNG" width="50%"><br>
<img src="/assets/images/opencv2/2-2.PNG" width="50%"><br>
<img src="/assets/images/opencv2/2-3.PNG" width="50%"><br>

## 3.28. 트랙바 이벤트 제어 - II
```angular2
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;

string title = "밝기변경", bar_name = "밝기값";
Mat image;
int posX, posY;

void onChange(int  value, void* userdata)
{
  int  add_value = value - 130;
  cout << "추가 화소값 " << add_value << endl;

  Mat tmp = image + add_value;
  imshow(title, tmp);
}

void onMouse(int event, int x, int y, int flags, void* param) {

  // 오른쪽 버튼 클릭시 영상 밝기 10씩 증가
  if (event == EVENT_RBUTTONDOWN) {

    add(image, 10, image);
    setTrackbarPos(bar_name, title, image.at<uchar>(0));

    Mat tmp = image + 10;
    imshow(title, tmp);

  }
  else if (event == EVENT_FLAG_LBUTTON) {

    subtract(image, 10, image);
    setTrackbarPos(bar_name, title, image.at<uchar>(0));

    Mat tmp = image - 10;
    imshow(title, tmp);

  }

}

// 응용 실습
void onMouse2(int event, int x, int y, int flags, void* param) {

  if (event == EVENT_LBUTTONDOWN) {
    posX = x;
  }

  // 마우스 왼쪽 버튼을 누른 채로 
  if (flags == (EVENT_MOUSEMOVE + EVENT_FLAG_LBUTTON)) {

    // 오른쪽으로 움직이면 밝아지고(밝기가 1씩 증가)
    if (x > posX) {

      add(image, 1, image);
      setTrackbarPos(bar_name, title, image.at<uchar>(0));

      Mat tmp = image + 1;
      imshow(title, tmp);

    }
    // 왼쪽으로 움직이면 어두워지는(밝기가 1씩 감소)
    else if (x < posX) {

      subtract(image, 1, image);
      setTrackbarPos(bar_name, title, image.at<uchar>(0));

      Mat tmp = image - 1;
      imshow(title, tmp);

    }

  }

}
int main() {

  int value = 130;
  image = Mat(300, 500, CV_8UC1, Scalar(130));

  namedWindow(title, WINDOW_AUTOSIZE);
  createTrackbar("밝기값", title, &value, 255, onChange);
  setMouseCallback(title, onMouse2, 0);

  imshow(title, image);
  waitKey(0);

  return 0;

}
```

## 3.29. 직선 및 사각형 그리기

    - 에일리어싱: 계단 현상(샘플링을 제대로 추출하지 못했을 때 발생하는 영향)
    - 안티에일리어싱: 계단 현상을 감소시키기 위해, 중간값을 추가
    - 푸리에 변환: 영상 -> 주파수
    
    - line(Mat& img, Point pt1, Point pt2, const Scalar& color, int thickness=1, int lineType=8, int shift=0)
      1) img: 그릴 대상 행렬
      2) pt1, pt2: 시작 좌표와 종료 좌표
      3) color: 선의 색상
      4) thickness: 선의 두께, -1이면 내부를 채움
      5) lineType: 선의 형태
         [옵션]
         5-1) LINE_4(4): 4-방향 연결선
         5-2) LINE_8(8): 8-방향 연결선
         5-3) LINE_AA(16): 계단현상 감소(안티에일리어싱) 선
      6) shift: 입력 좌표 (pt1, pt2)에 대해서 오른쪽 비트시프트 연산한 결과를 좌표로 지정해서 직선을 그림
                예를 들어 2가 되면, 1/2만큼 줄어듦

```angular2
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;
int main()
{
  Scalar white(255, 255, 255), yellow(0, 255, 255), blue(255, 0, 0);
  Scalar red = Scalar(0, 0, 255);
  Scalar green = Scalar(0, 255, 0);

  Mat image(400, 600, CV_8UC3, white);
  Point pt1(50, 130), pt2(200, 300), pt3(300, 150), pt4(400, 50);
  Rect  rect(pt3, Size(200, 150));

  line(image, pt1, pt2, red, 1, LINE_4);
  imwrite("result1.jpg", image);
  line(image, pt1, pt2, red, 1, LINE_8);
  imwrite("result2.jpg", image);
  line(image, pt1, pt2, red, 1, LINE_AA);
  imwrite("result3.jpg", image);

  line(image, pt1, pt2, red);
  line(image, pt3, pt4, green, 2, LINE_AA); // 안티에일리어싱 적용
  line(image, pt3, pt4, green, 3, LINE_8, 1); // 8방향 연결선, 1비트 시프트(반씩 줄어듦)

  rectangle(image, rect, blue, 2);
  rectangle(image, rect, blue, FILLED, LINE_4, 1);
  rectangle(image, pt1, pt2, red, 3);

  imshow("직선 & 사각형", image);
  waitKey(0);
  return 0;
}
```
<img src="/assets/images/opencv2/3.PNG" width="50%"><br>

## 3.30. 글자 쓰기

    - putText(Mat& img, const string& text, Point org, int fontFace, double fontScale,
              Scalar color, int thickness=1, int lineType=8, bool bottomLeftOrigin=false)
      1) img: 문자열을 작성할 대상 행렬(영상)
      2) text: 작성할 문자열
      3) org: 문자열의 시작 좌표, 문자열에서 가장 왼쪽 하단을 의미
      4) fontFace: 문자열에 대한 글꼴
      5) fontScale: 글자 크기 확대 비율
      6) color: 글자의 색상
      7) thickness: 글자의 굵기
      8) lineType: 글자 선의 형태
      9) bottomLeftOrigin: 영상의 원점 좌표

```angular2
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;
int main()
{
  Scalar olive(128, 128, 0), violet(221, 160, 221), brown(42, 42, 165);
  Point pt1(20, 100), pt2(20, 200), pt3(20, 250);

  Mat image(300, 500, CV_8UC3, Scalar(255, 255, 255)); // 3채널 uchar 행렬 선언 및 흰색(255) 초기화

  putText(image, "SIMPLEX", Point(20, 30), FONT_HERSHEY_SIMPLEX, 1, brown);
  putText(image, "DUPLEX", pt1, FONT_HERSHEY_DUPLEX, 2, olive);
  putText(image, "TRIPLEX", pt2, FONT_HERSHEY_TRIPLEX, 3, violet);
  putText(image, "ITALIC", pt3, FONT_HERSHEY_PLAIN | FONT_ITALIC, 2, violet);

  imshow("글자쓰기", image);
  waitKey(0);
  return 0;
}
```
<img src="/assets/images/opencv2/4.PNG" width="50%"><br>

## 3.31. 원 그리기

    - circle(Mat& img, Point center, int radius, const Scalar& color, int thickness=1, int lineType=8, int shift=0)
      1) Mat& img				: 원을 그릴 대상 행렬
      2) Point center			: 원의 중심 좌표
      3) int radius			: 원의 반지름
      4) const Scalar& color	: 선의 색상
      5) int thickness = 1		: 선의 두께
      6) int lineType = 8		: 선의 형태
      7) int shift = 0			: 좌표에 대한 비트 시프트 연산

```angular2
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;
int main()
{
  Scalar orange(0, 165, 255), blue(255, 0, 0), magenta(255, 0, 255);
  Mat image(300, 500, CV_8UC3, Scalar(255, 255, 255)); // 채널이 3개이므로, Scalar를 초기화할 때도 3개를 넣어야 함

  Point center = image.size() / 2;		// 영상의 중심점 구하기
  Point pt1(70, 50), pt2(350, 220);

  circle(image, pt1, 90, orange, 2, LINE_8, 0);
  circle(image, center, 100, blue, 2, LINE_8, 0);
  circle(image, pt2, 60, magenta, -1, LINE_8, 0); // 내부를 채우고 싶으면, thickness = -1로 설정

  int  font = FONT_HERSHEY_COMPLEX;
  putText(image, "center_blue", center, font, 1.2, blue);
  putText(image, "pt1_orange", pt1, font, 0.8, orange);
  putText(image, "pt2_magenta", pt2 + Point(2, 2), font, 0.5, Scalar(0, 0, 0), 2);
  putText(image, "pt2_magenta", pt2, font, 0.5, magenta, 1);

  imshow("원그리기", image);
  waitKey(0);
  return 0;
}
```
<img src="/assets/images/opencv2/5.PNG" width="50%"><br>

## 3.32. 타원 그리기

    - ellipse(Mat& img, Point center, Size axes, double angle, double startAngle, double endAngle,
              const Scalar& color, int thickness=1, int lineType=8, int shift=0)
      1) img: 그릴 대상 행렬(영상)
      2) center: 원의 중심 좌표
      3) axes: 타원의 크기(x축 반지름, y축 반지름)
      4) angle: 타원의 각도(3시 방향이 0도, 시계방향 회전)
      5) startAngle: 호의 시작 각도
      6) endAngle: 호의 종료 각도
      7) color: 선의 색상

```angular2
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;
int main()
{
  Scalar orange(0, 165, 255), blue(255, 0, 0), magenta(255, 0, 255);
  Mat image(300, 700, CV_8UC3, Scalar(255, 255, 255));

  Point pt1(120, 150), pt2(550, 150);
  circle(image, pt1, 1, Scalar(0), 1);
  circle(image, pt2, 1, Scalar(0), 1);

  ellipse(image, pt1, Size(100, 60), 0, 30, 270, blue, 2, 8, 0);
  ellipse(image, pt1, Size(100, 60), 0, -90, 30, orange, 2, 8, 0);

  ellipse(image, pt2, Size(100, 60), 30, -30, 160, blue, 2, 8, 0);
  ellipse(image, pt2, Size(100, 60), 30, -200, -30, orange, 2, 8, 0);

  imshow("타원 및 호 그리기", image);
  waitKey(0);
  return 0;
}
```
<img src="/assets/images/opencv2/6.PNG" width="50%"><br>

## 과제
오른쪽 클릭할 때마다 사각형, 왼쪽 클릭할 때마다 원을 그리기

