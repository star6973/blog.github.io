---
layout: post
title: "C Programming [String]"
date: 2014-06-02 15:00:00
categories: Language
tag: Language
---

# 문자열

1. 문자열의 끝은 반드시 NULL 문자로 표시해주어야 한다. 

2. NULL 문자의 필요성 : 정상적인 데이터와 쓰레기값을 분리하기 위해서이다. 문자열의 끝을 알아야 만이 반복을 끝낼 수 있다.

3. NULL 문자 = 아스키코드 ‘\0’ = 숫자 0

4. 문자 배열의 크기는 초기화 문자열의 길이보다 커야 한다. 마지막에 NULL 문자가 들어가야 하기 때문이다. 

5. 문자열의 길이는 라이브러리 함수 strlen() 함수를 이용하면 알 수 있다.

6. 문자열 초기화
    1) char str[3] = { 'a', 'b', 'c', '\0' };
    2) char str[3] = "abc";
    3) char str[3] = "";
    4) char str[] = "abc";
 
7. 문자열의 변경  
    1) 개별적으로 대입하는 방법  
    2) 라이브러리 함수 strcpy()를 이용하는 방법  
        ☆ 중요 ☆ 배열의 이름은 배열을 가리키는 주소로서 변경이 불가능하다.  

8. 포인터 변수 = 문자열 상수 / 문자열 배열 = 포인터 상수  
   - 주소값을 나타내기 위한 방법  
    1) int, double, char과 같은 일반 변수  
	=> ＆연산자를 붙여준다 -> %d(10진 정수형), %u(부호 없는 10진 정수형), %p(16진수형)으로 표현이 가능하다.<br>
	
    2) int *, double *, char *과 같은 포인터 변수  
       	=> & 연산자 안붙여준다 -> %d(10진 정수형), %u(부호 없는 10진 정수형), %p(16진수형)으로 표현이 가능하다.<br>
       
9. 문자열 상수는 프로그램 소스 안에 포함된 문자열을 의미한다. 문자열 상수는 ‘텍스트 세그먼트(text segment)’라고 불리는 특수한 메모리 영역에 저장된다. 읽기는 가능하지만 변경할 수 없는 메모리 영역이다.

10. 포인터 변수는 ‘데이터 세그먼트(data segment)’라고 불리는 영역에 저장되어 값을 변경할 수 있다.  
    ex1)
    
        char *p = "HelloWorld";
        strcpy(p, "GoodBye");     ---> 실행 오류

    ex2) 
        
        char *p = "HelloWorld";
        p = "GoodBye";     ---> 실행 가능 
   
    ex3)　
    
        char p[] = "HelloWorld";
        strcpy(p, "GoodBye");     ---> 실행 가능
        
    ex4) 
    
        char p[] = "HelloWorld";
        p = "GoodBye";     ---> 실행 오류

11. 문자 처리 라이브러리 함수 <stdio.h> <conio.h>

12. getchar()와 putchar()  
<p> 1) int getchar(void); <stdio.h></p>  
        - 하나의 문자를 입력  
        - 반환형이 char형이 아닌 int형인 이유: 입력의 끝(EOF) 문자를 체크하기 위해서이다.  
<p> 2) int putchar(void); <stdio.h></p>  
        - 하나의 문자를 출력  
<p> 3) 둘 다 버퍼 사용(엔터키를 눌러야만이 입력을 전달한다)</p>  

13. _getch()와 _putch()  
<p> 1) int _getch(void); <conio.h></p>  
        - 하나의 문자를 입력, 버퍼 사용x  
<p> 2) int _putch(void); <conio.h></p>  
        - 하나의 문자를 출력, 버퍼 사용x  
<p> 3) 둘 다 버퍼 사용하지 않음(글자가 입력되는 대로 전달한다)</p>  

14. gets()와 puts()  
<p> 1) char *gets(char *buffer); <stdio.h> - 한 줄을 입력</p>  
        - 줄 바꿈 문자(‘\n’)를 NULL 문자로 변환하여 저장  
        - 충분한 크기의 문자 배열을 사용하여야 한다.  
<p> 2) int puts(const char *str); <stdio.h> - 한 줄을 출력</p>

<p>15. 문자 검사 라이브러리 함수 <ctype.h></p>  
    1) isalpha(c) - c가 영문자인가?  
    2) isupper(c) - c가 대문자인가?  
    3) islower(c) - c가 소문자인가?  
    4) isdigit(c) - c가 숫자인가?  
    5) isalnum(c) - c가 영문자인가 숫자인가?  
    6) isxdigit(c) - c가 16진수의 숫자인가?  
    7) isspace(c) - c가 공백 문자인가?  
    8) ispunct(c) - c가 구두점 문자인가?  
    9) iscntrl(c) - c가 제어 문자인가?  
    10) isascii(c) - c가 아스키 코드인가?  

<p>16. 문자 변환 라이브러리 함수 <ctype.h></p>  
    1) toupper(c) - c를 대문자로 바꾼다.  
    2) tolower(c) - c를 소문자로 바꾼다.  
    3) toascii(c) - c를 아스키 코드로 바꾼다.  

<p>17. 문자열 처리 라이브러리 함수 <string.h></p>

18. strlen(const char *s) - 문자열 길이 계산

19. strcpy()와 strncpy()  
    1) char　*strcpy(char *dst, const char *src)  
        - src가 가리키는 문자열을 dst가 가리키는 배열로 복사한다. dst가 가지고 있던 문자열은 덮어씌워져서 없어진다. NULL 문자가 나올 때까지 복사를 한다. dst >= src  
    2) char *strncpy(char *dst, const char *src, size_t n)  
        - src를 dst로 n개의 문자만을 복사한다.  

20. strcat()과 strncat()  
    기존 문자열의 NULL 문자를 지우고 그 자리부터 시작하여 만들어진 문자열의 마지막에 NULL 문자를 삽입한다.  
    1) char *strcat(char *dst, const char *src)  
        - src를 dst에 붙인다.  
    2) char *strncat(char *dst, const char *src, size_t n)  
        - src의 n개의 문자만을 dst에 붙인다.  

21. char *strcmp(const char *s1, const char *s2)  
    - 문자열 s1과 s2를 비교하여 사전적인 순서에서 s1이 앞에 있으면 음수가 반환되고, 같으면 0, 뒤에 있으면 양수가 반환된다.  
    - 최대 n문자까지만 비교를 하고 싶다면 strncmp()함수를 이용한다.  

<p>22. 문자열 수치변환 - 출력 함수가 아니다! <stdio.h></p>  
    1) sscanf() - 문자열 s로부터 지정된 형식으로 수치를 읽어서 변수에 저장한다.   
    2) sprintf() - 변수의 값을 형식 지정자에 따라 문자열 형태로 문자 배열 s에 저장한다.  
        
        ex1) 

            char s1[] = "100 200 300";
            char s2[30];
            int value;
            
            sscanf(s1, "%d", &value); // 문자열에서 "%d"형식으로 읽어서 value에 저장한다. 문자열 -> 수치로 변환한다.
            printf("%d\n", value); // 100출력
            sprintf(s2, "%d", value); // value에 저장된 값을 문자열로 변환하여서 s2에 저장한다. 수치 -> 문자열로 변환한다. = itoa(value, s2, 10) 으로 사용가능하다.
            printf("%s\n", s2); // 100출력

23. int atoi(const char *str)  
<p> - <stdlib.h> str을 int형으로 변환한다.</p>  
    
24. double atof(const char *str)  
<p> - <stdlib.h> str을 double형으로 변환한다.</p>  
<br><br>

## 예제 
#### [피보나치 문자열]

```angular2
#include <stdio.h>
#include <string.h>

int main(void)
{
	char string1[1000] = "ab"; //  첫 번째 문자열 ab
	char string2[1000] = "xyy"; //  두 번째 문자열 xyy
	char string3[1000];
	
	int n;
	int i;

	printf("몇 번째 줄까지 생성하시겠습니까?: ");
	scanf("%d",&n);

	printf("[%d]%s\n", strlen(string1), string1); // 첫 번째 문자열 출력
	printf("[%d]%s\n", strlen(string2), string2); // 두 번째 문자열 출력

	for(i = 0; i <= n; i++)
	{
		strcat(string1, string2); 
		// strcat(dst, src) 함수는 dst에 src의 값을 더해준다. 
		// 따라서 string1에는 현재 string2가 덧붙여져있다. 
		// -> i=0; string1 = abxyy, string2 = xyy
		// -> i=1; string1 = xyyabxyy, string2 = abxyy

		strcpy(string3, string1);
		// strcpy(dst, src) 함수는 dst에 src의 값을 복사해준다. 
		// 따라서 string3에는 현재 string1의 값이 복사되었다. 
		// -> i=0; string3 = abxyy
		// -> i=1; string3 = xyyabxyy
		
		strcpy(string1, string2); 
		// string1에는 현재 string2의 값이 복사되었다. 
		// -> i=0; string1 = xyy, string2 = xyy
		// -> i=1; string1 = abxyy, string2 = abxyy

		strcpy(string2, string3); 
		// string2에는 현재 string3의 값이 복사되었다. 
		// -> i=0; string2 = xyyabxyy, string3 = abxyy 
		// -> i=1; string2 = xyyabxyy, string3 = xyyabxyy

		printf("[%d]%s\n", strlen(string3), string3); 
		// string3의 현재 값을 출력해준다. 
		// -> string1 = xyy, string2 = abxyy, string3 = abxyy
		// -> string1 = abxyy, string2 = xyyabxyy, string3 = xyyabxyy 
	}
}
```
