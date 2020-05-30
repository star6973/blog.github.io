---
layout: post
title: "C++ Programming [Day 2]"
date: 2020-04-06 09:00:00-18:00:00
categories: Language
tag: Language
use_math: true
---

# C++ 예제 3일만에 뽀개기 - Day 2

    #include <iostream>
    #include <string>
    using namespace std;
    
    class Car    //클래스 이름은 대문자로 시작
    {
    public:      //접근 제어
        int speed;     //멤버 변수
        int gear;
        string color;
    
        void speedUp()    //멤버 함수
        {
            speed+=10;
        }
        void speedDown()
        {
            speed-=10;
        }
    };       //클래스 정의 끝에 ; 반드시 필요
    
    void main()
    {
        Car myCar, yourCar;     //객체 변수 선언
    
        myCar.speed=100;
        yourCar.speed=60;
        myCar.color="white";
    
        yourCar.speedUp();
        cout<<myCar.speed<<endl;
        cout<<yourCar.speed<<endl;
        cout<<myCar.color<<endl;
    }
	
---

    #include <iostream>
    #include <string>
    using namespace std;
    
    class Car    //클래스 이름은 대문자로 시작
    {
    public:      //접근 제어
        int speed;     //멤버 변수
        int gear;
        string color;
    
        void speedUp()    //멤버 함수
        {
            speed+=10;
        }
        void speedDown()
        {
            speed-=10;
        }
    };       //클래스 정의 끝에 ; 반드시 필요
    
    void main()
    {
        Car myCar, yourCar;     //객체 변수 선언
    
        myCar.speed=100;
        yourCar.speed=60;
        myCar.color="white";
    
        yourCar.speedUp();
        cout<<myCar.speed<<endl;
        cout<<yourCar.speed<<endl;
        cout<<myCar.color<<endl;
    }
	
---

    #include <iostream>
    #include <string>
    using namespace std;
    
    class Employee
    {
        string name; // 접근제어 지정하지 않으면 default로 private
        int salary, age;
    public:
        void setAge(int a) // 자동적으로 inline함수가 된다
        {
            age=a;
        }
        int getAge()
        {
            return age;
        }
        void setName(string n)
        {
            name=n;
        }
        string getname()
        {
            return name;
        }
        void SetSalary(int s)
        {
            salary=s;
        }
        int getSalary()
        {
            return salary;
        }
    };
    
    void main()
    {
        Employee e;
        //e.salary=300;  //private member에  접근할 수 없다
        //e.age=20;
    
        e.setName("Hong Gilldong");
        e.setAge(20);
        string name=e.getname();
        int age=e.getAge();
        cout<<name<<" ,"<<age<<endl;
    }

-------------------------------------------

    #include <iostream>
    #include <string>
    using namespace std;
    
    class Employee
    {
        string name;   // 접근제어 지정하지 않으면 default로 private
        int salary,age;
    public:   // 코딩할때 먼저 함수의 원형을 앞에 선언하자 가독성이 좋아진다
                // 클래스 내부에는 함수원형, 클래스 밖에서 함수 정의
        void setAge(int);   
        int getAge();
        void setName(string);
        string getName();
        void setSalary(int);
        int getSalary();
    };
    
    void Employee::setAge(int a)
    {
        age=a;
    }
    int Employee::getAge()
    {
        return age;
    }
    void Employee::setName(string s)
    {
        name=s;
    }
    string Employee::getName()
    {
        return name;
    }
    void Employee::setSalary(int s)
    {
        salary=s;
    }
    int Employee::getSalary()
    {
        return salary;
    }

## 주사위 게임

    #include <iostream>
    #include <ctime>
    #include <cstdlib>
    #include <string>
    using namespace std;
    
    class DiceGame
    {
    private: // 정보은닉(information hiding)
        int diceFace;
        int userGuess;
    public:
        void rollDice()
        {
            srand((unsigned int)time(NULL));
            diceFace=rand()%6+1;
        }
        void getUserInput(string prompt)
        {
            cout<<prompt;
            cin>>userGuess;
        }
        void checkUserGuess()
        {
            if(diceFace==userGuess)
                cout<<"맞았습니다"<<endl;
            else
                cout<<"틀렸습니다"<<endl;
        }
    };
    
    void main()
    {
        DiceGame game;
    
        game.rollDice();
        game.getUserInput("예상값을 입력하시오:");
        game.checkUserGuess();
    }

## 램프

    #include <iostream>
    using namespace std;
    
    class DeskLamp
    {
    private:
        bool isOn; //꺼져있나 켜져있나의 상태를 나타내는 변수
    public:
        void turnOn();
        void turnOff();
        void print();
    };
    
    void DeskLamp::turnOn()
    {
        isOn=true;
    }
    void DeskLamp::turnOff()
    {
        isOn=false;
    }
    void DeskLamp::print()
    {
        cout<< "램프가 " << ( isOn==true ?  "켜짐"  :  "꺼짐" ) <<endl;
    }
    void main()
    {
        DeskLamp lamp;
        lamp.turnOn();
        lamp.print();
        lamp.turnOff();
        lamp.print();
    }
    
    //멤버함수를 클래스 내부에서 정의하면 자동으로 inline 함수(치환을 하게됨)가 된다
    //따라서 비효율적이다

---

    //생성자(constructor) 함수
    //클래스 이름과 동일하다
    //리턴 값이 없다
    //반드시 public 이어야 한다
    //생성자도 함수이기 때문에 중복정의 할 수 있다(오버로딩)
    //객체 생성시 자동으로 호출된다
    //주로 멤버변수값들을 초기화하는데 사용된다
    
    #include <iostream>
    #include <string>
    using namespace std;
    
    class Car
    {
        int speed, gear; //멤버변수에 초기값 지정 불가
        string color;     //멤버변수는 객체가 생성되어야 존재하기 때문
    public:
        Car() {  //디폴트 생성자(매개변수 없는 생성자)
            cout<<"default constructor(디폴트 생성자) 호출\n";
            speed=0;
            gear=1;
            color="white";
        }
        void print(){
            cout<<speed<<","<<gear<<","<<color<<endl;
        }
    };
    void main()
    {
        Car car; //객체를 생성하면 자동으로 생성자 호출
                  //디폴트 생성자 호출시 car() 아님. 주의
        car.print();
    }

-------------------------------------------------------------

    #include <iostream>
    #include <string>
    using namespace std;
    
    class Car
    {
        int speed,gear;
        string color;
    public:
        Car(int s,int g,string c) //매개변수 있는 생성자
        {
            cout<<"매개변수 있는 생성자 호출"<<endl;
            speed=s;
            gear=g;
            color=c;
        }
        void print()
        {
            cout<<speed<<","<<gear<<","<<color<<endl;
        }
    };
    void main()
    {
        Car car1(100,4,"blue"); //객체를 생성하면 자동으로 생성자 호출
        car1.print();
        
        Car car2=Car(10,2,"red"); //C 스타일 생성자 호출(사용하지 않는다)
        car2.print();
    
        Car car3;
    }

--------------------------------------------------------------

    // 디폴트 매개변수를 사용하는 경우
    
    #include <iostream>
    #include <string>
    using namespace std;
    
    class Car
    {
        int speed,gear;
        string color;
    public:
        Car(int s=0,int g=2,string c="blue") //매개변수 있는 생성자
        {
            cout<<"매개변수 있는 생성자 호출"<<endl;
            speed=s;
            gear=g;
            color=c;
        }
        void printf()
        {
            cout<<speed<<","<<gear<<","<<color<<endl;
        }
    };
    void main()
    {
        Car car1;
        Car car2(100);
        Car car3(10,3);
        Car car4(10,2,"red");
    
        car1.printf();
        car2.printf();
        car3.printf();
        car4.printf();
    }

--------------------------------------------------------------

    //소멸자(destructor) 함수
    //클래스 이름 앞에 ~가 붙는다
    //생성자와 마찬가지로 반환값 없다
    //매개변수가 없기 때문에 중복정의 할 수 없다
    //객체 소멸시 자동으로 호출된다
    //주로 할당된 자원(메모리,파일 등)을 반납하는 용도로 사용된다
    //지금까지 소멸자 정의하지 않았지만 실행이 되는 이유는?
    //소멸자가 없는 경우 컴파일러는 자동으로 소멸자를 만들어 준다
    //Car::~Car() {}
    //얕은 복사
    
    #include <iostream>
    #include <string>
    using namespace std;
    
    class Car
    {
        int speed,gear;
        char *color;
    public:
        Car(int s,int g,char *c)
        {
            cout<<"생성자 호출"<<endl;
            speed=s;
            gear=g;
            color=c; //얕은 복사의 경우 주소 복사//호출이 끝나면 c는 소멸됨(가지고 있는 정보 "white"도 같이 사라짐)
            printf("%s %s\n",c,color);
            printf("%d %d\n",c,color);
        }
        void print()
        {
            cout<<speed<<","<<gear<<","<<color<<endl;
        }
        ~Car()
        {
            cout<<"소멸자 호출"<<endl;
            delete []color; //위에서 이미 "white"라는 값을 소멸시켜서 더 소멸시킬 것이 없다
        }
    };
    void main()
    {
        Car car(100,5,"white");
        car.print();
        cout<<sizeof(car)<<endl;
    }
    
    ----> 해결하는 방법:깊은복사
    
    #include <iostream>
    #include <string>
    using namespace std;
    
    class Car
    {
        int speed,gear;
        char *color;
    public:
        Car(int s,int g,char *c)
        {
            cout<<"생성자 호출"<<endl;
            speed=s;
            gear=g;
            //color=c; //얕은 복사의 경우 주소 복사
            color=new char[strlen(c)+1]; //메모리 동적 할당(널문자 고려하자)
            strcpy(color,c); //깊은 복사
            printf("%s %s\n",c,color);
            printf("%d %d\n",c,color);
        }
        void print()
        {
            cout<<speed<<","<<gear<<","<<color<<endl;
        }
        ~Car()
        {
            cout<<"소멸자 호출"<<endl;
            delete []color;
        }
    };
    void main()
    {
        Car car(100,5,"white");
        car.print();
        cout<<sizeof(car)<<endl; //speed 4바이트, gear 4바이트, color 포인터변수이므로 4바이트
    }
 
--------------------------------------------------

    #include <iostream>
    #include <string>
    using namespace std;
    
    class Car
    {
        int speed,gear;
        char *color;
    public:
        Car(int s,int g,char *c) : speed(s),gear(g)  //초기화 리스트(이니셜 라이저)
        {
            cout<<"생성자 호출"<<endl;
            color=new char[strlen(c)+1];
            strcpy(color,c);
        }
        void print()
        {
            cout<<speed<<","<<gear<<","<<color<<endl;
        }
        ~Car()
        {
            cout<<"소멸자 호출"<<endl;
            delete []color;
        }
    };
    void main()
    {
        Car car(100,5,"white");
        car.print();
    }
    
    //1.멤버 중에 상수가 있을 경우, 반드시 이니셜 라이저 사용
    #include <iostream>
    #include <string>
    using namespace std;
    
    class Car
    {
        const int MAX_SPEED; //멤버 변수를 초기화할 수 없다
        int speed, gear;
        string color;
    public:
        Car(int s,int g,string c) : MAX_SPEED(200) //선언과 동시에 초기화시킴
        {
            //MAX_SPEED=200; //상수값을 재설정 할 수 없기 때문에 error
            speed=s;
            gear=g;
            color=c;
            cout<<"생성자 호출"<<endl;
        }
        void print()
        {
            cout<<speed<<","<<gear<<","<<color<<","<<MAX_SPEED<<endl;
        }
        ~Car()
        {
            cout<<"소멸자 호출"<<endl;
        }
    };
    void main()
    {
        Car car(100,2,"white");
        car.print();
    }
    
    //2.멤버 변수가 reference인 경우
    //절대 사용하지 않는다
    
    class Car
    {
        int &num; //선언과 동시에 초기화해야함
    }
    
    //3.멤버 변수중에 객체 변수가 있는 경우
    
    class Point
    {
        int x,y;
    public:
        Point(int ax,int ay) : x(ax),y(ay)
        {
            x=ax;
            y=ay;
            cout<<"Point 생성자\n";
        }
    };
    class Rectangle
    {
        Point p1,p2; //포함 관계(has-a관계)
    public:
        Rectangle(int x1,int y1,int x2,int y2) : p1(x1,y1),p2(x2,y2)
        {
            //p1(x1,y1); //생성자 함수는 명시적으로 호출할 수 없다
            //p2(x2,y2);
            cout<<"Rectangle 생성자\n";
        }
    };
    void main()
    {
        Rectangle r(1,2,3,4);
    }
    
    //멤버 중에 객체가 포함되어 있다면 포함된 객체가 먼저 생성되어야 한다
    //Rectangle 객체보다 Point 객체 p1,p2가 먼저 생성되어야 한다
    //Point의 생성자를 명시적으로 호출할 수 없기 때문에 콜론 초기화를 이용해서 
    //Point 객체 p1,p2를 먼저 생성한다.

------------------------------------------------------------------

    //복사생성자(copy constructor)
    //이미 생성된 객체와 멤버변수가 동일한 객체 생성시 자동 호출
    //컴파일러는 복사 생성자가 없으면 default 복사 생성자 제공
    #include <iostream>
    #include <string>
    using namespace std;
    
    class Car
    {
    private:
        int speed;
        int gear;
        string color;
    public:
        Car(int s,int g,string c) : speed(s),gear(g),color(c) 
        {
            cout<<"생성자 호출"<<endl;
        }
        Car(const Car& obj) //매개변수 형식에 주의(& 사용), 이유는?
                                    //그냥 쓸 경우 무한 복사가 되므로
        {
            cout<<"복사 생성자 호출"<<endl;
            speed=obj.speed;
            gear=obj.gear;
            color=obj.color;
        }
        void print()
        {
            cout<<speed<<","<<gear<<","<<color<<endl;
        }
        ~Car()
        {
            cout<<"소멸자 호출"<<endl;
        }
    };
    void main()
    {
        Car c1(0,1,"yellow");
        Car c2(c1);
    
        c1.print();
        c2.print();
    }
    
    //객체를 2개 생성하지만 생성자 호출은 1번, 소멸자 호출은 2번
    //c2 객체 생성은 복사생성자를 호출하기 때문이다
    //컴파일러가 제공하는 디폴트 복사 생성자 사용
    //디폴트 복사 생성자는 멤버 대 멤버를 복사하는 얕은 복사(shallow copy) 사용
    //포인터가 있으면 복사 생성자 생성 안할 경우 런타임 에러 뜸
    
    //복사생성자를 쓰는 경우
    //1. 기존 객체와 동일한 새로운 객체를 생성
    //2. 함수에 객체를 전달하는 경우
    //3. 함수에서 객체를 리턴하는 경우

------------------------------------------------------------

    #include <iostream>
    using namespace std;
    
    class Date
    {
    private:
        int year, month, day;
        int i; //소멸자 순서 public에 넣어도 실행 가능
    public:
        Date();
        Date(int,int,int);
        void setDate(int,int,int);
        Date(Date &date);
        void print();
        ~Date();
        //int i; //소멸자 순서
    };
    Date::Date()
    {
        cout<<"생성자 호출"<<endl;
        year=0;
        month=0;
        day=0;
        i=1;
    }
    Date::Date(int year, int month, int day) : year(year),month(month),day(day) 
    {
        cout<<"매개변수 생성자 호출"<<endl;
        i=2;
    }
    void Date::setDate(int year, int month, int day) //잘못된 달력을 넣을 수 있기에 점검하는 코드를 집어넣자
    {
        this->year=year;
        this->month=month;
        this->day=day;
        i=3;
    }
    Date::Date(Date &date) : year(date.year),month(date.month),day(date.day)
    {
        cout<<"복사생성자 호출"<<endl;
        i=4;
    }
    void Date::print()
    {
        cout<<"년도:"<<year<<endl;
        cout<<"달:"<<month<<endl;
        cout<<"일:"<<day<<endl;
    }
    Date::~Date()
    {
        cout<<i<<"소멸자 호출"<<endl;
    }
    void main()
    {
        Date date1;
        date1.print();cout<<endl;
        Date date2(2017, 5, 2);
        date2.print();cout<<endl;
        Date date3;
        date3.setDate(2017, 12, 4);
        date3.print();cout<<endl;
        Date date4(date3);
        date4.print();cout<<endl;
    }

---

    #include <iostream>
    using namespace std;
    
    class MyString
    {
        char *str;
    public:
        MyString();
        MyString(char *r);
        MyString(const MyString &);
        ~MyString();
        void print();
        int i; //소멸 순서
    };
    MyString::MyString()
    {
        str=new char[100];
        cout<<"디폴트 생성자 호출"<<endl;
        strcpy(str,"my name is jmh");
        i=1;
            //만약 입력할 거면 
            cout<<"입력:";
            cin.getline(str,100);
    }
    MyString::MyString(char *r)
    {
        cout<<"매개변수 생성자 호출"<<endl;
        str=new char[strlen(r)+1];
        strcpy(str, r);
        i = 2;
    }
    MyString::MyString(const MyString &mine)
    {
        cout<<"복사 생성자 호출"<<endl;
        str=new char[strlen(mine.str)+1];
        strcpy(str,mine.str);
        i=3;
    }
    MyString::~MyString()
    {
        cout<<i<<"소멸자 호출"<<endl;
        delete []str;
    }
    void MyString::print()
    {
        cout<<"글자 출력:"<<str<<endl;
    }
    void main()
    {
        MyString mine1;
        mine1.print();
        cout<<endl;
        MyString mine2("hi");
        mine2.print();
        cout<<endl;
        MyString mine3(mine2);
        mine3.print();
        cout<<endl;
    }

---

    //객체 동적 생성
    
    #include <iostream>
    #include <string>
    using namespace std;
    
    class Car
    {
        int speed, gear;
        string color;
    public:
        Car(int s=0, int g=1, string c="white") : speed(s), gear(g), color(c) {}
        void display();
    };
    void Car::display()
    {
        cout<<"Speed : "<<speed<<", Gear : "<<gear<<", Color : "<<color<<endl;
    }
    void main()
    {
        Car myCar;
        myCar.display(); //일반 객체의 멤버 접근은 . 연산자 사용
        cout<<sizeof(myCar)<<endl;
    
        Car *pCar=&myCar;  //객체 포인터가 일반 객체 참조
        pCar->display(); //객체 포인터는 -> 연산자로 멤버 호출
        cout<<sizeof(pCar)<<endl; //4(포인터)
        cout<<sizeof(*pCar)<<endl; //int(4),int(4),string(32)
    
        pCar=new Car(0,1,"blue"); //객체 포인터가 동적생성된 객체 참조
        pCar->display();
    
        delete pCar;
    }

------------------------------------------------------------------------

    /*this 포인터
    멤버함수 호출시 호출 객체를 묵시적으로 전달하고 함수는 this포인터로 받는다(숨겨져있음)
    */
    #include <iostream>
    #include <string>
    using namespace std;
    
    class Car
    {
        int speed, gear;
        string color;
    public:
        Car(int speed = 0, int gear = 1, string color = "white" )
        {
            //Car*this 숨겨져 있다
            this->speed = speed; //this->speed : 멤버변수, speed : 매개변수
            this->gear = gear;
            this->color = color;
        }
        void display();
    };
    void Car::display() { //void display(Car *this);  //호출객체 전달
        cout<<"Speed: "<<this->speed<<", Gear : "<<
            this->gear<<", Color : "<<this->color<<endl;
    }
    void main()
    {
        Car myCar;
        myCar.display();
        cout<<sizeof(myCar)<<endl;
    
        Car *pCar=&myCar;  //객체 포인터가 일반 객체 참조
        pCar->display(); //객체 포인터는 -> 연산자로 멤버 호출
        cout<<sizeof(pCar)<<endl; //4(포인터)
        cout<<sizeof(*pCar)<<endl; //int(4),int(4),string(32)
    
        pCar=new Car(0,1,"blue"); //객체 포인터가 동적생성된 객체 참조
        pCar->display();
    
        delete pCar;
    }

--------------------------------------------------

    //const key word의 사용법
    
    #include <iostream>
    #include <string>
    using namespace std;
    
    class ConstTest
    {
        const int MAXSPEED; //상수 멤버 선언(멤버변수에 있는 상태에선 초기화 안됨, 이니셜라이저로 초기화 가능)
        int speed;
    public:
        ConstTest(int s, int m) : MAXSPEED(m) //반드시 이니셜라이저 사용
        {
            //MAXSPEED=400;  컴파일 오류(상수의 값을 바꾸려하기때문에)
            speed = s;
        }
        void print() const  //const 함수 선언(함수에서 멤버 변수 변경 불가)
        {
            cout<<speed<<", "<<MAXSPEED<<endl;
            //speed=20;
            //setSpeed(100); //const 함수는 const가 아닌 함수 호출 불가 why? setSpeed함수가 const가 아니면 그 함수에서 값의 변경이 일어날수 있기 때문
        }
        void setSpeed(int s)
        {
            speed = s;
        }
        int getSpeed()
        {
            return speed;
        }
    };
    void main()
    {
        ConstTest obj1(10,100);
        obj1.print();
        cout<<obj1.getSpeed()<<endl;
    
        const ConstTest obj2(20,200); //const 객체 선언(멤버 변수 변경 불가)
        obj2.print();
            //obj2.setSpeed(30); //const 객체는 const가 아닌 멤버 함수 호출 불가
        //cout<<obj2.getSpeed()<<endl; //const 객체가 getSpeed() 함수를 호출
        //const 객체가 getSpeed() 함수를 호출하려면? getSpeed함수에 const를 붙인다. int getSpeed() const
    }
    
------------------------------------------

    #include <iostream>
    #include <string>
    using namespace std;
    
    class Car
    {
        int speed, gear;
        string color;
    public:
        Car(int s=0, int g=1, string c="white") : speed(s), gear(g), color(c) {}
        void display()
        {
            cout<<speed<<", "<<gear<<", "<<color<<endl;
        }
        void func1(Car car)  //멤버함수(call by value)
        {
            car.speed = 1000;
            car.gear = 5;
            car.color = "black";
        }
        void func2(Car *car)  //멤버함수(call by address)
        {
            car->speed = 15;
            car->gear = 3;
            car->color = "yellow";
        }
        void func3(Car &car) //멤버함수(call by reference)
        {
            car.speed = 30;
            car.gear = 3;
            car.color = "cyan";
        }
    };
    
    void main()
    {
        Car mine(0,1,"white");
        Car yours(100,5,"red");
        mine.display();
        yours.display();
    
        mine.func1(yours);
        yours.display();
    
        mine.func2(&yours);
        yours.display();
    
        mine.func3(yours);
        yours.display();
    }

-------------------------------------------------------

    //모든 객체(instance)는 자기만의 멤버변수를 갖는다(instance 변수)
    //static 변수는 모든 객체가 공유하는 변수(클래스 변수)
    //일반 멤버변수는 객체가 생성되어야 존재한다
    //static 변수나 함수는 객체 생성 이전에 생성된다(중요한 개념)
    //static은 게임에서 주로 사용
    
    #include <iostream>
    #include <string>
    using namespace std;
    
    class Car
    {
        int speed, gear, id; //id에 객체마다 고유번호를 주려고 한다
        string color;
    public:
        static int numCars; //private으로 선언되지만 설명을 위해 public 선언
        Car(int s=0, int g=1, string c="white") : speed(s),gear(g),color(c) 
        {
            id=++numCars; //첫번째 객체는 1, 두번째 객체는 2, 세번째 객체는 3....
        }
        void display()
        {
            cout<<speed<<", "<<gear<<", "<<color<<", "<<id<<endl;
        }
    };
    int Car::numCars=0; //static 변수는 반드시 클래스 외부에서 초기화
    
    void main()
    {
        cout<<Car::numCars<<endl;
        //static 변수나 함수는 객체와 무관하게 사용 가능(클래스 이름으로 호출)
        Car myCar;
        cout<<Car::numCars<<endl; //1이 나옴
        cout<<myCar.numCars<<endl; //static 변수나 함수는 객체가 생성되면 객체이름으로도 호출가능하나 사용하지 말자
        myCar.display();
    
        Car yourCar;
        cout<<Car::numCars<<endl; //2가 나옴
        yourCar.display();
    }

-------------------------------------------------------------

    //static 함수
    #include <iostream>
    #include <string>
    using namespace std;
    
    class Car
    {
        int speed, gear, id;
        string color;
        static int numCars; 
    public:
        Car(int s=0, int g=1, string c="white") : speed(s),gear(g),color(c) 
        {
            id=++numCars;
        }
        void display()
        {
            cout<<speed<<", "<<gear<<", "<<color<<", "<<id<<endl;
            cout<<numCars<<endl; //일반 멤버함수는 static 변수나 함수 호출 가능
            cout<<getNumCars()<<endl;
        }
        static int getNumCars()
        {
            //display(); //static 함수는 일반 멤버 변수나 함수를 호출할 수 없게 한다. 왜냐하면 static 함수는 이미 객체 생성
                           //이전에 존재하는데 일반 멤버 변수나 함수는 객체생성 이전에는 만들어지지않기때문에(생성 시점이 다르므로)
                           //호출이 불가능하다.
            //speed = 100;
            return numCars;
        }
    };
    int Car::numCars=0;
    void main()
    {
        cout<<Car::getNumCars()<<endl; //static 함수도 객체와 무관하게 사용가능(클래스 이름으로 호출)
        Car myCar;
        cout<<Car::getNumCars()<<endl; 
        cout<<myCar.getNumCars()<<endl; //static 함수도 객체이름으로 호출 가능(사용하지 않는다)
        myCar.display();
    
        Car yourCar;
        cout<<Car::getNumCars()<<endl; //2가 나옴
        yourCar.display();
    }
    
    //static 함수는 static 함수와 변수에만 접근 가능(멤버 변수와 함수는 객체가 생성되어야 존재)
    //static 함수는 this 사용 불가(객체 생성과 무관하기 때문에)
    //일반 멤버함수는 static 변수와 함수에 접근 가능
    //생성 시기가 다르기 때문임(중요한 개념)

----------------------------------------------

    #include <iostream>
    #include <string>
    using namespace std;
    
    class Employee
    {
        string name;
        int salary;
        static int count;
    public:
        Employee(string n = " ", int s = 0) : name(n), salary(s) 
        {
            count++;
        }
        ~Employee() //소멸자에서는 객체가 소멸될 때마다 객체 수 감소
        {
            count--;
            cout<<"소멸자 : "<<count<<endl;
        }
        static int getCount()
        {
            return count;
        }
    };
    int Employee::count=0;
    
    void main()
    {
        cout<<Employee::getCount()<<endl;
        Employee e1("Kim",35000);
        Employee e2("Choi",50000);
        Employee e3("Kim",20000);
    
        cout<<"Number of employee = "<<Employee::getCount()<<endl;
    }

---

    //포인터 객체를 사용해야 실행 도중 객체를 소멸시킬 수 있다, 일반 객체는 소멸 못한다
    
    #include <iostream>
    #include <string>
    using namespace std;
    
    class Employee
    {
        string name;
        int salary;
        static int count;
    public:
        Employee(string n = " ", int s = 0) : name(n), salary(s) 
        {
            count++;
        }
        ~Employee() //소멸자에서는 객체가 소멸될 때마다 객체 수 감소
        {
            count--;
            cout<<"소멸자 : "<<count<<endl;
        }
        static int getCount()
        {
            return count;
        }
    };
    int Employee::count=0;
    
    void main()
    {
        cout<<Employee::getCount()<<endl;
        Employee *e1=new Employee("Kim",35000);
        Employee *e2=new Employee("Choi",50000);
        Employee *e3=new Employee("Kim",20000);
    
        cout<<"Number of employee = "<<Employee::getCount()<<endl;
    
        delete e1; //소멸자 호출(포인터를 사용하기에 delete를 사용할 수 있다)
        cout<<"Number of employee = "<<Employee::getCount()<<endl;
    
        delete e2;
        cout<<"Number of employee = "<<Employee::getCount()<<endl;
    }
    
    // 왜 static은 non static을 호출 못하는가 non static은 static을 호출할 수 있는가

--------------------------------------------------------------------------

    //객체 배열,포인터
    #include <iostream>
    #include <string>
    using namespace std;
    
    class Car
    {
        int speed,gear;
        string color;
    public:
        Car(int s=0,int g=1,string c="white") : speed(s),gear(g),color(c) 
        {
            cout<<"생성자 호출"<<endl;
        }
        void display() 
        {
            cout<<speed<<","<<gear<<","<<color<<endl;
        }
    };
    void main()
    {
        Car objArray[3]={ Car(0,1,"white"),Car(0,1,"red"),Car(0,1,"blue") };
        cout<<sizeof(objArray)<<","<<sizeof(objArray[0])<<endl;
        cout<<objArray<<","<<&objArray[0]<<endl;
        cout<<&objArray[1]<<","<<&objArray[2]<<endl;
        for(int i=0;i<3;i++)
        {
            objArray[i].display();
            (objArray+i)->display();
        }
    
        Car *p=objArray; //객체 포인터 p가 배열 참조
        for(int i=0;i<3;i++)
        {
            p->display();
            p++;
        }
        //for 반복문이 종료되면 p는 배열을 벗어 난다. 반드시 배열의 처음으로 이동
        p=objArray;
        for(int i=0;i<3;i++)
            (p+i)->display();
        Car car[3]; //객체 배열을 선언만 하면 default 생성자 호출
    }

---

    //전역함수를 friend로 지정하는 경우
    //friend 함수는 클래스의 모든 멤버에 접근할 수 있다
    //friend 함수는 위치에 관계없이 public이나 private의 영향을 받지 않는다
    //멤버함수가 아닌 전역함수이기 때문이다
    
    #include <iostream>
    #include <string>
    using namespace std;
    
    class Company
    {
        int sales,profit;
        friend void print(Company &c); //멤버함수 아닌 전역함수
    public:
        Company() : sales(100),profit(10) {}
        //friend void print(Company &c);
    };
    void print(Company &c)
    {
        //friend로 지정된 함수는 클래스의 private 영역에 접근할 수 있다
        cout<<c.sales<<","<<c.profit<<endl;
    }
    void main()
    {
        Company company;
        print(company); //전역함수 호출
    }

------------------------------------

    //Vector 클래스에 add 함수 추가
    /*
    #include <iostream>
    using namespace std;
    
    class Vector
    {
        double x,y;
    public:
        Vector(double x=0.0, double y=0.0)
        {
            this->x=x;
            this->y=y;
        }
        void display() 
        {
            cout<<x<<","<<y<<endl;
        }
        Vector add(Vector v)
        {
            Vector vector;
            vector.x=x+v.x;
            vector.y=y+v.y;
            return vector;
        }
    };
    void main()
    {
        Vector v1(1,2),v2(2,3),v3;
    
        v3=v1.add(v2);
        v3.display();
        //v3=v1+v2; //vector 클래스에 +연산자에 대한 정의가 없기 때문에 error
    }
    */

    #include <iostream>
    using namespace std;
    
    class Vector
    {
        double x,y;
    public:
        Vector(double x=0.0, double y=0.0)
        {
            this->x=x;
            this->y=y;
        }
        void display() 
        {
            cout<<x<<","<<y<<endl;
        }
        Vector add(Vector v)
        {
            Vector temp;
            temp.x=x+v.x;
            temp.y=y+v.y;
            return temp;
        }
        Vector operator+(Vector &v) //Vector 객체들에 대한 +연산을 정의
        {
            Vector vector;
            vector.x=x+v.x;
            vector.y=y+v.y;
            return vector;
        }
        Vector operator-(Vector &v)
        {
            Vector vector;
            vector.x=x-v.x;
            vector.y=y-v.y;
            return vector;
        }
        Vector& operator=(const Vector &v) //Vector 객체들에 대한 = 연산을 정의(다른 연산자와 다름)
        {
            x=v.x;
            y=v.y;
            return *this; //대입연산자는 반드시 변경된 객체 자체를 반환
        }
    };
    void main()
    {
        Vector v1(1,2),v2(2,3),v3;
    
        v3=v1+v2; //v1.operator+(v2)로 변환된다. 이걸로 사용하자
        v3.display();
        v3=v1.operator+(v2); //가능하지만 사용하지 않는다
        v3.display();
        v3=v1-v2;
        v3.display();
        v3=v1;
        v3.display();
        v2=v1=v3;
        v2.display();
        v1.display();
        v3.display();
    }
