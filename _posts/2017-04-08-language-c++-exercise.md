---
layout: post
title: "C++ Programming [Exercise]"
date: 2017-04-08 09:00:00-18:00:00
categories: Language
tag: Language
use_math: true
---

>  POWER C++ 

## 문제 1
수학에서의 "random walk"라 불리는 문제를 배열을 이용하여 프로그래밍하여 보자. 문제는 다음과 같다. 술에 취한 딱정벌레가 n x m크기의 타일로 구성된 방안에 있다. 딱정벌레는 임의의(랜덤) 위치를 선택하여 여기저기 걸어 다닌다. 현재의 위치에서 주위의 8개의 타일로 걸어가는 확률은 동일하다고 가정하자. 그러면 최종적인 문제는 "딱정벌레가 방안의 모든 타일을 한 번씩 지나가는데 걸리는 시간은 얼마인가?"이다. 
방 전체를 2차원 배열 tile[n][m]로 모델링을 하고 처음에는 딱정벌레가 배열의 중앙에 있다고 가정하라. tile[][]의 초기 값은 0이다. 딱정벌레가 타일을 지나갈 때마다 2차원 배열의 값을 1로 만들어서 딱정벌레가 지나갔음을 나타낸다. 0부터 7까지의 랜덤한 숫자를 생성하여 다음과 같이 움직인다. 즉 0이면 북쪽으로 이동하고 4이면 남쪽으로 이동한다. 0부터 7까지의 랜덤한 숫자는 다음과 같이 움직인다. rand() 함수의 반환 값을 8로 나누어 나머지를 취한다. 모든 타일 값을 검사하여 전체 타일이 1이 되면 프로그램을 종료한다. n=5, m=5로 하고 시작점은 중앙으로 하라. 그리고 이동의 최대 한도를 100,000으로 하라. 딱정벌레의 총 이동수, 수행 시간을 출력하여 제출한다. 

```angular2
#include<iostream>
#include<cstdlib>     // 의사 난수를 위한 선언
#include<ctime>      // 시간에 따른 함수
using namespace std;

int Check(int arr[5][5])   // 배열의 모든 값이 1이 되었는지 아닌지를 확인하기 위한 함수
{
    int cnt=0;
    for(int i=0;i<5;i++)
    {
        for(int j=0;j<5;j++)
        {
            if(arr[i][j]==1)
                cnt++;
        }
    }
    return cnt;
}
void ShowRandomWalk(int arr[][5])  // 배열을 보여주기 위한 함수
{
    for(int i=0;i<5;i++)
    {
        for(int j=0;j<5;j++)
        {
            cout<<arr[i][j]<<" "
        }
        cout<<endl;
    }
    cout<<endl;
}

int main(void)
{
    int RandomWalk[5][5]={0};
    int i,j,number;  // im와 j는 배열의 인덱스값, number는 8가지 방향(랜덤)
    int count=0;   // 총 이동횟수를 세기 위해서
    time_t start,end;
    srand((unsigned int)time(NULL));

    start=clock();

    RandomWalk[2][2]=1;  // 초기 값
    i=2,j=2;
    while(Check(RandomWalk)!=25)   // Check함수의 모든 타일이 25개이면 무한루프 마침
    {
        number=rand()%8;

        switch(number)
        {
            case 0:  // 북쪽으로 갈 때
                i-=1;
                if(i==-1)
                {
                    i+=1;     // 다시 원위치로
                    continue;
                }
                else
                {
                    RandomWalk[i][j]=1;
                    count++;
                    break
                }
                case 1:   // 북동쪽으로 갈 때
                    i-=1,j+=1;
                    if(i==-1 || j==5)
                    {
                        i+=1,j-=1;  // 다시 원위치로
                        continue;
                    }
                    else
                    {
                        RandomWalk[i][j]=1;
                        count++;
                        break;
                    }
                case 2:   // 동쪽으로 갈 때
                    j+=1;
                    if(j==5)
                    {
                        j-=1;    // 다시 원위치로
                        continue;
                    }
                    else
                    {
                        RandomWalk[i][j]=1;
                        count++;
                        break;
                    }
                case 3:   // 동남쪽으로 갈 때
                    i+=1,j+=1;
                    if(i==5 || j==5)
                    {
                        i-=1,j-=1;  // 다시 원위치로
                        continue
                    }
                    else
                    {
                        RandomWalk[i][j]=1;
                        count++;
                        break;
                    }
                case 4:   // 남쪽으로 갈 때
                    i+=1;
                    if(i==5)
                    {
                        i-=1;   // 다시 원위치로
                        continue
                    }
                    else
                    {
                        RandomWalk[i][j]=1;
                        count++;
                        break;
                    }
                case 5:   // 남서쪽으로 갈 때
                    i+=1,j-=1;
                    if(i==5 || j==-1)
                    {
                        i-=1,j+=1;   // 다시 원위치로
                        continue
                    }
                    else
                    {
                        RandomWalk[i][j]=1;
                        count++;
                        break;
                    }
                case 6:  // 서쪽으로 갈 때
                    j-=1;
                    if(j==-1)  
                    {
                        j+=1;   // 다시 원위치로
                        continue;
                    }
                    else
                    {
                        RandomWalk[i][j]=1;
                        count++;
                        break;
                    }
                case 7:   // 북서쪽으로 갈 때
                    i-=1,j-=1;
                    if(i==-1 || j==-1)
                    {
                        i+=1,j+=1;  // 다시 원위치로
                        continue;
                    }
                    else
                    {
                        RandomWalk[i][j]=1;
                        count++;
                        break;
                    }
        }
        ShowRandomWalk(RandomWalk);

        if(count>100,000)  // 이동횟수 1000,000 한도
        {
            cout<<“이동횟수가 초과되었습니다”<<endl;
            break;
        }
    }
    end=clock();

    cout<<"총 이동수 "<<count<<endl;
    cout<<"수행시간 "<<(double)(end-start)/CLOCKS_PER_SEC<<endl<<endl;
    return 0;
} 
```

## 문제 2
배열 원소들의 순서를 반대로 하는 함수를 작성하고 테스트하라. 즉 예를 들어서 A[0]는 A[n-1]로 이동하고 A[n-1]은 A[0]로 이동한다.
함수원형 - void reverse(int *A, int n);

```angular2
#include <iostream>
using namespace std;

void reverse(int * A,int n); //주어진 문제의 함수원형을 선언
void main()
{
	int Arr[5]={1,2,3,4,5}; //int형 배열을 선언하여 임의로 5의 길이와 각 배열의 값을 1부터 5까지 초기화 시켜줌
	int len=sizeof(Arr)/sizeof(int); //배열의 길이를 sizeof로 구한다
	reverse(Arr,len); //함수원형의 매개변수에서 int * A는 int A[]와 같으므로 그대로 Arr를 보내주고 len은 배열의 길이이다
}
void reverse(int * A,int n)
{
	int temp; //배열의 값을 바꿔주기 위해 temp라는 변수 선언
	          //temp의 역할은 왼쪽부터 시작한 배열의 값을 저장, 마지막 부분에 넣어주는 역할
	for(int i=0;i<n/2;i++) //i가 n/2까지인 이유는 중간지점까지 설정해주어야 중간을 기점으로 좌우가 바뀐다. 하지만 n까지하면 다시 원위치로 가기 때문이다.
	{
		temp=A[i]; //temp라는 정수형 변수에 A[0]부터의 배열의 값을 저장
		A[i]=A[(n-i)-1]; //배열 A의 처음과 마지막을 바꿔주는 과정
		A[(n-i)-1]=temp; //배열 마지막에 temp의 값을 넣어준다(배열 A의 처음값과 동일)
	}
	for(int i=0;i<n;i++)
	{
		cout<<A[i]<<" "; //배열의 형식으로 배열 A의 값을 출력
	}
}
```

## 문제 3
실수값들이 저장되어 있는 double형 배열 A[]에서 평균값, 최대값, 전체의 합을 계산하여 포인터 인수를 통하여 반환하는 함수 get_stat(double A[], double *p_avg, double *p_max, double *p_sum)을 구현하라.

```angular2
#include <iostream>
using namespace std;

double get_stat(double A[],double *p_avg_double *p_max_double *p_sum); 
// 주어진 문제의 함수 선언(배열을 반환한다고 제시하기에 반환형을 void가 아닌 double형으로 설정함)

void main()
{
	double Arr[5]={1,2,3,4,5}; //double형 배열 선언 배열의 길이는 임의로 5로 정하며 배열의 값도 임의로 1부터 5로 초기화함
	double avg=0,max=0,sum=0; //avg(평균값), max(최대값), sum(전체의 합)의 변수를 0으로 초기해줌
	get_stat(Arr,&avg,&max,&sum); //주소값을 전달해주어야 되기 때문에 &를 붙임

	cout<<"평균값: "<<avg<<endl;
	cout<<"최대값: "<<max<<endl;
	cout<<"전체의 합: "<<sum<<endl;
}

double get_stat(double A[5],double *p_avg,double *p_max,double *p_sum)
{
	int i; // 배열의 인덱스값인 동시에 평균값을 구하기 위한 변수의 횟수
	for(i=0;i<5;i++)
	{
		*p_sum+=A[i]; //전체의 합을 구하기 위해 for문으로 한 번 반복될때마다 변수에 배열 A의 값을 더해줌
		if(*p_max<A[i])
			*p_max=A[i]; //최대값을 구하기 위해 반복할 때마다 배열 A의 값과 초기값을 0으로 최대값을 맞춰 준 p_max의 값과 더해줌
	}
	*p_avg=*p_sum/i; // 위에서 구한 전체의 합을 변수의 개수만큼 나눠 평균을 구함
	return *A; //배열의 주소값을 반환한다
}   
```

## 문제 4
string 클래스의 각종 메소드를 사용하여서 사용자로부터 받은 문자열이 올바른 물품 번호인지를 검사하는 프로그램을 작성하라. 물품 번호는 크기가 6인 문자열로 되어 있으며 앞의 2개의 알파벳 문자는 물건의 종류를 나타내고 뒤의 4개의 숫자는 모델 번호이다. 예를 들어서 TV1523는 텔레비전을 나타내고 모델 번호는 1523이라는 것을 의미한다. 문자열의 길이, 앞의 두 개의 문자가 알파벳인지, 나머지 문자가 숫자인지를 검사하라.

```angular2
/*
getline(cin,문자열,'\n') ---->  문자열 입력
string.at() ----> 문자열에서 특정위치의 문자에 접근한다
string.length() ----> 문자열의 크기 반환
////만약 char형으로 쓰고 싶다면 cin.getline(문자열,sizeof(문자열))
stinrg.clear() ----> 문자열의 내용을 모두 삭제
string.empty() ----> 문자열이 비었는지 확인
string.erase(시작위치,개수) ----> 문자열을 시작위치부터 정해진 개수만큼 지움
string.find(문자),(문자열),(문자열,시작위치) ----> 특정 문자열을 찾고, 그 시작위치를 반환
////만약 찾고자 하는 문자가 없다면 string::npos로 반환
string.replace(시작위치,개수,문자열) ----> 문자열을 바꿔줌
string.insert(시작위치,문자열) ----> 문자열을 정해진 위치에 삽입
string.compare(문자열) ----> 문자열을 비교

isalpha(문자) ----> 문자에 영문자이면 true를 반환
isdigit(문자) ----> 문자에 숫자이면 true를 반환
isspace(문자) ----> 문자에 스페이스이면 true를 반환
*/

#include <iostream>
#include <string>  //string 함수를 이용하기 위해 <string> 헤더 사용
using namespace std;

void main()
{
	string serial_num; //물품 번호의 변수 이름을 serial_num이라 지정
	cout<<"물품 번호를 입력하세요:";
	getline(cin,serial_num,'\n');  //string 함수의 입력 기능 중 하나인 getline  
	
        int serial_length=6; //주어진 문제에서 물품 번호의 길이는 6이라고 지정
	int len=serial_num.length(); //string 함수의 문자열 길이를 반환하는 length
	int i=0; //string 함수도 배열처럼 사용가능하다. i는 인덱스값이다.
	int check_alpha=0; //사용자가 넣어준 문자가 영문인지 확인해서 그 수를 세주기 위한 변수
	int check_digit=0; //사용자가 넣어준 문자가 숫자인지 확인해서 그 수를 세주기 위한 변수
	
	if(len!=serial_length) //사용자가 입력한 문자열의 길이가 6과 같지 않다면
	{
		cout<<"문품 번호의 길이가 다릅니다"<<endl;
		return;	 // 강제 종료를 시킨다
	}
	else //만약 문자열의 길이가 같다면 다음 행으로 넘어간다
		cout<<"물품 번호의 길이가 맞습니다"<<endl;
		
    
	for(i;i<2;i++) //string 함수를 배열처럼 사용가능 하기 때문에 첫 번째 인덱스와 { // 두 번째 인덱스의 값에 대해 문자가 영문인지 확인한다
		if(!(isalpha(serial_num[i]))==0) //!()==0은 ()가 참이면 조건이 성립한다
			check_alpha++;
	}
	for(i;i<serial_length;i++) //넘어온 인덱스값은 2부터 시작하며 나머지 문자열의 길 { // 이만큼 각 문자가 숫자인지 확인한다
		if(!(isdigit(serial_num[i]))==0)
			check_digit++;
        }
	if(check_alpha==2) //문자열의 앞의 두 자리가 영문이 맞다면 2개가 나온다 
		cout<<"물품 종류가 맞습니다"<<endl;
	else
		cout<<"물품 종류가 다릅니다"<<endl;
	
	if(check_digit==4) //문자열의 세 번째 배열값부터 나머지 배열값이 숫자가 맞다면 4개가 나온다 
		cout<<"물품 번호가 맞습니다"<<endl;
	else
		cout<<"물품 번호가 다릅니다"<<endl;
}
```

## 문제 5
사용자에게서 받은 문자열을 역순으로 화면에 출력하는 프로그램을 작성하여 보자. 예를 들어서 사용자가 “secret"을 입력하면 ”terces"를 출력한다.

```angular2
방법 1
#include <iostream>
#include <string>
using namespace std;

void main()
{
	string User_str; //사용자에게 받을 문자열 변수
	
	cout<<"문자열 입력:";
	getline(cin,User_str,'\n'); //string 함수의 입력 기증 중 getline
	
	for(int i=User_str.length()-1;i>=0;i--) //string 함수의 문자열 길이 반환 length
	{
		cout<<User_str[i]; //string도 배열처럼 사용할 수 있다
	}
}


방법 2
#include <iostream>
#include <string>
using namespace std;

void main()
{
	string str;
	char *temp; //temp라는 char형 포인터 선언
	cout<<"문자열 입력:";
	getline(cin,str,'\n');
	int len=str.length();
	temp=new char[len]; //temp를 동적할당함(len길이만큼)
	int p=0; //temp배열의 인덱스
	
	for(int i=0;i<len/2;i++) //과제의 문제1번과 같이 중간을 기점으로 앞뒤를 바꿔주는 방식을 써보았다.
	{
		temp[p]=str[i];
		str[i]=str[(len-i)-1];
		str[(len-i)-1]=temp[p];
		p++;
	}
	
	for(int i=0;i<len;i++)
		cout<<str[i];
	
	delete temp; //동적할당한 메모리를 해제한다
}
```

## 문제 6
복소수를 나타내는 Complex 클래스를 작성하라. 복소수는 실수부와 허수부로 이루어진다. 필요한 멤버 변수와 접근자와 설정자 함수를 정의하라. 복소수를 12.0 + 17.9i와 같이 출력하는 print() 멤버 함수를 정의하라. 복소수에 대한 덧셈 연산과 뺄셈 연산을 정의하라. Complex 객체를 생성하여 테스트하라.

```angular2
#include <iostream>
using namespace std;

class Complex
{
private:
	double Real_num; // 복소수 중 정수 부분 변수
	double Imaginary_num;  // 복소수 중 허수 부분 변수
public:
	void Set_Real_num(); // 정수 부분 setter함수
	double Get_Real_num(); // 정수 부분 getter함수
	void Set_Imaginary_num();  // 허수 부분 setter함수
	double Get_Imaginary_num(); // 허수 부분 getter함
	void print(); // 복소수의 출력
	void Add_Min_num(double r1,double r2,double i1,double i2); //복소수 합과 차를 구하는 함수
};
void Complex::Set_Real_num()
{
	cout<<"복소수의 실수부를 입력하시오:";
	cin>>Real_num;
}
double Complex::Get_Real_num()
{
	return Real_num;
}
void Complex::Set_Imaginary_num()
{
	cout<<"복소수의 허수부를 입력하시오:";
	cin>>Imaginary_num;
}
double Complex::Get_Imaginary_num()
{
	return Imaginary_num;
}
void Complex::print()
{
	if(Imaginary_num>0)
	cout<<"복수수 :"<<Real_num<<"+"<<abs(Imaginary_num)<<"i"<<endl<<endl;
	else
	cout<<"복수수 :"<<Real_num<<"-"<<abs(Imaginary_num)<<"i"<<endl<<endl; 
       // 허수부가 음수이면 -기호를 붙여준다(값은 절댓값으로)
}
void Complex::Add_Min_num(double r1,double r2,double i1,double i2)
{
	cout<<"복소수의 합:"<<r1+r2<<"+"<<i1+i2<<"i"<<endl;
	if((i1-i2)<0)
	    cout<<"복소수의 차:"<<r1-r2<<"-"<<abs(i1-i2)<<"i"<<endl; 
            //허수부가 음수이면 -기호를 붙여준다(값은 절댓값으로)
	else
		cout<<"복소수의 차:"<<r1-r2<<"+"<<i1-i2<<"i"<<endl;  
}
void main()
{
	Complex com1;
	com1.Set_Real_num();
	com1.Set_Imaginary_num();
	com1.print();

	Complex com2;
	com2.Set_Real_num();
	com2.Set_Imaginary_num();
	com2.print();
	// com1객체에 대한 클래스 내에서 합과 차를 구한다
        com1.Add_Min_num(com1.Get_Real_num(),com2.Get_Real_num(),com1.Get_Imaginary_num(),com2.Get_Imaginary_num());
	// 인자로 com1의 실수부와 허수부, com2의 실수부와 허수부를 전달한다
}
```

## 문제 7
삼각형을 나타내는 Triangle 클래스를 정의하여 보자. Triangle 클래스는 밑변과 높이, 면적을 나타내는 멤버 변수를 가진다. 각 멤버 변수에 대하여 접근자와 설정자를 작성한다. 삼각형의 면적을 구하는 멤버 함수 getArea()를 추가하라. Triangle 객체를 생성하여서 테스트하라.

```angular2
#include <iostream>
using namespace std;

class Triangle
{
private: // 멤버 변수를 정의하기 위한 private 접근제어자
	int base;  // 밑변의 변수
	int height;  // 높이의 변수
	double area;  // 면적의 변수
public: // 멤버 함수를 정의하기 위한 public 접근제어자
	void Set_base(); // 밑변의 setter함수
	int Get_base();  // 밑변의 getter함수
	void Set_height();  // 높이의 setter함수
	int Get_height();  // 높이의 getter함수
	double getArea();  // 면적을 구하는 함수
};
// 위의 클래스 내에서는 함수의 원형만 선언, 나머지 정의는 밖으로 꺼내어 표현해줌(가독성을 좋게 하기 위해서)
void Triangle::Set_base()
{
	cout<<"밑면을 입력하시오:";
	cin>>base;
}
int Triangle::Get_base()
{
	return base;
}
void Triangle::Set_height()
{
	cout<<"높이를 입력하시오:";
	cin>>height;
}
int Triangle::Get_height()
{
	return height;
}
double Triangle::getArea()
{
	area=base*height*(0.5);
	return area;
}

void main()
{
	Triangle tri;
	tri.Set_base();
	tri.Set_height();
	cout<<fixed;  // 소수점을 고정시켜준다
	cout.precision(1);  // ()안의 값만큼 소수자리를 보여주도록 한다
	cout<<"밑변 :"<<tri.Get_base()<<endl;
	cout<<"높이: "<<tri.Get_height()<<endl;
	cout<<"넓이: "<<tri.getArea()<<endl;
}
```

## 문제 8
본문의 은행 계좌를 나타내는 BankAccount 클래스에 다음과 같은 기능을 하는 멤버 함수를 추가하고 테스트하라. 

```angular2
#include <iostream>
#include <string>
using namespace std;

class BankAccount //은행 계좌
{
private:
	int accountNumber; //계좌 번호
	string owner; //예금주
	int balance; //잔액을 표시하는 변수
public:
	void set_account(); //계좌 번호 입력
	void set_owner(); //예금주 입력
	void set_balance(); //잔액 입력
	int get_balance(); //잔액 출력 
	
	void deposit(int amount); //저금 함수
	void withdraw(int amount); //인출 함수
	void print(); //현재 상태 출력
	
	//int transfer(int amount,BankAccount otherAccount); //다른 계좌로 송금하는 함수(값의 복사만 일어남)
	int transfer(int amount,BankAccount & otherAccount); //참조자를 통해 주소값을 전달하자
};
void BankAccount::set_account()
{
	cout<<"------------계좌 개설------------"<<endl;
	cout<<"계좌번호 입력:";
	cin>>accountNumber;
	//fflush(stdin); 이거나 밑에꺼나 둘 중 하나 쓰자 아마도 getline을 쓰면서 버퍼가 생긴듯 하다
	cin.ignore();
}
void BankAccount::set_owner()
{
	cout<<"예금주 입력:";
	getline(cin,owner,'\n');
}
void BankAccount::set_balance()
{
	balance=0;
}
int BankAccount::get_balance()
{
	return balance;
}
void BankAccount::deposit(int amount)
{
	balance+=amount; //잔액에서 예금액을 더해준다
}
void BankAccount::withdraw(int amount)
{
	if(balance>=amount)
 	    balance-=amount; //만약 잔액이 인출할 금액보다 크면 인출한다
	else
		cout<<"잔액이 부족합니다"<<endl; //잔액이 인출할 금액보다 작은 경우
}
void BankAccount::print()
{
	cout<<"계좌번호: "<<accountNumber<<endl;
	cout<<"예금주: "<<owner<<endl;
	cout<<"잔액: "<<balance<<endl;
}
/*
int BankAccount::transfer(int amount,BankAccount otherAccount)
{
	balance-=amount; //현재 객체의 잔액에서 송금해줄 돈만큼 뺀다
	otherAccount.balance+=amount; //다른 객체의 잔액에서 송금해준 돈만큼 더해준다
	return balance; //현재 객체의 잔액
}
*/ //값의 복사가 일어남
int BankAccount::transfer(int amount,BankAccount & otherAccount)
{
	balance-=amount; //현재 객체의 잔액에서 송금해줄 돈만큼 뺀다
	otherAccount->balance+=amount; //다른 객체의 잔액에서 송금해준 돈만큼 더해준다
	return balance; //현재 객체의 잔액
}

void main()
{
	BankAccount account1;
	account1.set_account();
	account1.set_owner();
	account1.set_balance();
	cout<<endl;
	account1.print();

	cout<<"----------계좌 입금--------"<<endl;
	account1.deposit(10000);
	account1.print();

	BankAccount account2;
	account2.set_account();
	account2.set_owner();
	account2.set_balance();
	cout<<endl;
	account2.print();

	cout<<"----------다른 계좌로 송금--------"<<endl;
	//cout<<"account1의 잔액:"<<account1.transfer(3000,account2)<<endl; 이런 식으로 하면 값의 복사만 일어남
	cout<<"account1의 잔액:"<<account1.transfer(3000,account2)<<endl; 
	cout<<"account2의 잔액:"<<account2.get_balance()<<endl;
}
```

## 문제 9
비행기를 나타내는 Plane라는 이름의 클래스를 설계하라. Plane 클래스는 식별번호, 모델, 승객수를 멤버 변수로 가지고 있다. ① 멤버 변수를 정의하라. 모든 멤버 변수는 전용 멤버로 하라. ② 모든 멤버 변수에 대한 접근자와 설정자 멤버 함수를 작성한다. ③ Plane 클래스의 생성자 몇 개를 중복 정의하라. 생성자는 모든 데이터를 받을 수도 있고 아니면 하나도 받지 않을 수 있다. ④ Plane 객체의 현재 상태를 콘솔에 출력하는 print() 함수도 포함시켜라. ⑤ main()에서 Plane 객체 여러 개를 생성하고 접근자와 설정자를 호출하여 보라.

```angular2
#include <iostream>
using namespace std;

class Plane
{
private:
	int id; //식별번호
	char * model; //모델명
	int numPassengers; //승객수
public:
	void setter_id(int i);//식별번호 setter 함수
	int getter_id(); //식별번호 getter 함수
	void setter_model(char * mod); //모델명 setter 함수
	char* getter_model(); //모델명 getter 함수
	void setter_numPassengers(int person); //승객수 setter 함수
	int getter_numPassengers(); //승객수 getter 함수
	Plane(); //디폴트 생성자
	Plane(int i, char * mod, int people); //매개변수 생성자
	Plane(const Plane &plane); //복사 생성자
	~Plane(); //소멸자
	void print(); //출력 함수
};
void Plane::setter_id(int i)
{
	id=i;
}
int Plane::getter_id()
{
	return id;
}
void Plane::setter_model(char * mod)
{
	model=new char[strlen(mod)+1];
	strcpy(model,mod);
}
char* Plane::getter_model()
{
	return model;
}
void Plane::setter_numPassengers(int person)
{
	numPassengers=person;
}
int Plane::getter_numPassengers()
{
	return numPassengers;
}
Plane::Plane() : id(0),numPassengers(0)
{
	cout<<"디폴트 생성자 호출"<<endl;
	model=new char[10];
	strcpy(model,"Ananymous");
}
Plane::Plane(int i, char * mod, int people) : id(i),numPassengers(people)
{
	cout<<"매개변수 생성자 호출"<<endl;
	model=new char[strlen(mod)+1];
	strcpy(model,mod);
}
Plane::Plane(const Plane &plane) : id(plane.id),numPassengers(plane.numPassengers)
{
	cout<<"복사 생성자 호출"<<endl;
	model=new char[strlen(plane.model)+1];
	strcpy(model,plane.model);
}
Plane::~Plane()
{
	cout<<"소멸자 호출"<<endl;
	delete []model;
}
void Plane::print()
{
	cout<<"식별번호:"<<id<<endl;
	cout<<"모델명:"<<model<<endl;
	cout<<"승객수:"<<numPassengers<<endl;
}
void main()
{
	Plane plane1; //디폴트 생성자로 생성
	cout<<"plane1의 정보"<<endl;
	plane1.print();
	cout<<endl;

	Plane plane2(1023,"ASIA",2900); //매개변수 생성자로 생성
	cout<<"plane2의 정보"<<endl;
	plane2.print();
	cout<<endl;

	Plane plane3; //setter,getter함수로 생성
	plane3.setter_id(2234);
	plane3.setter_model("KOREA");
	plane3.setter_numPassengers(1000);
	cout<<"plane3의 정보"<<endl;
	cout<<"식별 번호:"<<plane3.getter_id()<<endl;
	cout<<"모델명:"<<plane3.getter_model()<<endl;
	cout<<"승객수:"<<plane3.getter_numPassengers()<<endl<<endl;

	Plane plane4(plane3); //복사 생성자로 생성
	cout<<"복사된 plane4의 정보"<<endl;
	plane4.print();
	cout<<endl;
}
```

## 문제 10
원을 나타내는 클래스 Circle를 가지고 실습하여 보자. 

    #include <iostream>
    #include <string>
    using namespace std;
    
    class Circle
    {
    double radius;
    };
    
    int main()
    {
    Circle c1;               //❶
    Circle c2();             //❷
    Circle c3(5.0);          //❸
    Circle c4(c1);           //❹
    Circle c5 = Circle(5.0)  //❺
    return 0;
    }

① 위의 프로그램을 입력하여서 컴파일하여 보자. 컴파일 오류가 발생하는 문장은 어느 것들인가? 이유를 설명하라. ② 특히 ❷에 대하여 자세히 살펴보자. 이 문장은 c2라는 객체를 생성하고 디폴트 생성자를 호출하는 문장인가? 아니면 Circle을 반환하는 c2()라는 함수의 원형을 선언하는 문장일까? 컴파일러의 입장에서 생각하여 보자. ③ 원의 반지름을 0,0으로 초기화하는 디폴트 생성자를 추가하라. ④ 원의 반지름을 매개 변수로 받는 생성자를 작성하여 추가하라. 초기화 리스트를 사용하여 보라. 컴파일 오류들이 없어졌는가? ⑤ Circle 클래스에 접근자와 설정자, 원의 면적을 계산하는 getArea()와 원주를 계산하는 getPerimeter()를 추가하여 보자. ⑥ 각 객체를 통하여 getArea()와 getPerimeter()를 호출하여서 어떤 값이 반환되는 지 출력하여 보자. ➆ 위의 문장 중에서 복사 생성자가 호출되는 문장은 어느 것인가? ⑧ Circle 클래스를 UML로 그려보라. 필드나 메소드가 전용인지 공용인지도 표시하라. ⑨ 점을 나타내는 Point 클래스를 정의하고 위의 Circle 클래스에 원의 중심을 나타내는 Point 객체를 추가하라. Circle의 생성자에서 Point의 생성자를 호출하도록 하라. ⑩ Circle 클래스에 소멸자를 추가하여 보자. 소멸자가 호출되는지를 확인하라. ⑪ 만약 Circle 클래스에 const double PI; 라는 실수형 상수 정의가 포함되면 디폴트 생성자가 자동으로 생성되지 않는다. 왜 그럴까? 이유를 생각하여 보자.

```angular2
#include <iostream>
#include <string>
using namespace std;

const double PI=3.14; //원주율의 값

//Point 클래스
class Point
{
	int xpos; //x좌표
	int ypos; //y좌표
	int i; //소멸 순서
public:
	Point(); //디폴트 생성자
	Point(int x,int y); //매개변수 생성자
	~Point(); //소멸자
	void setter_xpos(int x); //xpos_setter함수
	int getter_xpos(); //xpos_getter함수
	void setter_ypos(int y); //ypos_setter함수
	int getter_ypos(); //ypos_getter함수
};
Point::Point() : xpos(0),ypos(0) 
{
	cout<<"Point 디폴트 생성자 호출"<<endl;
	i=1;
}
Point::Point(int x,int y) : xpos(x),ypos(y)
{
	cout<<"Point 매개변수 생성자 호출"<<endl;
	i=2;
}
Point::~Point()
{
	cout<<i<<"번 Point 소멸자 호출"<<endl;
}
void Point::setter_xpos(int x)
{
	xpos=x;
}
int Point::getter_xpos()
{
	return xpos;
}
void Point::setter_ypos(int y)
{
	ypos=y;
}
int Point::getter_ypos()
{
	return ypos;
}
//Circle 클래스
class Circle
{
	double radius;
	Point pos;
	int j; //소멸 순서
public:
	Circle(); //디폴트 생성자
	Circle(double r,int x,int y); //매개변수 생성자
	Circle(const Circle &circle); //복사 생성자
	~Circle(); //소멸자
	void setter_Circle(double r); //setter함수
	double getter_Circle(); //getter함수
	double getArea(); //원의 면적을 계산하는 함수
	double getPerimeter(); //원주를 계산하는 함수
	void print(); //함수 정보 출력함수
};
Circle::Circle() : radius(0.0),pos() 
{
	cout<<"Circle 디폴트 생성자 호출"<<endl;
	j=1;
}
Circle::Circle(double r,int x,int y) : radius(r),pos(x,y)
{
	cout<<"Circle 매개변수 생성자 호출"<<endl;
	j=2;
}
Circle::Circle(const Circle &circle) : radius(circle.radius)
{
	cout<<"Circle 복사 생성자 호출"<<endl;
	j=3;
}
Circle::~Circle()
{
	cout<<j<<"번 Circle 소멸자 호출"<<endl;
}
void Circle::setter_Circle(double r)
{
	radius=r;
}
double Circle::getter_Circle()
{
	return radius;
}
double Circle::getArea()
{
	double area;
	area=radius*radius*PI;
	return area;
}
double Circle::getPerimeter()
{
	double perimeter;
	perimeter=2*radius*PI;
	return perimeter;
}
void Circle::print()
{
	cout<<"원의 좌표: "<<"["<<pos.getter_xpos()<<","<<pos.getter_ypos()<<"]"<<endl;
	cout<<"원의 반지름: "<<radius<<endl;
}
int main()
{
	Circle c1; //디폴트 생성자 호출
	c1.print();
	cout<<"c1의 넓이:"<<c1.getArea()<<endl;
	cout<<"c1의 원주:"<<c1.getPerimeter()<<endl<<endl;
	Circle c2(); //함수의 원형을 호출
	Circle c3(5.0,2,3); //매개변수 생성자 호출
	c3.print();
	cout<<"c3의 넓이:"<<c3.getArea()<<endl;
	cout<<"c3의 원주:"<<c3.getPerimeter()<<endl<<endl;
	Circle c4(c1); //복사 생성자 호출
	c4.print();
	cout<<"c4의 넓이:"<<c4.getArea()<<endl;
	cout<<"c4의 원주:"<<c4.getPerimeter()<<endl<<endl;
	Circle c5 = Circle(5.0,7,4); //매개변수 생성자 호출
	c5.print();
	cout<<"c5의 넓이:"<<c5.getArea()<<endl;
	cout<<"c5의 원주:"<<c5.getPerimeter()<<endl<<endl;
	return 0;
}
```

## 문제 11
회사에서 근무하는 직원들을 나타내는 클래스들을 상속을 이용하여 작성하여 보자.
① Employee 클래스를 설계하라. Employee 클래스는 이름,사번 등의 정보를 멤버 변수로 가져야 한다. 생성자를 정의하고 접근자와 설정자도 작성하라. 월급을 계산하는 멤버 함수인 computeSalary()를 구현하라.
② Employee 클래스에서 상속받아서 SalariedEmployee라는 클래스를 정의하여 보자. 이 클래스는 월급이라는 멤버 변수를 추가로 가진다. 역시 생성자를 정의하고 접근자와 설정자 멤버 함수도 작성하라. 부모 클래스의 computeSalary()를 재정의하라.
③ Employee 클래스에서 상속받아서 시간제 직원을 나타내는 HourlyEmployee 클래스를 정의하라. 시간당 임금과 일한 시간을 멤버 변수로 추가하라. 역시 생성자를 정의하고 접근자와 설정자 멤버 함수도 작성하라. 부모 클래스의 computeSalary()를 재정의하라.

```angular2
#include <iostream>
using namespace std;

class Employee
{
private:
	char *name; //사원의 이름
	char *id; //사원의 사번
protected:
	int salary; //사원의 월급, protected로 접근지정을 하여 자식클래스의
	            //computeSalary()함수의 오버라이딩이 가능하도록 하였다
public:
	Employee(char *n,char *i,int m); //매개변수 생성자로 생성
	void set_name(char *n); //이름 설정자
	void set_id(char *i); //사번 설정자
	void set_salary(int m); //월급 설정자
	char* get_name(); //이름 접근자
	char* get_id(); //사번 접근자
	int get_salary(); //월급 접근자
	void print() const; 
	int computeSalary(); //오버라이딩하려는 함수
	~Employee();
};
Employee::Employee(char *n,char *i,int m) : salary(m)
{
	name=new char[strlen(n)+1];
	strcpy(name,n);
	id=new char[strlen(i)+1];
	strcpy(id,i);
	cout<<"Employee 생성자 호출"<<endl;
}
void Employee::set_name(char *n)
{
	delete []name; //설정자에서 이미 만들어져 있는 메모리를 없애준다
	name=new char[strlen(n)+1];
	strcpy(name,n);
}
void Employee::set_id(char *i)
{
	delete []id; //설정자에서 이미 만들어져 있는 메모리를 없애준다
	id=new char[strlen(i)+1];
	strcpy(id,i);
}
void Employee::set_salary(int m)
{
	salary=m;
}
char* Employee::get_name()
{
	return name;
}
char* Employee::get_id()
{
	return id;
}
int Employee::get_salary()
{
	return salary;
}
void Employee::print() const
{
	cout<<"이름 : "<<name<<endl;
	cout<<"사번 : "<<id<<endl;
}
Employee::~Employee()
{
	delete []name;
	delete []id;
	cout<<"Employee 소멸자 호출"<<endl;
}
int Employee::computeSalary()
{
	cout<<"Employee salary 함수 호출"<<endl;
	return salary;
}
class SalariedEmployee : public Employee
{
private:
	int pay; //월급이라는 추가 멤버변수
public:
	SalariedEmployee(char *n,char *i,int s,int p); //자식클래스에서 부모클래스의 변수를 이니셜라이저를 이용해 초기화 해준다
	void set_pay(int p);
	int get_pay();
	void print() const;
	int computeSalary(); //오버라이딩된 함수
	~SalariedEmployee();
};
SalariedEmployee::SalariedEmployee(char *n,char *i,int s,int p) :
Employee(n,i,s),pay(p)
{
	cout<<"SalariedEmployee 생성자 호출"<<endl;
}
void SalariedEmployee::set_pay(int p)
{
	pay=p;
}
int SalariedEmployee::get_pay()
{
	return pay;
}
void SalariedEmployee::print() const
{
	Employee::print();
	cout<<"월급 : "<<pay<<endl;
}
int SalariedEmployee::computeSalary()
{
	cout<<"SalariedEmployee salary 함수 호출"<<endl;
	return pay;
}	
SalariedEmployee::~SalariedEmployee()
{
	cout<<"SalariedEmployee 소멸자 호출"<<endl;
}
class HourlyEmployee :  public Employee
{
private:
	int per_hour_pay; //시간당 임금
	int working_hour; //일한 시간
public:
	HourlyEmployee(char *n,char *i,int p,int ph,int h); //자식 클래스에서 부모 클래스의 멤버 변수를 초기화 해준다
	void set_pay(int payhour);
	void set_hour(int hour);
	int get_pay();
	int get_hour();
	void print() const;
	int computeSalary(); //오버라이딩된 함수
	~HourlyEmployee();
};
HourlyEmployee::HourlyEmployee(char *n,char *i,int p,int ph,int h) : 
Employee(n,i,p),per_hour_pay(ph),working_hour(h) 
{
	cout<<"HourlyEmployee 생성자 호출"<<endl;
}
void HourlyEmployee::set_pay(int payhour)
{
	per_hour_pay=payhour;
}
void HourlyEmployee::set_hour(int hour)
{
	working_hour=hour;
}
void HourlyEmployee::print() const
{
	Employee::print();
	cout<<"시간당 임금 : "<<per_hour_pay<<endl;
	cout<<"일한 시간 : "<<working_hour<<endl;
}
int HourlyEmployee::computeSalary()
{
	cout<<"HourlyEmployee salary 함수 호출"<<endl;
	return per_hour_pay;
}
HourlyEmployee::~HourlyEmployee()
{
	cout<<"HourlyEmployee 소멸자 호출"<<endl;
}
void main()
{
	Employee emp1("지명화","2014136124",330);
	cout<<"이번 달 월급 : "<<emp1.computeSalary()<<"원"<<endl;
	emp1.print();
	cout<<endl;

	SalariedEmployee emp2("김수미","20304344",200,100);
	cout<<"이번 달 월급 : "<<emp2.computeSalary()<<"원"<<endl;
	emp2.print();
	cout<<endl;

	HourlyEmployee emp3("노윤정","204311353",250,180,8);
	cout<<"이번 달 월급 : "<<emp3.computeSalary()<<"원"<<endl;
	emp3.print();
	cout<<endl;
}
```

## 문제 12
Person 클래스를 설계하라. Person 클래스는 이름, 주소, 전화번호를 멤버 변수로 가진다. 하나 이상의 생성자를 정의하고 각 멤버 변수에 대하여 접근자와 설정자 함수를 작성하라. 이어서 Person을 상속받아서 Custome를 작성하여 보자. 
Customer는 고객 번호와 마일리지를 멤버 변수로 가지고 있다. 한 개 이상의 생성자를 작성하고 적절한 접근자 함수와 설정자 함수를 작성하라.

```angular2
#include <iostream>
#include <string>
using namespace std;

class Person
{
private:
	string name;
	string address;
	string phone;
public:
	Person(string n,string a,string p) : name(n),address(a),phone(p) { //매개변수 생성자로 생성자 호출
		cout<<"Person 생성자 호출"<<endl;
        }
	void set_name(string n) { name=n; }
	void set_address(string a) { address=a; }
	void set_phone(string p) { phone=p; }
	string get_name() { return name; }
	string get_address() { return address; }
	string get_phone() { return phone; }
	void print() const
	{
		cout<<"이름 : "<<name<<endl;
		cout<<"주소 : "<<address<<endl;
		cout<<"번호 : "<<phone<<endl;
	}
	~Person() { cout<<"Person 소멸자 호출"<<endl; }
};
class Customer : public Person
{
private:
	int id;
	int point;
public:
	//자식 클래스의 생성자에서 부모 클래스의 멤버 변수를 초기화 해주어야 한다 
	Customer(string n,string a,string p,int i,int po) : Person(n,a,p),id(i),point(po) 
	{
		cout<<"Customer 생성자 호출"<<endl;
	}
	void set_id(int i) { id=i; }
	void set_point(int po) { point=po; }
	int get_id() { return id; }
	int get_point() { return point; }
	void print() const
	{
		Person::print();
		cout<<"고객 번호 ; "<<id<<endl;
		cout<<"마일리지 : "<<point<<endl;
	}
	~Customer()
	{
		cout<<"Customer 소멸자 호출"<<endl;
	}
};
void main()
{
	Person person1("지명화","서울시","010-3253-3523");
	person1.print();
	cout<<endl;
	Person person2("김수미","경기도","010-5924-7754");
	person2.print();
	cout<<endl;
	Customer cus1("지명화","서울시","010-3253-3523",1393,50000);
	cus1.print();
	cout<<endl;
	Customer cus2("김수미","경기도","010-5924-7754",3002,44000);
	cus1.print();
	cout<<endl;
}
```

## 문제 13
일반적인 책을 나타내는 Book 클래스를 상솎받아서 잡지를 나타내는 Magazine 클래스를 작성하여 보자. Book 클래스는 제목, 페이지수, 저자 등의 정보를 가진다. magazine 클래스는 추가로 발매일 정보를 가진다. 먼저 UML을 그리고 생성자를 포함하여서 구현하여 보라.

```angular2
#include <iostream>
using namespace std;

class Book
{
private:
	char *title; //책 제목
	int page; //책 페이지수
	char *editor; //책 저자
public:
	Book(char *t,int p,char *e) : page(p)
	{
		title=new char[strlen(t)+1];
		strcpy(title,t);
		editor=new char[strlen(e)+1];
		strcpy(editor,e);
		cout<<"Book 매개변수 생성자 호출"<<endl;
	}
	void print() const
	{
		cout<<"제목 : "<<title<<endl;
		cout<<"페이지수 : "<<page<<endl;
		cout<<"저자 : "<<editor<<endl;
	}
	~Book()
	{
		delete []title;
		delete []editor;
		cout<<"Book 소멸자 호출"<<endl;
	}
};
class Magazine : public Book
{
private:
	char *date; //책 출판일
public:
	Magazine(char *t,int p,char *e,char *d) : Book(t,p,e)
	{
		date=new char[strlen(d)+1];
		strcpy(date,d);
		cout<<"Magazine 매개변수 생성자 호출"<<endl;
	}
	void print() const
	{
		Book::print();
		cout<<"출판일 : "<<date<<endl;
	}
	~Magazine()
	{
                delete []date;
		cout<<"Magazine 소멸자 호출"<<endl;
	}
};
void main()
{
	Magazine magazine1("노인과 바다",845,"해밍웨이","1952년");
	magazine1.print();
	cout<<endl;
	Magazine magazine2("장발장",683,"빅토르 위고","1862년");
	magazine2.print();
	cout<<endl;
} 
```

## 문제 14
은행의 계좌를 나타내는 BankAcct라는 부모 클래스를 작성한다. Account 클래스는 deposit()와 withdraw()라는 멤버 함수를 가진다. getInterest()라는 이름의 가상 함수를 정의한다. getInterest()는 현재 계좌의 이자율을 반환한다. SavingAcct과 CheckingAcct 클래스를 BankAcct로부터 상속받아서 작성한다. SavingAcct 클래스는 이자율이 연 9%인 저축 예금 계좌를 나타내고 CheckingAcct 클래스는 이자율이 연 5%인 당좌 예금 계좌를 나타낸다. main() 안에서 각 객체를 생성하여서 돈은 저축하고 인출하면서 이자를 계산하여 보자. 

    기반 클래스(Account)
    
    멤버 변수 :
        int balance
    멤버 함수 :
        default 생성자
        매개변수 있는 생성자
        복사 생성자
        소멸자(가상함수)
        getter, setter
        deposit
        withdraw(잔액이 출금액보다 많은지 체크)
        print(가상함수, const)
        getInterest(순수 가상함수)
        setInterest(순수 가상함수)
    
    파생 클래스(SavingAcnt, CheckingAcnt)
    멤버변수 :
        double interest
        SavingAcnt는 9%
        CheckingAcnt는 5%
    멤버 함수 :
        기반 클래스를 충족시킬 수 있도록 정의
        = 연산자 중복 정의(balance와 interest 대입)
    
    main 함수
        기반 클래스의 객체 포인터 배열(크기 2) 선언하고 파생 객체 참조
        모든 가상 함수 호출하여 다형성 테스트
        
```angular2
#include <iostream>
using namespace std;

class Account
{
protected:
	int balance;
public:
	Account() : balance(0) { cout<<"Account 디폴트 생성자"<<endl; }
	Account(int b) : balance(b) { cout<<"Account 매개변수 생성자"<<endl; }
	Account(const Account &acct) : balance(acct.balance) { cout<<"Account 복사 생성자"<<endl; }
	void set_balance(int money) { balance+=money; }
	int get_balance() { return balance; }
	void deposit(int money) { balance+=money; }
	void withdraw()
	{
		int money;
		cout<<"얼마를 인출하시겠습니까?:";
		cin>>money;
		if(balance-money<0)
			cout<<"잔액이 부족합니다"<<endl;
		else
			balance-=money;
	}
	virtual void setInterest() = 0;
	virtual double getInterest() = 0;
	virtual void print() const
	{
		cout<<"Account의 잔액:"<<balance<<endl;
	}
	virtual ~Account() { cout<<"Account 소멸자"<<endl; }
};
class SavingAcnt : public Account
{
private:
	double interest; //9% 이자율
public:
	SavingAcnt() : Account(0),interest(0.09) { cout<<"SavingAcnt 디폴트 생성자"<<endl; }
	//자식 클래스에서 부모 클래스의 멤버 변수를 초기화 시켜주어야 하므로 디폴트 생성자라도 0이라고 초기화 해준다
	//생성자를 생성하면 부모 클래스는 무조건 디폴트 생성자가 아닌 매개변수 생성자가 호출된다(0으로 초기화 했기에)
	SavingAcnt(int b,double i) : Account(b),interest(i) { cout<<"SavingAcnt 매개변수 생성자"<<endl; }
	SavingAcnt(const SavingAcnt &acct) : Account(acct),interest(acct.interest) { cout<<"SavingAcnt 복사 생성자"<<endl; } 
	//자식 클래스 복사 생성자에서는 부모 클래스의 복사 생성자를 생성해주기 위해서 이니셜라이저를 통해 명시한다
	void set_balance(int money) { balance+=money; }
	int get_balance() { return balance; }
	SavingAcnt& operator=(SavingAcnt &acct) //대입연산자는 굳이 call-by-value에서는 디폴트 대입연산자가 있기에 의미가 없으나 깊은 복사가 이루어지는 경우(char *name과 같은) 따로 정의해야 한다
	{
		this->balance=acct.balance;
		this->interest=acct.interest;
		return *this;
	}
	void deposit(int money) { balance+=money; }
	void withdraw()
	{
		int money;
		cout<<"얼마를 인출하시겠습니까?:";
		cin>>money;
		if(balance-money<0)
			cout<<"잔액이 부족합니다"<<endl;
		else
			this->balance-=money;
	}
	virtual void setInterest() { interest=0.09; } //9% 이자율
	virtual double getInterest() { return interest; }
	void print() const
	{
		cout<<"SavingAcnt의 잔액:"<<balance+balance*interest<<endl;
	}
	~SavingAcnt() { cout<<"SavingAcnt 소멸자"<<endl; }
};
class CheckingAcnt: public Account
{
private:
	double interest; //5% 이자율
public:
	CheckingAcnt() : Account(0),interest(0.05) { cout<<"CheckingAcnt 디폴트 생성자"<<endl; }
	CheckingAcnt(int b,double i) : Account(b),interest(i) { cout<<"CheckingAcnt 매개변수 생성자"<<endl; }
	CheckingAcnt(const CheckingAcnt &acct) : Account(acct),interest(acct.interest) { cout<<"CheckingAcnt 복사 생성자"<<endl; }
	void set_balace(int money) {  }
	int get_balace() {  }
	CheckingAcnt& operator=(CheckingAcnt &acct)
	{
		this->balance=acct.balance;
		this->interest=acct.interest;
		return *this;
	}
	void deposit()
	{
		int money;
		cout<<"얼마를 입금하시겠습니까?:";
		cin>>money;
		this->balance+=money;
	}
	void withdraw()
	{
		int money;
		cout<<"얼마를 인출하시겠습니까?:";
		cin>>money;
		if(balance-money<0)
			cout<<"잔액이 부족합니다"<<endl;
		else
			this->balance-=money;
	}
	virtual void setInterest() { interest=0.05; } //5% 이자율
	virtual double getInterest() { return interest; }
	void print() const
	{
		cout<<"CheckingAcnt의 잔액:"<<balance+balance*interest<<endl;
	}
	~CheckingAcnt() { cout<<"CheckingAcnt 소멸자"<<endl; }
};
void main()
{
	//SavingAcnt,CheckingAcnt 디폴트 생성자 
	Account *acct1[2];
	acct1[0]=new SavingAcnt;
	acct1[1]=new CheckingAcnt;
	acct1[0]->set_balance(5000);
	acct1[1]->set_balance(6000);
	acct1[0]->setInterest();
	acct1[1]->setInterest();
	cout<<"acct1[0]의 이자율:"<<acct1[0]->getInterest()<<endl;
	cout<<"acct1[1]의 이자율:"<<acct1[1]->getInterest()<<endl;
	acct1[0]->print();
	acct1[1]->print();
	delete acct1[0];
	delete acct1[1];
	cout<<endl;

	//SavingAcnt,CheckingAcnt 매개변수 생성자
	Account *acct2[2];
	acct2[0]=new SavingAcnt(10000,0.09);
	acct2[1]=new CheckingAcnt(13000,0.05);
	cout<<"acct2[0]의 이자율:"<<acct2[0]->getInterest()<<endl;
	cout<<"acct2[1]의 이자율:"<<acct2[1]->getInterest()<<endl;
	acct2[0]->print();
	acct2[1]->print();
	delete acct2[0];
	delete acct2[1];
	cout<<endl;

	//SavingAcnt,CheckingAcnt 복사 생성자
	Account *acct3[2];
	acct3[0]=new SavingAcnt(12000,0.09);
	acct3[1]=new CheckingAcnt;
	//acct3[1]=acct3[0]; //CheckingAcnt를 SavingAcnt로 대입연산 불가
        acct3[1]=new SavingAct;
        //acct3[1]=acct3[0]; //마지막 소멸에서 컴파일 오류
        acct3[1]=acct1[0]; //acct1은 이미 위에서 소멸시켰으나 데이터만 사라진 상태
	cout<<"acct3[0]의 이자율:"<<acct3[0]->getInterest()<<endl;
	cout<<"acct3[1]의 이자율:"<<acct3[1]->getInterest()<<endl;
	acct3[0]->print();
	acct3[1]->print();
	delete acct3[0];
	//delete acct3[1]; //써주면 컴파일 오류가 생긴다.
}
```

## 문제 15
사용자로부터 다음과 같은 형식으로 사용자의 번호, 이름, 전화번호, 이메일 주소 등을 입력받아서 파일로 저장한다. 입력이 끝나면 사용자로부터 번호를 입력받아서 그 번호에 해당하는 전화번호를 출력하는 프로그램을 작성하라. 


| 번호 | 이름 | 전화번호  |  이메일주소 |
|---|---|---|---|
| 1 | 홍길동 | 011-111-1111 | hong@hanmail.net |
| 2 | 김유신 | 010-222-2222 | kim@hanmail.net |

```angular2
#include <iostream>
#include <fstream>
using namespace std;

struct Person //사용자에게 받을 입력 변수들을 구조체로 만든다.
{
	int number;
	char name[30];
	char phonenumber[30];
	char email[30];
};

void main()
{
	ofstream os; //파일 출력 stream
	ifstream is; //파일 입력 stream
        Person person; //구조체의 객체 생성
	
	os.open("Person.bin",ios::binary); //출력용으로 파일을 하나 생성 후 연다(바이너리 파일).
	if(os.fail()) //만약 파일 열기가 실패한다면
	{
		cout<<"File open fail"<<endl;
		exit(1);
	}
	
	while(1)
	{
		cout<<"Enter number(-1이면 종료):";
		cin>>person.number;
		if(person.number==-1)
			break;
		fflush(stdin); //cin을 하게되면 개행문자('\n')는 스트림에 넘어가지 않고 남게 되어 다음 출력 스트림에 영향을 준다. 따라서 fflush를 이용하여 강제로 넘긴다.
		cout<<"Enter name:";
		cin.getline(person.name,30);
		cout<<"Enter phonenumber:";
		cin.getline(person.phonenumber,30);
		cout<<"Enter email:";
		cin.getline(person.email,30);
		os.write((char *)&person, sizeof(Person)); //구조체 한 블록만큼 출력한다.
		cout<<"현재 커서 위치:"<<os.tellp()<<endl; //커서 위치는 계속해서 뒤로 늘어난다.
	}
	os.close(); //출력용 파일을 닫음
	is.open("Person.bin",ios::binary); //입력용 파일 스트림을 연다(바이너리 파일).
	if(is.fail()) //만약 파일 열기가 실패한다면
	{
		cout<<"File open fail"<<endl;
		exit(1);
	}
	cout<<"현재 커서 위치:"<<is.tellg()<<endl; //현재 커서는 파일을 새로 열었으므로 0으로 이동한다.
	//문장을 전체 읽고 싶을때
	/*
	is.read((char *)&person,sizeof(Person));
	while(!is.eof())
	{
		cout<<"phonenumber:"<<person.phonenumber<<endl;
		is.read((char *)&person,sizeof(Person));
	}
	*/ 
	int n; //번호를 입력하여 해당하는 번호에 맞는 구조체 변수 전화번호를 출력하게 한다.
	cout<<"Enter number:";
	cin>>n; 
	is.seekg((n-1)*sizeof(person),ios::beg); //커서 포인터를 처음 위치에서 입력해준 번호의 구조체 크기만큼 이동
	cout<<"현재 커서 위치:"<<is.tellg()<<endl; 
	is.read((char *)&person,sizeof(Person)); //커서 포인터가 가리키는 구조체만큼 읽는다
	cout<<person.phonenumber<<endl;
	is.close(); //입력용 스트림을 닫는다.
}
```

## 문제 16
배열을 나타내는 SafeArray 클래스를 작성하여 보자. SafeArray는 기존의 배열을 향상시킨 것으로 사용자가 잘못된 인덱스로 요소에 접근하는 경우에 예외를 발생한다. SafeArray 클래스의 [] 연산자를 중복 정의하고 이 연산자 함수에서 매개 변수로 전달된 인덱스가 잘못된 경우에 예외를 발생한다. SafeArray 객체를 main() 안에서 생성하여서 테스트하라. try/catch 블록을 이용하여서 예외를 처리하도록 하라.

```angular2
#include <iostream>
using namespace std;

const int size = 10; //배열의 인덱스 값을 const 상수로 정의(전역변수)

class SafeArray
{
private:
	int Arr[size];
public:
	SafeArray()
	{
		cout<<"디폴트 생성자"<<endl;
		for(int i=0;i<size;i++)
			Arr[i]=i;
	}
	int get_array(int idx)
	{
		return Arr[idx];
	}
};

void main()
{
	SafeArray arr; //SafeArray 클래스 객체 디폴트 생성자 생성
	int number_idx; //사용자가 입력해줄 인덱스 값 변수

	try
	{
		cout<<"인덱스 값을 입력하시오:";
		cin>>number_idx;

		if(number_idx<0 || number_idx>=size) //입력한 인덱스 값이 음수인 경우와 배열의 크기를 벗어난 경우
			throw number_idx;
		cout<<"입력한 인덱스의 배열 값은 "<<arr.get_array(number_idx)<<"입니다(정상처리)"<<endl;
	}
	catch(int e) //입력한 인덱스 값이 음수인 경우와 배열의 크기를 벗어난 경우 예외 처리
	{
		cout<<"배열의 인덱스 값을 "<<e<<"로 잘못 입력하셨습니다(예외처리)"<<endl;
	}
	cout<<"try, catch를 이용한 exception handling"<<endl;
}
```
