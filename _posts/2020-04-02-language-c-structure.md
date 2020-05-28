---
layout: post
title: "C Programming [Structure]"
date: 2020-04-02 19:00:00
categories: Language
tag: Language
use_math: true
---

# 구조체

1. 구조체는 서로 다른 형태의 데이터를 결합시키는 아주 유용한 파생 자료형(배열, 포인터, 열거형, 구조체)이다.
2. 여러 개의 자료형들을 묶어서 새로운 자료형을 만들 수 있는 방법이다.
3. 배열은 여러 개의 같은 자료형을 하나로 묶는 것
4. struct라는 키워드를 사용

        struct student {
              int number;
              char name[10];
              double grade;
        };  
        struct student s1 = { 24, "Ji", 4.3 };

5. 구조체 태그: 구조체의 이름(변수가 아니다)
6. 주의해야 할 점! 구조체 선언은 변수 선언이 아니다. 구조체 선언은 구조체 안에 어떤 변수들이 들어간다는 것만 말해주는 것이다. 아직은 데이터를 저장할 수 없다.
7. 구조체 태그와 구조체 변수의 차이점
    1. 구조체 태그는 구조체의 정의를 나타낸다. 따라서 구조체 태그에는 메모리 공간이 할당되지 않는다.
    2. 구조체 변수에는 메모리 공간이 할당된다. 
   
8. 구조체 멤버 참조: 멤버 연산자(.)을 사용한다
9. 만약 멤버가 문자열이라면 멤버에 값을 대입할 때, strcpy()를 사용한다
10. 구조체를 정의할 때 태그 이름은 생략하여도 무방하나 생략하지 않는 것이 좋다.
11. 구조체 안에 구조체를 포함시킬 수 있다.
12. 구조체를 다른 구조체에 대입하는 것은 가능하다.(=연산자 사용 가능) 그러나 구조체 변수와 구조체 변수를 서로 비교하는 것은 허용되지 않는다
13. 구조체의 배열: 구조체가 여러 개 모인 구조
14. 구조체 배열의 초기화에는 중괄호 안에 또 중괄호가 필요하다. 주의해야 할 점은 각 요소들의 초기화 값 사이에는 콤마가 있어야 한다는 것이다.
15. 구조체 배열의 원소의 개수 = sizeof(list) / sizeof(list[0]) or = sizeof(list) / sizeof(struct student)
16. 구조체와 포인터
    1. 구조체를 가리키는 포인터
    2. 포인터를 멤버로 가지는 구조체
17. 구조체를 가리키는 포인터

        struct student s = { 2014136124, "지명화“, 4.3 };
        struct student *p;
        p = &s;
        printf("학번: %d", (*p).number); // 반드시 괄호 사용(. 연산자가 *연산자보다 우선 순위가 높기 때문이다)

18. -> : 간접 멤버 연산자 

        (*p).number = p->number

19. 다음 네 가지의 경우를 잘 이해하자!
    1. (*p).number : 포인터 p가 가리키는 구조체의 멤버 number
    2. p->number(=(*p).number : 포인터 p가 가리키는 구조체의 멤버 number
    3. <p>*p.number(=*(p.number)) : 구조체 p의 멤버 number가 가리키는 것, 이때 number는 반드시 포인터여야 한다.</p>
    4. <p>*p->number(=*(p->number)) : p가 가리키는 구조체의 멤버 number가 가리키는 내용, 이때 number는 반드시 포인터여야 한다.</p>

20. 연결리스트
    - 자기 자신을 가리키는 포인터
21. 문자 배열과 문자 포인터
    1. 문자 배열은 구조체 안에 문자열을 저장하고 변경이 가능하다.
    2. 문자 포인터는 문자열이 이미 저장되어 있는 경우에만 문자 포인터로 그곳을 가리키게 하여 사용할 수 있지만 저장할 수는 없다.(scanf 사용불가능)

22. 구조체와 함수
    - 구조체가 인수나 반환 값으로 사용될 때는 “call by value"이다. 즉, 구조체 변수의 모든 내용이 복사되어 함수로 전달되고 반환된다. 따라서 원본 구조체에 영향이 없다.
    - 구조체의 크기가 큰 경우 상당한 시간이 소요되므로 구조체를 직접 전달하거나 반환하는 것보다 구조체의 포인터를 사용하는 것이 좋다.
    - 함수로 포인터가 전달되고 원본을 변경할 필요가 없으면 가급적 const를 적어주자. 
    - 만약 배열을 구조체 안에 포함시켜서 함수의 인수로 전달하면 구조체 안에 들어 있는 배열은 복사되어 전달된다.
23. 공용체  
    - 같은 메모리 영역을 여러 개의 변수들이 공유할 수 있게 하는 기능   
    - 배열 : 같은 타입의 변수들이 여러 개 모여 있음
    - 공용체 : 다른 타입의 변수들이 여러 개 모여 있음
    - 가장 큰 멤버의 크기만큼의 메모리가 할당 된다
    - union
        + 선택된 멤버에 따라 저장된 값이 다르게 해석 된다
        + 중요한 용도 중 하나는 똑같은 값을 서로 다른 표현 방법으로 보고 싶은 경우이다

24. 열거형
    - 변수가 가질 수 있는 값들을 나타내는 상수들을 모아놓은 자료형
    - 사용 이유 : 프로그램에서는 가질 수 있는 값을 제한하는 것이 프로그래밍 오류를 줄이기 때문이다. 어떤 변수가 정해진 값만을 갖는 경우 선언한다
    - 디폴트로 열거형 안에 기호 상수들은 0에서 1씩 증가하는 정수 값이다
    - 사용자가 지정해줄 수 있다
    - 다른 방법과 비교
        1. 정수 사용 : 컴퓨터는 알기 쉬우나 사람은 기억하기 어렵다     
        2. 기호 상수 사용 : 기호 상수를 작성할 때 오류를 저지를 수 있다
        3. 열거형 : 컴파일러가 중복이 일어나지 않도록 체크 한다


25. typedef
    - 새로운 자료형을 정의하는 것
    - 장점
        1. 이식성을 높여준다  
            자신의 코드를 하드웨어에 독립적으로 만들 수 있다
        
        2. <p>#define과의 차이점</p> 
            typedef은 컴파일러가 직접 처리, 배열 기호 상수는 #define은 못함
            
        3. 문서화의 역할도 한다  
            주석을 붙이는 것과 같은 효과가 있다
          
26. 요약 및 정리
    - 구조체와 배열의 차이점은 구조체는 다른 자료형을 하나로 묶어서 쓰는 것이고 배열은 같은 자료형을 하나로 묶어서 쓰는 것이다
    - 구초체는 키워드 struct으로 선언하고 공용체는 union, 열거형은 enum으로 선언한다
    - 구조체의 선언만으로 변수가 만들어지는가? 정의될뿐 만들어지지 않는다
    - 구조체를 가리키는 포인터 p를 통하여 구조체 안의 변수 x를 참조하는 수식은 p->x 이다
    - 원본 구조체를 포인터로 함수에 전달하는 경우, 원본 구조체를 훼손하지 않게 하려면 어떻게 하면 되는가? 구조체 태그 뒤에 const를 붙여서 사용한다
    - 공용체는 다른 타입의 변수들이 동일한 기억 공간을 공유할 수 있도록 만든 것이다
    - 열거형은 정수형 상수값들을 나열해 놓은 자료형이다
    - <p>#define 대신에 열거형을 사용하는 장점은? 이식성을 높여준다. #define으로 할 수 없는 것을 가능하게 해준다. 문서화가 가능하다.</p>
    - 새로운 자료형을 정의하기 위하여 사용되는 키워드는 typedef이다.
<br><br>

## 예제 
#### [벡터]

```angular2
#include <stdio.h>
#include <math.h>

typedef struct Pointer1 {
	int x, y;
} Pointer1;

typedef struct Pointer2 {
	int x, y, z;
} Pointer2;

// 문제 1번. 몇 사분면에 위치해있는지 알려주는 함수
void whatDimension(Pointer1 p)
{
	if(p.x > 0 && p.y > 0)
		printf("제 1사분면\n");
	else if(p.x < 0 && p.y > 0)
		printf("제 2사분면\n");
	else if(p.x < 0 && p.y < 0)
		printf("제 3사분면\n");
	else if(p.x > 0 && p.y < 0)
		printf("제 4사분면\n");
	else
		printf("원점");
}
// 문제 2-1번. 두 점 사이의 거리
double distance(Pointer1 p1, Pointer1 p2)
{
	return sqrt(pow((p1.x - p2.x), 2.0) + pow((p1.y - p2.y), 2.0));
}
// 문제 2-2번. 벡터의 합
double vector_sum(Pointer1 p1, Pointer1 p2)
{
	return sqrt(pow((p1.x + p2.x), 2.0) + pow((p1.y + p2.y), 2.0));
}
// 문제 2-3번. 벡터의 내적
double vector_inner_product_space(Pointer1 p1, Pointer1 p2)
{
	return (p1.x * p2.x) + (p1.y * p2.y);
}
// 문제 3-1번. 벡터의 외적(좌표)
Pointer2 vector_outter_product(Pointer2 p1, Pointer2 p2)
{
	Pointer2 outter = { (p1.y * p2.z - p1.z * p2.y), (p1.z * p2.x - p1.x * p2.z), (p1.x * p2.y - p1.y * p2.x) };
	return outter;
}
// 문제 3-2번. 삼각형의 넓이
// 벡터의 외적으로 구하기
double triangleArea(Pointer1 p1, Pointer1 p2, Pointer1 p3)
{	
	return 0.5 * abs(((p2.x - p1.x) * (p3.y - p1.y)) - ((p3.x - p1.x) * (p2.y - p1.y)));
}
// 문제 4번. 평면의 법선 벡터
void normal_vector(Pointer2 p1, Pointer2 p2, Pointer2 p3)
{
	Pointer2 pos1 = { (p2.x - p1.x), (p2.y - p1.y), (p2.z - p1.z) };
	Pointer2 pos2 = { (p3.x - p1.x), (p3.y - p1.y), (p3.z - p1.z) };
	Pointer2 normal_vector_pos = vector_outter_product(pos1, pos2);

	printf("법선 벡터 : (%d, %d, %d)\n", normal_vector_pos.x, normal_vector_pos.y, normal_vector_pos.z);
}
// 문제 5-1번. 직사각형인지 정사각형인지 확인하고, 사각형의 넓이, 둘레를 출력하는 함수
// 임의의 두 점을 잇고, 다른 두 점을 이으면 두개의 선분이 완성되고
// 두 선분의 벡터의 내적이 0이면 직교 & 두 선분의 길이가 같으면 정사각형
void isRectangle_Or_Square(Pointer1 p1, Pointer1 p2, Pointer1 p3, Pointer1 p4)
{
	Pointer1 pos1 = { (p1.x - p2.x), (p1.y - p2.y) };
	Pointer1 pos2 = { (p1.x - p3.x), (p1.y - p3.y) };
	Pointer1 pos3 = { (p1.x - p4.x), (p1.y - p4.y) };
	Pointer1 pos4 = { (p2.x - p3.x), (p2.y - p3.y) };
	Pointer1 pos5 = { (p2.x - p4.x), (p2.y - p4.y) };
	Pointer1 pos6 = { (p3.x - p4.x), (p3.y - p4.y) };

	double vec_dis1 = distance(p1, p2);
	double vec_dis2 = distance(p1, p3);
	double vec_dis3 = distance(p1, p4);
	double vec_dis4 = distance(p2, p3);
	double vec_dis5 = distance(p2, p4);
	double vec_dis6 = distance(p3, p4);

	// p1, p2가 대각선이라고 기준을 잡았을 때, 두 선분의 내적이 0, 두 선분의 길이가 같다면 정사각형
	if(vector_inner_product_space(pos1, pos6) == 0 && vec_dis1 == vec_dis3)
	{
		printf("정사각형\n");
		printf("사각형의 넓이: %d\n", (int)(vec_dis2 * vec_dis3));
		printf("사각형의 둘레: %d\n", (int)(vec_dis2 * 4));
	}
	// p1, p2가 대각선이라고 기준을 잡았을 때, 두 선분의 내적이 0이 아니고, 두 선분의 길이가 같다면 직사각형
	else if(vector_inner_product_space(pos1, pos6) != 0 && vec_dis1 == vec_dis3)
	{
		printf("직사각형\n");
		printf("사각형의 넓이: %d\n", (int)(vec_dis2 * vec_dis3));
		printf("사각형의 둘레: %d\n", (int)((vec_dis2 + vec_dis3) * 2));
	}

	////////////////////////////////////////////////////////////////////////////////////////////////////
	
	
	// p1, p3가 대각선이라고 기준을 잡았을 때, 두 선분의 내적이 0, 두 선분의 길이가 같다면 정사각형
	else if(vector_inner_product_space(pos2, pos5) == 0 && vec_dis2 == vec_dis5)
	{
		printf("정사각형\n");
		printf("사각형의 넓이: %d\n", (int)(vec_dis1 * vec_dis3));
		printf("사각형의 둘레: %d\n", (int)(vec_dis1 * 4));
	}
	// p1, p3가 대각선이라고 기준을 잡았을 때, 두 선분의 내적이 0이 아니고, 두 선분의 길이가 같다면 직사각형
	else if(vector_inner_product_space(pos2, pos5) != 0 && vec_dis2 == vec_dis5)
	{
		printf("직사각형\n");
		printf("사각형의 넓이: %d\n", (int)(vec_dis1 * vec_dis3));
		printf("사각형의 둘레: %d\n", (int)((vec_dis1 + vec_dis3) * 2));
	}
	

	///////////////////////////////////
	// p1, p4가 대각선이라고 기준을 잡았을 때, 두 선분의 내적이 0, 두 선분의 길이가 같다면 정사각형
	else if(vector_inner_product_space(pos3, pos4) == 0 && vec_dis3 == vec_dis4)
	{
		printf("정사각형\n");
		printf("사각형의 넓이: %d\n", (int)(vec_dis1 * vec_dis2));
		printf("사각형의 둘레: %d\n", (int)(vec_dis1 * 4));
	}
	// p1, p4가 대각선이라고 기준을 잡았을 때, 두 선분의 내적이 0이 아니고, 두 선분의 길이가 같다면 직사각형
	else if(vector_inner_product_space(pos3, pos4) != 0 && vec_dis3 == vec_dis4)
	{
		printf("직사각형\n");
		printf("사각형의 넓이: %d\n", (int)(vec_dis1 * vec_dis2));
		printf("사각형의 둘레: %d\n", (int)((vec_dis1 + vec_dis2) * 2));
	}
	// 두 선분의 내적이 0이 아니고, 두 선분의 길이가 다르다면 예외
	else
		printf("예외처리\n");
}

int main(void)
{
	Pointer1 pos1 = { 3, 5 };
	Pointer1 pos2 = { 1, 5 };
	Pointer1 pos3 = { 3, 1 };
	Pointer1 pos4 = { 1, 1 };

	Pointer2 pos5 = { 1, 2, 3 };
	Pointer2 pos6 = { 2, 6, 7 };
	Pointer2 pos7 = { 3, 4, 5 };

	// 문제 1번. 몇 사분면인지 구하기
	printf("문제 1번. pos1은 몇 사분면입니까? ");
	whatDimension(pos1);
	printf("\n");

	// 문제 2번. 두 점 사이의 거리, 벡터 합, 벡터 내적
	printf("문제 2-1번. pos1과 pos2의 거리는 얼마입니까? ");
	printf("%.2f\n", distance(pos1, pos2));
	printf("\n");
	printf("문제 2-2번. pos1과 pos2의 벡터의 합은 얼마입니까? ");
	printf("%.2f\n", vector_sum(pos1, pos2));
	printf("\n");
	printf("문제 2-3번. pos1과 pos2의 벡터의 내적은 얼마입니까? ");
	printf("%.2f\n", vector_inner_product_space(pos1, pos2));
	printf("\n");

	// 문제 3번. 삼각형의 넓이
	printf("문제 3번. pos1과 pos2, pos3가 이루는 삼각형의 넓이는 얼마입니까? ");
	printf("%.2f\n", triangleArea(pos1, pos2, pos3));
	printf("\n");

	// 문제 4번. 평면의 법선 벡터
	printf("문제 4번. pos5와 pos6, pos7의 법선 벡터는 얼마입니까? ");
	normal_vector(pos5, pos6, pos7);
	printf("\n");

	// 문제 5번. 직사각형인지 정사각형인지/ 사각형의 넓이/ 사각형의 둘레
	printf("문제 5번. pos1과 pos2, pos3, pos4는 어떤 사각형이고, 넓이와 둘레는 얼마입니까? ");
	isRectangle_Or_Square(pos1, pos2, pos3, pos4);

	return 0;
}
```
