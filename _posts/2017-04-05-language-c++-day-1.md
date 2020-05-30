---
layout: post
title: "C++ Programming [Day 1]"
date: 2017-04-05 09:00:00-18:00:00
categories: Language
tag: Language
use_math: true
---

# C++ 예제 3일만에 뽀개기 - Day 1

    //go to 는 절대 쓰지 않는다.
    //switch 의 경우 쓸일이 적음

    #include <iostream>
    using namespace std;
    
    void main(void)
    {
        int income, tax;
    
        cout <<"Enter income: ";
        cin>>income;
    
        if(income<=1000)
            tax =(int)(0.09*income);  // 형변환(type casting)
        else if(income<4000)
            tax =(int)(0.18*income);
        else if(income<=8000)
            tax =(int)(0.27*income);
        else
            tax =(int)(0.36*income);
        cout<<tax<<endl;
    
    }

---

    #include <iostream>
    using namespace std;
    
    void main(void)
    {
        int fruit;
    
        cout<<"Enter number: ";
        cin>>"fruit";
        
        switch (fruit)
        {
        case 1 : cout<<"apple \n";
            break;
        case 2 : cout<<"pear \n";
            break;
        case 3 : cout<<"banana \n";
            break;
        default:cout<<"fruit \n";
            break;
        }
    }

---

    #include <iostream>
    using namespace std;
    
    void main(void)
    {	
        int days, month, year;
    
        cout << "일수를 알고 싶은 해의 달을 입력하시오: "; 
        cin>> year>>month;
    
        switch (month){
            case 1 : case 3 : case 5:
            case 7 : case 8 : case 10: case 12:
                days=31;
                break;
            case 4 : case 6: case 9: case 11:
                days=30;
                break;
            case 2:
                if(((year%4==0)&&(year%100 !=0)) ||(year%400==0))
                    days=29;
                else
                    days=28;
                break;
            default:
                cout <<"입력이상"<<endl;
                break;
        }
        cout<<"월의 날수는"<<days<<endl;
    
    }

## 구구단

    #include <iostream>
    using namespace std;
    
    void main(void)
    {	
        int n, i;
        cout <<"출력단을 입력:";
        cin>> n;
    
        i=1;
        while(i<=9)
        {
            cout<<n<<"*"<<i<<"="<<n*i<<endl; //구구단 만들기
            i++; //지속적으로 값을 높여줘야지 가능
        }
    
    }


## 최대 공약수 만들기

    #include <iostream>
    using namespace std;
    
    void main(void)
    {	
        int x,y,r; //최대 공약수 만들기
    
        cout<< "두개의 정수 입력(큰수, 작은수): "; // 숫자에 대해서 나오는 순서는 상관이 없다
        cin >>x>>y;
    
        while(y !=0)
        {
            r=x%y; //나머지 계산하기
            x=y;
            y=r;
        }
        cout <<"최대공약수는"<<x<<"입니다.\n";

## 난수 발생기를 통해서 숫자 찾기

    #include <iostream>
    #include <cstdlib> //시간값
    #include <ctime>
    using namespace std;
    
    void main(void)
    {	
        int answer, guess;
        int tries=0;
    
        srand(time(NULL)); //시간에 따라 난수 발생기 변환
        answer = 1 + rand()%50; //1~50까지 만드는거
    
        do
        {
            cout<<"정답 추측:";
            cin>>guess;
            tries++;
            if(guess>answer)
                cout<<"너무 큼.\n";
            if(guess<answer)
                cout<<"너무 작음.\n";
        }
        while (guess!=answer);
        cout<<"횟수: "<<tries<<endl;

---

    #include <iostream>
    using namespace std;
    
    void main(void)
    {
        double fact =1; //곱을 구하는 경우 1로 초기화
        int n;
        
        cout<<"정수 입력: ";
        cin >>n;
    
        for(int i =1; i<=n; i++)
            fact *=1; //fact=fact*i;
        cout<<n<<"!은"<<fact<<"입니다\n";
    
        cout <<fixed;
        cout.precision(4);
        cout <<n<<"!은 "<<fact<<"입니다\n";
    
        printf("%.4lf\n", fact);
    }

## 제곱근 구하기

    #include <iostream>
    using namespace std;
    
    void main(void)
    {
        double value;
    
        while(true) //무한 loop, for(;;)
        {
            cout<<"실수값 입력: ";
            cin>>value;
    
    
            if(value<0.0)
                break;
            cout<<value<<"의 제곱근은"<<sqrt(value)<<"입니다.\n";
        }
    
    }

## 대문자 만들기

    #include <iostream>
    using namespace std;
    
    void main(void)
    {
        char letter;
    
        while(true)
        {
            cout<<"ENTER LOWER CASE LETTER:";
            cin>>letter;
    
            if(letter == 'Q')
                break;
            if(letter <'a'|| letter>'z')
                continue;
            letter-=32;
            cout<<"대문자:"<<letter<<endl;
        }
    
    }

## 기울기를 이용해 삼각형 가능/불가능 찾기

    #include <iostream>
    using namespace std;
    
    void main(void)
    {
        double x1, y1, x2, y2, x3, y3;
        double slope1, slope2;
    
        cout<<"첫번째 점의 위치를 알려주세요(X,Y 좌표)"<<endl;
        cin>>x1>>y1;
        cout<<"두번째 점의 위치를 알려주세요(X,Y 좌표)"<<endl;
        cin>>x2>>y2;
        cout<<"세번째 점의 위치를 알려주세요(X,Y 좌표)"<<endl;
        cin>>x3>>y3;
    
        slope1 = (y2-y1)/(x2-x1);
        slope2 = (y3-y1)/(x3-x1);
        cout<<slope1<<","<<slope2;
    
        if(slope1==slope2)
            cout<<"삼각형 불가"<<endl;
        else
            cout<<"삼각형 가능"<<endl;
    
    }

## 2진수를 10진수로 만드는 방법
 
    #include <iostream>
    using namespace std;
    
    void main(void)
    {
        int num, i=0;
        int decimal =0;
        int digit;
    
        cout<<"2진수를 입력해 주시길 바랍니다"<<endl;
        cin>> num;
    
        cout<<"2진수를 10진수로"<<endl;
    
        while(num !=0)
        {
            digit= num%10;
            decimal = decimal + digit * pow(2.0, i);
            num /=10;
            i++;
        }
        cout <<decimal<<endl;
    }

---

    #include <iostream>
    #include <cmath>
    using namespace std;
    
    void main()
    {
        int num, i, decimal = 0;
        int digit;
    
        cout << "Enter binary number : ";
        cin >> num;
    
        while(num != 0)
        {
            digit = num %10;
            decimal = decimal + digit * pow(2.0, i);
            num /= 10;
            i++;
        }
        cout << decimal << endl;
    }

---

    #include <iostream>
    #include <cctype>
    using namespace std;
    
    void main()
    {
        char ch;
        cout<<"문자를 입력:";
        cin>>ch;
    
        if(isupper(ch))   // ----> 대문자로 변환하고 싶으면 islower(ch)
        {
            ch=tolower(ch); //소문자로 변환   // ----> 대문자로 변환하고 싶으면 toupper(ch)
            cout<<ch<<endl;
        }
        else
            cout<<"대문자를 입력"<<endl;
    }

---

    //cin은 공백, 개행문자 무시
    //따라서 공백이나 개행문자를 읽으려면 cin.get() 사용 (엔터키를 쓸때까지)

    void main()
    {
        char ch;
        int count=0;
    
        cout<<"입력:";
        //cin>>ch;  cin은 안됨
        ch=cin.get();
        while(ch!='\n')
        {
            if(ch=='a')
                count++;
            ch=cin.get();
        }
        cout<<"a 개수"<<count<<endl;
    }
    
---

    int compute_sum(int);
    
    void main()
    {
        int n,sum;
        cout<<"Enter number:";
        cin>>n;
    
        sum=compute_sum(n); /// 값에 의한 호출 Call-by-Value
        cout<<sum<<endl;
        cout<<&n<<endl;
    }
    
    int compute_sum(int n) 
    {
        int result=0;
    
        cout<<&n<<endl;
        for(int i=0;i<=n;i++)
            result+=i;
        return result;
    }

---

    // 디폴트 매개변수는 함수 원형에만 정의, 반드시 뒤에서부터 정의한다, 되도록 사용하지 않는다
    int calc_deposit(int salary=200,int month=12); 
    
    void main()
    {
        cout<<"default parameter 0\n";
        cout<< calc_deposit(100,6) <<endl;
    
        cout<<"default parameter 1\n";
        cout<< calc_deposit(100) <<endl;
    
        cout<<"default parameter 2\n";
        cout<< calc_deposit() <<endl;
    }
    
    int calc_deposit(int s,int m)
    {
        return s*m;
    }

---

    #include <iostream>
    using namespace std;
       
    // inline 함수, 길이가 짧은 함수의 경우 함수 호출에 대한 overhead를 줄일 수 있다.
    inline double square(double);  
    
    void main()
    {
        double result;
        cout<<square(2.0)<<endl;  // 2.0*2.0으로 치환
        cout<<square(3.0)<<endl;  // 3.0*3.0으로 치환
    }
    double square(double n)
    {
        return n*n;
    }

---

    // 변수의 scope(사용 영역) 예제
    void Function(int);
    
    int num; // 전역변수,함수 외부에서 선언
             // 모든 함수에서 사용 가능
             // 자동으로 초기화(0)
             // 되도록 사용하지 않는다
    
    void main()
    {
        int val=10;   // 지역변수,함수 내부에서 선언
                      // 지역변수는 선언된 함수 내에서만 사용 가능
                      // 자동 초기화 안됨
        
        cout<<num<<","<<val<<endl;
        function(val); // 함수에서 값을 바꿔도 변하지 않는다
        cout<<num<<","<<val<<endl;
    }
    void function(int n)
    {
        int val=100;  // 함수가 다르면 동일 이름의 변수 사용 가능
                      // 지역변수는 사용영역이 함수 내부로 제한되기 때문
    
        n=1000;
        num=200;
    }

---

    // static변수
    void function();
    
    void main() 
    {
        for(int i=0;i<5;i++)
            function();
    }
    
    void function()
    {
        int num=0; // 지역변수
        static int snum=0;  // static 변수는 단 한번만 초기화 된다.
                            // 호출되면 변화된 값을 유지한다.
        cout<<num<<","<<snum<<endl;
        num++;
        snum++;
    
        cout<<"function 호출 횟수: "<<snum<<endl;
    }


---

    extern int sum;  // 외부 파일에 있는 변수, 전역변수만 extern이 될 수 있다
    extern int get_square(int); // 외부 파일에 있는 함수;
    
    void main()
    {
        int num=5;
        cout<<get_square(num)<<endl;
        cout<<sum<<endl;
    }
 
---

    const int STUDENTS=5; // 통상 상수는 main 함수 이전에 선언
    
    void main()
    {
        int grade[STUDENTS];  // 변수로는 배열 크기 설정할 수 없음
        int sum=0,i,average;
        /*
        int num=10;
        int score[num];
        */     // 컴파일 에러----> const를 붙여주자
    
        for(i=0;i<STUDENTS;i++)
        {
            cout<<"성적 입력: ";
            cin>>grade[i];
        }
        for(i=0;i<STUDENTS;i++)
            sum+=grade[i];
    
        average=sum/STUDENTS;
        cout<<"평균: "<<average<<endl;
    }

---

    const int SIZE=5;
    
    void main()
    {
        int a[SIZE] = {1,2,3,4,5};
        int b[SIZE],i;
    
        for(i=0;i<SIZE;i++)       // b=a; error
            b[i]=a[i];
    
        for(i=0;i<SIZE;i++)       // a=b; error
            if(a[i]!=b[i])
            {
                cout<<"다른 배열\n";
                break;
            }
        if(i==SIZE)
            cout<<"같은 배열\n";
    }
 
---
 
    void main()
    {
        int grade[100];
        int i,max,min;
    
        srand((unsigned int)time(NULL));
        for(i=0;i<10;i++)
        {
            grade[i]=rand();
            cout<<grade[i]<<endl;
        }
        max=min=grade[0];
        for(i=1;i<10;i++)
        {
            if(max<grade[i])
                max=grade[i];
            if(min>grade[i])
                min=grade[i];
        }
        cout<<"Max: "<<max<<endl;
        cout<<"Min: "<<min<<endl;
    }

---

    int global_array[5]; // 전역 배열은 자동 초기화
    
    void main()
    { 
        int local_array[5]={0}; // 지역 배열은 자동 초기화 되지 않는다
        int grade[]={31,52,43,34,25};
    
        for(int i=0;i<5;i++)
        {
            cout<<global_array[i]<<endl;
            cout<<local_array[i]<<endl;
            cout<<grade[i]<<endl<<endl;
        }
    }

------------------------------------------------------

    void main()
    {
        int freq[10];
        int i,score;
    
        for(i=0;i<10;i++)
            freq[i]=0;
        while(true)
        {
            cout<<"숫자 입력(0-9(종료-1)) : ";
            cin>>score;
            if(score<0)
                break;
            freq[score]++;  // 입력값을 배열의 첨자로 활용
        }
        cout<<"값 빈도\n";
        for(i=0;i<10;i++)
            cout<<i<<" "<<freq[i]<<endl;
    }
    
---

    void main()
    {
        int array[10]={0}, i;
        int num,max;
    
        srand((unsigned int)time(NULL));
        for(i=0;i<100;i++)
        {
            num=rand()%10;
            array[num]++;  // array[rand()%10]++;
        }
    
        for(i=0;i<10;i++)  // 배열 출력
            cout<<i<<" : "<<array[i]<<endl;
    
        max=0;  // 최댓값 찾기
        for(i=1;i<10;i++)
            if(array[max]<array[i])
                max=i;
    
        cout<<"가장 많이 생성된 수는: "<<max<<endl;
        cout<<"횟수 : "<<array[max]<<endl;
    }

----------------------------------------

    #include <iostream>
    #include <iomanip>         // setw() 칸수 제어
    using namespace std;
    
    
    void main()
    {
        int s[3][5] = { 0 };
        int i, j, value = 0;
    
        for (i = 0; i < 3; i++)
            for (j = 0; j < 5; j++)
                s[i][j] = value++;
        for (i = 0; i < 3; i++)
        {
            for (j = 0; j < 5; j++)
                cout << setw(3) << s[i][j];  // ==printf("%3d",s[i][j])
        }
        cout << endl;
    }

--------------------------------------------------------

    #include <iostream>
    using namespace std;
    
    void main()
    {
        int grade[8] = { 3,6,1,3,6,7,5,7};
        int select[8] = { 0 };
    
        for (int i = 0; i < 8; i++)
            cout << grade[i] << " ";
    
        cout << endl << endl;
    
        for (int i = 0; i < 8; i++)
        {
            for(int j=0;j<8;j++)
                if (grade[j] > grade[j + 1])
                {
                    select[j] = grade[j];
                    grade[j] = grade[j + 1];
                    grade[j + 1] = select[j];
                }
        }
    
        for (int i = 0; i < 8; i++)
            cout << grade[i] << " ";
    }

--------------------------------------

    #include <iostream>
    #include <ctime>
    using namespace std;
    
    void main()
    {
        int i;
        time_t start, end; // time_t == long
    
        start = clock();
    
        for (int i = 0; i < 10000000; i++); // 천만번
    
        end = clock();
        cout << (double)(end - start) / CLOCKS_PER_SEC << endl << endl;
    
    }  // 천만번까지 세는데 시간

## 순차배열

    #include <iostream>
    using namespace std;
    
    const int SIZE=10;
    
    void selection_sort(int list[],int n);
    void print_list(int list[],int n);
    
    int main()
    {
        int grade[SIZE]={3,2,9,7,1,4,5,9,2,8};
        cout<<"원래의 배열"<<endl;
        print_list(grade,SIZE);
    
        selection_sort(grade,SIZE);
    
        cout<<"정렬된 배열"<<endl;
        print_list(grade,SIZE);
    
        return 0;
    }
    
    void print_list(int list[],int n)
    {
        int i;
        for(i=0;i<n;i++)
            cout<<list[i]<<" ";
        cout<<endl;
    }
    
    void selection_sort(int list[],int n)
    {
        int i,j,temp,least;
    
        for(i=0;i<n-1;i++)
        {
            least=i;
    
            for(j=i+1;j<n;j++)
            {
                if(list[j]<list[least])
                    least=j;
            }
            temp=list[i];
            list[i]=list[least];
            list[least]=temp;
        }
    }
 
## 이진탐색

    #include <iostream>
    using namespace std;
    
    const int SIZE=10;
    
    int binary_search(int list[],int n,int key);
    
    int main(void)
    {
        int key;
        int grade[SIZE]={1,2,3,4,5,6,7,8,9};
    
        cout<<"탐색할 값을 입력하시오:";
        cin>>key;
        cout<<"탐색결과:"<<binary_search(grade,SIZE,key)<<endl;
        return 0;
    }
    int binary_search(int list[],int n,int key)
    {
        int low,high,middle;
    
        low=0;
        high=n-1;
    
        while(low<=high)
        {
            middle=(low+high)/2;
            if(key==list[middle])
                return middle;
            else if(key>list[middle])
                low=middle+1;
            else
                high=middle-1;
        }
        return -1;
    }


    //모든 포인터 변수는 타입에 관계 없이 4바이트를 할당한다
    //포인터 변수는 메모리 주소를 갖기 위한 변수이고
    //주소는 4바이트를 사용하기 때문이다.

    #include <iostream>
    using namespace std;
    
    void main()
    {
        int i=30;
        int *pi=&i;
    
        printf("%d %d\n",&i,i);
        printf("%d %d %d\n",pi,*pi,&pi);
        // pi: 포임터 변수가 참조하는 곳의 주소
        // *pi: 포인터 변수가 참조하는 곳의 값
        // &pi: 포인터 변수의 메모리 주소
    
        char ch='a';
        char *pc;
        pc=&ch;
        printf("%c %d\n",ch,&ch);
        printf("%c %d %d\n",*pc,pc,&pc);
    
        double d=3.14;
        double *pd=&d;
        printf("%.2lf %d\n",d,&d);
        printf("%.2lf %d %d\n",*pd,pd,&pd);
    
        printf("%d %d %d\n",sizeof(pi),sizeof(pc),sizeof(pd));
    }

-----------------------------------------------------------

    void main()
    {
        int i=10;
        int *ptr;
    
        ptr=&i;
        printf("%d %d\n",i,&i);
        printf("%d %d %d\n",*ptr,ptr,&ptr);
    
        *ptr=100;
        printf("%d %d\n",i,&i);
        printf("%d %d %d\n",*ptr,ptr,&ptr);
    
        (*ptr)++;
        printf("%d %d\n",*ptr,i);
    
        *ptr+=1;
        printf("%d %d\n",*ptr,i);
    
        printf("%d %u %x %x %p\n",&i,&i,&i,&i,&i);
    }

------------------------------------------------------------------

    void main()
    {
        char ch[]={'a','b','c'};
        int num[]={1,2,3};
        double d[]={1.1,2.2,3.3};
    
        char *pc=ch;   // 배열의 이름은 배열의 시작 주소를 의미한다.
        int *pi=num;
        double *pd=d;
    
        printf("%d %d %d\n",ch,num,d);
        printf("%d %d %d\n",pc,pi,pd);
        pc++; // 자료형에 따라 더하는 값이 바뀜 -> char형이므로 1바이트므로 1개씩
        pi++; // int형이므로 4바이트이므로 4개
        pd++;
        printf("%d %d %d\n",pc,pi,pd);
    
        pi=&num[2];
        cout<<*pi<<","<<num[2]<<endl;
        //num=pi; 배열의 주소는 바꿀수 없다.(배열상수,포인터변수)
    
    }

------------------------------------------------------

    void main()
    {
        int num[]={10,20,30};
        int *pi=num;
    
        for(int i=0;i<3;i++)
        {
            cout<<num[i]<<","<<*(pi+i)<<endl;
            cout<<&num[i]<<","<<pi+i<<endl;
        }
        for(int i=0;i<3;i++)
        {
            cout<<num[i]<<","<<*pi<<endl;
            cout<<&num[i]<<","<<pi<<endl;
            pi++; // pi의 기준값이 바뀜
        }
        cout<<pi<<endl; // 범위에 벗어난 값
        pi=num;
        cout<<pi<<endl;  // 배열의 시작위치로 이동
    }

-------------------------------------------------

    void main()
    {
        int *pi=NULL;  // NULL로 초기화하는 경우(아무것도 가리키지 않는다)
    
        pi=new int;  // pi가 참조하는 4바이트 할당
        cout<<pi<<" : "<<*pi<<endl;
        *pi=100;
        cout<<pi<<" : "<<*pi<<endl;
    
        delete pi;
    
        int *ptr=new int[5];
    
        for(int i=0;i<5;i++)
        {
            cout<<"Enter number :";
            cin>>ptr[i];
        }
        for(int i=0;i<5;i++)
            cout<<ptr[i]<<endl;
        delete []ptr;
    }

--------------------------------------------

    void main()
    {
        int num;
        int *ptr;
    
        cout<<"Enter number of data: ";
        cin>>num; 
            // int arr[]; 배열을 쓰면 메모리를 무조건 정해주어야한다. 만약에 10000개를 써놓고 100개만 쓰면 
                   // 엄청난 데이터의 손실이 되기때문에 메모리 동적할당을 하게 되면 나중에 메모리를
                   // 쓰고 싶은 만큼 쓸 수 있다
                
        ptr=new int[num];  // 동적 메모리 할당
                         // 실행 시간에 필요한 메모리 할당 가능
    
        for(int i=0;i<num;i++)
        {
            cout<<"Enter data: ";
            cin>>ptr[i];
        }
        for(int i=0;i<num;i++)
            cout<<ptr[i]<<endl;
        delete []ptr;
    }

-------------------------------------

    #include <iostream>
    using namespace std;
    
    void swap2(int *,int *); //call by reference(주소는 반드시 포인터로 받는다)
    
    void main()
    {
        int n1=10,n2=20;
    
        cout<<&n1<<","<<&n2<<endl;
    
        swap2(&n1,&n2);
        cout<<n1<<","<<n2<<endl;
    }
    void swap2(int *n1,int *n2) // 주소는 반드시 포인터로 받는다
    {
        int temp;
        cout<<n1<<","<<n2<<endl; // main의 변수들과 같은 주소
        temp=*n1;
        *n1=*n2;
        *n2=temp;
        cout<<"swap2: "<<*n1<<","<<*n2<<endl;
    }

-----------------------------------------------
    
    #include <iostream>
    using namespace std;
    
    void min_max(int [],int *,int *);
    
    void main()
    {
        int array[]={3,2,5,6,4};
        int min,max;
    
        min_max(array,&min,&max); // min,max의 주소 전달
        cout<<"Min: "<<min<<endl;
        cout<<"Max: "<<max<<endl;
    }
    
    void min_max(int a[],int *min,int*max)
    {
        int i;
    
        *min=*max=a[0];
        for(i=1;i<5;i++)
        {
            if(*min>a[i])
                *min=a[i];
            if(*max<a[i])
                *max=a[i];
        } //main에 있는 min,max의 주소에 값을 저장
    }

--------------------------------------------------

    #include <iostream>
    using namespace std;
    /*
    void get_sum_diff(int *a,int *b,int *p_sum,int *p_diff);
    
    void main()
    {
        int A=10,B=20;
        int sum;
        int diff;
        get_sum_diff(&A,&B,&sum,&diff);
        cout<<"두 수의 합:"<<sum<<"두 수의 차:"<<diff<<endl;
    }
    
    void get_sum_diff(int *a,int *b,int *p_sum,int *p_diff)
    {
        *p_sum=*a+*b;
        *p_diff=*a-*b;
    }
    */  ---------------> 수의 합과 차
    
    void get_sum_diff(int A[],int B[],int *p_sum,int *p_diff)
    {
        for(int i=0;i<5;i++)
        {
            p_sum[i]=A[i]+B[i];
            p_diff[i]=A[i]-B[i];
        }
        cout<<"두 배열의 합"<<endl;
        for(int i=0;i<5;i++)
        {
            cout<<p_sum[i]<<" ";
        }
        cout<<endl<<endl;
        cout<<"두 배열의 차"<<endl;
        for(int i=0;i<5;i++)
        {
            cout<<p_diff[i]<<" ";
        }
    }
    
    void main()
    {
        int A[5]={5,4,3,2,1};
        int B[5]={1,2,3,4,5};
        int sum[5];
        int diff[5];
        get_sum_diff(A,B,sum,diff);
    }  ----------------------> 배열의 각각 합과 차

--------------------------------------------------

    #include <iostream>
    #include <string>
    using namespace std;
    
    void Swap(int *,int *);
    
    void main()
    {
        int A[5]={1,2,3,4,5};
        int B[5]={9,8,7,6,5};
        
        Swap(A,B);
    }
    
    void Swap(int *a,int *b)
    {
        int temp[5];
        for(int i=0;i<5;i++)
        {
            temp[i]=a[i];
            a[i]=b[i];
            b[i]=temp[i];
        }
        cout<<"A[]"<<endl;
        for(int i=0;i<5;i++)
        {
            cout<<a[i]<<" ";
        }
        cout<<endl<<endl;
        cout<<"B[]"<<endl;
        for(int i=0;i<5;i++)
        {
            cout<<b[i]<<" ";
        }
    } ---------------> 배열의 값을 서로 바꾸는것
    /*
    void main()
    {
        string s1="This is a test";
        string s2;
    
        cout<<s1<<endl;
        cout<<"Enter string:";
        cin>>s2;
        cout<<s2<<endl;
    }
    */ ---------------> string 함수

-----------------------------------------------

    void main()
    {
        string s1; // ----> string은 널문자 포함 안함
    
        cout<<"enter string: ";
        getline(cin,s1,'\n'); // 전체 라인 입력, '\n'은 default(지워도 상관없음) 끝점의 기준을 정해준
        cout<<s1.size()<<endl; //문자열의 길이
        cout<<s1.length()<<endl; //문자열의 길이
        s1.insert(4,"Hello"); // 4번째 문자에 "Hello"를 넣어라
        cout<<s1<<endl;
    
        s1.erase(4,5); // 4번에서부터 5글자를 삭제하라
        cout<<s1<<endl;
    
        if(s1.empty())
            cout<<"empty string"<<endl;
        else
            cout<<"not empty string"<<endl;
        s1.erase(); // 전체 삭제
        cout<<s1<<endl;
    }

---------------------------

    void main()
    {
        string s1="Money";
        string s2=" has no value";
        string s3=s1+s2; //  +연산자로 문자열 연결, =연산자로 문자열 치환
    
        s1+=s2;
        cout<<s1<<endl;
        cout<<s2<<endl;
        cout<<s3<<endl;
    
        string str1("Slow"),str2("steady"); // string객체를 만드는 다른방법
        string str3="the race";
    
        string s4=str1+" and "+str2+" wins "+str3;
        cout<<s4<<endl;
    
        if(s1==s2) //string 비교  ------> strcmp사용할 필요 없다
            cout<<"same"<<endl;
        else
            cout<<"different"<<endl;
    }

-------------------------------------------------------

    /*
    void main()
    {
        string s;
        cout<<"주민번호 입력(-포함) :";
        cin>>s;
        for(int i=0;i<s.size();i++) // ---> string은 배열처럼 사용가능
        {
            if(s[i]!='-') // string의 각 문자 점검(배열 사용)
            {
                cout<<s[i];
            }
            //if(s.at(i)!='-') // 함수 사용
                cout<<s.at(i);//
            
        }
    }
    */
    void main()
    {
        char str[30];
        string name;
    
        cout<<"Enter string:";
        cin.getline(str,30); // iostream 함수 --> 배열인 경우에만 사용가능
        cout<<str<<endl;
    
        cout<<"Enter string:";
        getline(cin,name,'\n'); // '\n'은 default,string 함수 --> string인 경우에만 사용가능(이걸 자주 사용하자)
        cout<<name<<endl;
    }

------------------------------------------------

    void main()
    {
        string str;
        int numA=0,numB=0,numC=0;
    
        cout<<"문자열을 입력하시오:";
        getline(cin,str); // cin에 엔터키를 칠때까지 str을 넣어라
        for(int i=0;i<str.length();i++)
        {
            if(isalpha(str.at(i)))
                numA++;
            else if(isdigit(str.at(i)))
                numB++;
            else if(isspace(str.at(i)))
                numC++;
            else
                cout<<"Incorrect character"<<endl;
        }
        cout<<"alphabet :"<<numA<<endl;
        cout<<"numeric :"<<numB<<endl;
        cout<<"space :"<<numC<<endl;
    }

----------------------------------------

    /*
    void main()
    {
        string str, search, replace;
    
        cout<<"Enter string: ";
        getline(cin,str);
        cout<<"Enter string to find: ";
        getline(cin,search);
        cout<<"Enter string to replace: ";
        getline(cin,replace);
    
        int position=str.find(search); // 없으면 string::npos 리턴
        if(position!=string::npos) // 문자열의 최대길이 +1
        {
            str.replace(position,search.length(),replace);
            str.erase(position,search.length());
            //str.erase(psition,search.length()); //두번째 방법임
            //str.insert(position,replace);
            cout<<str<<endl;
        }
        else 
            cout<<search<<" is not exist"<<endl;
    }*/
    
    // 바꿀 문자열이 여러개 있는 경우
    
    void main()
    {
        string str, search, replace;
        int position=0,start=0;
    
        cout<<"Enter string: ";
        getline(cin,str);
        cout<<"Enter string to find: ";
        getline(cin,search);
        cout<<"Enter string to replace: ";
        getline(cin,replace);
    
        while(true)
        {
            position=str.find(search,start);
            if(position==string::npos)
                break;
            str.replace(position,search.length(),replace);
    
            cout<<str<<endl;
            start=position+search.length();
        }
    }

---

    #include <iostream>
    using namespace std;
    
    void main()
    {
        int var;
        int &ref=var; // var는 ref라는 별명을 갖는다
    
        var=10;
        cout<<var<<","<<ref<<endl;
    
        ref=20;
        cout<<var<<","<<ref<<endl;
    
        int num=100;
        ref=num;  // ref가 num을 참조하는 것이 아니고 num의 값을 ref에 대입
        cout<<var<<","<<ref<<endl;
        cout<<&var<<","<<&ref<<","<<&num<<endl;
    }
    
    // reference를 위한 메모리는 할당되지 않는다
    // 선언과 동시에 기존의 변수로 초기화 되어야 한다
    // 상수 초기화 불가(int &ref=100;)

--------------------------------

    #include <iostream>
    using namespace std;
    
    void swap1(int *, int *);
    void swap2(int &, int &);
    
    void main()
    {
        int a=10,b=20;
    
        swap1(&a,&b);
        cout<<a<<","<<b<<endl;
    
        swap2(a,b);
        cout<<a<<","<<b<<endl;
    }
    void swap1(int *pa,int *pb)
    {
        int temp;
        temp=*pa;
        *pa=*pb;
        *pb=temp;
    }
    void swap2(int &ra,int &rb)
    {
        int temp;
    
        temp=ra;
        ra=rb;
        rb=temp;
    }

-----------------------------------

    #include <iostream>
    using namespace std;
    
    int squre(int);
    double square(double);
    
    void main()
    {
        int i=10;
        double d=3.4;
    
        cout<<square(i)<<endl;
        cout<<square(d)<<endl;
    }
    int squrae(int i)
    {
        return i*i;
    }
    double square(double d)
    {
        return d*d;
    }

----------------------------------

    #include <iostream>
    using namespace std;
    
    void main()
    {
        int *p1=new int;
        *p1=100;
        cout<<p1<<","<<*p1<<endl;
        delete p1;
    
        int *p2=new int[10];
        for(int i=0;i<10;i++)
        {
            p2[i]=i;
            cout<<&p2[i]<<" : "<<p2[i]<<endl;
        }
        delete [] p2;
    }

---------------------------

    #include <iostream>
    using namespace std;
    
    int mode;
    
    namespace Graphics
    {
        int mode, x, y;
    
        void message()
        {
            cout<<"Graphics namespace의 message()"<<endl;
        }
    }
    
    namespace Network
    {
        int mode, speed;
    
        void send()
        {
            cout<<"Network namespace의 send()"<<endl;
        }
        void message()
        {
            cout<<"Network namespace의 message()"<<endl;
        }
    }
    
    void main()
    {
        //x=y=100;
        //speed=200;
        Graphics::x = Graphics::y =100;  // :: 영역지정연산자
        Network::speed = 200;
    
        mode = 1;  // 전역변수
        Graphics::mode = 1;
        Network::mode = 2;
    
        Graphics::message();
        Network::message();
    }
    
    /*
    using namespace Graphics;
    using namespace Network;
    
    ---> ::mode ---> 전역변수
    */

--------------------------------------------

    #include <iostream>
    using namespace std;
    
    void get_input(int &x)
    {
        cout<<"enter num:";
        cin>>x;
    }
    
    void get_input(int *y)
    {
        cout<<"enter num:";
        cin>>*y;
    }
    
    void main()
    {
        int num1;
        int num2;
    
        get_input(num1);
        cout<<num1<<endl;
    
        get_input(&num2);
        cout<<num2<<endl;
    }

----------------------------------------------

    #include <iostream>
    using namespace std;

    void get_input(int &x)
    {
        cout<<"enter num:";
        cin>>x;
    }
    
    void get_input(int *y)
    {
        cout<<"enter num:";
        cin>>*y;
    }
    
    void get_input(int &z, int &w)
    {
        cout<<"enter num:";
        cin>>z>>w;
    }
    
    void main()
    {
        int num1;
        int num2;
    
        get_input(num1);
        cout<<num1<<endl;
    
        get_input(&num2);
        cout<<num2<<endl;
    
        get_input(num1,num2);
        cout<<num1<<" "<<num2<<endl;
    }

---

    int& get_max(int &x,int &y)
    {
        if(x>y)
            return x;
        else
            return y;
    }
    
    double get_max(double &x,double &y)
    {
        if(x>y)
            return x;
        else
            return y;
    }
    
    void main()
    {
        int num1=10,num2=20;
        double num3=2.3,num4=1.3;
        cout<<"정수형 큰 값:"<<get_max(num1,num2)<<endl;
        cout<<"실수형 큰 값:"<<get_max(num3,num4)<<endl;
    }

---

    int make_double(int &x)
    {
        x*=2;
        return x;
    }
    
    void main()
    {
        int num=20;
        make_double(num);
        cout<<"두 배의 값:"<<num<<endl;
    }
