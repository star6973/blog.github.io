---
layout: post
title: Computer Vision and OpenCV [Day 3]
date: 2020-06-08 09:00:00-15:00:00
categories: OpenCV
tag: OpenCV
---

## 4. 사용자 인터페이스 및 I/O 처리 및 실습
### 4.1. 이미지 파일 처리

    Mat imread(const string& filename, int flags=1)
        - filename: 로드되는 영상 파일 이름
        - flags: 로드된 영상이 행렬로 반환될 때 컬러 타입을 결정하는 상수
        
            1) IMREAD_UNCHANGED(-1): 파일에 지정된 컬러 영상을 반환
            2) IMREAD_GRAYSCALE(0): 명암도 영상으로 변환하여 반환
            3) IMREAD_COLOR(1): 컬러 영상으로 변환하여 반환
            4) IMREAD_ANYDEPTH(2): 입력파일에 정의된 깊이에 따라 16비트/32비트 영상으로 변환,
                                   설정되지 않으면 8비트 영상으로 변화나
            5) IMREAD_ANYCOLOR(4): 파일에 정의된 타입으로 변환
        
    bool imwrite(const string& filename, InputArray img, const vector<int>& params)
        - filename: 저장되는 영상 파일 이름, 확장자명에 따라 영상파일 형식 결정
        - inputArray img: 저장하고자 하는 행렬
        - vector<int>& params: 압축 양식에 사용되는 인수쌍들의 벡터

```angular2
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
  CV_Assert(gray2gray.data && gray2color.data); // 데이터를 읽어서 파일이 있는지 없는지 Assert 에러 확인(디버깅)

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
```

```angular2
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
```


### 4.2. 행렬을 영상 파일로 저장 - I

    cv::imwrite(): 확장자명으로 영상파일을 쉽게 저장
        - IMWRITE_JPEG_QUALITY: JPG 파일 화질, 높은 값일 수록 화질이 좋음
        - IMWRITE_PNG_COMPRESSION: PNG 파일 압축레벨, 높은 값일 수록 적은 용량, 긴 압축시간
        - IMWRITE_PXM_BINARY: PPM, PGM 파일의 이진 포맷 설정

```angular2
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;
int main()
{
	Mat img8 = imread("../image/read_color.jpg", IMREAD_COLOR);
	CV_Assert(img8.data);

	vector<int> params_jpg, params_png;
	params_jpg.push_back(IMWRITE_JPEG_QUALITY); // 영상 파일 저장, JPEG 압축 옵션
	params_jpg.push_back(50);
	params_png.push_back(IMWRITE_PNG_COMPRESSION);
	params_png.push_back(9);

	imwrite("../image/write_test1.jpg", img8);
	imwrite("../image/write_test2.jpg", img8, params_jpg);
	imwrite("../image/write_test.png", img8, params_png);
	imwrite("../image/write_test.bmp", img8);
	return 0;
}
```

### 4.2. 행렬을 영상 파일로 저장 - II
```angular2
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;
int main()
{
	Mat img8 = imread("../image/read_color.jpg", IMREAD_COLOR);
	Mat img16, img32;
	CV_Assert(img8.data);

	// converTo를 할 때, scaling 필수
	img8.convertTo(img16, CV_16U, 65535 / 255.0);
	img8.convertTo(img32, CV_32F, 1 / 255.0f);

	Rect  roi(10, 10, 3, 3);
	cout << "img8 행렬의 일부 " << endl << img8(roi) << endl << endl;
	cout << "img16 행렬의 일부 " << endl << img16(roi) << endl << endl;
	cout << "img32 행렬의 일부 " << endl << img32(roi) << endl;

	imwrite("../image/write_test_16.tif", img16);
	imwrite("../image/write_test_32.tif", img32);

	imshow("img16", img16);
	imshow("img32", img32);
	waitKey();
	return 0;
}
```
### 4.3. 비디오 처리

    비디오 파일 - 본질적으로 대용량이기에 압축이 필요
    
    bool VideoCapture()
        - string& filename: 개방할 동영상 파일의 이름 혹은 이미지 시퀀스
        - int device: 개방할 동영상 캡처 장치의 id(카메라 한 대만 연결되면 0을 지정)

    bool open()
        - string& filename: 개방할 동영상 파일의 이름 혹은 이미지 시퀀스
        - int device: 개방할 동영상 캡처 장치의 id
        
    bool isOpened(): 캡처 장치의 연결 여부를 반환한다.

    bool release(): 동영상 파일이나 캡처 장치를 해제한다(클래스 소멸자에 의해서 자동으로 호출되어 명시적으로 수행하지 않아도 됨)

    double get(): 비디오 캡처의 속성 식별자로 지정된 속성의 값을 반환한다. 캡처 장치가 제공하지 않는 속성은 0을 반환한다.
        - int propId: 속성 식별자
        
            1) CAR_PROP_POS_MSEC: 동영상 파일의 현재 위치(ms)
            2) CAR_PROP_POS_FRAMES: 캡처되는 프레임의 번호
            3) CAR_PROP_POS_AVI_RATIO: 동영상 파일의 상대적 위치(0 시작, 1 끝)
            4) CAR_PROP_FRAME_WIDTH: 프레임의 너비
            5) CAR_PROP_FRAME_HEIGHT: 프레임의 높이
            6) CAR_PROP_FPS: 초당 프레임의 수
            7) CAR_PROP_FOURCC: 코덱의 4문자
            8) CAR_PROP_FRAME_COUNT: 동영상 파일의 총 프레임 수
            9) CAR_PROP_MODE: retrieve()에 의해 반환되는 Mat 영상 포맷
            10) CAR_PROP_BRIGHTNESS: 카메라에서 영상의 밝기
            11) CAR_PROP_CONTRAST: 카메라에서 영상의 대비
            12) CAR_PROP_SATURATION: 카메라에서 영상의 포화도
            13) CAR_PROP_HUE: 카메라에서 영상의 색조
            14) CAR_PROP_GAIN: 카메라에서 영상의 Gain
            15) CAR_PROP_EXPOSURE: 카메라에서 노출
            16) CAR_PROP_AUTOFOCUS: 자동 초점 조절

    bool set(): 지정된 속성 식별자로 비디오캡처의 속성을 설정한다.
        - int propId: 속성 식별자
        - double value: 속성 값
    
    bool grab(): 캡처 장치나 동영상 파일로부터 다음 프레임을 잡는다.
    
    bool retrive(): grab()으로 잡은 프레임을 디코드해서 image 행렬로 전달한다.
        - Mat& image: 잡은 프레임이 저장되는 행렬
        - int channel: 프레임의 채널 수
    
    bool read(), >>: 다음 동영상 프레임을 잡아서 디코드하고 image 행렬로 전달한다.
                     즉, grab()과 retrieve()를 동시에 수행한다.

    VideoWriter()
        - string& filename: 출력 동영상 파일의 이름
        - int fourcc: 프레임 압축에 사용되는 코덱의 4문자
        - double fps: 생성된 동영상 프레임들의 프레임 레이트
        - Size frameSize: 동영상 프레임의 크기(가로 x 세로)
        - bool isColor: true이면 컬러 프레임으로 인코딩, false이면 명암도 프레임으로 인코딩
        
    bool open(): 영상을 동영상 파일의 프레임으로 저장하기 위해 동영상 파일을 개방한다. 인수는 생성자의 인수와 동일하다.
    
    bool isOpend(): 동영상 파일 저장을 위해 VideoWriter 객체의 개방 여부를 확인한다.
    
    void write(), <<: image 행렬(프레임)을 동영상 파일로 저장한다.


```angular2
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;

// 문자열 출력 함수 - 그림자 효과 
void put_string(Mat& frame, string text, Point pt, int value)
{
	// 화면에 보이는 EXPOSE 문자열의 그림자 효과
	text += to_string(value);
	Point shade = pt + Point(2, 2);
	int font = FONT_HERSHEY_SIMPLEX;
	putText(frame, text, shade, font, 0.7, Scalar(0, 0, 0), 2);		// 그림자 효과
	putText(frame, text, pt, font, 0.7, Scalar(120, 200, 90), 2);	// 작성 문자
}

int main()
{
    // 0번 카메라 연결
	VideoCapture capture(0);
	if (!capture.isOpened())
	{
		cout << "카메라가 연결되지 않았습니다." << endl;
		exit(1);
	}
	cout << "너비 " << capture.get(CAP_PROP_FRAME_WIDTH) << endl;
	cout << "높이 " << capture.get(CAP_PROP_FRAME_HEIGHT) << endl;
	cout << "노출 " << capture.get(CAP_PROP_EXPOSURE) << endl;
	cout << "밝기 " << capture.get(CAP_PROP_BRIGHTNESS) << endl;

	for (;;) {
		Mat frame;
		capture.read(frame);

		// (10, 40) 위치에 노출
		put_string(frame, "EXPOS: ", Point(10, 40), capture.get(CAP_PROP_EXPOSURE));

		imshow("카메라 영상보기", frame);
		if (waitKey(30) >= 0) break;
	}
	return 0;
}
```

### 4.4. 카메라 속성 설정하기
```angular2
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;
// 문자열 출력 함수 - 그림자 효과 
void put_string(Mat& frame, string text, Point pt, int value)
{
	text += to_string(value);
	Point shade = pt + Point(2, 2);
	int font = FONT_HERSHEY_SIMPLEX;
	putText(frame, text, shade, font, 0.7, Scalar(0, 0, 0), 2);
	putText(frame, text, pt, font, 0.7, Scalar(120, 200, 90), 2);
}

VideoCapture capture;					// 전역 변수 선언

void zoom_bar(int value, void*) {		// 트랙바 콜백 함수들
	capture.set(CAP_PROP_ZOOM, value);	// 카메라 속성 지정
}
void focus_bar(int value, void*) {
	capture.set(CAP_PROP_FOCUS, value); // 카메라 속성 지정
}

int main()
{
	capture.open(0);												// 0번 카메라 연결
	CV_Assert(capture.isOpened());								// 카메라 연결 예외 처리

	capture.set(CAP_PROP_FRAME_WIDTH, 400); 	// 카메라 프레임 크기 설정
	capture.set(CAP_PROP_FRAME_HEIGHT, 300);
	capture.set(CAP_PROP_AUTOFOCUS, 0);
	capture.set(CAP_PROP_BRIGHTNESS, 150);

	int zoom = capture.get(CAP_PROP_ZOOM);				// 카메라 속성 가져오기
	int focus = capture.get(CAP_PROP_FOCUS);

	string title = "카메라 원본";							// 윈도우 이름 지정
	namedWindow(title);											// 윈도우 생성
	createTrackbar("zoom", title, &zoom, 10, zoom_bar); 		// 윈도우에 줌 트랙바 추가
	createTrackbar("focus", title, &focus, 40, focus_bar);

	for (;;) {

		Mat frame;
		capture >> frame;										// 카메라 영상받기
		
		Mat x_axis, y_axis, xy_axis;
		flip(frame, x_axis, 0);   // 상하 반전
		flip(frame, y_axis, 1);   // 좌우 반전
		flip(frame, xy_axis, -1); // 상하좌우 반전

		put_string(frame, "zoom: ", Point(10, 240), zoom);		// 줌 값 영상 표시
		put_string(frame, "focus: ", Point(10, 270), focus);	// 포커스 

		imshow(title, frame);
		imshow("상하 반전", x_axis);
		imshow("좌우 반전", y_axis);
		imshow("상하좌우 반전", xy_axis);

		if (waitKey(30) >= 0) break;
	}
	return 0;
}
```

### 4.5. 카메라 프레임 동영상 파일 저장
```angular2
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;
int main()
{
	VideoCapture capture(0);
	CV_Assert(capture.isOpened());

	double fps = 29.97; // 초당 프레임 수
	int  delay = cvRound(1000.0 / fps); // 프레임간 지연시간
	Size size(640, 360); // 동영상 파일 해상도
	int  fourcc = VideoWriter::fourcc('D', 'X', '5', '0'); // 압축 코덱 설정

	capture.set(CAP_PROP_FRAME_WIDTH, size.width); // 해상도 설정
	capture.set(CAP_PROP_FRAME_HEIGHT, size.height);

	cout << "width x height : " << size << endl;
	cout << "VideoWriterfourcc : " << fourcc << endl;
	cout << "delay : " << delay << endl;
	cout << "fps : " << fps << endl;

	VideoWriter  writer;
	writer.open("../image/video_file.avi", fourcc, fps, size);
	CV_Assert(writer.isOpened());

	Mat frame;

	for (;;) {
		
		capture.read(frame);
		//capture >> frame; 		// 카메라 영상받기
		//writer << frame; 		// 프레임을 동영상으로 저장
		writer.write(frame);

		imshow("카메라 영상보기", frame);
		if (waitKey(delay) >= 0)
			break;
	}
	return 0;
}
```

### 4.6. 비디오 파일 읽기
```angular2
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;

void put_string(Mat& frame, string text, Point pt, int value)
{
	text += to_string(value);
	Point shade = pt + Point(2, 2);
	int font = FONT_HERSHEY_PLAIN;
	putText(frame, text, shade, font, 1.8, Scalar(0, 0, 0), 2);
	putText(frame, text, pt, font, 1.8, Scalar(120, 200, 90), 2);
}

int main()
{
	VideoCapture capture;
	capture.open("../image/video_file.avi");
	CV_Assert(capture.isOpened());

	double frame_rate = capture.get(CAP_PROP_FPS);
	int delay = 1000 / frame_rate;
	int frmae_cnt = 0;
	Mat  frame;

	while (capture.read(frame))
	{
		if (waitKey(delay) >= 0) break;

		if (frmae_cnt < 100);
		else if (frmae_cnt < 200)	frame -= Scalar(0, 0, 100); // 영상이 전체적으로 붉은빛이 사라짐
		else if (frmae_cnt < 300) 	frame += Scalar(100, 0, 0); // 영상이 전체적으로 푸른빛이 생겨짐
		else if (frmae_cnt < 400)	frame = frame * 1.5; // 영상이 전체적으로 대비가 높아짐
		else if (frmae_cnt < 500)	frame = frame * 0.5; // 영상이 전체적으로 대비가 낮아짐

		put_string(frame, "frmae_cnt ", Point(20, 50), frmae_cnt++);
		imshow("동영상 파일읽기", frame);
	}
	return 0;
}
```

### 4.7. FileStorage 클래스

    FileStorage(): 생성자
    bool open()
        - string source: 개방할 동영상 파일 이름
        - string filename: 개방할 동영상 파일 이름
        - int flags: 연산 모드
            1) READ(0): 읽기 전용 열기
            2) WRITE(1): 쓰기 전용 열기
            3) APPEND(2): 추가 전용 열기
            4) MEMORY(4): source로부터 데이터를 읽고, 외부버퍼에 저장

        - string& encoding: 저장 데이터의 문자 인코딩 방식을 지정

    bool isOpend(): 클래스에 지정된 파일이 열려 있는지 확인하여 열려있으면 true를 반환한다.

    bool release(): 파일을 닫고, 모든 메모리 버퍼를 해제한다.
    
    void writeRaw(): 다중의 숫자들을 저장한다. 데이터를 raw 파일로 저장한다.
        - string& fmt: 배열 원소의 자료형에 대한 명세
        - uchar* vec: 저장될 배열의 포인터
        - size_t len: 저장할 원소의 개수


### 4.8. FileNode 클래스

    FileNode()
        - CvFileStorage* fs: 파일 저장 구조에 대한 포인터
        - CvFileStorage* node: 생성되는 파일 노드를 위한 초기화에 사용되는 파일 노드

    string name(): 노드 이름을 반환한다.
    
    size_t size(): 노드에서 원소의 개수를 반환한다.
        - int type()
            1) None(0): empty node
            2) INT(1): 정수형
            3) REAL(2): 부동 소수형
            4) FLOAT(2): 부동 소수형
            5) STR(3): 문자열 utf-8 인코딩
            6) STRING(3): 문자열 utf-8 인코딩
            7) REF(4): size_t 크기의 정수형
            8) SEQ(5): 시퀀스
            9) MAP(6): 매핑

    bool empty(): 노드가 비어있는지 확인한다.
    
    bool isNamed(): 노드가 이름이 있는지 확인한다.
    
    bool isNone(): 노드가 "none" 객체인지 확인한다.
    
    bool isInt(), bool isReal(): 노드 타입이 정수형, 실수형인지 확인한다.
    
    bool isString(): 노드타입이 문자형인지 확인한다.
    
    bool isMap(), bool isSeq(): 노드의 종류가 매핑인지, 시퀀스인지 확인한다.

    operator <<: 연산자 메서드, 템플릿 타입으로 데이터를 저장한다.
        - FileStorage& fs: 파일 저장 구조에 대한 포인터
        - _Tp& value: 저장하고자 하는 데이터(템플릿 자료형)
        - vector<_Tp>& vec: 저장하고자 하는 벡터 데이터
        
    operator >>: 파일 스토리지로부터 데이터를 읽는다.
        - FileNode& n: 읽은 데이터가 저장되는 노드
        - _TP& value: 파일 스토리지로부터 읽은 데이터
        - vector<_Tp>& vec: 파일 스토리지로부터 읽은 벡터 데이터

### 4.9. XML/YAML 파일 저장
- 스트림 연산자(<<) 이용

```angular2
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;
int main()
{
	FileStorage fs("test.xml", FileStorage::WRITE);
	string name = "이정진";
	fs << "name" << name;
	fs << "age" << 20;
	fs << "university" << "숭실대학교";
	// 시퀀스 노드(배열 처럼 저장)
	fs << "picture" << "[" << "mine1.jpg" << "mine2.jpg" << "mine3.jpg" << "]";
	
	// 매핑 노드
	fs << "hardware" << "{";
	fs << "cpu" << 25;
	fs << "mainboard" << 10;
	fs << "ram" << 6 << "}";

	int  data[] = { 1, 2, 3, 4, 5 , 6 };
	vector <int> vec(data, data + sizeof(data) / sizeof(float));
	fs << "vector" << vec;
	Mat m(2, 3, CV_32S, data);
	fs << "Mat" << m;

	Point2d  pt(10.5, 200);
	Rect     rect(pt, Size(100, 200));
	fs << "Point" << pt;
	fs << "Rect" << rect;

	fs.release();
	return 0;
}
```

### 4.10. XML/YAML 파일 읽기
- 스트림 연산자(>>) 이용

```angular2
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;
int main()
{
	FileStorage fs("test.xml", FileStorage::READ); // 읽기모드로 연결
	CV_Assert(fs.isOpened());

	string name, university, picture;
	int age;
	fs["university"] >> university; // 노드 접근 방법
	fs["name"] >> name;
	fs["age"] >> age;
	cout << "university " << university << endl;
	cout << "name " << name << endl;
	cout << "age " << age << endl;

	// 컬렉션 노드 가져오기
	FileNode node_pic = fs["picture"]; // 시퀀스 노드
	FileNode node_hd = fs["hardware"]; // 매핑 노드

	try {
		if (node_pic.type() != FileNode::SEQ)
			CV_Error(Error::StsError, "시퀀스 노드가 아닙니다.");
		if (!node_hd.isMap())
			CV_Error(Error::StsError, "매핑 노드가 아닙니다.");
	}
	catch (Exception& e) {
		exit(1);
	}

	cout << "[picture]  ";
	// 시퀀스 노드 - 인덱스로 접근
	cout << (string)node_pic[0] << ", ";
	cout << (string)node_pic[1] << ", ";
	cout << (string)node_pic[2] << endl << endl;

	// 매핑 노드 - 키값으로 접근
	cout << "[hardware]" << endl;
	cout << "  cpu  " << (int)node_hd["cpu"] << endl;
	cout << "  mainboard  " << (int)node_hd["mainboard"] << endl;
	cout << "  ram  " << (int)node_hd["ram"] << endl;

	Point pt;
	Rect  rect;
	Mat   mat;
	vector<float> vec;
	fs["vector"] >> vec;

	// 행렬 데이터 접근
	fs["Point"] >> pt;
	fs["Rect"] >> rect;
	fs["Mat"] >> mat;

	cout << "[vec] = " << ((Mat)vec).t() << endl;
	cout << "[pt] = " << pt << endl;
	cout << "[rect] = " << rect << endl << endl;
	cout << "[mat] = " << endl << mat << endl;

	fs.release();
	return 0;
}
```

```angular2
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;
int main()
{
	FileStorage fs_r("test.xml", FileStorage::READ);
	CV_Assert(fs_r.isOpened());

	FileNode node_pic = fs_r["picture"];			// 시퀀스 노드
	vector<Mat> images;
	for (int i = 0; i < node_pic.size(); i++)
	{
		Mat tmp = imread("../image/" + (string)node_pic[i], IMREAD_UNCHANGED);
		CV_Assert(tmp.data);
		images.push_back(tmp);
		imshow(node_pic[i], images[i]);
	}
	
	FileStorage fs_w("result.xml", FileStorage::WRITE);
	CV_Assert(fs_w.isOpened());

	vector<double> mean, dev;
	for (int i = 0; i < images.size(); i++) {
		string pic_name = ((string)node_pic[i]).substr(0,5); // 파일 이름만 가져오기

		meanStdDev(images[i], mean, dev); // 평균과 표준편차를 벡터로 변환
		fs_w << pic_name + "mmean" << "["; // 시퀀스 노드로 저장
		for (int j = 0; j < (int)mean.size(); j++) { // 각 채널 평균은 원소로 저장
			fs_w << mean[j];
		}
		fs_w << "]";
		fs_w << pic_name + "ddev" << dev;
	}

	fs_r.release();
	fs_w.release();
	waitKey(100);
	return 0;
}
```

### 4.11. 심화 실습 1
인터넷 등에서 네 개 칼라 영사의 jpg 파일을 구해서 XML 문서를 만들어서 각 파일명들을 시퀀스 노드로 저장하는 코드로 작성
```angular2
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;
int main()
{
	
	FileStorage fs("test2.xml", FileStorage::WRITE);
	fs << "picture" << "[";
	for (int i = 1; i < 4; i++) {
//		string pci_name = ((string)node_pic[i]).sub_str(0, 4);
		string filename = "m" + to_string(i) + ".jpg";
		Mat gray2color = imread(filename, IMREAD_COLOR); // RGB가 들어가서, 채널이 3개
		fs << filename;
	}

	fs << "]";

	fs.release();
	return 0;
}
```

### 4.12. 심화 실습 2

심화 실습 #1에서 생성된 XML 문서를 읽어서 4가지 컬러 영상의 영상들의 채널별 화소평균과 표준편차를 계산하여 XML 형식으로 저장하는 코드를 작성하시오.
```angular2
#include <opencv2/opencv.hpp>
using namespace cv;
using namespace std;
int main()
{
	FileStorage fs_r("test2.xml", FileStorage::READ);
	CV_Assert(fs_r.isOpened());

	FileNode node_pic = fs_r["picture"];			// 시퀀스 노드
	vector<Mat> images;
	for (int i = 0; i < node_pic.size(); i++)
	{
		Mat tmp = imread("../image/" + (string)node_pic[i], IMREAD_UNCHANGED);
		CV_Assert(tmp.data);
		images.push_back(tmp);
		imshow(node_pic[i], images[i]);
	}

	FileStorage fs_w("result2.xml", FileStorage::WRITE);
	CV_Assert(fs_w.isOpened());

	vector<double> mean, dev;
	for (int i = 0; i < images.size(); i++) {
		string pic_name = ((string)node_pic[i]).substr(0, 1); // 파일 이름만 가져오기

		meanStdDev(images[i], mean, dev); // 평균과 표준편차를 벡터로 변환
		fs_w << pic_name + "mmean" << "["; // 시퀀스 노드로 저장
		for (int j = 0; j < (int)mean.size(); j++) { // 각 채널 평균은 원소로 저장
			fs_w << mean[j];
		}
		fs_w << "]";
		fs_w << pic_name + "ddev" << dev;
	}

	fs_r.release();
	fs_w.release();
	waitKey(100);
	return 0;
}
```
