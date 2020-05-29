---
layout: post
title: "C++ Programming [Day 3]"
date: 2020-04-07 09:00:00-18:00:00
categories: Language
tag: Language
use_math: true
---

```angular2
#if 0

    1. 멤버 데이터을 "외부에서 직접 접근하면 객체가 잘못된 상태를 가지게 될수" 있다.
    2. "멤버 함수를 통해서만  멤버 데이터를 변경" 할 수 있게 한다.
       ==>멤버 함수는 "인자의 유효성 여부를 조사" 할 수 있다.

    3. 접근 지정자(access specifiers)
      private:  멤버 함수에서만 접근 할 수 있는 멤버
      public:   멤버 함수가 아닌 함수 에서도 접근 가능한 멤버
      protected: 자신과 파생 클래스의 멤버 함수에서 접근 가능한 멤버

    4.멤버 데이터와 멤버 함수
       ==> 멤버 데이터는 private 만드는 경우가  많다
       ==> 멤버 함수는 public 영역에 만드는 경우가 많다.


    용어 정리 -- 정보은닉, 캡슐화, setter/getter
    1.객체의 "사용자는 객체의 내부 멤버 데이터의 구조에 대해서는 알 필요가 없다" 멤버 함수만 알면된다.
      ==> 정보 은닉(information hiding)

    2. 멤버 함수를 통해서만 객체의 상태를 변경 할 수 있기 때문에
       "객체의 상태를 항상 안전하게 유지" 할 수 있다.
      |-----------------|
      |                 |
      |   --------      |
      |   | gear |      |       캡슐화(encapsulation)
      |   --------      |
      |                 |
      |	chagneGear()    |
      ------------------|

      3. setter/getter
       ==> 멤버 데이터의 값을 변경하거나 얻기 위해 사용하는 멤버 함수를 나타내는 용어

     4. struct vs class
       struct : 접근 지정자 생략시 기본 설정 값은 "public"
       class  :           ""                      "private"

#include <iostream>
//struct Bike {
class Bike {

    int color;
    int gear;
public:
    Bike() {
        color = 0;
        gear = 0;
    }
    void changeGear(int n)
    {
        if (n < 1)
            return;
        gear = n;
    }
    int GetGear() {
        return gear;
    }
    void SetGear(int val) {
        if (val < 1)
            return;
        gear = val;
    }

private:

};

int main()
{
    Bike b;
    //   b.gear = -7;
    //   b.gear = 3;
    //   b.changeGear(-7);
    b.gear = 8;
    std::cout << b.GetGear() << std::endl;
    b.SetGear(5);
    std::cout << b.GetGear() << std::endl;
    b.SetGear(-7);
    std::cout << b.GetGear() << std::endl;
}
#endif
```

```angular2
#if 1

    frined 함수 개념

    1. friend 함수
        ==> "멤버 함수는 아니지만 private 멤버에 접근" 할 수 있게 하고 싶을때
    2. 왜 멤버 함수로 만들지 않는가 ?
        ==>"멤버 함수로 만들 수 없는 경우"가 있다(연산자 재정의)
    3. 왜 getter/setter를 사용하지 않는가 ?
        ==> getter/setter를 제공하면 모든 함수에서 접근" 할 수 있다.
        ==> 모든 함수가 아닌 "특정 함수에서만 접근" 할 수 있게 하고 싶을때

#include <iostream>
class Airplane
{
    int color;
    int speed;
    int engineTemp = 20;

public:
    int getSpeed() { return speed; }
    //    int getTemp() { return engineTemp; }
    friend void fixAirplane(Airplane& a);
};

void fixAirplane(Airplane& a)
{
    int n = a.engineTemp;
}

void foo(Airplane a)
{
    //    std::cout << a.getTemp() << std::endl;
}
int main()
{
    Airplane a;
    foo(a);
    a.getSpeed();
    fixAirplane(a);
    //   std::cout << a.getTemp() << std::endl;
    return 0;
}
#endif
```

```angular2
#if 2

    1. 생성자를 사용하는 이유
        ==>"객체를 자동으로 초기화"하기 위해서
    2. 생성자 모양
        ==>"클래스 이름과 동일한 함수"
        ==>리턴 타입을 표기하지 않는다(즉 리턴타입이 없다)
        ==> 인자는 있어도 되고 없어도 된다. -- 2개 이상 만들수 있다(함수 오버로딩)
    3.객체를 생성하면
        ==> 객체의 메모리 크기 만큼 메모리를 할당하고
        ==> "생성자가 호출" 된다
        ==> 생성자가 없으면 객체를 만들 수 없다.
    4. "디폴트 생성자"
        ==>사용자가 생성자를 한개도 만들지 않으면" 컴파일러가
        인자 없는 생성자를 제공"해 준다.

#include <iostream>
using namespace std;
class Point
{
    int x;
    int y;
public:
    Point() { x = 0; y = 0; cout << "1" << endl; }
    Point(int a, int b) { x = a; y = b; cout << "2" << endl; }
};
int main()
{
    Point p1;
    Point p2(1, 2);

}
#endif
```

```angular2
#if 3
//객체를 생성하는 다양한 방법 
#include <iostream>
using namespace std;
class Point
{
    int x, y;
public:
    Point() { x = 0; y = 0; cout << "1" << endl; }
    Point(int a, int b) { x = a; y = b; cout << "2" << endl; }
};

int main()
{
    int a = 1;
    int b(1);   //b = 1
    int c{ 1 }; //c = 1;
    int d = { 1 };
    /*
        Point p1(1, 2);  //
        Point p2{ 1,2 };  //직접 초기화
        Point p3 = { 1,2 }; //복사 초기화
        p3 = p1;   //대입

        Point p4;
        Point p5();  //함수 선언
        Point p6{};
        Point p7 = {};
    */
    int k[3] = { 1 };
    Point p8[3];
    Point p9[3] = { Point(1,1) };
    Point p10[3] = { {1,1},{2,3}, };
    cout << endl;
    Point p11[3]{ {1,1},{2,3}, };

    Point* p12;

    p12 = static_cast<Point*>(malloc(sizeof(Point)));
    free(p12);

    p12 = new Point;
    delete p12;

    p12 = new Point(1, 2);
    delete p12;
}
#endif
```

```angular2
#if 4
#include <iostream>

class Point
{
    int x, y;
public:
    Point() { std::cout << "Point()" << std::endl; }
    ~Point() { std::cout << "~Point()" << std::endl; }
};
class Rect
{
    Point p1;
    Point p2;
public:
    Rect() { std::cout << "Rect()" << std::endl; }
    ~Rect() { std::cout << "~Rect()" << std::endl; }
};

int main()
{
    Rect r;
}
#endif
```

```angular2
#if 5
#include <iostream>

    위임 생성자(delegate constructor)
    1. 생성자에서 다른 생성자를 호출 하는 문법
    ==> C++11 부터 지원하는 문법
    ==> 클래스의 구현부에 표기" 한다.

#include "Point.h"
int main()
{
    Point p1;
    Point p2(1, 2);
    std::cout << "p1.x: " << p1.x << "   p1.y: " << p1.y << std::endl;
    std::cout << "p2.x: " << p2.x << "   p2.y: " << p2.y << std::endl;
}
#endif
```

```angular2
#if 6
#include <iostream>
/*
class Point
{
    int x; int y;
public:
    Point() = default;
    Point(int a, int b) {}
};
*/
#include "Point.h"
int main()
{
    Point p1;
    Point p2(1, 2);
}
#endif
```

```angular2
#if 7
#include <stdio.h>
#pragma warning(disable: 4996)

    1. C 스타일 코드의 단점
    ==> 획득한 자원은 "반드시 사용자가 직접 반납" 해야 한다.
    ==> 자원의 핸들을 담은 "변수에 직접 접근" 할 수 있어서
    
    변수의 값이 변질(잘못된 값으로 설정) 될 수 있다.

int main()
{
    FILE* f = fopen("skypwk.txt", "w");

    f = 0;
    fputs("hello skypwk", f);

    fclose(f);
}
#endif
```

```angular2
#if 8
#include <iostream>
#include <cstdio>
#include <string>
#pragma warning(disable: 4996)

class File
{
    FILE* f = 0;
public:
    File(std::string filename, std::string mode)
    {
        f = fopen(filename.c_str(), mode.c_str());
    }
    ~File() {
        fclose(f);
    }
    void puts(std::string s)
    {
        fputs(s.c_str(), f);
    }
};

int main()
{
    //   FILE* f = fopen("skypwk.txt", "w");
    //   f = 0;
    File f("skypwk.text", "w");


    //  fputs("hello skypwk", f);
    f.puts("hello world C++");
    // fclose(f);
}
#endif
```

```angular2
#if 9
#include <iostream>

    1. 멤버 초기화 리스트(member initializer lists)
        ==> 생성자 괄호()뒤에 "콜론(:)을 표기하고 멤버를 초기화 하는것
    2. 특징
        ==> 대입(assignment)이 아닌 "초기화(initialization)"

    const 상수 : const int i = 10;
    reference 변수:  int& r = a;

class Point
{
public:
    int x;
    int y;
    const int i;
    int& r;
    Point(int a, int b, int ci, int ri) :x(a), y(b), i(ci), r(ri)
    {
        //   x = a;
        //   y = b;
    }
};
int main()
{

    int a;
    int c = 30;
    const int b = 30;
    int& r = a;
    r = 10;
    r = a;
    r = c;

    // b = 20;
    a = 10;

    Point p(1, 2, 10, a);
    std::cout << "p.x :  " << p.x << "  p.y: " << p.y << std::endl;
}
#endif
```

```angular2
#if 10
#include <iostream>

    멤버 초기화 리스트 사용시 초기화 순서
    ==> 초기화 리스트의 순서대로 초기화 되지 않고
    ==> 멤버 데이터의 선언 순서대로 초기화 된다

class Point
{
public:
    int x;
    int y;
public:
    Point() :y(0), x(y)
    {

    }
};
int main()
{
    Point p;
    std::cout << p.x << ",  " << p.y << std::endl;
}
#endif
```

```angular2
#if 11

    멤버 데이터를 초기화하는 3가지 방법
    1. member field initialization
       ==> 생성자로 전달된 값을 사용 할 수 없다.
    2. member initializer list
       ==>가장 널리 사용되는 방법
       ==> 대입이 아닌 "초기화
    3. 생성자 블록안에서 초기화(대입)
       ==> 초기화가 아닌 "대입"

#include <iostream>
class Point
{
    int x = 0;  // 1   C++11
    int y = 0;
public:
    Point(int a, int b) : x(a), y(b)  // 2
    {
        x = a; y = b;   // 3
    }
};
int main()
{
    Point p(1, 3);
}
#endif
```

```angular2
#if 12
#include<iostream>
#pragma warning (disable: 4996)

    객체를 초기화 하는 방법
    1. 직접 초기화(direct initialization):  = 없이 초기화
                OFile f1("a.txt");
    2. 복사 초기화(copy initializatio) : = 를 사용해서 초기화 하는 것
                OFile f2 = "a.txt");

    함수 인자 전달과 초기화 방법
    1. 함수 인자 전달시 "복사 초기화를 사용" 한다.
    2. 특정 클래스 설계시 복사 초기화를 사용하지 못하게 하는것이 좋을 때가 있다.

class OFile
{
    FILE* file;
public:
    explicit OFile(const char* filename)
    {
        file = fopen(filename, "r");
    }

    ~OFile() { fclose(file); }
};
void foo(OFile f) { //OFile f = "hello.txt"
}
int main()
{
    OFile f1("a.txt");
    OFile f2 = "a.txt";
    foo(f1); // OFile f = f1    복사 생성자 호출 된다.
    foo("hello.txt");
}
#endif
```

```angular2
#if 13
#include <iostream>
#include <vector>
#include <string>
#include <memory>
int main()
{
    std::string s1("hello");
    std::string s2 = "hello";

    std::vector<int> v1(10);
    //std::vector<int> v2 = 10;

    std::shared_ptr<int> p1(new int);
    //    std::shared_ptr<int>p2 = new int;
}
#endif
```

```angular2
#if 14
#include <iostream>

    1. 복사 생성자
        ==> 자신과 동일한 타입 한개를 인자로 가지는 생성자
    2. 사용자가 복사 생성자를 만들지 않으면
        ==> 컴파일러가 제공(복사 생성자 자체를 만들지 않으면 만들어 준다)
        ==> 디폴트 복사 생성자
        ==> 모든 멤버를 복사(bitwise copy)한다.

class Point
{
public:
    int x;
    int y;
    Point() :x(0), y(0) {}
    Point(int a, int b) : x(a), y(b) {}
    Point(const Point& p) :x(p.x), y(p.y) {
        std::cout << "copy ctor" << std::endl;
    }
};
int main()
{
    Point p1;
    //   Point p4(1);
    Point p2(1, 2);
    Point p3(p2);

    std::cout << p3.x << std::endl;
    std::cout << p3.y << std::endl;

}
#endif
```

```angular2
#if 15

    복사 생성자가 호출되는 3 가지 경우
    1. 자신과 동일한 타입의 객체로 초기화 될때
        ==> Point p2(p1);
        ==> Point p2{p1};
        ==> Point p2 = p1; ==> explicit가 아닌 경우만

    2. 함수 인자를 call by value로 받을 경우
        ==> 함수 인자를 const reference로 사용 하면 복사본을 만들지
            않으므로 복사 생성자가 호출 되지 않는다.

    3. 함수가 객체 값으로 반환 할때
        ==> 참조로 반환 하면 리턴용 임시객체가 생성되지 않는다.
        ==>단. 지역변수는 참조로 반환 하면 안된다.

#include <iostream>
class Point
{
public:
    int x;
    int y;
    Point() :x(0), y(0) {}
    Point(int a, int b) : x(a), y(b) {}
    Point(const Point& p) :x(p.x), y(p.y) {
        std::cout << "copy ctor" << std::endl;
    }
    Point& aaa()
    {
        std::cout << "aaa()" << std::endl;
        return *this;
    }
};
void foo(const Point& pt) {}  //Point pt =  p1
Point p;

Point& goo() {

    return p;
}
int main()
{
    Point p1;
    Point p2(p1);  //직접 초기화 복사 생성자
    Point p3{ p1 };
    //   Point p4 = { p1 };
    //   Point p5 = p1;

    foo(p1);
    goo().aaa().aaa().aaa();
}

#endif
```

```angular2
#if 16
#include <iostream>
#include <cstring>
#pragma warning(disable:4996)

class Person {
    char* name;
    int age;
public:
    Person(const char* n, int a) : age(a)
    {
        name = new char[strlen(n) + 1];
        strcpy(name, n);
    }
    Person(const Person& p) :age(p.age) {
        name = new char[strlen(p.name) + 1];
        strcpy(name, p.name);
        //age = p.age;
    }
    ~Person() { delete[] name; }
};
int main()
{
    Person p1("park", 20);
    Person p2(p1);
}
#endif
```

```angular2
#if 17
#include <iostream>
#include <cstring>
#pragma warning(disable:4996)

class Person {
public:
    std::string name;
    int age;
public:
    Person(std::string inname, int a) :name(inname), age(a)
    {}
    /*
    Person(const Person& p) :age(p.age) {

    }
    */
    void setname(std::string inname) {
        name = inname;
    }
    ~Person() {  }
};
int main()
{
    Person p1("park", 20);
    Person p2(p1);
    std::cout << "p1.name: " << p1.name << std::endl;
    std::cout << "p2.name: " << p2.name << std::endl;
    p2.setname("kim");
    std::cout << "p1.name: " << p1.name << std::endl;
    std::cout << "p2.name: " << p2.name << std::endl;
}
#endif
```

```angular2
#if 18
#include <iostream>
#include <cstring>
#pragma warning(disable:4996)

class Person {
public:
    char* name;
    int* ref;
    int age;
public:
    Person(const char* n, int a) : age(a)
    {
        name = new char[strlen(n) + 1];
        strcpy(name, n);
        ref = new int(1);
        // ref = new int;
        // *ref = 1;

    }
    Person(const Person& p) :name(p.name), age(p.age), ref(p.ref) {
        ++(*ref);
    }
    ~Person() {
        if (--(*ref) == 0)
        {
            delete[] name;
            delete ref;
        }
    }
};
int main()
{
    Person p1("park", 20);
    Person p2(p1);
    Person p3(p1);

    std::cout << "p1.name: " << p1.name << "  p1.age: " << p1.age << std::endl;
    std::cout << "p2.name: " << p2.name << "  p2.age: " << p2.age << std::endl;
    std::cout << "p3.name: " << p3.name << "  p3.age: " << p3.age << std::endl;
    p1.age = 30; p2.age = 40; p3.age = 50;
    std::cout << "p1.name: " << p1.name << "  p1.age: " << p1.age << std::endl;
    std::cout << "p2.name: " << p2.name << "  p2.age: " << p2.age << std::endl;
    std::cout << "p3.name: " << p3.name << "  p3.age: " << p3.age << std::endl;
}
#endif
```

```angular2
#if 19
#include <iostream>
//int cnt = 0;
class Car {
    int speed;
    int color;
    static int cnt;

public:

    Car() { ++cnt; }
    ~Car() { --cnt; }
    static int getCount() {
        // speed = 3;
        return cnt;
    }
    static void setCount(int val) {
        cnt = val;
    }
    void hund(int val) {
        cnt = val;
    }
};
int Car::cnt = 0;
int main()
{
    Car::setCount(100);

    std::cout << Car::getCount() << std::endl;
    Car c1;
    Car c2;
    Car c3;
    Car* p = new Car;
    std::cout << Car::getCount() << std::endl;
    delete p;
    std::cout << Car::getCount() << std::endl;
}
#endif
```

```angular2
#if 20
#include <iostream>
using namespace std;
class Point {
public:
    int x = 0;
    int y = 0;
public:
    void set(int x, int y) {  // set(Point * this, int a, int b)
        this->x = x; this->y = y;
        cout << this << endl;
    }
};
int main()
{
    Point p1;
    Point p2;
    Point p3;

    cout << "p1.x: " << p1.x << "  , p1.y: " << p1.y << endl;
    cout << "p2.x: " << p2.x << "  , p2.y: " << p2.y << endl;
    cout << "p3.x: " << p3.x << "  , p3.y: " << p3.y << endl;

    cout << "p1: " << &p1 << endl;
    p1.set(1, 2); // set(&p1,1,2);
    p2.set(3, 4);
    p3.set(5, 6);

    cout << "p1.x: " << p1.x << "  , p1.y: " << p1.y << endl;
    cout << "p2.x: " << p2.x << "  , p2.y: " << p2.y << endl;
    cout << "p3.x: " << p3.x << "  , p3.y: " << p3.y << endl;
}
#endif
```

```angular2
#if 21
#include <iostream>
class Test
{
    static   int a;
    static   int b;
    int c = 0;
public:
    static void foo(int x, int y)
    {
        a = x;
        b = y;
        //    c = a + b;
        //    std::cout << this << std::endl;
    }
    void goo(int x, int y)
    {
        a = x;
        b = y;

        std::cout << this << std::endl;
    }
};
int Test::a = 10;
int Test::b = 20;
int main()
{
    Test::foo(1, 2);



    Test t;
    Test i;
    Test j;
    t.goo(1, 2);
    t.foo(1, 3);

}
#endif
```