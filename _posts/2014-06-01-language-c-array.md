---
layout: post
title: "C Programming [Array]"
date: 2014-06-01 10:00:00-12:00:00
categories: Language
tag: Language
---

# 배열

1. 배열은 동일한 타입의 데이터가 여러 개 저장되어 있는 데이터 저장 장소이다.
2. 배열을 사용하면 같은 종류의 대량의 데이터를 효율적이고 간편하게 처리할 수 있다.
3. 배열에 저장된 데이터들을 배열 원소 또는 배열 요소라고 한다.
4. 배열 원소에는 번호가 붙어 있는데 이를 인덱스 또는 첨자라고 한다.
5. 배열의 크기를 나타내는 기호 상수를 정의한다.(ex : #define SIZE 5) 이렇게 정의하는 것이 편리하다.
6. 인덱스가 배열의 크기를 벗어나게 되면 프로그램에 치명적인 오류를 발생시킨다.
7. 초기화만 하고 배열의 크기를 비워놓으면 컴파일러가 자동으로 초기 값들의 개수만큼의 배열 크기를 잡는다.
8. 배열 요소의 개수를 계산하는 방법은 sizeof연산자를 이용한다.
9. 배열의 크기 = sizeof(배열) / sizeof(배열 원소)
10. 배열의 복사나 비교를 하기 위해서는 각 요소별로 복사 및 비교를 해야한다. 이름으로 사용하면 안된다.
11. 배열의 이름은 배열이 저장된 메모리의 주소와 같다. 따라서 주소를 비교하면 서로 다를 수 밖에 없다.
12. 배열이 인수인 경우에는 배열의 원본이 매개 변수를 통하여 전달된다. 
13. 배열은 원본이 전달되지만 배열 원소는 복사본이 전달된다.
14. 원본 배열의 변경을 금지하는 방법은 const 지정자를 사용하는 것이다.
15. 배열의 응용 - 선택 정렬, 순차 탐색, 이진 탐색
16. 2차원 배열은 행과 열을 나타내는 2개의 인덱스를 가진다. int arr[가로(행)][세로(열)]
17. 가로(행)의 개수는 지정하지 않아도 되지만 세로(열)의 개수는 반드시 지정한다.
<br><br>

## 예제
#### [10진수를 2진수로 변환하기]

```angular2
#include <stdio.h>

int main(void)
{
	int binary[32]; //2진수 출력 변수
	int n; //10진수 입력 변수
	int i; //binary배열의 인덱스 변수

	printf("10진수 입력:");
	scanf("%d",&n);

	for(i = 0; i < 32 && n > 0; i++)
	{
		binary[i] = n % 2; //2로 나눈 나머지
		n = n / 2;
		
		if(n == 0 || i == 32)
		{
			for(int j = i + 1; j < 32; j++)
				binary[j] = 0;

			break;
		}
		
	}

	for(int i = 31; i >= 0; i--)
		printf("%d",binary[i]);

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
<br>

#### [응용]
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
```
<br>
