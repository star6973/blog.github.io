---
layout: post
title: "C Programming [Pointer 1]"
date: 2014-06-01 12:00:00-13:00:00
categories: Language
tag: Language
use_math: true
---

# 포인터

1. 포인터는 메모리의 주소를 가지고 있는 변수이다.
2. 변수의 크기에 따라 차지하는 메모리 공간의 크기가 달라진다.(int - 4바이트, double - 8바이트)
3. 주소 연산자(&)는 변수의 이름을 받아서 변수의 주소를 반환한다.
4. 포인터에도 타입이 있다. (int * p : int형 포인터, double * p : double형 포인터)
5. double * p 데이터는 8바이트이지만 주소 값은 4바이트이다.              
6. sizeof(p) = 4 / sizeof(*p) = 8 / 주소 값은 항상 4바이트
7. 포인터는 사용하기 전에 반드시 초기화를 하여야 한다.(int * p = &i 또는 int * p / p = &i)
8. 간접 참조 연산자(*)는 포인터가 가리키는 위치의 내용을 추출하는 연산자이다.
9. 포인터는 변수이기 때문에 저장된 주소를 다른 값으로 얼마든지 변경할 수 있다.
10. 초기화가 안 된 상태에서 포인터를 사용하는 것은 위험하다. --> NULL포인터를 사용하자.
11. 포인터 타입과 변수의 타입은 일치하여야 한다.
12. 만약 double형 포인터에 int형 변수의 주소가 대입되면 이웃 바이트들을 덮어쓰게 된다.
13. 포인터에 증가 연산이니 ++를 적용하면 증가되는 값은 포인터가 가리키는 객체의 크기이다.(char --> 1. int --> 4, double --> 8)
15. 포인터가 가리키는 자료형의크기가 s일때, 포인터에 정수 n을 더하면 포인터의 값은 s * n 만큼 증가된다.
16. ++연산자를 사용할 경우는 기준 포인터의 값은 변경되지만 +연산을 사용할 경우는 기준 포인터의 값이 변경되지 않는다.
17. 다음 연산의 차이점을 분명히 해두자.
    + v = *p++ : p가 가리키는 값을 v에 대입한 후에 p를 증가한다.
    + v = (*p)++ : p가 가리키는 값을 v에 대입한 후에 p가 가리키는 값을 증가한다. 
    + v = *++p : p를 증가시킨 후에 p가 가리키는 값을 v에 대입한다. 
    + v = ++*p : p가 가리키는 값을 가져온 후에 그 값을 증가하여 v에 대입한다.
18. 포인터의 형변환 : 명시적 형변환(double형 포인터를 int형 포인터로 타입을 변경할 수 있다.)
19. 포인터와 배열의 관계 : 배열 이름이 바로 포인터이다. 배열 이름은 배열이 시작되는 주소와 같다.(a + i == &a[i], *(a + i) == a[i]) 
20. 배열의 이름이 포인터이기는 하지만 배열의 이름에다 다른 변수의 주소를 대입할 수 없다. 왜냐하면 배열의 이름은 포인터 상수이기 때문이다.
21. 포인터와 함수 : call-by-value(복사본이 전달) / call-by-reference(원본이 전달)
22. 함수의 반환 값으로도 포인터를 사용할 수 있는데, 함수가 종료되더라도 남아 있는 기억 장소의 변수를 반환해야 한다. 
23. 함수가 종료되면 사라지는 지역변수의 주소를 반환하면 안 된다.
<br><br>

## 예제
#### [오름차순, 내림차순 배열]

```angular2
#include <stdio.h>
#include <stdlib.h>
#include <time.h>
#define NUM 10 // Random 배열의 인덱스 변수

void merge_Upper(int *A, int *B, int *C) // 오름차순 정렬
{
	int i = 0, j = 0, k = 0;

	while(1)
	{
		if(A[i] < B[j]) // 만약 A의 원소가 작다면 
		{
			C[k] = A[i];  // C 배열에 A 원소 대입
			k++; 
			i++; 
		}
		else // 만약 B의 원소가 작다면
		{
			C[k] = B[j]; // C 배열에 B 원소 대입
			k++;  
			j++;
		}

		if(i == 5) // A 배열의 인덱스 값이 5이면(모두 사용했을 때)
		{
			for( ; k < 10; k++) 
			{
				C[k] = B[j]; // C 배열의 나머지 부분을 B 배열의 원소로 채운다 
				j++;
			}
			break;
		}

		if(j == 5) // B 배열의 인덱스 값이 5이면(모두 사용했을 때)
		{
			for( ; k < 10; k++)
			{
				C[k] = A[i]; // C 배열의 나머지 부분을 A 배열의 원소로 채운다 
				i++;
			}
			break;
		}

	}
}

void merge_Lower(int *A, int *B, int *C) // 내림차순 정렬
{
	int i = 0, j = 0, k = 0;

	while(1)
	{
		if(A[i] > B[j]) // 만약 B의 원소가 작다면 
		{
			C[k] = A[i];  // C 배열에 A 원소 대입
			k++; 
			i++; 
		}
		else // 만약 A의 원소가 작다면
		{
			C[k] = B[j]; // C 배열에 B 원소 대입
			k++;  
			j++;
		}

		if(i == 5) // A 배열의 인덱스 값이 5이면(모두 사용했을 때)
		{
			for( ; k < 10; k++) 
			{
				C[k] = B[j]; // C 배열의 나머지 부분을 B 배열의 원소로 채운다 
				j++;
			}
			break;
		}

		if(j == 5) // B 배열의 인덱스 값이 5이면(모두 사용했을 때)
		{
			for( ; k < 10; k++)
			{
				C[k] = A[i]; // C 배열의 나머지 부분을 A 배열의 원소로 채운다 
				i++;
			}
			break;
		}

	}
}

int main(void)
{
	int Random[NUM] = { 0 };
	int check[10] = { 0 }; // check[10] 배열로 Random[NUM] 배열의 원소가 중복되는지 확인 
	int A[5] = { 0 };
	int B[5] = { 0 };
	int C[10] ={ 0 };
	int i = 0, j = 0, temp, random; // i와 j는 인덱스 변수, temp는 버블 정렬을 위한 swap 변수, random은 Random[NUM] 배열의 원소를 정하기 위한 변수
	int choice; // 오름차순 또는 내림차순
	srand((unsigned int)time(NULL));

	// Random[NUM] 배열의 원소 정하기(0부터 9까지 중복 없는 난수 배열)
	for(i = 0; i < NUM; )
	{
		random = rand() % 10; // 0부터 9까지 난수 발생
		if(check[random] == 0)
		{
			check[random] = 1; // 1의 값을 넣어주어 중복되지 않게 해줌
			Random[i] = random;
			i++;
		}
	}

	// Random[NUM] 배열을 반으로 잘라 A[5]와 B[5] 배열에 각각 원소를 넣어주기

	for(i = 0; i < 5; i++)
	{
		A[j] = Random[i];
		j++;
	}

	j = 0;

	for(i = 5; i < 10; i++)
	{
		B[j] = Random[i];
		j++;
	}

	// Random, A, B 배열 출력하기

	printf("Step 1. Random 배열 출력\n");
	printf("→ ");

	for(i = 0; i < 10; i++)
		printf("%d ",Random[i]);
	printf("\n\n");

	printf("Step 2. A 배열 출력\n");
	printf("→ ");

	for(i = 0; i < 5; i++)
		printf("%d ",A[i]);
	printf("\n\n");

	printf("Step 3. B 배열 출력\n");
	printf("→ ");

	for(i = 0; i < 5; i++)
		printf("%d ",B[i]);
	printf("\n\n");

	printf("어떤 정렬을 원하시나요?\n");
	printf("1.오름차순 정렬\n");
	printf("2.내림차순 정렬\n");
	printf("→ ");
	scanf("%d",&choice);
	printf("\n");

	switch(choice)
	{
	case 1:
		printf("*****오름차순 정렬*****\n\n");
		printf("Step 4. 정렬된 A 배열 출력}\n");
		printf("→ ");

		for(i = 0; i < 5; i++)
		{	
			for(j = i; j < 5; j++)
			{
				if(A[i] > A[j])
				{ 
					temp = A[i];
					A[i] = A[j];
					A[j] = temp;
				}
				else
					continue;
			}
		}	

		for(i = 0; i< 5; i++)
			printf("%d ",A[i]);
		printf("\n\n");

		printf("Step 5. 정렬된 B 배열 출력\n");
		printf("→ ");

		for(i = 0; i < 5; i++)
		{	
			for(j = i; j < 5; j++)
			{
				if(B[i] > B[j])
				{ 
					temp = B[i];
					B[i] = B[j];
					B[j] = temp;
				}
				else
					continue;
			}
		}	

		for(i = 0; i< 5; i++)
			printf("%d ",B[i]);
		printf("\n\n");

		// merge_Upper 함수를 이용해 C[10] 배열 출력하기

		printf("Step 6. 정렬된 C 배열 출력\n");
		printf("→ ");
		merge_Upper(A,B,C);

		for(i = 0; i < 10; i++)
			printf("%d ",C[i]);
		printf("\n\n");

		break;

	case 2:
		printf("*****내림차순 정렬*****\n\n");
		printf("Step 4. 정렬된 A 배열 출력}\n");
		printf("→ ");

		for(i = 0; i < 5; i++)
		{	
			for(j = i; j < 5; j++)
			{
				if(A[i] < A[j])
				{ 
					temp = A[i];
					A[i] = A[j];
					A[j] = temp;
				}
				else
					continue;
			}
		}	

		for(i = 0; i< 5; i++)
			printf("%d ",A[i]);
		printf("\n\n");

		printf("Step 5. 정렬된 B 배열 출력\n");
		printf("→ ");

		for(i = 0; i < 5; i++)
		{	
			for(j = i; j < 5; j++)
			{
				if(B[i] < B[j])
				{ 
					temp = B[i];
					B[i] = B[j];
					B[j] = temp;
				}
				else
					continue;
			}
		}	

		for(i = 0; i< 5; i++)
			printf("%d ",B[i]);
		printf("\n\n");

		// merge_Lower 함수를 이용해 C[10] 배열 출력하기

		printf("Step 6. 정렬된 C 배열 출력\n");
		printf("→ ");
		merge_Lower(A,B,C);

		for(i = 0; i < 10; i++)
			printf("%d ",C[i]);
		printf("\n\n");

		break;
	}

	return 0;
}
```
<br>

#### [달팽이 배열]
```angular2
#include <stdio.h>

int main(void)
{
	int Snail_Array[50][50] = {0}; //아직 동적할당을 안배웠기에 정적할당을 해줌
	int num, size, origin_num;
	// i와 j는 배열의 인덱스
	// k는 배열의 정수 값(1부터 num까지의 수)
	// t는 아래쪽으로 가는 코드의 인덱스 변수 
	// s는 왼쪽으로 가는 코드의 인덱스 변수 
	// u는 오른쪽으로 가는 코드의 인덱스 변수
	// w는 위쪽으로 가는 코드의 인덱스 변수
	int i = 0, j, k = 1, t = 0,s = 0, u = 0, w = 0;

	printf("숫자를 입력 : ");
	scanf("%d",&num);

	size = num * num; // size에 num*num의 값을 할당하여 증감연산으로 while문 탈출 조건을 생성해준다.
	origin_num = num;

	for(j=0; j<num; j++) //처음 오른쪽
	{
		Snail_Array[i][j] = k++;
		printf("%3d ", Snail_Array[i][j]);
		size--;
	}
	j--;k--;
	printf("\n");
	while(1)
	{	
		for(i=t; i<num; i++) //아래쪽
		{
			Snail_Array[i][j] = k++;
			printf("%3d ", Snail_Array[i][j]);
			size--;
		}
		i--;k--;t++;size++;
		printf("\n");
		if(size == 0)
			break;
		
		for(j=(num-1); j>=s; j--) //왼쪽
		{
			Snail_Array[i][j] = k++;
			printf("%3d ", Snail_Array[i][j]);
		        size--;
		}
		j++;k--;s++;size++;num--;
		printf("\n");
		if(size == 0)
			break;

		for(i=num; i>w; i--) //위쪽
		{

			Snail_Array[i][j] = k++;
			printf("%3d ", Snail_Array[i][j]);
			size--;
		}
		i++;k--;w++;size++;
		printf("\n");
		if(size == 0)
			break;

		for(j=u; j<num; j++) //오른쪽
		{
			Snail_Array[i][j] = k++;
			printf("%3d ", Snail_Array[i][j]);
			size--;
		}
		j--;k--;u++;size++;
		printf("\n");
		if(size == 0)
			break;
	}
	
	printf("\n");

	for(i=0; i<origin_num; i++)
	{
		for(j=0; j<origin_num; j++)
		{
			printf("%3d ", Snail_Array[i][j]);
		}
		printf("\n");
     }
	 
	return 0;
}
```

<응용>
```angular2
#include <stdio.h>
#include <Windows.h>

void Print(char arr[50][50],int length);

int main(void)
{
	char Snail_Array[50][50] = {" "}; //아직 동적할당을 안배웠기에 정적할당을 해줌
	int num, size, origin_num;
	// i와 j는 배열의 인덱스
	// k는 배열의 정수 값(1부터 num까지의 수)
	// t는 아래쪽으로 가는 코드의 인덱스 변수 
	// s는 왼쪽으로 가는 코드의 인덱스 변수 
	// u는 오른쪽으로 가는 코드의 인덱스 변수
	// w는 위쪽으로 가는 코드의 인덱스 변수
	int i = 0, j, t = 0,s = 0, u = 0, w = 0;

	printf("숫자를 입력 : ");
	scanf("%d",&num);

	size = num * num; // size에 num*num의 값을 할당하여 증감연산으로 while문 탈출 조건을 생성해준다.
	origin_num = num;

	for(j=0; j<num; j++) //처음 오른쪽
	{
		Snail_Array[i][j] = '*';
		Print(Snail_Array, origin_num);
		size--;
	}
	j--;

	while(1)
	{	
		for(i=t; i<num; i++) //아래쪽
		{
			Snail_Array[i][j] = '*';
			Print(Snail_Array, origin_num);
			size--;
		}
		i--;t++;size++;
		printf("\n");
		if(size == 0)
			break;
		
		for(j=(num-1); j>=s; j--) //왼쪽
		{
			Snail_Array[i][j] = '*';
			Print(Snail_Array, origin_num);
		    size--;
		}
		j++;s++;size++;num--;
		printf("\n");
		if(size == 0)
			break;

		for(i=num; i>w; i--) //위쪽
		{

			Snail_Array[i][j] = '*';
			Print(Snail_Array, origin_num);
			size--;
		}
		i++;w++;size++;
		printf("\n");
		if(size == 0)
			break;

		for(j=u; j<num; j++) //오른쪽
		{
			Snail_Array[i][j] = '*';
			Print(Snail_Array, origin_num);
			size--;
		}
		j--;u++;size++;
		printf("\n");
		if(size == 0)
			break;
	}
	 
	return 0;
}

void Print(char arr[50][50], int length)
{
        int i, j;

	for(i = 0; i < length; i++)
	{
		for(j = 0; j < length; j++)
		{
			printf("%3c ", arr[i][j]);
		}
		printf("\n");
	}

	Sleep(1000);
	system("cls");
}
``
