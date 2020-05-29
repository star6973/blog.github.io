---
layout: post
title: "C++ Programming [Day 1]"
date: 2020-04-05 09:00:00-18:00:00
categories: Language
tag: Language
use_math: true
---

```angular2
#if 0

	namespace의 필요성
	1. 하나의 프로그램은 수십 ~ 수백개의 파일로 구성될 수 있고,
	   수십명의 개발자가 같이 작업을 할 수도 있다.
	
	2. 함수 및 구조체의 이름 충돌이 발생할 수 있다.

	namespace의 장점
	1. 프로그램의 다양한 요소(함수, 구조체 등)를 연관된 요소끼리 묶어서 관리할 수 있다.

	2. 기능별로 다른 이름 공간을 사용함으로써 함수/구조체의 이름 충돌을 막을 수 있다.

#include <stdio.h>
namespace Audio {
    void init() {
        printf("Audio init\n");
    }
}

namespace Video {
    void init() {
        printf("Video init\n");
    }
}

// global namespace (namespace를 설정안하면, 자동으로 global로 인식)
void init() {
    printf("System init\n");
}

int main() {

    init();
    Audio::init();
    Video::init();

}
#endif
```

```angular2
#if 1

	namespace에 있는 요소 접근 방법 3가지
	1. "한정된 이름(qualified name)"을 사용한 접근 방법 => Audio::init();

	2. "using 선언(declaration)"을 사용한 접근 => Audio::init();
											   => init 함수는 Audio 이름없이 사용가능

	3. "using 지시어 (directive)"을 사용한 접근 => using namespace Audio;
											    => Audio namespace의 모든 요소를 Audio 이름 없이 사용가능
#include <stdio.h>
namespace Audio {
	void init() { printf("Audio init\n"); }
	void reset() { printf("Audio reset"); }
}
void init() { printf("System init\n"); }

int main() {

	Audio::init();
	// using Audio::init; // using 선언 -> AUdio 안에 있는 특정 함수를 오픈해서 접근 가능하도록
	// init();
	// using Audio::reset;
	using namespace Audio; // Audio 자체를 오픈함 -> Audio 안에 있는 함수 접근 가능
	reset();
	::init(); // global로 선언된 함수를 선언

}
#endif
```

```angular2
#if 2
#include <algorithm>
using namespace std;
int main() {

	int n = max(1, 2);

}
#endif
```

```angular2
#if 3
	표준의 count 함수와 전역변수의 이름 충돌 발생의 예

	count 함수
	1. C++ 표준이 제공하는 함수 "std" 이름 공간안에 있다.
	2. <algorithm> 헤더

#include <stdio.h>
#include <algorithm>
int count = 10;
using std::max;
using namespace std; // 라이브러리 안에 count라는 내장함수도 있음
int main() {

	printf("%d\n", max(11234143, 2324213));
	printf("%d\n", std::max(1, 2));
	printf("count = %d\n", ::count); // global로 선언해줘야, std 라이브러리 안에 있는 내장함수랑 중복되지 않음

}
#endif
```

```angular2
#if 4

	C++ 헤더파일의 특징
	<stdio.h> vs <cstdio>

#include <cstdio>
#include <algorithm>
int main() {

	std::printf("Hello World\n");
	printf("%d\n", std::max(1, 2));

	int i = 10;
	printf("i = %d\n", i);
	{
		int i = 20;
		printf("i = %d\n", i);
	}
	printf("i = %d\n", i);

}
#endif
```

```angular2
#if 5
#include <cstdio>
namespace Audio { void init() { std::printf("Audio::init()\n"); } }
void PNT() { printf("foo()\n"); }
namespace STD { 
	using::PNT;
	void init() { std::printf("STD::init()\n"); } 
}
int main() {

	Audio::init();
	STD::PNT(); // PNT() 선언과 같음
	STD::init();

}
#endif
```

```angular2
#if 6
#include <iostream>
using namespace std;
int main() {

	int age = 0;
	std::cout << "How old are you ? >> ";
	std::cin >> age;
	std::cout << "age = " << age << std::endl;
	cout << "Hello World" << endl;

}
#endif
```

```angular2
#if 7
#include <iostream>
#include <iomanip>
int main() {
	int n = 10;
	std::cout << n << std::endl;
	std::cout << std::hex << n << std::endl; // 16진수로 출력
	printf("n = %d\n", n);
	printf("n = %x\n", n);
	std::cout << "Hello" << std::endl;
	std::cout << std::setw(10) << "Hello" << std::endl;
	std::cout << std::setw(10) << std::setfill('#') << "Hello" << std::endl;
	std::cout << std::setw(10) << std::setfill('#') << std::left << "Hello" << std::endl;
}
#endif
```

```angular2
#if 8

	일관된 초기화(uniform initialization)
	1. 변수를 초기화할 떄, "중괄호 {}를 사용해서 초기화"하는것

	일관된 초기화의 장점
	1. 모든 종류의 변수를 초기화할 때, "하나의 방식(uniform)으로 초기화"할 수 있다.

	2. "데이터 손실이 발생할 경우 컴파일 에러 발생" (prevent narrow)

#include <iostream>
using namespace std;
struct Point {
	int x;
	int y;
};
int main() {

	int n1 = 0xb;
	cout << n1 << endl;

	bool b = true;
	b = 3; // b에 3을 넣더라도, bool형이기 때문에 0 아니면 1로 나온다
	cout << b << endl;

	long long c = 10;
	cout << sizeof(c) << endl;

	const char* s1 = "aaa";
	cout << s1 << endl;

	int n2 = 10.5; // 암시적 형변환에 따른 데이터 손실 warning 처리
	int a[3] = { 1, 2, 3 };
	int n3 = { 10 };
	Point pt = { 99, 98 };

}
#endif
```

```angular2
#if 9

auto, decltype
	1. 변수의 타입을 컴파일러가 결정하는 문법 ==> 컴파일 시간에 결정되므로 실행시간 오버헤드는 없다.

	2. auto	: 우변의 수식(expression)으로 촤변의 타입 결정 ==> 반드시 "초기값(우변식)이 필요"하다.

	3. decltype : "() 안에 표현식"을 가지고 타입을 결정 ==> 초기값이 없어도 된다.
	

#include <iostream>
using namespace std;
int main() {

	int x1 = 10;
	int x2 = x1;
	auto x3 = x2; // n2의 타입을 자동으로 맞춰주기(x의 값에 따라)

	decltype(x3) x4;
	x4 = x2;

	cout << "x1 = " << x1 << endl;
	cout << "x2 = " << x2 << endl;
	cout << "x3 = " << x3 << endl;
	cout << "x4 = " << x4 << endl;

	int y1 = 10;
	auto y2 = y1;
	decltype(y1) y3;
	y3 = y2;

	int y4[3] = { 1, 2, 3 };
	auto y5 = y4; // 1. int y5[3] = y4 이라고 추론하면 error
				  // 2. int* y5 = y4라고 추론된다.

	cout << "y1 = " << y1 << endl;
	cout << "y2 = " << y2 << endl;
	cout << "y3 = " << y3 << endl;
	cout << "y4 = " << y4 << endl;
	cout << "y5 = " << y5 << endl;
	cout << endl;

	for (int i = 0; i < 3; i++)
		cout << "y4[" << i << "]" << " = " << y4[i] << endl;
	cout << endl;

	// y4의 값들도 변환됨(y5는 int* 타입이기 떄문에)
	y5[0] = 4;
	y5[1] = 5;
	y5[2] = 6;

	for (int i = 0; i < 3; i++)
		cout << "y5[" << i << "]" << " = " << y5[i] << endl;
	cout << endl;

	for (int i = 0; i < 3; i++)
		cout << "y4[" << i << "]" << " = " << y4[i] << endl;

	

	int z1[2] = { 1, 2 };
	auto z2 = z1[0]; // int z2라고 추론된다.
	
	cout << z1[0] << ", " << z1[1] << endl;
	
	z2 = 7;

	cout << z1[0] << ", " << z1[1] << ", " << z2 << endl;

}
#endif
```

```angular2
#if 10

	decltype과 함수 호출식
	1. decltype(함수이름) ==> 함수 타입
	
	2. decltype(&함수이름) ==> 함수 포인터 타입
	
	3. decltype(함수 호출식) ==> 함수 반환 타입
							 ==> 실제로 함수가 호출되는 것은 아님(평가되지 않는 표현식, unevaluated expression)

	변수의 타입을 조사하는 방법
	1. typeid(변수이름).name() ==> <typeinfo> 헤더파일 필요
							   ==> 완벽하게 정확한 타입이 나오지는 않음(const, volatile, reference, static)

#include <iostream>
using namespace std;
int foo(int a, double b) {

	std::cout << "foo" << std::endl;
	return 0;

}

int main() {

	decltype(foo) d1;			// 함수 타입 ==> int(int.double)
	decltype(&foo) d2;			// 함수 포인터 타입 ==> int(*)(int, double) int(*d2)(int, doublee)
	decltype(foo(1, 3.12)) d3;	// int 형 타입

	d2 = foo;
	d2(10, 10.1);

	d2 = &foo;
	d2(10, 10.1);

	d3 = 5;
	cout << d3 << endl;

	cout << typeid(d1).name() << endl;
	cout << typeid(d2).name() << endl;
	cout << typeid(d3).name() << endl;

	const int c = 0;
	cout << typeid(c).name() << endl;

}
#endif
```

```angular2
#if 11

	1. using => 기존 "타입의 별칭(alias)"을 만들 때 사용
			 => C++11부터 도입된 문법

	2. using vs typedef ==> typedef는 "타입의 별칭만 만들 수 있다"
						==> using은 타입뿐 아니라 "템플릿의 별칭"도 만들 수 있다.
						==> "template alias"라 함

#include <iostream>
using namespace std;
typedef int i32;
// typedef void *FP(void);
using FP = void(*)(void); // typedef와 따로 함수에 호출할 필요없음.
using FG = int(*)(int a, double b);

void foo(void) { cout << "foo() 호출" << endl; }
int goo(int a, double b) { cout << "goo() 호출" << endl; return 0; }

int main() {

	i32 a = 30;
	cout << "a = " << a << endl;
	
	// void (*fp)(void) = foo;
	// fp();
	// 간편하게 사용하기 위해서 위의 typedef 형태로 선언
	FP fp = &foo;
	fp();

	FG ft = &goo;
	ft(3, 5.5);

}
#endif
```

```angular2
#if 12

	constexpr
	1. 컴파일 시간 상수를 만드는 새로운 키워드 ==> "컴파일 시간에 결정되는 상수 값"으로만 초기화할 수 있다.
	2. "C++11"에서 도입된 문법

	compile time : 파일을 만들 때 실행되는 시간
	run time : 메모리에 올라온 변수를 확인하는 시간

#include <cstdio>
int main() {

	const int c1 = 10;		// 변경 불가능
	constexpr int c2 = 0;	// 변경 불가능

	int i = 10;				// 변수의 선언은 런타임에 결정이 됨 ==> 또 다른 배열의 크기로 선언할 수 없음(늦기 때문에)
	// int arr1[i];			// 배열의 선언은 컴파일 타임에 결정이 되므로, 런타임에 결정되는 상수로는 불가능

	const int j = 10;		// 컴파일 타임에 j의 값이 결정된다.  
	int arr2[j];

	const int k = i;		// 런타임에 k의 값이 결정된다.
	// int arr3[k];

	constexpr int m = 10;	// constexpr는 무조건 컴파일 타임에 결정이 됨
	int arr4[m];

	constexpr int n = j;
	int arr5[n];

}
#endif
```

```angular2
#if 13
	
	structured binding 개념, 사용법

#include <iostream>
using namespace std;
struct Point {
	int x{ 10 };
	int y{ 20 };
};
int main() {

	Point pt;

//	int x = pt.x;
//	int y = pt.y;

	// 위의 변수 선언이랑 같은 방법
	// C++17 버전부터 사용가능
	// i = 10, j = 20
	auto [i, j] = pt;
	cout << i << ", " << j << endl;

	int arr[2] = { 1, 2 };
	auto [m, n] = arr;
	cout << m << ", " << n << endl;

	const auto [p, q] = pt;
	auto& [c, d] = pt; // int& c = pt.x, double& d = pt.y

}
#endif
```

```angular2
#if 14
	
	C++에서 문자열 처리하는 방법(std::string 사용법)


#include <cstdio>
#include <cstring>
#include <iostream>
using namespace std;

void foo(const char* s) {
	printf("foo: %s\n", s);
}

int main() {

	// char s1[32] = "hello";
	string s1 = "hello";

	// const char* s2 = "world";
	string s2 = "world";

	s1 = s2;	
	cout << s1 << " " << s2 << endl;

	if (s1 == s2) { cout << "같은 문자열" << endl; }
	else { cout << "다른 문자열" << endl; }

	string s3 = "c++";
	foo(s3.c_str()); // const char* 형태로 변환해서 넘겨줌(객체 -> 문자열로 변환)

}
#endif
```

```angular2
#if 15

	default parameter
	1. 함수 호출시 인자를 전달하지 않으면 "미리 지정된 인자값"을 사용할 수 있다.
	
	주의사항 2가지
	1. 함수의 "마지막 인자부터 차례대로 디폴트값을 지정"해야 한다.
	2. 함수를 선언과 구현으로 분리할 때는 "함수 선언부에만 디폴트값을 표기"해야 한다.


#include <iostream>
using namespace std;

void setAlarm(int h, int m = 0, int s = 0) { cout << h << "시 " << m << "분 " << s << "초입니다" << endl;  }
// void setAlarm(int h, int m) { cout << h << "시 " << m << "분입니다" << endl; }
// void setAlarm(int h) { cout << h << "시입니다" << endl; }

void foo(int a, int b = 0, int c = 10) { cout << a << ", " << b << ", " << c << endl; }

int main() {
	
	setAlarm(3, 4, 5);
	setAlarm(3, 0, 0);
	setAlarm(3);
	setAlarm(3, 30);

	foo(10);
	foo(10, 20, 30);

}
#endif
```

```angular2
#if 16

	함수 오버로딩(function overloading)
    // int add(int a, int b) { std::cout << "int add()" << std::endl;  return a + b; }
    // double add(double a, double b) { std::cout << "double add()" << std::endl; return a + b; }
    
    // 위의 두 가지 형태의 함수를 하나로 묶기

	템플릿을 만들고 사용하는 방법
	1. template parameter ==> 컴파일 시간에 전달되어서 함수가 생성
					      ==> 함수가 생성되는 과정을 "템플릿 인스턴스화(template instantiation)"이라고 한다.

	2. call parameter ==? 실행시간에 함수에 전달
	
	3. template parameter를 표기할 때, "typename" 또는 "class" 키워드 사용가능

	4. 함수 템플릿 사용시 타입을 명시적으로 지정하지 않으면 "함수 호출 인자를 보고 컴파일러가 결정(type deduction, 추론/연역)"

#include <iostream>

template<typename T> 
// call parameter
T add(T a, T b) { std::cout << "add(): " << a + b << std::endl; return a + b; }

int main() {

	add(10, 20);
	// add(10, 30.5);	// 명시적으로 지정되지 않은 함수 호출 시, 인자를 보고 결정하지만, 선언된 함수는 이 조건에 부합하지 않음.
	add(10.2, 20.2);

	add<int>(10, 20);
	add<double>(10.2, 20.20);
	add<char>('a', 'b');

	return 0;

}
#endif
```