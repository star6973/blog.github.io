---
layout: post
title: "C Programming [Preprocessing and Bit]"
date: 2014-06-03 18:00:00-19:00:00
categories: Language
tag: Language
use_math: true
---

# 전처리 및 비트 필드

1. 전처리기 : 컴파일하기에 앞서서 소스 파일을 처리하는 컴파일러의 한 부분

2. 전처리기 지시자

        1) #define : 매크로 정의  </p>
        2) #include : 파일 포함  </p>
        3) #undef : 매크로 정의 해제  
        4) #if : 조건이 참일 경우  
        5) #else : 조건이 거짓일 경우  
        6) #endif : 조건 처리 문장 종료  
        7) #ifdef : 매크로가 정의되어 있는 경우  
        8) #ifndef : 매크로가 정의되어 있지 않은 경우  
        9) #line : 행번호 출력  
        10) #pragma : 시스템에 따라 의미가 다름  

3. 단순 매크로 : #define 문을 이용하여 숫자 상수를 기호 상수로 만든 것
   -> 사용 이유 : 가독성을 높여준다, 상수의 변경이 용이하다  
   
        #define getchar() -> getc(stdin)
        #define putchar() -> putc(stdout)

4. 함수 매크로 : 매크로가 함수처럼 매개 변수를 가지는 것  
    + 주의해야 할 점  
        1) 매개 변수의 자료형을 쓰지 않는다.
        2) 매개 변수들을 괄호로 묶어주어야 한다.
        3) 매개 변수는 모두 사용되어야 한다.
        4) 매크로 이름과 괄호 사이에 공백이 있으면 안 된다.

                #define ADD (x, y) ((x) + (y))  

    + 참고사항 : 매크로를 한 줄 이상으로 만들 때 \를 사용 한다.  
        ex) #define SQUARE(x)  ((x) * (x)) 

            x = 2;
            SQUARE(++x); // 16이 출력
            // ++x * ++x로 두 번 증가한 뒤 연산이 되기 때문이다.

   + 매크로에 전달된 실제 인수를 문자열로 바꾸는 방법 : 매개변수 앞에 #붙이기  
        1) <P># : 문자열 변환 연산자</P> 
        2) <P>### : 토큰 병합 연산자 - 문자열끼리 결합해서 이어서 써줌</P>
         
5. 내장 매크로 
   1. _DATE_ : 소스가 컴파일된 날짜(월 일 년)로 치환된다.
   2. _TIME_ : 소스가 컴파일된 시간(시:분:초)로 치환된다.
   3. _LINE_ : 소스 파일에서의 현재의 라인 번호로 치환된다.
   4. _FILE_ : 소스 파일 이름으로 치환된다.
   
6. ASSERT 매크로 : 프로그램을 디버깅할 때 사용

7. 비트 관련 매크로

   1. <p>#define GET_BIT(w, k) (((w) >> (k)) & 0x01)</p>
      // 변수의 w의 k번째 비트 값 반환
 
   2. <p>#define SET_BIT_ON(w, k) ((w) |= (0x01 << (k)))</p>
      // 변수의 w의 k번째 비트 값을 1로 설정

   3. <p>#define SET_BIT_OFF(w, k) ((w) & ~(0x01 << (k)))</p>
      // 변수의 w의 k번째 비트 값을 0으로 설정

8. 함수 매크로 vs 함수
   1. 매크로 장점 : 속도가 빠르다.  
   2. 매크로 단점 : 코드의 길이가 제한이 된다.  

9. 조검부 컴파일 : 어떤 조건이 만족되는 경우에만 지정된 소스 코드 블록을 컴파일 하는 것. 
    1. <P>#ifdef ~ #endif 사이에 있는 모든 문장들을 컴파일</P>
         ex1)
    
            #define DEBUG // DEBUG가 정의
            
            int aberage(int x, int y) {
               #ifdef DEBUG
                   printf("x=%d, y=%d\n", x, y);
               #endif
                   return (x+y)/2;
            } // 색깔 표시된 부분이 컴파일 포함

            -> 만약 #define으로 DEBUG를 정의해주지 않으면 표시된 부분은 컴파일 포함하지 않는다.
    
    2. <P>#ifndef ~ #endif // 어떤 매크로가 정의되어 있지 않으면 저 사이의 문장이 컴파일에 포함된다</P>
         ex2)
    
            #ifndef LIMIT // LIMIT가 정의되어 있지 않으면
            #define LIMIT 1000 // LIMIT을 정의해준다.
            #endif

    3. <p>#undef // 매크로의 정의를 취소한다</p>

10. <p>#if, #else, #endif</p>

        1. #if 조건 
              문장들
           #endif
        
            -> 매크로의 값이 참이면 #if와 #endif 사이에 있는 모든 문장들을 컴파일
        
        2. #if 조건1
              문장1
           #elif 조건2
              문장2
           #else 
              문장3
           #endif
        
            -> 매크로의 값이 참이면 문장 1을 컴파일, 거짓이면 문장 2를 컴파일
                * 조건에서 매크로를 실수나 문자열은 비교할 수 없다.

11. 다중 소스 파일과 외부 변수
    1. 외부 변수 : extern
    2. 여러 소스 파일로 만드는 편이 소스를 유지, 보수, 관리하기가 편하다  
    
            #ifndef RECT_H // 만약 RECT_H라는 기호 상수가 아직까지 정의되지
                               않았다면 아래를 컴파일하라. 
            #define RECT_H // 처음 컴파일 되어 실행이 되고 그 다음에 정의되어
                               도 그냥 지나감
            struct rect { 
               int x, y, w, h;
            }
            typedef struct rect RECT;
            
            void draw_rect(const RECT *);
            double calc_area(const RECT *);
            void move_rect(RECT *, int, int);
            #endif
   
12. 비트 필드 구조체  
   : 구조체의 일종으로서 멤버들의 크기가 비트 단위로 나누어져 있는 구조체  
   : 비트들을 멤버로 가지는 구조체  
   : 비트 필드의 크기는 멤버 이름 다음 (:)을 사용한다.  
   : 자료형은 unsigned형이나 unsigned int형을 사용해야 한다.  

    * 주의 해야 할 점 
      1. 비트 필드 멤버 이름은 생략 가능
      2. 이름이 없는 비트 필드 중에서 크기가 0인 비트 필드를 둘 수 있음
         // 현재 워드의 남아있는 비트를 버림
      3. 비트 필드의 크기는 워드의 크기(32)를 넘어설 수 없음
      4. 비트 필드 구조체 안에 일반 멤버도 같이 선언 가능
    
    * 사용 이유
      1. 데이터를 저장할 때 저장 장소를 절약하기 위해
      2. 하드웨어를 제어하기 위해
<br><br>

## 예제 
#### [짝수 마방진(4의 배수)]

```angular2
#include <stdio.h>
#define DEBUG
#define INCLINE(i) i++ // 행, 열의 증가
#define RAISE(num) ++num // 원소의 증가

#define METHOD_ONE(row, col, p, q, r) ((row == p) && (col >= q && col <= r))
#define METHOD_TWO(row, col, p, q, r) ((row >= q && row <= r) && (col == p))

int main(void) {
    int n; // 몇 차 행렬을 만들지 정해주는 변수
	int magicSquare[100][100] = { 0 };
	int row, col; // 각각 행, 열
	int p, q, r; // 대칭을 이루어 주기 위한 범위의 변수(밑에 부가 설명)
	int magicNum = 1; // n x n 
	int magicCnt = 0; // 시행 횟수 - 4x4 행렬은 8번, 8x8 행렬은 32번, 16x16 행렬은 128번 ... 
	int count; // 몇 번 시행할지(while문 탈출 조건)
	int remainder; // 위의 탈출 조건에 보조 역할
	int temp; // 바꿔줄 때 필요한 변수

#ifdef DEBUG
#ifdef INCLINE
#ifdef RAISE
#ifdef METHOD_ONE
#ifdef METHOD_TWO

    // 사용자로부터 몇 차 행렬을 만들지 정하기(4의 배수 마방진만)
	printf("input(Fourth number): "); scanf("%d", &n);

	// 배열의 모든 원소는 n * n까지 순차적으로 초기화
	for(row = 0; row < n; INCLINE(row))
	{
		for(col = 0; col < n; INCLINE(col))
		{
			magicSquare[row][col] = magicNum;
			RAISE(magicNum);
		}
	}

	// 우선, 제가 생각한 알고리즘은 대칭을 이용하였습니다. 각 차수 행렬마다 쓰이는 원소가 정해진 패턴이 있고
	// 정방행렬이라는 것에 중점을 둔다면 대각선을 기준으로 대칭을 사용하면 쉽게 풀 수 있습니다. 
	// 4x4 행렬의 경우, row = 0일때, 1<= col <=2의 범위와 1<= col <= 2일때, col = 0인 범위에서만 행렬의
        // 행렬의 대칭이 이루어집니다. 또, 8x8 행렬의 경우, row = 0일때, 2<= col <= 5의 범위와 row = 1일때, 2<= col <= 5의 범위와
	// 2 <= row <= 5일때, col = 0의 범위, 2 <= row <= 5일때, col = 1의 범위에서 가능합니다. 마찬가지로 12차 행렬에서도 
	// 이런 식으로 규칙을 찾아볼 수 있습니다. 따라서 row나 col의 범위를 지정해주기 위해서 p와 q의 변수를 선언해주었습니다.
	
	row = 0;
	col = 0;
	p = 0;
	q = n / 4; // 규칙을 통해 알아냄
	r = n - (q + 1); // 규칙을 통해 알아냄 
	remainder = n / 4; // 규칙을 통해 알아냄
	count = n * remainder; // 규칙을 통해 알아냄

	while(magicCnt != count)
	{
		for(p = 0; p < n / 4; RAISE(p))
		{
			for(row = 0; row < n; INCLINE(row))
			{
				for(col = 0; col < n; INCLINE(col))
				{
					if(METHOD_ONE(row, col, p, q, r) || METHOD_TWO(row, col, p, q, r))
					{
						printf("row:%d , col:%d, count:%d\n", row, col, magicCnt); // 현재 상황 출력
						// swap하기
						temp = magicSquare[row][col];
						magicSquare[row][col] = magicSquare[(n - row) - 1][(n - col) - 1];
						magicSquare[(n - row) - 1][(n - col) - 1] = temp;
						RAISE(magicCnt);
					}
				}
			}
		}
	}

	printf("\n");

	for(row = 0; row < n; INCLINE(row))
	{
		for(col = 0; col < n; INCLINE(col))
		{
			printf("%3d ", magicSquare[row][col]);
		}
		printf("\n");
	}

#endif
#endif
#endif
#endif
#endif

	return 0;
} 
```

#### [짝수 마방진(4의 배수가 아닌 짝수 - 1)]
```angular2
#include <stdio.h>

int main(void)
{
	int magicSquare[100][100] = { 0 };
	int row, col;
	int n;
	int k;
	int temp;
	int magicNum = 1;

	printf("input(odd number): "); scanf("%d", &n);

	for(row = 0; row < n; row++)
	{
		for(col = 0; col < n; col++)
		{
			magicSquare[row][col] = magicNum;
			magicNum++;
		}
	}

	// k : 1 : 2k : 1 : k로 행과 열의 선을 구분

	k = (n - 2) / 4;

	// B와 H, D와 F의 원점 대칭

	for(row = 0; row  < n; row++)
	{
		for(col = 0; col < n; col++)
		{
			if(((row >= 0 && row <= k - 1) && (col >= k + 1 && col <= 3 * k)) || ((col >= 0 && col <= k - 1) && (row >= k+1 && row <= 3 * k))) 
			{
				temp = magicSquare[row][col];
				magicSquare[row][col] = magicSquare[(n - row) - 1][(n - col) - 1];
				magicSquare[(n - row) - 1][(n - col) - 1] = temp;
			}
		}
	}
	
	// 세로 경계선의 상하 반전(경계선 제외)
	
	col = 3 * k + 1;

	for(row = 0; row < n / 2; row++)
	{
		if(row == k)
		{
			continue;
		}

		temp = magicSquare[row][col];
		magicSquare[row][col] = magicSquare[(n - row) - 1][col];
		magicSquare[(n - row) - 1][col] = temp;
	}
	
	// 세로 경계선의 교차 지점의 안쪽 숫자들 좌우 반전

	for(row = 0; row < n; row++)
	{
		for(col = 0; col < n / 2; col++)
		{
			if((row >= k + 1 && row <= 3 * k) && col == k)
			{
				if(row == 2 * k + 1) // 경계선의 바로 밑에 칸만 제외
				{
					continue;
				}
				temp = magicSquare[row][col];
				magicSquare[row][col] = magicSquare[row][(n - col) - 1];
				magicSquare[row][(n - col) - 1] = temp;
			}
		}
	}
	
	// 가로 경계선 교차점 제외하고 좌우 반전

	for(row = 0; row < n; row++) // 행은 1행부터 n행까지 모두 쓰기에
	{
		for(col = 0; col < n / 2; col++) // 열은 반만 필요하므로
		{
			if(row == k || row == 3 * k + 1)
			{
				if(col == k || col == 3 * k + 1) // 경계선을 제외하고
				{
					continue;
				}

				temp = magicSquare[row][col];
				magicSquare[row][col] = magicSquare[row][(n - col) - 1];
				magicSquare[row][(n - col) - 1] = temp;
			}
		}
	}
	
	// k개의 칸만 제외하고 나머지 모두 상하 반전(맨 오른쪽 칸들을 기준으로 잡음)
	
	for(row = 0; row < n / 2; row++) // 행은 반만 필요하므로
	{
		for(col = 0; col < n; col++) // 열은 1열부터 n열까지 모두 쓰기에
		{
			if(row == k || row == 3 * k + 1)
			{
				if(col == k || (col >= 3 * k + 1 && col <= n - 1))
				{
					continue;
				}

				temp = magicSquare[row][col];
				magicSquare[row][col] = magicSquare[(n - row) - 1][col];
				magicSquare[(n - row) - 1][col] = temp;
			}
		}
	}
	
	// B영역의 첫 행을 좌우 반전, D영역과 F영역의 첫 행을 좌우 반전

	for(row = 0; row < n / 2; row++)
	{
		for(col = 0; col < n / 2; col++)
		{
			if((row == 0 && (col >= k + 1 && col <= 2 * k)) || (row == k + 1 && (col >= 0 && col <= k - 1)))
			{
				temp = magicSquare[row][col];
				magicSquare[row][col] = magicSquare[row][(n - col) - 1];
				magicSquare[row][(n - col) - 1] = temp;
			}
		}
	}
	
	// 출력

	for(row = 0; row < n; row++)
	{
		for(col = 0; col < n; col++)
		{
			printf("%3d ", magicSquare[row][col]);
		}
		printf("\n");
	}

	return 0;
}
```

#### [짝수 마방진(4의 배수가 아닌 짝수 - 2)]
```angular2
#include <stdio.h>
#define DEBUG
#define INCLINE(i) i++ // 행, 열의 증가
#define RAISE(num) ++num // 원소 증가

#define METHOD_ONE(row, col, k) ((row >= 0 && row <= k - 1) && (col >= k + 1 && col <= 3 * k))
#define METHOD_TWO(row, col, k) ((col >= 0 && col <= k - 1) && (row >= k+1 && row <= 3 * k))
#define METHOD_THREE(row, col, k) (row >= k + 1 && row <= 3 * k) && col == k
#define METHOD_FOUR(row, col, k) row == k || row == 3 * k + 1
#define METHOD_FIVE(row, col, k) col == k || col == 3 * k + 1
#define METHOD_SIX(row, col, k) col == k || (col >= 3 * k + 1 && col <= n - 1)
#define METHOD_SEVEN(row, col, k) (row == 0 && (col >= k + 1 && col <= 2 * k))
#define METHOD_EIGHT(row, col, k) (row == k + 1 && (col >= 0 && col <= k - 1))

int main(void)
{
	int magicSquare[100][100] = { 0 };
	int row, col;
	int n;
	int k;
	int temp;
	int magicNum = 1;

#ifdef DEBUG
#ifdef INCLINE
#ifdef RAISE
#ifdef METHOD_ONE
#ifdef METHOD_TWO
#ifdef METHOD_THREE
#ifdef METHOD_FOUR
#ifdef METHOD_FIVE
#ifdef METHOD_SIX
#ifdef METHOD_SEVEN
#ifdef METHOD_EIGHT

	printf("input(odd number): "); scanf("%d", &n);

	for(row = 0; row < n; INCLINE(row))
	{
		for(col = 0; col < n; INCLINE(col))
		{
			magicSquare[row][col] = magicNum;
			RAISE(magicNum);
		}
	}

	// k : 1 : 2k : 1 : k로 행과 열의 선을 구분

	k = (n - 2) / 4;

	// B와 H, D와 F의 원점 대칭

	for(row = 0; row  < n; INCLINE(row))
	{
		for(col = 0; col < n; INCLINE(col))
		{
			if(METHOD_ONE(row, col, k) || METHOD_TWO(row, col, k)) 
			{
				temp = magicSquare[row][col];
				magicSquare[row][col] = magicSquare[(n - row) - 1][(n - col) - 1];
				magicSquare[(n - row) - 1][(n - col) - 1] = temp;
			}
		}
	}
	
	// 세로 경계선의 상하 반전(경계선 제외)
	
	col = 3 * k + 1;

	for(row = 0; row < n / 2; INCLINE(row))
	{
		if(row == k)
		{
			continue;
		}

		temp = magicSquare[row][col];
		magicSquare[row][col] = magicSquare[(n - row) - 1][col];
		magicSquare[(n - row) - 1][col] = temp;
	}
	
	// 세로 경계선의 교차 지점의 안쪽 숫자들 좌우 반전

	for(row = 0; row < n; INCLINE(row))
	{
		for(col = 0; col < n / 2; INCLINE(col))
		{
			if(METHOD_THREE(row, col, k))
			{
				if(row == 2 * k + 1) // 경계선의 바로 밑에 칸만 제외
				{
					continue;
				}
				temp = magicSquare[row][col];
				magicSquare[row][col] = magicSquare[row][(n - col) - 1];
				magicSquare[row][(n - col) - 1] = temp;
			}
		}
	}
	
	// 가로 경계선 교차점 제외하고 좌우 반전

	for(row = 0; row < n; INCLINE(row)) // 행은 1행부터 n행까지 모두 쓰기에
	{
		for(col = 0; col < n / 2; INCLINE(col)) // 열은 반만 필요하므로
		{
			if(METHOD_FOUR(row, col,k))
			{
				if(METHOD_FIVE(row, col, k)) // 경계선을 제외하고
				{
					continue;
				}

				temp = magicSquare[row][col];
				magicSquare[row][col] = magicSquare[row][(n - col) - 1];
				magicSquare[row][(n - col) - 1] = temp;
			}
		}
	}
	
	// k개의 칸만 제외하고 나머지 모두 상하 반전(맨 오른쪽 칸들을 기준으로 잡음)
	
	for(row = 0; row < n / 2; INCLINE(row)) // 행은 반만 필요하므로
	{
		for(col = 0; col < n; INCLINE(col)) // 열은 1열부터 n열까지 모두 쓰기에
		{
			if(METHOD_FOUR(row, col, k))
			{
				if(METHOD_SIX(row, col, k))
				{
					continue;
				}

				temp = magicSquare[row][col];
				magicSquare[row][col] = magicSquare[(n - row) - 1][col];
				magicSquare[(n - row) - 1][col] = temp;
			}
		}
	}
	
	// B영역의 첫 행을 좌우 반전, D영역과 F영역의 첫 행을 좌우 반전

	for(row = 0; row < n / 2; INCLINE(row))
	{
		for(col = 0; col < n / 2; INCLINE(col))
		{
			if(METHOD_SEVEN(row, col, k) || METHOD_EIGHT(row, col, k))
			{
				temp = magicSquare[row][col];
				magicSquare[row][col] = magicSquare[row][(n - col) - 1];
				magicSquare[row][(n - col) - 1] = temp;
			}
		}
	}
	
	// 출력

	for(row = 0; row < n; INCLINE(row))
	{
		for(col = 0; col < n; INCLINE(col))
		{
			printf("%3d ", magicSquare[row][col]);
		}
		printf("\n");
	}
#endif
#endif
#endif
#endif
#endif
#endif
#endif
#endif
#endif
#endif
#endif
	return 0;
}
```

#### [홀수 마방진]
```angular2
// 마방진
// 가로, 세로, 대각선 모두 각각 더했을 때 같은 것
// 짝수 마방진 혹은 홀수 마방진만 하기
// 예시) 홀수 마방진
// 홀수 input : 3 -> 3 x 3 마방진

// 홀수 마방진 푸는 법
// 첫 번째 행의 한가운데 열에 1을 넣는다.
// 다음 숫자를 대각선 방향으로 오른쪽 위 칸에 넣는다. 
// 이때 해당하는 칸이 마방진의 위쪽으로 벗어난 경우에는 반대로 가장 아래쪽의 칸으로, 
// 마방진의 오른쪽으로 벗어나는 경우는 가장 왼쪽의 칸으로 각각 한번 더 이동한다. 
// 그리고 오른쪽인 동시에 위쪽으로 벗어나는 경우 및 오른쪽 위에 다른 숫자가 이미 있는 경우에는 오른쪽위 대신 원래 칸의 한 칸 아래에 넣는다

// input : 3
// 8 1 6
// 3 5 7
// 4 9 2

// input : 5
// 17  24   1   8  15
// 23   5   7  14  16
//  4   6  13  20  22 
// 10  12  19  21   3
// 11  18  25   2   9

// 예시) 짝수 마방진
// 짝수 input : 4 -> 4 x 4 마방진

// 짝수 마방진 푸는 법
// input이 4라면 1:2:1로, 8이라면 2:4:2로 설정(4의 배수만, 그 이외의 배수는 다른 방법으로)
// 가로, 세로를 1:2:1로 나누어서 순차적으로 넣은 수에 대해서 가장 끝에 숫자들만 냅두고 고정

// input : 4
//  1)  2   3  (4 
//  5  (6   7)   8
//  9 (10  11)  12
// 13) 14  15 (16

// 위와 같이 고정한 후, 2를 15와 바꾸고, 2을 14와, 5를 12와, 9를 8과 바꿔주면 된다
// 홀수 마방진
#include <stdio.h>
#define DEBUG
#define DECLINE(i) i-- // 행, 열의 증감
#define INCLINE(i) i++ // 행, 열의 증가
#define RAISE(num) ++num // 원소의 증가

#define METHOD_ONE(n, row, col) row < 0 || col > n - 1 // 행의 값이나 열의 값이 범위를 넘어갈 경우
#define METHOD_TWO(n, row, col) row < 0 && col < n // 행의 값만 범위를 넘어갈 경우
#define METHOD_THREE(n, row, col) row >= 0 && col > n - 1 // 열의 값만 범위를 넘어갈 경우
#define METHOD_FOUR(n, row, col) row < 0 && col > n - 1 // 행과 열의 값 모두 범위를 넘어갈 경우

int main(void) {
	int n; // 몇 차 행렬을 만들지 정해주는 변수
	int magicSquare[10][10] = { 0 };
	int row, col;
	int magicNum = 1; // n x n 

    // 사용자로부터 몇 차 행렬을 만들지 정하기(홀수 마방진만)
	printf("input(odd number): "); scanf("%d", &n);
	// 첫 번째 행의 가운데 원소는 1
	row = 0;
	col = n / 2;
	magicSquare[row][col] = magicNum;

	while(magicNum <= n * n) // magicNum > 9
	{
#ifdef DECLINE
#ifdef INCLINE
#ifdef RAISE
		DECLINE(row); INCLINE(col);
		if(METHOD_ONE(n, row, col) || magicSquare[row][col] != 0) // 매크로 METHOD_ONE 실행 혹은 숫자가 채워져 있는 경우
		{
			if(METHOD_TWO(n, row, col)) // 매크로 METHOD_TWO 실행
			{
				row = n - 1; // 행이 가장 마지막으로 이동
			    magicSquare[row][col] = RAISE(magicNum);
			}
			else if(METHOD_THREE(n, row, col)) // 매크로 METHOD_THREE 실행
			{
				col = 0;
				magicSquare[row][col] = RAISE(magicNum);
			}
			else if(magicSquare[row][col] != 0) // 숫자가 채워져 있는 경우
			{
				INCLINE(row); INCLINE(row); DECLINE(col); // 바로 밑에 칸에 숫자를 넣어줌
				magicSquare[row][col] = RAISE(magicNum);
			}
			else if(METHOD_FOUR(n, row, col)) // 매크로 METHOD_FOUR 실행
			{
				INCLINE(row); INCLINE(row); DECLINE(col); // 바로 밑에 칸에 숫자를 넣어줌
				magicSquare[row][col] = RAISE(magicNum);
			}
			else
			{
				continue;
			}
		}
		else
		{
			magicSquare[row][col] = RAISE(magicNum);
		}
	}
#endif
#endif
#endif

#ifdef DEBUG
	for(row = 0; row < n; INCLINE(row))
	{
		for(col = 0; col < n; INCLINE(col))
		{
			printf("%3d ", magicSquare[row][col]);
		}
		printf("\n");
	}
#endif

	return 0;
}
```

#### [마방진 최종]
```angular2
#include <stdio.h>

#define DEBUG
#define DECLINE(i) i-- // 행, 열의 증감
#define INCLINE(i) i++ // 행, 열의 증가
#define RAISE(num) ++num // 원소의 증가
// 홀수 마방진 메소드
#define METHOD_ONE(n, row, col) row < 0 || col > n - 1 // 행의 값이나 열의 값이 범위를 넘어갈 경우
#define METHOD_TWO(n, row, col) row < 0 && col < n // 행의 값만 범위를 넘어갈 경우
#define METHOD_THREE(n, row, col) row >= 0 && col > n - 1 // 열의 값만 범위를 넘어갈 경우
#define METHOD_FOUR(n, row, col) row < 0 && col > n - 1 // 행과 열의 값 모두 범위를 넘어갈 경우
// 4의 배수 짝수 마방진 메소드
#define METHOD_FIVE(row, col, p, q, r) ((row == p) && (col >= q && col <= r))
#define METHOD_SIX(row, col, p, q, r) ((row >= q && row <= r) && (col == p))
// 4의 배수가 아닌 짝수 마방진 메소드
#define METHOD_SEVEN(row, col, k) ((row >= 0 && row <= k - 1) && (col >= k + 1 && col <= 3 * k))
#define METHOD_EIGHT(row, col, k) ((col >= 0 && col <= k - 1) && (row >= k+1 && row <= 3 * k))
#define METHOD_NINE(row, col, k) (row >= k + 1 && row <= 3 * k) && col == k
#define METHOD_TEN(row, col, k) row == k || row == 3 * k + 1
#define METHOD_ELEVEN(row, col, k) col == k || col == 3 * k + 1
#define METHOD_TWELVE(row, col, k) col == k || (col >= 3 * k + 1 && col <= n - 1)
#define METHOD_THIRTEEN(row, col, k) (row == 0 && (col >= k + 1 && col <= 2 * k))
#define METHOD_FOURTEEN(row, col, k) (row == k + 1 && (col >= 0 && col <= k - 1))

void oddMagicSquare(int m[][100], int, int, int, int);
void fourthEvenMagicSquare(int m[][100], int, int, int, int);
void notFourthEvenMagicSquare(int m[][100], int, int, int, int);
void PrintMagicSquare(int m[][100], int, int, int);

int main(void) {
	int n; // 몇 차 행렬을 만들지 정해주는 변수
	int magicSquare[100][100] = { 0 };
	int row = 0, col = 0; // 각각 행, 열
	int magicNum = 1; // n x n 
	

    // 사용자로부터 몇 차 행렬을 만들지 정하기
	printf("input number: "); scanf_s("%d", &n);

	if(n % 2 != 0) {
		oddMagicSquare(magicSquare, row, col, n, magicNum);
		PrintMagicSquare(magicSquare, row, col, n);
	}
	else if(n % 4 == 0) {
		fourthEvenMagicSquare(magicSquare, row, col, n, magicNum);
		PrintMagicSquare(magicSquare, row, col, n);
	}
	else {
		notFourthEvenMagicSquare(magicSquare, row, col, n, magicNum);
		PrintMagicSquare(magicSquare, row, col, n);
	}

	return 0;
}
// 홀수 마방진
void oddMagicSquare(int magicSquare[][100], int row, int col, int n, int magicNum) { 
	row = 0; // 첫 번째 행의 가운데 원소는 1
	col = n / 2;
	magicSquare[row][col] = magicNum;

	while(magicNum <= n * n) // magicNum > 9
	{
#ifdef DECLINE
#ifdef INCLINE
#ifdef RAISE
		DECLINE(row); INCLINE(col);
		if(METHOD_ONE(n, row, col) || magicSquare[row][col] != 0) // 매크로 METHOD_ONE 실행 혹은 숫자가 채워져 있는 경우
		{
			if(METHOD_TWO(n, row, col)) // 매크로 METHOD_TWO 실행
			{
				row = n - 1; // 행이 가장 마지막으로 이동
			    magicSquare[row][col] = RAISE(magicNum);
			}
			else if(METHOD_THREE(n, row, col)) // 매크로 METHOD_THREE 실행
			{
				col = 0;
				magicSquare[row][col] = RAISE(magicNum);
			}
			else if(magicSquare[row][col] != 0) // 숫자가 채워져 있는 경우
			{
				INCLINE(row); INCLINE(row); DECLINE(col); // 바로 밑에 칸에 숫자를 넣어줌
				magicSquare[row][col] = RAISE(magicNum);
			}
			else if(METHOD_FOUR(n, row, col)) // 매크로 METHOD_FOUR 실행
			{
				INCLINE(row); INCLINE(row); DECLINE(col); // 바로 밑에 칸에 숫자를 넣어줌
				magicSquare[row][col] = RAISE(magicNum);
			}
			else
			{
				continue;
			}
		}
		else
		{
			magicSquare[row][col] = RAISE(magicNum);
		}
	}
#endif
#endif
#endif
}
// 4의 배수 짝수 마방진
void fourthEvenMagicSquare(int magicSquare[][100], int row, int col, int n, int magicNum) {
	int p, q, r; // 대칭을 이루어 주기 위한 범위의 변수(밑에 부가 설명)
	int magicCnt = 0; // 시행 횟수 - 4x4 행렬은 8번, 8x8 행렬은 32번, 16x16 행렬은 128번 ... 
	int count; // 몇 번 시행할지(while문 탈출 조건)
	int remainder; // 위의 탈출 조건에 보조 역할
	int temp; // 바꿔줄 때 필요한 변수

#ifdef DEBUG
#ifdef INCLINE
#ifdef RAISE
#ifdef METHOD_FIVE
#ifdef METHOD_SIX

	// 배열의 모든 원소는 n * n까지 순차적으로 초기화
	for(row = 0; row < n; INCLINE(row))
	{
		for(col = 0; col < n; INCLINE(col))
		{
			magicSquare[row][col] = magicNum;
			RAISE(magicNum);
		}
	}

	row = 0;
	col = 0;
	p = 0;
	q = n / 4; // 규칙을 통해 알아냄
	r = n - (q + 1); // 규칙을 통해 알아냄 
	remainder = n / 4; // 규칙을 통해 알아냄
	count = n * remainder; // 규칙을 통해 알아냄

	while(magicCnt != count)
	{
		for(p = 0; p < n / 4; RAISE(p))
		{
			for(row = 0; row < n; INCLINE(row))
			{
				for(col = 0; col < n; INCLINE(col))
				{
					if(METHOD_FIVE(row, col, p, q, r) || METHOD_SIX(row, col, p, q, r))
					{
						// swap하기
						temp = magicSquare[row][col];
						magicSquare[row][col] = magicSquare[(n - row) - 1][(n - col) - 1];
						magicSquare[(n - row) - 1][(n - col) - 1] = temp;
						RAISE(magicCnt);
					}
				}
			}
		}
	}
#endif
#endif
#endif
#endif
#endif
}
// 4의 배수가 아닌 짝수 마방진
void notFourthEvenMagicSquare(int magicSquare[][100], int row, int col, int n, int magicNum) {
	int k;
	int temp;

#ifdef INCLINE
#ifdef RAISE
#ifdef METHOD_SEVEN
#ifdef METHOD_EIGHT
#ifdef METHOD_NINE
#ifdef METHOD_TEN
#ifdef METHOD_ELEVEN
#ifdef METHOD_TWELVE
#ifdef METHOD_THIRTEEN
#ifdef METHOD_FOURTEEN

	for(row = 0; row < n; INCLINE(row))
	{
		for(col = 0; col < n; INCLINE(col))
		{
			magicSquare[row][col] = magicNum;
			RAISE(magicNum);
		}
	}

	// k : 1 : 2k : 1 : k로 행과 열의 선을 구분

	k = (n - 2) / 4;

	// B와 H, D와 F의 원점 대칭

	for(row = 0; row  < n; INCLINE(row))
	{
		for(col = 0; col < n; INCLINE(col))
		{
			if(METHOD_SEVEN(row, col, k) || METHOD_EIGHT(row, col, k)) 
			{
				temp = magicSquare[row][col];
				magicSquare[row][col] = magicSquare[(n - row) - 1][(n - col) - 1];
				magicSquare[(n - row) - 1][(n - col) - 1] = temp;
			}
		}
	}
	
	// 세로 경계선의 상하 반전(경계선 제외)
	
	col = 3 * k + 1;

	for(row = 0; row < n / 2; INCLINE(row))
	{
		if(row == k)
		{
			continue;
		}

		temp = magicSquare[row][col];
		magicSquare[row][col] = magicSquare[(n - row) - 1][col];
		magicSquare[(n - row) - 1][col] = temp;
	}
	
	// 세로 경계선의 교차 지점의 안쪽 숫자들 좌우 반전

	for(row = 0; row < n; INCLINE(row))
	{
		for(col = 0; col < n / 2; INCLINE(col))
		{
			if(METHOD_NINE(row, col, k))
			{
				if(row == 2 * k + 1) // 경계선의 바로 밑에 칸만 제외
				{
					continue;
				}
				temp = magicSquare[row][col];
				magicSquare[row][col] = magicSquare[row][(n - col) - 1];
				magicSquare[row][(n - col) - 1] = temp;
			}
		}
	}
	
	// 가로 경계선 교차점 제외하고 좌우 반전

	for(row = 0; row < n; INCLINE(row)) // 행은 1행부터 n행까지 모두 쓰기에
	{
		for(col = 0; col < n / 2; INCLINE(col)) // 열은 반만 필요하므로
		{
			if(METHOD_TEN(row, col,k))
			{
				if(METHOD_ELEVEN(row, col, k)) // 경계선을 제외하고
				{
					continue;
				}

				temp = magicSquare[row][col];
				magicSquare[row][col] = magicSquare[row][(n - col) - 1];
				magicSquare[row][(n - col) - 1] = temp;
			}
		}
	}
	
	// k개의 칸만 제외하고 나머지 모두 상하 반전(맨 오른쪽 칸들을 기준으로 잡음)
	
	for(row = 0; row < n / 2; INCLINE(row)) // 행은 반만 필요하므로
	{
		for(col = 0; col < n; INCLINE(col)) // 열은 1열부터 n열까지 모두 쓰기에
		{
			if(METHOD_TEN(row, col, k))
			{
				if(METHOD_TWELVE(row, col, k))
				{
					continue;
				}

				temp = magicSquare[row][col];
				magicSquare[row][col] = magicSquare[(n - row) - 1][col];
				magicSquare[(n - row) - 1][col] = temp;
			}
		}
	}
	
	// B영역의 첫 행을 좌우 반전, D영역과 F영역의 첫 행을 좌우 반전

	for(row = 0; row < n / 2; INCLINE(row))
	{
		for(col = 0; col < n / 2; INCLINE(col))
		{
			if(METHOD_THIRTEEN(row, col, k) || METHOD_FOURTEEN(row, col, k))
			{
				temp = magicSquare[row][col];
				magicSquare[row][col] = magicSquare[row][(n - col) - 1];
				magicSquare[row][(n - col) - 1] = temp;
			}
		}
	}

#endif
#endif
#endif
#endif
#endif
#endif
#endif
#endif
#endif
#endif
}
// 출력 함수
void PrintMagicSquare(int magicSquare[][100], int row, int col, int n) {
#ifdef DEBUG
	for(row = 0; row < n; INCLINE(row))
	{
		for(col = 0; col < n; INCLINE(col))
		{
			printf("%3d ", magicSquare[row][col]);
		}
		printf("\n");
	}
#endif
}
```
