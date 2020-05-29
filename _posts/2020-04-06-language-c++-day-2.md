---
layout: post
title: "C++ Programming [Day 2]"
date: 2020-04-06 09:00:00-18:00:00
categories: Language
tag: Language
use_math: true
---

```angular2
#if 0

    namespace의 필요성 
    1. 하나의 프로그램은 수십 ~ 수백개의 파일로 구성 될수 있고
       수십명의 개발자가 같이 작업을 할 수도 있다.
    2. 함수 및 구조체의 이름 충돌이 발생할 수 있다.

    1. 프로그램의 다양한 요소(함수, 구조체등)을 연관된 요소끼리 묶어서 관리 할 수 있다.
    2. 기능별로 다른 이름 공간을 사용할으로써 함수/구조체의 이름 충돌을 막을 수 있다.

#include <stdio.h>
namespace Audio {


    void init()
    {
        printf("Audio init\n");
    }
 }
namespace Video {
    void init()
    {
        printf("Video init\n");
    }
 }
//global namespace 
void init()  
{
    printf("System init\n");
}

int main()
{
    init();
    Audio::init();
    Video::init();
}
#endif
```

```angular2
#if 1

    namespace에 있는 요소 접근 방법 3가지 
    1. "한정된 이름(qualified name)"을 사용한 접근 방법
        ==>Audio::init();

    2."using 선언(declaration)"을 사용한 접근
        ==> using Audio::init;
        ==>init함수는 Audio 이름 없이 사용가능

    3. "using 지시어(directive)"을 사용한 접근 
        ==> using namespace Audio;
        ==> Audio namespace 의 모든 요소를 Audio 이름 없이 사용 가능 

#include<stdio.h>
using namespace Audio;
namespace Audio {
    void init() { printf("Audio init\n"); }
    void reset() { printf("Audio reset\n"); }
}
void init() { printf("System init\n"); }

using Audio::init;
void init() { printf("Audio init\n"); }
int main()
{
/*
    int i = 10;
    printf("i: %d\n", i);
    {
        int i = 20;
        printf("i: %d\n", i);
    }

    printf("i: %d\n", i);
*/

//  Audio::init();
//  using 선언 
   
//  using namespace Audio;
    using namespace Audio;
    init();
//  using Audio::reset;
//  reset();
}
#endif
```

```angular2
#if 2
#include <algorithm>
//using std::max;
using namespace std;
int main()
{
    int n = std::max(1, 2);
    n = max(1, 2);
    printf("n: %d\n", n);
}
#endif
```

```angular2
#if 3

    표준의 count 함수와 전역변수의 이름 충돌 발생의 예

    count함수
    1. C++표준이 제공하는 함수 "std" 이름 공간안에 있다.
    2. <algorithm>헤더 

#include <stdio.h>
#include <algorithm>
int count = 10;
using std::max;
using namespace std;
int main()
{
    printf("%d\n", std::max(1, 2));
    printf("%d\n", max(1, 2));
    printf("count: %d\n", ::count);
}
#endif
```

```angular2
#if 4
//  C++ 헤더 파일의 특징
//  <stdio.h>   vs  <cstdio>

#include <cstdio>
//  #include <algorithm>
int main()
{
    printf("Hello World\n");
    std::printf("hello world\n");
   // printf("%d\n", std::max(1, 2));
}
#endif
```

```angular2
#if 5
#include <cstdio>

void PNT()
{
    printf("foo()\n");
}
namespace STD
{
    using ::PNT;
    void init() { std::printf("Audio::init()"); }
}
int main()
{
    STD::init();
    PNT();         //printf(" ")
    STD::PNT();  //std::printf(" ")
}
#endif
```

```angular2
#if 6
#include <stdio.h>
#pragma warning(disable:4996)
int main()
{
    int age = 0;
    printf("How old are you ? >> ");
    scanf("%d", &age);
    printf("age: %d\n", age);
}
#endif
```

```angular2
#if 7
#include <iostream>
int main()
{
    int age = 0;
    std::cout << "How old are you ? >>";
    std::cin >> age;
    std::cout << "age: " << age << std::endl;
}
#endif
```

```angular2
#if 8
#include <iostream>
//  using std::cout;
//  using std::endl;
using namespace std;
int main()
{
    //std::cout << "hello" << std::endl;
    cout << "hello" << endl;
}
#endif
```

```angular2
#if 9
#include <iostream>
#include <stdio.h>
#include <iomanip>
int main()
{
    int n = 10;
    std::cout << n << std::endl;
    std::cout << std::hex<<n<< std::endl;
    std::cout << std::dec;
    std::cout << n << std::endl;
    std::cout << "hello" << std::endl;
    std::cout << std::setw(10) << "hello" << std::endl;
    std::cout << std::setw(10) << std::setfill('#')<<"hello" << std::endl;
    std::cout << std::setw(10) << std::setfill('#') <<std::left<<"hello" << std::endl;

    printf("n: %d\n", n);
    printf("n: %x\n", n);
}
#endif
```

```angular2
#if 10

    C언어와는 다른 C++ 변수의 특징

#include <iostream>
struct Point
{
    int x;
    int y;
};

int main()
{
    Point pt;
    int n1 = 0b0101;    //   06;//20;   //0xa8;
    printf("n1: %d\n", n1);

    bool b = true;
    b = 0;
    printf("b: %d\n", b);
    long long n3 = 10;   
    std::cout << "sizeof(n3): " << sizeof(n3) << std::endl;

   const char* s1 ="aaa";
   const wchar_t* s2 = L"AAA";
}
#endif
```

```angular2
#if 11
    일관된 초기화(uniform initialization)

    1. 변수를 초기화 할때 "중괄호{}를 사용해서 초기화"하는 것
        ==> "중괄호 초기화(brace-init)"이라고도 표현 
    2. "C++11"에서 도입됨 

int main()
{
    int n1 = 10;
    int a[3] = { 1,2 };
    int n2 = { 10 };
}
#endif
```

```angular2
#if 12

    일관된 초기화(uniform initialization)의 장점
    1. 모든 종류의 변수를 초기화 할때 "하나의 방식(uniform)으로 초기화" 할 수 있다.
    2. "데이터 손실이 발생할 경우 컴파일 에러 발생"
        (prevent narrow)

struct Point
{
    int x;
    int y;
};
int main()
{
    int n1 = 10;
    int n2(10);
    int n3{ 10 };

    int x[2] = { 1,2 };
    Point pt = { 1,2 };

    int n4 = 3.4;   //암시적 형변환에 따른 데이터 손실 warning 처리 
    char c = 300;   //

    int n5 = 3.4 ;
    char c = { 300 };
}
#endif
```

```angular2
#if 13
//  auto / decltype

    1. 변수의 타입을 컴파일러가 결정 하는 문법
        ==> "컴파일 시간에 결정"되므로 실행 시간 오버헤드는 없다.
    2. auto
        ==> 우변의 수식(expression)으로 좌변의 타입 결정
        ==> 반드시 "초기값(우변식)이 필요" 하다
    3. decltype
        ==> "()안에 표현식"을 가지고 타입을 결정
        ==> 초기값이 없어도 된다.

int main()
{
    //int x[5] = { 1,2,3,4,5 };
    int x=10;
    char ch;
    int n1 = x;
    auto n2 = x;
    auto ac = ch;

    decltype(n1)n3;
    n3 = n1;
}
#endif
```

```angular2
#if 14
#include <iostream>

    1. auto
        ==> 우변이 배열이면 "요소를 가리키는 포인터 타입"으로 타입이 결정
    2. decltype
        ==> ()안의 표현식이 배열이면 "()안의 표현식과 완전히 동일한 배열 타입"으로 결정 
        ==> 이 경우 동일 표현식으로 초기화 할 수 없다.
    3. auto와 decltype은 "표현식에 따라 타입이 다르게 결정" 되는 경우가 있다.

using std::cout;
using std::endl;

int main()
{

    int x[3] = { 1,2,3 };  //  int x[3]==>  int[3]  x
    auto a2 = x;    //1. int[3] a2 ==> int a2[3];라고 추론하면 error
                    //2. int* a2 = x 라고 추론 된다.
    int w[3] = { 1,2,3 };
    int k[3];
    k[0] = w[0];

    decltype(x) d2 = { 3,4,5 };
  //  decltype(x) d2 = x;
    int* pa = x;

    for (int i = 0; i < 3; i++)
        cout << "x[" << i << "] :" << x[i] << endl;
    cout << endl;
    cout << endl;
    a2[0] = 3;
    a2[1] = 4;
    a2[2] = 5;   

    for (int i=0; i < 3; i++)
        cout << "a2[" <<i<< "] :" << a2[i] << endl;
    cout << endl;
    cout << endl;
    for (int i = 0; i < 3; i++)
        cout << "x[" << i << "] :" << x[i] << endl;
     
/*
    int x[3] = { 1,2,3 };

    decltype(x) d2;  //int d2[d3]로 추론 

    d2[0] = 100;
    d2[1] = 200;
    d2[2] = 300;

    for (int i = 0; i < 3; i++)
        cout << "x[" << i << "] :" << x[i] << endl;
    cout << endl;
    cout << endl;
    
    for (int i = 0; i < 3; i++)
        cout << "d2[" << i << "] :" << d2[i] << endl;
    cout << endl;
    cout << endl;
    

    for (int i = 0; i < 3; i++)
        cout << "x[" << i << "] :" << x[i] << endl;
    cout << endl;
    cout << endl;
*/

    int y[2] = { 1,2 };
    auto a4 = y[0];// --> int a4;
    printf("y[0]: %d y[1]: %d\n", y[0], y[1]);

    a4 = 7;
    printf("y[0]: %d y[1]: %d a4: %d\n", y[0], y[1], a4);

    decltype(y[0])d4 = y[0];  //int d4 = 7;  error
                              //int& d4; 
                              //int *d4;

    printf("y[0]: %d y[1]: %d\n", y[0], y[1]);
    d4 = 30;
    printf("y[0]: %d y[1]: %d\n", y[0], y[1]);

    int i = 10;
    int& r = i;

    printf("i: %d\n", i);

    r = 20;
    printf("i: %d\n", i);


}
#endif
```

```angular2
#if 15
//  decltype 과 함수 호출식

    1. decltype(함수이름)
        ==> 함수 타입
    2. decltype(&함수이름)
        ==> 함수 포인터 타입
    3. decltype(함수 호출식)
        ==> 함수 반환 타입
        ==> 실제로 함수가 호출되는 것은 아님(평가되지 않는 표현식, unevaluated expression)
    
    변수의 타입을 조사하는 방법
    1. typeid(변수이름).name()
        ==> <typeinfo>헤더 파일 필요 
        ==> 완벽하게 정확한 타입이 나오지는 않음(const, volatile, reference ,static 가 출력안됨)


#include <iostream>
//#include <typeinfo>
int foo(int a, double d) {
    std::cout << "foo" << std::endl;
    return 0;
}

int main()
{
    foo(1, 3.1);

    int (*fp)(int, double);

    decltype(foo) d1; // 함수 타입 ---int(int,double)
    decltype(&foo)d2;  //함수 포인터 타입   int(*)(int , double)  int(*d2)(int , double)
    decltype(foo(1, 3.12))d3; 

    d2 = &foo;
    d2(10,10.1);
    fp = foo;
    fp(10, 35.4);

    std::cout << typeid(d1).name() << std::endl;
    std::cout << typeid(d2).name() << std::endl;
    std::cout << typeid(d3).name() << std::endl;

    int& c = d3;
    std::cout << typeid(c).name() << std::endl;
}
#endif
```

```angular2
#if 16
#include <iostream>

typedef int i32;
typedef void (*FP)(void);
void foo(void)
{
    std::cout << "foo()" << std::endl;
}
int main()
{
    int i = 30;

 //   void (*fp)(void);
    FP fp;

    fp = foo;
    fp();

    i32 a = 30;

    std::cout << "a : " << a << std::endl;
}
#endif
```

```angular2
#if 17
#include <iostream>

    1. using
       ==> 기존 "타입의 별칭(alias)"을 만들때 사용
       ==> C++11 부터 도입된 문법
    2. using vs typedef
       ==> typedef는 "타입의 별칭만 만들 수 있다"
       ==> using은 타입 뿐 아니라 "템플릿의 별칭"도 만들수 있다.
       ==> "template alias"라 함 

//typedef int i32;
using i32= int;
//typedef void (*FP)(void);
using FP = void(*)(void);
using FG = int (*)(int a, double b);
void foo(void) 
{
    std::cout << "foo()" << std::endl;
}
int goo(int a, double b)
{
    std::cout << "goo()" << std::endl;
    return a;
}
int main()
{
    int i = 30;

    //   void (*fp)(void);
    FP fp;
    FG ft;

    ft = &goo;
    ft(3, 5.5);

    fp = foo;
    fp();

    i32 a = 30;

    std::cout << "a : " << a << std::endl;
}
#endif
```

```angular2
#if 18

    constexpr
    1. 컴파일 시간 상수를 만드는 새로운 키워드
        ==> "컴파일 시간에 결정되는 상수 값"으로만 초기화 할 수 있다.
    2. "C++ 11"에서 도입된 문법 

#include<cstdio>
int main()
{

   const int c1 = 10;
   c1 = 12;
  
   constexpr int c2 = 0;
   c2 = 20;
}
#endif
```

```angular2
#if 19
#include <cstdio>
int main()
{
    int arr1[10]; //ok
    int s1 = 10;  //

  //  int arr2[s1];   //

    const int s2 = 10;  //컴파일 타임에 s2값이 결정된다
    int arr3[s2];

    const int s3 = s1;//실행 타임에 s3값이 결정된다.(실행 타임에 결정되는 상수)

    constexpr int c3 = 10;
    constexpr int c4 = s1;

    printf("hello\n");
}
#endif
```

```angular2
#if 20
//structed binding 개념, 사용법
//c++17
#include <iostream>
struct Point {
    int x{ 10 };
    double y{ 20.7 };
};
int main()
{
    Point pt;

 //   int x = pt.x;
 //   int y = pt.y;

    auto [i, j] = pt;
    std::cout << "pt.x: " << pt.x << "   pt.y: " << pt.y << std::endl;
    std::cout << "i: " << i << "j: " << j << std::endl;
    int arr[2] = { 1,2 };
    auto [x,y] = arr;  //변수 갯수가 맞아야 한다.
    x = 20;
   // int[x, y] = arr;
    const auto [a, b] = pt; 
    //a = 7;
    auto& [c, d] = pt;   //int& c = pt.x  double& d = pt.y

}
#endif
```

```angular2
#if 21

    C++에서 문자열 처리하는 방법 
    std::string 사용법 

#include <stdio.h>
#include <string.h>
#pragma warning(disable:4996)
int main()
{
    char s1[32] = "hello";
    const char* s2 = "world";

   // s1 = s2;
    printf("s1: %s , s2: %s\n", s1, s2);
    strcpy(s1, s2);
    printf("s1: %s , s2: %s\n", s1, s2);
 //   if (s1 == s2) { printf("같은 문자열\n");}
    if(!strcmp(s1,s2)) { printf("같은 문자열\n"); }

}

#endif
```

```angular2
#if 22

    C++에서 문자열 처리하는 방법 
    std::string 사용법 

#include <cstdio>
#include <cstring>
#include <iostream>
#pragma warning(disable:4996)

void foo(const char* s)
{
    printf("foo: %s\n", s);
}
int main()
{

    std::string s = "hello";
 //   foo(s);
 //   foo((const char*)s);
    foo(s.c_str());

    //char s1[32] = "hello";
    std::string s1 = "hello";
    
    //const char* s2 = "world";
    std::string s2 = "world";

    // s1 = s2;
    //printf("s1: %s , s2: %s\n", s1, s2);
    std::cout << "s1: " << s1 << "  s2: " << s2 << std::endl;
    //strcpy(s1, s2);
    s1 = s2;

    //printf("s1: %s , s2: %s\n", s1, s2);
    std::cout << "s1: " << s1 << "  s2: " << s2 << std::endl;
    //   
    //if (!strcmp(s1, s2)) { printf("같은 문자열\n"); }
    if (s1 == s2) { printf("같은 문자열\n"); }
}

#endif
```

```angular2
#if 23

    default parameter
    1. 함수 호출시 인자를 전달하지 않으면 "미리 지정된 인자 값"을 사용할 수 있다.

    주의 사항 2가지
    1. 함수의 "마지막 인자 부터 차례대로 디폴트 값을 지정"해야 한다.
    2. 함수를 선언과 구현으로 분리 할때는 "함수 선언부에만 디폴트 값을 표기"해야 한다.

#include <iostream>
void setAlarm(int h=0, int m=0, int s = 0)
{
    std::cout << "h: " << h << "  m:  " << m << " s: " << s << std::endl;
}

int main()
{
    setAlarm(3, 4, 5);
    setAlarm(3, 30);
    setAlarm(3);
    setAlarm();
   
}
#endif
```

```angular2
#if 24

    1. 함수의 "마지막 인자 부터 차례대로 디폴트 값을 지정"해야 한다.
    2. 함수를 선언과 구현으로 분리 할때는 "함수 선언부에만 디폴트 값을 표기"해야 한다.

void foo(int a, int b=0, int c = 0);
int main()
{
    foo(10);
   // foo(10, 20)
}

void foo(int a, int b, int c ) {
}
#endif
```

```angular2
#if 25

    함수 오버로딩(function overloading) - I

#include <iostream>
int add(int a, int b)
{
    return a + b;
}
double add(double a, double b)
{
    return a + b;
}

int main()
{
    add(10, 20);
    add(10.2, 20.2);
    return 0;
}
#endif
```

```angular2
#if 26

    함수 오버로딩(function overloading) - II

//#include <iostream>
int add(int a, int b)
{
 //   std::cout << "int add()" << std::endl;
    return a + b;
}
double add(double a, double b)
{
  //  std::cout << "double add()" << std::endl;
    return a + b;
}

int main()
{
    add(10, 20);
    add(10.2, 20.2);
    return 0;
}
#endif
```

```angular2
#if 27

    템플릿을 만들고 사용하는 방법
    1.template parameter
      ==> "컴파일 시간에 전달" 되어서 "함수가 생성"
      ==> 함수가 생성되는 과정을 "템플릿 인스턴수화(template instantiation)"이라고 한다.
    2. call parameter
      ==> "실행 시간에 함수에 전달"
    3. template parameter를 표기할때 "typename" 또는 "class" 키워드 사용가능 

    4. 함수 템플릿 사용시 타입을 명시적으로 지정하지 않으면 "함수 호출 인자를 
       보고 컴파일러가 결정(type deduction, 추론/연역)"

    //template<class T> //template parameter  컴파일 시간에 전달

template<typename T, typename A>
T add(T a, A b)      //call parameter      실행시간에 함수에 전달
{
 //   std::cout << "ret: " <<a+b<< std::endl;
    return a + b;
}

int  add(int a, int b)
{
    //   std::cout << "ret: " <<a+b<< std::endl;
    return a + b;
}

double add(double a, double b)
{
    //   std::cout << "ret: " <<a+b<< std::endl;
    return a + b;
}

int main()
{
    add(10, 20);
    add(10, 30.5);
    add(3.5, 5);
    add(10.2, 20.2);
    add('a', 'b');
    return 0;
}
#endif
```

```angular2
#if 28

    inline 함수의 개념

int add1(int a, int b)
{
    return a + b;
}
inline int add2(int a, int b)
{
    return a + b;
}
int main()
{
    int n1 = add1(1, 2);
    int n2 = add2(1, 2);
}
#endif
```

```angular2
#if 29

    함수 삭제(delete function) C++11

    1. "함수를 삭제"하는 방법 
        함수선언부 = delete;
    2. 삭제된 함수를 호출하면 "컴파일 시간에 오류"가 발생한다.

void foo(int)= delete;

int main()
{
    foo(10);
}
#endif
```

```angular2
#if 30
int gcd(int a, int b)
{
    return b != 0 ? gcd(b, a % b) : a;
}
double gcd(double a, double b) = delete;
int main()
{
    gcd(2, 10); // ok;
    gcd(2.3, 4.3);
}
#endif
```

```angular2
#if 31
/*
int square(int a)
{
    return a * a;
}
double square(double a)
{
    return a * a;
}
*/
template<typename T>
T square(T a)
{
    return a * a;
}
char square(char ) = delete;
int main()
{
    square(3);
    square(3.3);
//  square('a');
}
#endif
```

```angular2
#if 32

    후위 반환 타입(trailing return type)
    람다 표현식 이나 함수 템플릿을 만들때 주로 사용 

    auto square(int a)->int
{
    return a * a;
}

//int main()
auto main()->int
{
    int i = 3;
    auto ai = i;
    square(3);
}
#endif
```

```angular2
#if 33
/*
int add(int a, int b)
{ return a+ b;}

double add(double a, double b)
{ return a + b; }
*/
/*
template<typename T>
T add(T a, T b)
{
    return a + b;
}
*/
template<typename T1, typename T2>
//??? add(T1 a, T2 b)
 auto add(T1 a, T2 b)->decltype(a + b)
{
    return a + b;
}

int main()
{
    int i;
    decltype(i) k;
    
 //   a = 3;
 //   int a;
    add(1, 2);
   add(1.1, 2.2);
    add(1, 2.2);
    add(2.2, 1);
}
#endif
```

```angular2
#if 34
constexpr int add(int a, int b)
{
    return a + b;
}

int main()
{
    int x = 1;
    int y = 1;

    int n1 = add(1, 1);
    int n2 = add(x, y);
}
#endif
```

```angular2
#if 35

    constexpr function의 의미
    1. 함수의 인자 값을 컴파일 시간에 결정할 수 있으면 컴파일 시간에 함수의 결과값을 계산하라
    2. 함수의 인자 값을 컴팡딜 시간에 결정할 수 없으면 "실행 시간에 함수를 실행 "

constexpr int add(int a, int b)
{
    return a + b;
}
template<typename T,int N>
struct Buffer 
{
    T data[N];
};
int main()
{
   const int x = 1;
    int y = 1;

    Buffer<int,10> b1;
 //   Buffer<int, x> b2;
    Buffer<char, add(10, 10)>b3;
    Buffer<int, add(x, 1)>b4;
}
#endif
```

```angular2
#if 36
#include<iostream>
void foo(int a)
{
    std::cout << "foo: " << a << std::endl;
}
int add(int a, int b) { return a + b; }
int main()
{
    int n1 = add(1, 2);
    int n2 = [](int a, int b) { return a + b; }(1, 5);

    foo(1); //함수이름(함수 인자): 함수 호출 

    [](int a)
    {
        std::cout << "foo: " << a << std::endl;
    };

    [](int a)
    {
        std::cout << "foo: " << a << std::endl;
    }(10);
}
#endif
```

```angular2
#if 37
#include <iostream>
#include <algorithm>
bool cmp(int a, int b) { return a > b; }

bool (*test())(int,int)
{
    bool (*fp)(int, int);
    fp = [](int a, int b) { return a > b; };
    return fp;
}

int main()
{
    int x[10] = { 1,3,5,7,9,2,4,6,8,10 };
    for (int i = 0; i < 10; i++)
        std::cout  << x[i] << "  ";
    

    bool (*fp)(int, int);
    //fp = [](int a, int b) { return a > b; };
    //std::sort(x, x + 10);
    //std::sort(x, x + 10, cmp);
    //std::sort(x, x + 10, [](int a, int b) { return a > b; });
    fp = test();

    std::sort(x, x + 10, fp);
    std::cout << std::endl << std::endl;
    for (int i = 0; i < 10; i++)
        std::cout  << x[i] << "  ";
}
#endif
```

```angular2
#if 38
#include <iostream>
//range_for
int main()
{
    int x[10] = { 1,2,3,4,5,6,7,8,9,10 };

    for (int i = 0; i < 10; i++)
    {
        std::cout << x[i] << "  ";
    }
    std::cout << std::endl << std::endl;
    for(int n : x)
    {
        std::cout << n << "  ";
    }
   
    std::cout << std::endl << std::endl;
    for (auto  n : x)
    {
        std::cout << n << "  ";
    }
}
#endif
```

```angular2
#if 39

    if init, switch init

#include <stdio.h>
int foo() { return -1; }
int main()
{
    /*
    int ret = foo();

    if (ret == -1)
    {
        printf("오류발생 \n");
    }
    */
    if (int ret = foo(); ret == -1)
    {
        printf("오류발생\n");
    }

    /*
    int n = foo();
    switch (n) {
    case 10:
        //......
        break;
    case 20:
        //.....
        break;
    }
    */
    switch (int n = foo(); n) {
    case 10: 
        //...
        break;
    case 20:
        //...
        break;
    }
    return 0;
}
#endif
```

```angular2
#if 40

    1. 변수
        ==> "메모리의 특정 위치를 가리키는 이름"
        ==> 코드안에서 해당 메모리에 접근하기 위해서 사용
    2. 레퍼런스 변수
        ==> "기존 변수(메모리)에 또 다른 이름(alias)를 부여"하는 것

#include <iostream>
int main()
{
    int n = 10;
    int* p = &n;
    int& r = n;

    std::cout << n << std::endl;
    std::cout << r << std::endl;
    std::cout << *p << std::endl;
    std::cout << std::endl<<std::endl;

    std::cout << &n << std::endl;
    std::cout << &r << std::endl;
    std::cout << p << std::endl;
    std::cout << &p << std::endl;
    std::cout << 1 << 2 << 3 << 4;

}
#endif
```

```angular2
#if 41
#include <stdio.h>
int aaa()
{
    printf("aaa()\n");
    return 0;
}

int (*aa())()
{
    printf("aa()\n");
    return aaa;
}

int (*(*bb())())()
{
    printf("bb()\n");
    return aa;
}

int main()
{
    bb()()();
}

#endif
```

```angular2
#if 42
#include <iostream>
struct REF {

    REF& get() {
        std::cout << "REF::get()" << std::endl;
        return *this;
    }
    REF& set() {
        std::cout << "REF::set()" << std::endl;
        return *this;
    }
    REF& test() {
        std::cout << "REF::test()" << std::endl;
        return *this;
    }
};

int main()
{
    REF ref;

    ref.get().set().set().test().get();
}
#endif
```

```angular2
#if 43
#include <iostream>
void f1(int n) { ++n; }
void f2(int* p) { ++(*p); }
void f3(int& r) { ++r; }

int main()
{
    int a = 0, b = 0, c = 0;
    f1(a);
    f2(&b);
    f3(c);
    int i[2] = { 10,20 };
    int* p = i;
    int*& r = p;
    

    std::cout << i[0] << std::endl;
    std::cout << *p << std::endl;
    std::cout << *r << std::endl;
    std::cout << std::endl << std::endl;
    *r = 30;
    std::cout << i[0] << std::endl;
    std::cout << *p << std::endl;
    std::cout << *r << std::endl;
    std::cout << std::endl << std::endl;
    r++;
    std::cout << i[0] << std::endl;
    std::cout << *p << std::endl;
    std::cout << *r << std::endl;


    std::cout << std::endl << std::endl;

    std::cout << a << std::endl;
    std::cout << b << std::endl;
    std::cout << c << std::endl;
}
#endif
```

```angular2
#if 44
#include <iostream>
struct Date
{
    int year;
    int month;
    int day;
};
void foo(const Date& d) {

    std::cout << "d.year: " << d.year << std::endl;
    d.year = 2021;
}

int main()
{
    Date date = { 2020,3,28 };
    std::cout << "date.year: " << date.year << std::endl;
    foo(date);
    std::cout << "d.year: " << date.year << std::endl;
}
#endif
```

```angular2
#if 45
#include <iostream>
void add(double ar, double ai, double br, double bi,   // in parameter
                double *sr, double* si)  //out parameter
{
    //double sr = ar + br;
    //double si = ai + bi;
    *sr = ar + br;
    *si = ai + bi;

    //return ???
}
int main()
{
    double xr = 1, xi = 1; //1 + 1i
    double yr = 2, yi = 2; //2 + 2i
    double sr, si;
    add(xr, xi, yr, yi, &sr, &si);
    std::cout << "sr: " << sr << ", si  " << si << std::endl;
}
#endif
```

```angular2
#if 46
#include <iostream>

struct Complex
{
    double real;
    double image;

};

Complex  add(const Complex& c1, const Complex& c2)
{
    Complex temp;
    temp.real = c1.real + c2.real;
    temp.image = c1.image + c2.image;
    return temp;
}
int main()
{
    Complex c1 = { 1,1 };
    Complex c2 = { 2,2 };
    Complex ret = add(c1, c2);
    std::cout << "ret.real: " << ret.real << ", ret.image  " << ret.image << std::endl;
}
#endif
```

```angular2
#if 47

    1. 프로그램에서 "필요한 타입을 설계" 한다
    2. 현실세계에 존재 하는 사물은 "상태" 와 "동작"이 있다.

                    상태                  동작 
    자동차         색상,속도           달린다, 멈춘다

    사람          나이, 몸무게         웃는다. 말한다

    게임캐릭터       색상               싸운다.
    복소수         실수부, 허수부        더한다. 절대값 구하기 

    3. 타입을 설계할때
      ==> 상태와 동작을 표현할 수 있어야 한다.
      ==> 상태는  변수로 동작은 함수로 표현된다.

    4. C의 구조체와 C++의 구조체
      ==> C : 데이터만 포함 할 수 있다.
      ==> C++ : 데이터 뿐 아니라 "함수도 포함 할 수" 있다

#include <iostream>
int buf[10];
int idx = 0;
void push(int value)
{
    buf[idx++] = value;
}
int pop()
{
    return buf[--idx];
}
int main()
{
    push(10);
    push(20);
    push(30);
//    int n1 = pop(); //30
//    int n2 = pop(); //20
    std::cout << pop() << std::endl;  //30
    std::cout << pop() << std::endl;  // 20
}
#endif
```

```angular2
#if 48
#include <iostream>
int buf[10];
int idx = 0;
void push(int * buf, int* idx, int value)
{
    buf[(*idx)++] = value;
}
int pop(int* buf,int* idx)
{
    return buf[--(*idx)];
}
int main()
{
    int buf1[10];
    int idx1=0;

    int buf2[10];
    int idx2 = 0;


    push(buf1,&idx1, 10);
    push(buf1,&idx1, 20);
    push(buf2,&idx2, 30);
    push(buf2,&idx2, 40);
    //    int n1 = pop(); //30
    //    int n2 = pop(); //20
    std::cout << pop(buf1,&idx1) << std::endl;  //20
    std::cout << pop(buf1,&idx1) << std::endl;  // 10

    std::cout << pop(buf2, &idx2) << std::endl;  //40
    std::cout << pop(buf2, &idx2) << std::endl;  // 30

}
#endif
```

```angular2
#if 49
#include <iostream>
struct Stack {
    int buf[10];
    int idx = 0;
};

void push(Stack* s, int value)
{
  //  buf[(*idx)++] = value;
    s->buf[s->idx++] = value;
}
int pop(Stack* s)
{
    return s->buf[--(s->idx)];
}
int main()
{
  //  int buf1[10];
  //  int idx1 = 0;
    Stack s1;
    s1.idx = 0;
    //int buf2[10];
    //int idx2 = 0;
    Stack s2;
    s2.idx = 0;
    push(&s1, 10);
    push(&s1, 20);

    push(&s2, 30);
    push(&s2, 40);
    //    int n1 = pop(); //30
    //    int n2 = pop(); //20
    std::cout << pop(&s1) << std::endl;  //20
    std::cout << pop(&s1) << std::endl;  // 10

    std::cout << pop(&s2) << std::endl;  //40
    std::cout << pop(&s2) << std::endl;  // 30

}
#endif
```

```angular2
#if 50
#include <iostream>

    정보은닉
    1. 접근지정자
       ==>  private : 멤버 함수에서만 접근 할 수 있다.
       ==> public : 멤버 함수가 아닌 외부에서도 접근 할 수 있다.
       그래서 idx가 private이 되므로 객체 에서 접근 불가

struct Stack {
private:
    int buf[10];
    int idx;

public:
    void init() { idx = 0; }
    void push(int value) {
        buf[idx++] = value;
    }
    int pop() {
        return buf[--idx];
    }
};

int main(){
    Stack s1;
    s1.init();
    Stack s2;
    s2.init();

   // push(&s1, 10);  //함수중심
    s1.push(10);      //객체 중심 
    s1.push(20);
 
    //push(&s2, 30);
    //push(&s2, 40);
    s2.push(30);
    s2.push(40);
    std::cout << s1.pop() << std::endl;      //20
    std::cout << s1.pop() << std::endl;      //10 
    std::cout << s2.pop() << std::endl;      //40
    std::cout <<s2.pop() << std::endl;       //30
}
#endif
```

```angular2
#if 51
#include <iostream>

    생성자
    ==> "클래스 이름과 동일한 이름"을 가지는 함수
    ==> "리턴 타입이 없다"
    ==> 변수(객체)를 생성하면 자동으로 생성자가 호출 된다.


struct Stack {
private:
    int buf[10];
    int idx;

public:
 //   void init() { idx = 0; }
    Stack() {
        idx = 0;
    }
    void push(int value) {
        buf[idx++] = value;
    }
    int pop() {
        return buf[--idx];
    }
};

int main() {
    Stack s1;
    s1.push(10);      //객체 중심 
    s1.push(20);

   std::cout << s1.pop() << std::endl;  //20
    std::cout << s1.pop() << std::endl;  // 10 

}
#endif
```

```angular2
#if 52
#include <iostream>

    생성자
    ==> "클래스 이름과 동일한 이름"을 가지는 함수
    ==> "리턴 타입이 없다"
    ==> 변수(객체)를 생성하면 자동으로 생성자가 호출 된다.

    소멸자(destructor)
    ==> ~클래스이름() 의 모양의 함수
    ==> 리턴 타입을 표기하지 않으며 인자도 가질 수 없다.
    ==> 변수(객체)가 파괴 될때 자동으로 호출 된다.
    ==> 변수(객체)가 생성자에서 자원을 할당한 경우 소멸자에서
        자원을 반납한다.

struct Stack {
private:
    int* buf;
    int idx;

public:
    //   void init() { idx = 0; }
    Stack(int size) {
        idx = 0;
        buf = (int*)malloc(sizeof(int) * size);
    }
    ~Stack() {
        std::cout << "~Stack()" << std::endl;
        free(buf);
    }
    void push(int value) {
        buf[idx++] = value;
    }
    int pop() {
        return buf[--idx];
    }
};

int main() {
    Stack s1(20);
    Stack s2(50);
    s1.push(10);      //객체 중심 
    s1.push(20);

    std::cout << s1.pop() << std::endl;  //20
    std::cout << s1.pop() << std::endl;  // 10 

}
#endif
```

```angular2
#if 53
#include <stdio.h>
#include <stdlib.h>
int main()
{
    int* p;
    p = (int*)malloc(sizeof(int) * 5);

    for (int i = 0; i < 5; i++)
        p[i] = i + 1;
    for (int i = 0; i < 5; i++)
        printf(" %d  ", p[i]);

    free(p);
    return 0;

}
#endif
```

```angular2
#if 54
#include <iostream>

template<typename T>
class Stack {
private:
    T* buf;
    int idx;
public:
    Stack(int size) {
        idx = 0;
        //buf = (int*)malloc(sizeof(int) * size);
        buf = new T[size];
    }
    ~Stack() {
        std::cout << "~Stack()" << std::endl;
        //free(buf);
        delete[] buf;
    }
    void push(T value) {
        buf[idx++] = value;
    }
    T pop() {
        if (idx == 0)
        {
            std::cout << "값이 없습니다." << std::endl;
            return ;
        }
        return buf[--idx];
    }
};

struct info {
    std::string name;
    int id;
};

int main() {
    Stack<int> s1(20);
    Stack<char>s2(5);
    Stack<info>s3(5);

    s1.push(50);      //객체 중심 
    s1.push(60);
    std::cout << s1.pop() << std::endl;  //20
    std::cout << s1.pop() << std::endl;  // 10 
    std::cout << s1.pop() << std::endl;  // 10 

    s2.push('a');      //객체 중심 
    s2.push('b');
    std::cout << s2.pop() << std::endl;  //20
    std::cout << s2.pop() << std::endl;  // 10 

    info test;
    test.name = "aaa";
    test.id = 1234;
    s3.push(test);

    test.name = "bbb";
    test.id = 5678;
    s3.push(test);


    test = s3.pop();
    std::cout << test.name << " , " << test.id << std::endl;
    test = s3.pop();
    std::cout << test.name << " , " << test.id << std::endl;
}
#endif
```

```angular2
#if 55
#include <iostream>
#include <stack>

int main()
{
    std::stack<int> s;
    s.push(10);
    s.push(20);
    s.push(30);

    std::cout << s.top() << std::endl;
    s.pop();
    std::cout << s.top() << std::endl;
    s.pop();
    std::cout << s.top() << std::endl;
}
#endif
```

```angular2
#if 56
#include <iostream>
#include <vector>

int main() {
    //int x[10] = { 1,2,3,4,5 };

    //std::vector<int>x; //빈 벡터 생성
    std::vector<int> x = { 1,2,3,4,5 };

    x[0] = 10;
    x.resize(20);

    for (auto n : x)
        std::cout << n << std::endl;
    std::cout << std::endl << std::endl;
    x.resize(10);
    for (auto n : x)
        std::cout << n << std::endl;

}
#endif
```