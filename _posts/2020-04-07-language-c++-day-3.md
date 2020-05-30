---
layout: post
title: "C++ Programming [Day 3]"
date: 2020-04-07 09:00:00-18:00:00
categories: Language
tag: Language
use_math: true
---

    #include <iostream>
    #include <string>
    using namespace std;
    
    class Car
    {
    public:  //예를 보여주기 위새 멤버변수를 public으로 선언
        int speed, gear;
        string color;
        void setGear(int g) { gear=g; }
        void setSpeed(int s) { speed=s; }
        void speedUp(int inc) { speed+=inc; }
        void speedDown(int dec) { speed-=dec; }
    };
    class SportsCar : public Car { bool turbo;  //SportsCar 클래스는 Car 클래스를 상속받는다
    public:
        void setTurbo(bool t) { turbo=t; }
    };
    void main() //SportsCar 클래스 객체 c는 Car 클래스의 모든 public 멤버를 사용할 수 있다
    {
        SportsCar c;
        c.color="red";
        c.setGear(3);
        c.speedUp(100);
        c.speedDown(40);
        c.setTurbo(true); //당연히 자신이 추가한 함수도 사용
    }

------------------------------------------------------------------

    #include <iostream>
    #include <string>
    using namespace std;
    
    class Employee
    {
    private:
        int id;
    protected: 
        int salary; //상속 받은 클래스 내부에서는 public처럼 쓸 수 있으나 외부함수나 다른 클래스에서는
                    //private으로 받아진다
    public:
        string name;
        void setSalary(int s) { salary=s; }
        int getSalary() { return salary; }
    };
    class Manager : public Employee
    {
        int bonus;
    public:
        Manager(int b=0) : bonus(b) {}
        void Modify(int s,int b) 
        {
            //id=100; //부모 클래스의 private 멤버 접근 불가
            salary=s;
            bonus=b;
        }
        void display()
        {
            cout<<salary<<","<<bonus<<endl;
            //cout<<id<<endl; //부모 클래스의 private 멤버에는 직접 접근할 수 없다
                              //getter,setter 함수를 이용해서 접근할 수 있다
        }
    };
    void main()
    {
        Manager m;
    
        m.setSalary(2000);
        m.display();
        m.Modify(1000,500);
        m.display();
        cout<<m.getSalary()<<endl;
        //m.salary=100;
    }

-------------------------------------------------------------------------

    //상속에서의 생성과 소멸 순서
    #include <iostream>
    #include <string>
    using namespace std;
    
    class Shape
    {
        int x,y;
    public:
        Shape() { cout<<"Shape 생성자"<<endl; }
        ~Shape() { cout<<"Shape 소멸자"<<endl; }
    };
    class Rectangle : public Shape
    {
        int width,height;
    public:
        Rectangle() : Shape() { //자식 class의 생성자는 부모 class의 생성자를 먼저 호출한다. 없으면
                                // : Shape()을 디폴트로 호출
            cout<<"Rectangle 생성자"<<endl;
        }
        ~Rectangle() //소멸은 생성과 역순(스택을 사용하기 때문)
        {
            cout<<"Rectangle 소멸자"<<endl;
        }
    };
    void main()
    {
        Rectangle r;
    } //소멸은 생성의 역순으로 진행된다
------------------------------------------------
    //상속에서의 생성과 소멸 순서
    #include <iostream>
    #include <string>
    using namespace std;
    
    class Shape
    {
        int x,y;
    public:
        Shape() { cout<<"Shape 생성자"<<endl; }
        Shape(int ax,int ay) : x(ax),y(ay) { cout<<"Shape(x,y)의 생성자"<<endl; }
        ~Shape() { cout<<"Shape 소멸자"<<endl; }
    };
    class Rectangle : public Shape
    {
        int width,height;
    public:
        Rectangle(int x=0, int y=0, int w=0, int h=0) : Shape(x,y) //순서 지키기/ 없으면 디폴트 생성자 호출
        {
            //자식 클래스의 생성자는 부모 클래스의 생성자를 : 초기화를 이용하여 먼저 호출한다
            //부모 클래스 생성자 호출이 없으면 default 생성자 자동 호출
            width=w;
            height=h;
            cout<<"Rectangle 생성자"<<endl;
        }
        ~Rectangle() //소멸은 생성과 역순(스택을 사용하기 때문)
        {
            cout<<"Rectangle 소멸자"<<endl;
        }
    };
    void main()
    {
        Rectangle r;
    } //소멸은 생성의 역순으로 진행된다
----------------------------------------------
    //부모 클래스의 멤버함수를 자식 클래스에서 overriding(재정의)할 수 있다
    #include <iostream>
    #include <string>
    using namespace std;
    
    class Shape
    {
        int x,y;
    public:
        Shape() { cout<<"Shape 생성자"<<endl; }
        Shape(int ax,int ay) : x(ax),y(ay) { cout<<"Shape(x,y)의 생성자"<<endl; }
        ~Shape() { cout<<"Shape 소멸자"<<endl; }
        void print();
    };
    class Rectangle : public Shape
    {
        int width,height;
    public:
        Rectangle(int x=0, int y=0, int w=0, int h=0) : Shape(x,y)
        {
            width=w;
            height=h;
            cout<<"Rectangle 생성자"<<endl;
        }
        ~Rectangle()
        {
            cout<<"Rectangle 소멸자"<<endl;
        }
        void print() //Shape class의 print를 overriding
        {
            Shape::print(); //Shape class의 print를 호출
            cout<<width<<","<<height<<endl;
        }	
    };
    void main()
    {
        Rectangle r(10,10,100,100);
        r.print();
    }

---------------------------------------------------

    LAB
    
    #include <iostream>
    #include <string>
    using namespace std;
    
    class Human
    {
    private:
        string name;
        int age;
    public:
        Human() { name="unknown",age=0; cout<<"Human 디폴트 생성자 호출"<<endl; }
        Human(string n,int a) : name(n),age(a) { cout<<"Human 매개변수 생성자 호출"<<endl; }
        ~Human() { cout<<"Human 소멸자 호출"<<endl; }
        void set_name(string n) { name=n; }
        void set_age(int a) { age=a; }
        string get_name() { return name; }
        int get_age() { return age; }
        void print() { cout<<"이름:"<<name<<", "<<"나이:"<<age<<endl; }
    };
    class Student : public Human
    {
    private:
        string major;
    public:
        Student() : Human() { major="unknown"; cout<<"Student 디폴트 생성자 호출"<<endl; }
        Student(string n,int a,string m) : Human(n,a),major(m) { cout<<"Student 매개변수 생성자 호출"<<endl; }
        ~Student() { cout<<"Student 소멸자 호출"<<endl; }
        void set_major(string m) { major=m; }
        string get_major() { return major; }
        void print() { Human::print(); //함수 오버라이딩 
            cout<<"학과:"<<major<<endl; 
        }
    };
    void main()
    {
        Human h1("지명화",24);
        Human h2("지명와",23);
        h1.print();
        h2.print();
        //h1.get_major(); //부모 클래스 객체는 자식 클래스에 추가된 내용을 알 수 없다
    
        Student std1;
        std1.print();
        cout<<endl;
        Student std2("지명화",24,"컴퓨터공학과");
        std2.print();
        cout<<endl;
    }
    //함수 오버라이딩 vs 오버로딩 구분하기
    
---
    
    //***부모 클래스 객체 포인터는 자식 클래스 객체를 참조할 수 있다
    #include <iostream>
    using namespace std;
    
    class Shape
    {
        int x, y;
    public: 
        void setOrigin(int ax, int ay)
        {
            x = ax;
            y = ay;
        }
        void draw()
        {
            cout << "Shape draw" << endl;
        }
        void print()
        {
            cout << x << "," << y << endl;
        }
    };
    class Rectangle : public Shape
    {
        int width, height;
    public:
        void setWidth(int w)
        {
            width = w;
        }
        void draw()
        {
            cout << "Rectangle draw" << endl;
        }
        void print()
        {
            Shape::print();
            cout << width << "," << height << endl;
        }
    };
    class Circle : public Shape
    {
        int radius;
    public:
        void setRadius(int r)
        {
            radius = r;
        }
        void draw()
        {
            cout << "Circle draw" << endl;
        }
        void print()
        {
            Shape::print();
            cout << radius << endl;
        }
    };
    void main()
    {
        Shape *ps = new Rectangle(); //부모 클래스의 객체 포인터 ps는 자식 클래스 객체를 참조할 수 있다(상향 형변환)
        ps->setOrigin(10, 10); //호출할 수 있는 함수는 부모 클래스에 정의된 함수들로 제한된다
        ps->draw();
        //ps->setWidth(100);
    
        //방법 1
        ((Rectangle *)ps)->setWidth(100);
        //객체 포인터 ps를 자식 클래스 포인터로 하향 형변환을 할 수 있다
        ((Rectangle *)ps)->print();
        //그러면 ps를 통해 자식 클래스의 멤버에 접근할 수 있다
        ((Rectangle *)ps)->draw();
    
        //방법 2
        Rectangle *pr = (Rectangle *)ps; //Rectangle 객체 포인터 pr에 ps를 하향형변환
        pr->setWidth(200);
        //pr=ps; //역은 성립하지 않는다(자식 객체 포인터는 부모 객체 참조 불가능)
    
        Circle c;
        c.setOrigin(10, 10);
        c.draw();
        ps = &c; //부모 클래스의 객체 포인터 ps가 자식 클래스 c 참조 가능
        ((Circle *)ps)->setRadius(5);
        ((Circle *)ps)->print();
        ((Circle *)ps)->draw();
    }
---------------------------------------------------------------
    //***부모 클래스 객체 포인터는 자식 클래스 객체를 참조할 수 있다
    #include <iostream>
    using namespace std;
    
    class Shape
    {
        int x, y;
    public: 
        void setOrigin(int ax, int ay)
        {
            x = ax;
            y = ay;
        }
        void draw() 
        {
            cout << "Shape draw" << endl;
        }
        void print()
        {
            cout << x << "," << y << endl;
        }
        void move(Shape &obj, int x, int y)
        {
            //기반 클래스 Shape 변수는 파생 객체를 모두 받을 수 있다
            obj.setOrigin(x, y);
        }
    };
    class Rectangle : public Shape
    {
        int width, height;
    public:
        void setWidth(int w)
        {
            width = w;
        }
        void draw()
        {
            cout << "Rectangle draw" << endl;
        }
        void print()
        {
            Shape::print();
            cout << width << "," << height << endl;
        }
    };
    class Circle : public Shape
    {
        int radius;
    public:
        void setRadius(int r)
        {
            radius = r;
        }
        void draw()
        {
            cout << "Circle draw" << endl;
        }
        void print()
        {
            Shape::print();
            cout << radius << endl;
        }
    };
    void main()
    {
        Shape s;
        Rectangle r;
        s.move(r, 10, 10);
        r.print();
    
        Circle c;
        s.move(c, 20, 20);
        c.print();
    }
-------------------------------------------
    가상함수를 만들어보자
    
    //***부모 클래스 객체 포인터는 자식 클래스 객체를 참조할 수 있다
    #include <iostream>
    using namespace std;
    
    class Shape
    {
        int x, y;
    public: 
        void setOrigin(int ax, int ay)
        {
            x = ax;
            y = ay;
        }
        virtual void draw() //draw 함수를 가상함수로 정의
        {
            cout << "Shape draw" << endl;
        }
        void print()
        {
            cout << x << "," << y << endl;
        }
        void move(Shape &obj, int x, int y)
        {
            //기반 클래스 Shape 변수는 파생 객체를 모두 받을 수 있다
            obj.setOrigin(x, y);
        }
    };
    class Rectangle : public Shape
    {
        int width, height;
    public:
        void setWidth(int w)
        {
            width = w;
        }
        void setHeight(int h)
        {
            height = h;
            cout << height << endl;
        }
        virtual void draw() //모든 파생클래스에서 draw가 호출가능함 //부모 클래스의 가상함수를 자식클래스에서 
                            //오버라이딩 하면 draw가 호출가능하다
        {
            cout << "Rectangle draw" << endl;
        }
        void print()
        {
            Shape::print();
            cout << width << "," << height << endl;
        }
    };
    class Circle : public Shape
    {
        int radius;
    public:
        void setRadius(int r)
        {
            radius = r;
        }
        virtual void draw()
        {
            cout << "Circle draw" << endl;
        }
        void print()
        {
            Shape::print();
            cout << radius << endl;
        }
    };
    void main()
    {
        Shape *ps = new Rectangle(); //부모 클래스의 객체 포인터 ps는 자식 클래스 객체를 참조할 수 있다(상향 형변환)
        ps->setOrigin(10, 10); //호출할 수 있는 함수는 부모 클래스에 정의된 함수들로 제한된다
        ps->draw();
        //ps->setWidth(100);
    
        Circle c;
        c.setOrigin(10, 10);
        c.draw();
        ps = &c; //부모 클래스의 객체 포인터 ps가 자식 클래스 c 참조 가능
        ((Circle *)ps)->setRadius(5);
        ((Circle *)ps)->print();
        ((Circle *)ps)->draw();
    
        Shape s;
        Rectangle r;
        s.move(r, 10, 10);
        r.print();
    
        Circle cd;
        s.move(c, 20, 20);
        cd.print();
        ps = &cd;
        ps->draw();
        ((Circle*)ps)->setRadius(5);
    
        Rectangle r1;
        Shape s1 = r1;
        s1.draw();
    }
    //부모 클래스의 객체 포인터를 사용하여 가상함수를 호출하면
    //실제 참조하는 객체의 오버라이딩된 함수를 호출한다
    //가상함수는 객체포인터를 통해서만 구현된다
    //일반 객체변수로는 구현되지 않는다
    //Rectangle r;
    //Shape s=r;
    //s.draw(); --> 작동 안됨
    
--------------------------------------------------------------------------------------------------
    #include <iostream>
    using namespace std;
    
    class Shape
    {
    public:
        Shape() { cout<<"Shape 생성자"<<endl; }
        virtual void draw() { //draw 함수를 가상함수로 정의 
            cout<<"Shape draw"<<endl; }
        virtual ~Shape() { cout<<"Shape destructor"<<endl; } //소멸자를 가상함수로 정의
    };
    class Rectangle : public Shape
    {
    public:
        Rectangle() { cout<<"Rectangle 생성자"<<endl; }
        virtual void draw() { //부모 클래스의 가상함수를 자식 클래스에서 재정의하면 자동으로 가상함수가 된다
            cout<<"Rectangle draw"<<endl;
        }
        ~Rectangle() { cout<<"Rectangle destructor"<<endl; }
    };
    class Circle : public Shape
    {
    public:
        Circle() { cout<<"Circle 생성자"<<endl; }
        virtual void draw() {
            cout<<"Circle draw"<<endl;
        }
        ~Circle() { cout<<"Circle destructor"<<endl; }
    };
    class Triangle : public Shape
    {
    public:
        Triangle() { cout<<"Triangle 생성자"<<endl; }
        virtual void draw() { 
            cout<<"Triangle draw"<<endl;
        }
        ~Triangle() { cout<<"Triangle destructor"<<endl; }
    };
    //클래스가 추가되어도 이상없이 동작한다
    
    class Parallelogram : public Shape
    {
    public:
        Parallelogram() { cout<<"Parallelogram 생성자"<<endl; }
        virtual void draw() { cout<<"Parallelogram draw"<<endl; }
        ~Parallelogram() { cout<<"Parallelogram destructor"<<endl; }
    };
    void main()
    {
        Shape *array[4]; //객체 포인터 배열(반드시 포인터 배열)
        array[0]=new Rectangle();
        array[1]=new Triangle();
        array[2]=new Circle();
        array[3]=new Parallelogram();
        
        for(int i=0;i<4;i++)
            array[i]->draw();
        for(int i=0;i<4;i++)
            delete array[i]; //객체 포인터의 소멸자 호출(객체 포인터가 생성한 메모리를 반납) (순서를 정해주었기 때문에 조심)
        //부모 클래스의 객체 포인터가 자식 객체를 참조하는 경우
        //부모 클래스의 소멸자만 호출한다(가상함수로 선언 안하면)
        //컴파일러는 부모 객체로 생각하기 때문이다.
        //자식 클래스의 메모리는 남아있고 부모 클래스의 메모리만 반납한다.
        //때문에 가상함수가 있는 경우 소멸자를 가상함수로 만들어 해결한다.
    
        //기반 클래스의 reference도 다형성이 적용된다
        /*
        Rectangle r;
        Shape &s=r;
        s.draw();
        //Rectangle 클래스의 draw함수 호출
        */
    }
    //부모 클래스의 객체 포인터 배열에 자식 클래스의 객체들을 저장
    //부모 클래스 객체 포인터는 자식 객체를 참조할 수 있기 때문에 가능
    //이 경우에도 실제 참조하는 객체의 가상함수 호출
    //파생 객체들의 배열을 따로 만들 필요 없이 기반 클래스의 포인터 배열에
    //파생 객체들을 저장하여 다형성을 구현
    
    /*
    //기반 클래스의 일반 배열에서는 다형성이 동작하지 않는다
    
      Shape array[3];
      Triangle t;
      Circle c;
    
      array[0]=t;
      array[1]=c;
    
      array[0].draw();
      array[1].draw();
    
      //모두 Shape의 draw함수가 호출
    */
    
---------------------------------------------------------------------------
    //순수 가상함수(pure virtual function) : 함수의 몸체(function body)가 없는 함수
    //순수 가상함수가 있는 클래스를 추상클래스(abstract class)라 한다
    //추상클래스는 불완전한 멤버가 존재하기 때문에 객체를 생성할 수 없다.
    //그러나 객체 포인터를 사용하여 파생 객체를 참조할 수 있다.
    
    //Shape 클래스의 draw 함수는 그려야 할 주체가 불분명하기 때문에 함수의 
    //몸체를 작성하는 것은 의미있다.
    
    #include <iostream>
    using namespace std;
    
    class Shape
    {
    public:
        Shape() { cout<<"Shape 생성자"<<endl; }
        virtual void draw() = 0; //draw함수를 순수가상함수로 정의 --> 어떤 것을 그려야될지 불분명함
        virtual ~Shape() { cout<<"Shape destructor"<<endl; }
    };
    class Rectangle : public Shape
    {
    public:
        Rectangle() { cout<<"Rectangle 생성자"<<endl; }
        virtual void draw() { //직사각형을 그려라(구체화됨)
            cout<<"Rectangle draw"<<endl;
        }
        ~Rectangle() { cout<<"Rectangle destructor"<<endl; }
    };
    class Circle : public Shape
    {
    public:
        Circle() { cout<<"Circle 생성자"<<endl; }
        virtual void draw() { //원을 그려라(구체화됨)
            cout<<"Circle draw"<<endl;
        }
        ~Circle() { cout<<"Circle destructor"<<endl; }
    };
    class Triangle : public Shape
    {
    public:
        Triangle() { cout<<"Triangle 생성자"<<endl; }
        virtual void draw() { //삼각형을 그려라(구체화됨)
            cout<<"Triangle draw"<<endl;
        }
        ~Triangle() { cout<<"Triangle destructor"<<endl; }
    };
    void main()
    {
        //Shape s; //객체 생성 불가 
        Shape *ps=new Rectangle(); //객체 포인터를 사용하여 파생 객체 참조는 가능
        ps->draw();
    
        delete ps;
    }
    
---------------------------------------------------------
    //구체화 되지 않은 부모 클래스의 함수들을 순수가상함수로 선언하고
    //상속 클래스에서 반드시 정의하도록 한다
    
    #include <iostream>
    using namespace std;
    
    class Animal
    {
    public:
        virtual void move() = 0;
        virtual void eat() = 0;
        virtual void speak() = 0;
    };
    class Lion : public Animal
    {
        void move() { //부모 클래스의 추상 함수를 반드시 구체화 시켜서 만들어 놓는다
            cout<<"Lion move"<<endl; }
        void eat() { cout<<"Lion eat"<<endl; }
        void speak() { cout<<"Lion speak"<<endl; }
    };
    void main()
    {
        Animal *a=new Lion;
        //추상 클래스는 객체를 생성할 수 없지만 파생 객체를 참조할 수는 있다
        a->move();
        a->eat();
        a->speak();
    }
    
    //순수 가상함수를 만드는 이유는 기반 클래스에서 구체화 할 수 없는 부분이 있을 때
    //억지로 만들필요 없이 파생 클래스에서 구체화 시켜주면 된다.
----------------------------------------------------
    //순수 가상함수로만 구성된 클래스를 interface라고 한다
    //반드시 구현해야 되는 함수들을 모아 interface로 작성
    //interface를 상속받은 클래스는 모든 순수가상함수를 구현해야 한다.
    #include <iostream>
    using namespace std;
    
    class RemoteControl
    {
        virtual void turnOn() = 0; //사용자가 원하는 기능들을 표준화 시켜준다
        virtual void turnOff() = 0;
    };
    class TV : public RemoteControl
    {
    public:
        virtual void turnOn() { cout<<"TV turnOn"<<endl; }
        virtual void turnOff() { cout<<"TV turnOff"<<endl; }
    };
    //TV를 생산하는 모든 회사는 구현 방식은 다르더라도
    //순수 가상함수로 정의된 모든 함수를 구현해야 한다.
    
    void main()
    {
        TV tv;
        tv.turnOn();
        tv.turnOff();
    }
-------------------------------------------------------
    //RTTI(Run Time Type Identification)
    //참조 객체의 type에 대한 정보
    //상속의 관계가 많이 복잡한 경우 
    //어떤 클래스가 어디에 상속되었는지에 대한 정보를 알 필요가 있음
    
    #include <iostream>
    #include <typeinfo>
    using namespace std;
    
    class A
    {
    public:
        virtual void print() { cout<<"class A"<<endl; }
    };
    class B : public A
    {
    public:
        void functionB() { cout<<"functionB"<<endl; }
        virtual void print() { cout<<"class B"<<endl; }
    };
    class C : public A
    {
    public:
        void functionC() { cout<<"functionC"<<endl; }
        virtual void print() { cout<<"class C"<<endl; }
    };
    void main()
    {
        A *a=new A();
        B b;
        C c;
    
        cout<<typeid(a).name()<<endl; //a의 type
    
        a=&b;
        cout<<typeid(*a).name()<<endl; //a의 참조하는 객체의 type
        cout<<typeid(a).name()<<endl; //a의 type
        
        a=&c;
        cout<<typeid(*a).name()<<endl; //a의 참조하는 객체의 type
        cout<<typeid(a).name()<<endl; //a의 type
    
        a->print(); //실제 참조하는 객체의 print()함수 호출(가상함수)
        
        //기반 객체 포인터가 실제 참조하는 객체의 type에 따라
        //가상함수가 아닌 일반 멤버함수를 호출하려면?
    
        if(strcmp(typeid(*a).name(), "class B") == 0)
            ((B *)a)->functionB();
        else
            ((C *)a)->functionC();
    }
    
    // project 속성 - 구성속성 - C/C++ - 언어 - 런타임 형식 정보 사용 - 예
---------------------------------------------------------------------
    #include <iostream>
    using namespace std;
    
    class Base
    {
    protected:
        int x;
    public:
        Base() : x(0) { cout<<"Base 생성자"<<endl; }
        void print() { cout<<"Base"<<endl; }
    };
    class Derived : public Base
    {
    private:
        int y;
    public:
        Derived() : y(20) { cout<<"Derived 생성자"<<endl; }
        void print() { cout<<"Derived"<<endl; }
    };
    void main()
    {
        Base *b1=new Base();
        Derived *d1=new Derived();
        Base *b2=new Derived();
        b1->print();
        d1->print();
        b2->print();
    
        Base *bb1=new Derived(); //기반 객체 포인터는 파생 객체 참조 가능
        //Derived *dd1=new Base(); //파생 객체 포인터는 기반 객체 참조 불가능
        Derived *dd2=(Derived *)bb1; //기반 객체 포인터를 파생 객체로 형변환하면 가능
        Derived *dd3=new Derived();
        Base *bb2=dd3; //기반 객체 포인터는 파생 객체 참조 가능
    }
    
---
    
    //ifstream으로 파일을 열 경우 파일이 없으면 error
    //ofstream으로 파일을 열 경우 파일이 없으면 생성
    //             파일이 있으면 덮어 쓴다
    //기본 입출력 연산자 (<<,>>)을 이용한 파일 입출력
    
    #include <iostream>
    #include <fstream>
    #include <string>
    using namespace std;
    
    void main() 
    {
        ofstream os; //파일 출력 stream(output file stream)
        ifstream is; //파일 입력 stream(input file stream)
        int number,score;
        string name;
    
        os.open("result.txt"); //출력용으로 파일 열기 (입력용으로 하면 에러, 읽어들일 파일이 없기에)
        if(!os) { //file open 실패, 혹은 os.fail()을 사용(!os 대신)
            cout<<"File open error"<<endl;
            exit(1); //프로그램을 빠져나간다
        }
        cout<<"Enter number, name, score : ";
        cin>>number>>name>>score; //표준 입력(keyboard)
        os<<number<<" "<<name<<" "<<score<<endl;
        os.close(); //출력 파일 닫기
    
        //파일에서 읽어보자
        is.open("result.txt"); //입력용으로 파일 열기
        if(is.fail()) { //혹은 !is
            cout<<"File open error"<<endl;
            exit(1);
        }
        is>>number>>name>>score;
        cout<<number<<" "<<name<<" "<<score<<endl; //표준 출력(모니터)
        is.close(); //입력 파일 닫기
    }
-------------------------------------------------------
    //get(),put() 함수를 이용한 문자 단위 파일 입출력
    
    #include <iostream>
    #include <fstream>
    using namespace std;
    
    void main()
    {
        ofstream os;
        ifstream is;
        char ch;
    
        is.open("Practice.cpp"); //현재 프로그램을 입력 파일로 활용(현재 소스 파일 이름)
        if(!is) {
            cout<<"File open error"<<endl;
            exit(1);
        }
        os.open("result.txt");
        if(os.fail()) {
            cout<<"File open error"<<endl;
            exit(1);
        }
        is.get(ch); //입력 파일에서 한 글자 입력
        while(!is.eof()) { //파일의 끝(end of file)에 도달했는지 검사
            cout.put(ch); //표준 출력
            os.put(ch); //파일 출력
            is.get(ch);
        }
        is.close();
        os.close();
    }
    
----------------------------------------------------------
    //keyboard 입력을 파일에 저장
    
    #include <iostream>
    #include <fstream>
    using namespace std;
    
    void main()
    {
        ofstream os;
        ifstream is;
        char ch;
    
        os.open("test.txt"); 
        if(os.fail()) {
            cout<<"File open error"<<endl;
            exit(1);
        }
        while(cin.get(ch))  //keyboard 입력
    //  while((ch=cin.get() != EOF) //keyboard 입력이 끝날때까지 //EOF == ^Z(cotrol z - 입력의 끝)
            os.put(ch); //파일 출력
        os.close();
    
        is.open("test.txt");
        if(is.fail()) {
            cout<<"File open error"<<endl;
            exit(1);
        }
        while(is.get(ch))
    //  while((ch=is.get()) != EOF) //파일의 끝에 도달할때까지
            cout.put(ch);
        //  cout<<ch;
        is.close();
    }
--------------------------------------------------------------------
    //라인 번호 추가하여 출력 파일에 출력(문자 단위 입출력)
    
    #include <iostream>
    #include <fstream>
    using namespace std;
    
    void main()
    {
        ifstream is;
        ofstream os;
        char ch;
        int number=1;
    
        is.open("Practice.cpp"); //source 프로그램을 입력파일로 사용
        os.open("test.cpp");
        if(is.fail() || os.fail()) {
            cout<<"File open error"<<endl;
            exit(1);
        }
        is.get(ch);
        os<<number<<" : ";
        while(!is.eof()) {  //입력 파일의 끝이 아니면
            os.put(ch);
            if(ch=='\n') {  //개행 문자면
                number++;
                os<<number<<" : "; //라인 번호 출력
            }
            is.get(ch);
        }
        is.close();
        os.close();
    }
-------------------------------------------------------------
    //bianary file(2진 파일)에 대한 입출력
    //read, write 함수 사용(바이너리 모드로 할때는) but 형식에 주의하자
    //정수 하나씩 binary 모드로 입출력하는 예
    /*
    #include <iostream>
    #include <fstream>
    using namespace std;
    
    void main()
    {
        int buffer[] = {10,20,30,40,50};
        ofstream os;
        ifstream is;
        int num;
        int array[5];
    
        os.open("test.bin",ios::binary); //binary 출력 지정, 지정안하면 디폴트로 txt가 됨
        if(os.fail()) {
            cout<<"File open error"<<endl;
            exit(1);
        }
        // (char *)출력 데이터의 주소, 크기
    
        for(int i=0;i<5;i++) //숫자 하나씩 출력
            os.write((char *)&buffer[i],sizeof(int)); //형식에 주의(반드시 형변환(출력 데이터의 주소), 자료형의 크기)
        os.close();
    
        is.open("test.bin",ios::binary); //binary 입력 지정
        if(is.fail()) {
            cout<<"File open error"<<endl;
            exit(1);
        }
        is.read((char *)&num,sizeof(int)); //(char *)입력 데이터의 주소, 크기
        while(!is.eof()) {
            cout<<num<<endl;
            is.read((char *)&num,sizeof(int));
        }
        is.close();
    }
    */
    
    #include <iostream>
    #include <fstream>
    using namespace std;
    
    void main()
    {
        int buffer[] = {10,20,30,40,50};
        ofstream os;
        ifstream is;
        int num;
        int array[5];
    
        os.open("test.bin",ios::binary); //binary 출력 표시
        if(os.fail()) {
            cout<<"File open error"<<endl;
            exit(1);
        }
        os.write((char *)&buffer,sizeof(buffer)); //한꺼번에 출력
        os.close();
    
        is.open("test.bin",ios::binary); //binary 입력 표시
        if(is.fail()) {
            cout<<"File open error"<<endl;
            exit(1);
        }
        is.read((char *)&array,sizeof(array)); //한꺼번에 입력
        for(int i=0;i<5;i++)
            cout<<array[i]<<endl;
        is.close();
    }
----------------------------------------------------------
    //라인 번호 추가하여 출력 파일에 출력(라인 단위 입출력)
    
    #include <iostream>
    #include <fstream>
    #include <string>
    using namespace std;
    
    void main()
    {
        ifstream is;
        ofstream os;
        char line[100]; //string인 경우는?
        int number=1;
    
        is.open("iostream.cpp"); //source 프로그램을 입력 파일로 사용
        os.open("test.cpp");
        if(is.fail() || os.fail())
        {
            cout<<"File open error"<<endl;
            exit(1);
        }
        is.getline(line,100);
        os<<number<<" : "; //파일 출력
        cout<<number<<" : "; //표준 출력 --> 파일과 표준, 2군데에 출력
        while(!is.eof()) { //입력 파일의 끝이 아니면
            cout<<line<<endl;
            cout<<++number<<" : ";
            os<<line<<endl;
            os<<number<<" : ";
            is.getline(line,100);
        }
        is.close(); //메모리 동적 할당하면 소멸도 해주는 것처럼
        os.close(); //파일도 오픈을 하면 클로스를 해주어야 한다
    }
------------------------------------------------------------------
    string인 경우
    #include <iostream>
    #include <fstream>
    #include <string>
    using namespace std;
    
    void main()
    {
        ifstream is;
        ofstream os;
        string line; 
        int number=1;
    
        is.open("iostream.cpp"); //source 프로그램을 입력 파일로 사용
        os.open("test.cpp");
        if(is.fail() || os.fail())
        {
            cout<<"File open error"<<endl;
            exit(1);
        }
        getline(cin,line);
        os<<number<<" : "; //파일 출력
        cout<<number<<" : "; //표준 출력 --> 파일과 표준, 2군데에 출력
        while(!is.eof()) { //입력 파일의 끝이 아니면
            cout<<line<<endl;
            cout<<++number<<" : ";
            os<<line<<endl;
            os<<number<<" : ";
            getline(cin,line);
            if(is.eof()=='^Z')
                break;
        }
        is.close(); //메모리 동적 할당하면 소멸도 해주는 것처럼
        os.close(); //파일도 오픈을 하면 클로스를 해주어야 한다
    }
---------------------------------------------------------------------------
    //file pointer를 이용한 임의 위치 이동
    //seekg, tellg는 ifstream에서 사용
    //seekp, tellp는 ofstream에서 사용
    
    #include <iostream>
    #include <fstream>
    using namespace std;
    
    void main()
    {
        int table[10],data,pos;
        ofstream os;
        ifstream is;
    
        for(int i=0;i<10;i++)
            table[i]=i;
        os.open("test.bin",ios::binary);
        if(os.fail())
        {
            cout<<"File open fail"<<endl;
            exit(1);
        }
            /*
                os.write((char *)table,sizeof(table)); //배열 전체를 파일에 출력
            */
        for(int i=0;i<10;i++)  //배열 1개씩 파일에 출력
            os.write((char *)&table[i],sizeof(int));
        cout<<os.tellp()<<endl; //현재 파일 포인터의 위치
        os.close();
    
        is.open("test.bin",ios::binary);
        if(is.fail())
        {
            cout<<"File open fail"<<endl;
            exit(1);
        }
        cout<<is.tellg()<<endl; //file pointer 위치 출력
    
        while(1) 
            {
                cout<<"파일에서의 위치 입력(0~9, 종료 -1) : ";
            cin>>pos;
            if(pos==-1)
                    break;
            is.seekg(pos * sizeof(int),ios::beg);
            cout<<is.tellg()<<endl;
            is.read((char *)&data,sizeof(int));
            cout<<pos<<"위치의 값:"<<data<<endl;
            cout<<is.tellg()<<endl;
            }
    
        //마지막 데이터를 읽으려면 끝에서 4byte 떨어진 곳으로 이동해야 한다
        is.seekg(0,ios::end);
        cout<<is.tellg()<<endl;
        is.seekg(-4,ios::end);
        cout<<is.tellg()<<endl;
        is.read((char *)&data,sizeof(int));
        cout<<data<<endl;
        is.close();
    }
    
---------------------------------------------------------------------
    //입출력을 동시에 할 수 있는 방법
    //입출력을 동시에 할 수 있도록 파일을 열면
    //seekg, seekp, tellg, tellp 모두 사용 가능
    //한 가지로 통일해서 사용한다
    //ios::in이 있기 때문에 미리 파일을 만들어야 한다
    
    #include <iostream>
    #include <fstream>
    using namespace std;
    
    void main()
    {
        int table[10],data,pos;
        fstream f;
    
        for(int i=0;i<10;i++)
            table[i]=i;
        f.open("test.bin",ios::out | ios::binary | ios::in);
        if(f.fail())
        {
            cout<<"File open fail"<<endl;
            exit(1);
        }
        f.write((char *)&table,sizeof(table));
        cout<<f.tellp()<<endl; //파일 포인터는 파일 끝에 있다
    
        f.seekg(0,ios::beg); //파일의 처음으로 이동
        while(1) {
            cout<<"파일에서의 위치 입력(0~9, 종료 -1) : ";
            cin>>pos;
            if(pos==-1)
                break;
            f.seekg(pos *sizeof(int), ios::beg);
            cout<<f.tellg()<<endl; //file pointer 위치 출력
            f.read((char *)&data, sizeof(int));
            cout<<pos<<"위치의 값: "<<data<<endl;
            cout<<f.tellp()<<endl;
        }
        f.close();
    }
---------------------------------------------------------
    #include <iostream>
    #include <fstream>
    using namespace std;
    
    struct score
    {
        int number;
        char name[30];
        int g;
    };
    void main()
    {
        score grade;
        fstream f;
        int num;
    
        f.open("test.bin",ios::binary | ios::out | ios::in);
        if(f.fail())
        {
            cout<<"File open error"<<endl;
            exit(1);
        }
        while(1)
        {
            cout<<"Enter number(종료 -1) : ";
            cin>>grade.number;
            if(grade.number==-1)
                break;
            //fflush(stdin); //입력 buffer 전체 삭제
            cin.ignore();  //개행문자 처리
            cout<<"Enter name: ";
            cin.getline(grade.name,30); //전체 라인 입력
            cout<<"Enter score: ";
            cin>>grade.g;
            f.write((char *)&grade,sizeof(grade));
        }
        f.seekg(0,ios::beg); //파일 포인터를 처음으로 이동
        f.read((char *)&grade, sizeof(score));
        while(!f.eof())
        {
            cout<<grade.number<<","<<grade.name<<","<<grade.g<<endl;
            f.read((char *)&grade,sizeof(score));
        }
        cout<<f.tellg()<<endl; //eof를 지나면 -1 리턴
    
        f.clear(); //eofbit을 reset 무조건 해주어야 한다
        //eof 지나면 eofbit이 set 되고 이후로는 seekg가 동작하지 않는다
        f.seekg(0,ios::beg);
        cout<<f.tellg()<<endl;
        cout<<"Enter number: ";
        cin>>num;
        f.seekg(num *sizeof(score),ios::beg);
        f.read((char *)&grade,sizeof(grade));
        cout<<grade.number<<","<<grade.name<<","<<grade.g<<endl;
    
        f.close();
    }
    
-----------------------------------------------------------------------------------------
    #include <iostream>
    #include <fstream>
    using namespace std;
    
    struct score
    {
        int number;
        char name[30];
        int g;
    };
    void main()
    {
        score grade;
        fstream f;
        int num;
    
        f.open("test.txt", ios::binary | ios::out | ios::in);  // ios::app 끝에 추가
        if(f.fail())
        {
            cout << "File open error\n";
            exit(1);
        }
        while(1)
        {
            cout << "Enter number(종료 -1) : ";
            cin >> grade.number;
            if(grade.number == -1)
                break;
            //   fflush(stdin);   // 입력 buffer 전체 삭제
            cin.ignore();    // 개행문자 처리
            cout << "Enter name : ";
            cin.getline(grade.name, 30);  // 전체라인 입력 // 데이터 타입이 배열이기 때문에 cin.getline을 사용
            // string 인 경우 getline(cin, , ) 을 사용
            cout << "Enter score : ";
            cin >> grade.g;
            f.write((char *)&grade, sizeof(grade));
        }
        f.seekg(0, ios::beg);        // 파일 포인터를 처음으로 이동
        f.read((char *)&grade, sizeof(score));
        while(!f.eof()){
            cout << grade.number << ", " << grade.name << ", " << grade.g << endl;
            f.read((char *)&grade, sizeof(score));
        }
        cout << f.tellg() << endl;   // eof를 지나면 -1 리턴
    
        // eof를 지나면 eofbit가 set되고 이후로는 seekg가 동작하지 않기 때문에
        f.clear();   // eofbit을 리셋한다.
        f.seekg(0, ios::beg);
        cout << f.tellg() << endl;
        cout << "Enter number : ";
        cin >> num;
        f.seekg(num * sizeof(score), ios::beg);
        f.read((char *)&grade, sizeof(grade));
        cout << grade.number << ", " << grade.name << ", " << grade.g << endl;
        f.close();
    }
    
---
    
    //Exception handler(예외 처리기)를 사용한 처리 방법
    #include <iostream>
    using namespace std;
    
    void main()
    {
        int pizza_slices=0;
        int persons=0;
        int slices_per_persons=0;
    
        try
        {
            cout<<"피자 조각수 입력:";
            cin>>pizza_slices;
            cout<<"사람수 입력:";
            cin>>persons;
    
            if(persons==0)
                throw persons; //catch 블락에 매개변수로 int 타입 persons 전달
            slices_per_persons=pizza_slices/persons;
            cout<<slices_per_persons<<endl;
        }
        catch(int e) //int 타입 매개변수만 받는다(persons를 받는다,매개변수가 만약 char이면 에러가 뜸(persons는 int형이므로))
        {
            cout<<"사람이"<<e<<"명 입니다"<<endl;
        }
            catch(...) {} //어떠한 변수든 모든 것을 잡는다
        //예외가 발생하건 하지 않건 다음 문장으로 진행한다. 바로 실행이 종료되지 않는다.(graceful end)
        //런타임 에러가 뜨는 문장이 실행되지 않는다.
        cout<<"try, catch를 이용한 exception handling"<<endl;
    }
-------------------------------------------------------------------------------------
    //exception을 호출 함수로 전달하는 방법
    
    #include <iostream>
    using namespace std;
    
    //int dividePizza(int,int);
    int dividePizza(int,int) throw(int); //이 dividePizza 변수는 예외 처리로 int형 매개변수를 사용한다고 명시 가능(option)
    
    void main()
    {
        int pizza_slices=0;
        int persons=0;
        int slices_per_persons=0;
    
        try
        {
            cout<<"피자 조각수 입력:";
            cin>>pizza_slices;
            cout<<"사람수 입력:";
            cin>>persons;
    
            slices_per_persons=dividePizza(pizza_slices, persons); //만약 함수에서 throw가 실행되면 아랫문장 실시 안하고 바로 catch로 넘어감
            cout<<slices_per_persons<<endl;
        }
        catch(int e)
        {
            cout<<"사람이 "<<e<<"명 입니다"<<endl;
        }
        cout<<"try, catch를 이용한 exception handling"<<endl;
    }
    int dividePizza(int ps,int pe) throw(int) //throw(int)는 option이지만 쓰는 것이 명시적이어서 좋다
    {
        if(pe==0)
            throw pe; //호출 함수에 예외 전달
        return ps/pe;
    }
----------------------------------------------------------------------------------------------------
    //exception을 호출 함수로 전달하는 방법
    
    #include <iostream>
    using namespace std;
    
    //int dividePizza(int,int);
    int dividePizza(int,int) throw(int); 
    
    void main()
    {
        int pizza_slices=0;
        int persons=0;
        int slices_per_persons=0;
    
        try
        {
            cout<<"피자 조각수 입력:";
            cin>>pizza_slices;
            cout<<"사람수 입력:";
            cin>>persons;
    
            slices_per_persons=dividePizza(pizza_slices, persons); 
            cout<<slices_per_persons<<endl;
        }
        catch(int e)
        {
            cout<<"사람이 "<<e<<"명 입니다"<<endl;
        }
        cout<<"try, catch를 이용한 exception handling"<<endl;
    }
    int dividePizza(int ps,int pe) 
    {
        try
        {
            if(pe==0)
                throw pe; //함수에서 자체적으로 예외 처리
            return ps/pe;
        }
        catch(int e)
        {
            cout<<"분모는 0이 될 수 없음"<<endl;
            throw; //예외 처리 후 호출 함수에도 예외 전달-->main함수의 catch에서도 실행
        }
    }
----------------------------------------------------------------------------------
    //다중 catch을 호출 함수로 전달하는 방법
    
    #include <iostream>
    using namespace std;
    
    //int dividePizza(int,int);
    int dividePizza(int,int) throw(int); 
    
    void main()
    {
        int pizza_slices=0;
        int persons=0;
        int slices_per_persons=0;
    
        try
        {
            cout<<"피자 조각수 입력:";
            cin>>pizza_slices;
            cout<<"사람수 입력:";
            cin>>persons;
    
            if(persons==0)
                throw persons; //catch 블락에 매개변수로 persons 전달
            if(persons<0)
                throw "negative"; //catch 블락에 매개변수로 문자열 전달
            slices_per_persons=pizza_slices/persons;
            cout<<slices_per_persons<<endl;
        }
        catch(int e) //매개변수로 int가 넘어오는 경우
        {
            cout<<"사람이 "<<e<<"명 입니다"<<endl;
        }
        catch(char *e) //매개변수로 문자열이 넘어오는 경우(주소값이 넘어오기 때문에 포인터 사용)
                           //string을 쓸 수 없다
                           //매개변수 안에 ...을 쓰면 가능(...은 항상 맨 뒤에 나와야함, 앞에 있으면
                           //앞에 있는 매개변수들을 모두 잡아 먹는다-->unreachable code)
                           //범위가 좁은 것부터 시작해서 내려와야 한다.
        {
            cout<<e<<"exception 발생"<<endl;
        }
        //예외가 발생하건 하지 않건 다음 문장으로 진행한다
        cout<<"try, catch를 이용한 exception handling"<<endl;
    }
-------------------------------------------------------------------------
    //사용자 정의 예외 클래스(user defined exception class) 작성하는 방법
    
    #include <iostream>
    using namespace std;
    
    class NoPersonException
    {
        int person;
    public:
        NoPersonException(int p=0) { person=p; }
        int get_person() { return person; }
    };
    void main()
    {
        int pizza_slices=0;
        int persons=0;
        int slices_per_persons=0;
    
        try
        {
            cout<<"피자 조각수 입력:";
            cin>>pizza_slices;
            cout<<"사람수 입력:";
            cin>>persons;
    
            if(persons<=0)
                throw NoPersonException(persons); //NoPersonException 생성자 호출하여 객체 전달
            
            slices_per_persons=pizza_slices/persons;
            cout<<slices_per_persons<<endl;
        }
        catch(NoPersonException e) //매개변수는 NoPersonException 객체
        {
            cout<<"사람이 "<<e.get_person()<<"명 입니다"<<endl;
        }
        //예외가 발생하건 하지 않건 다음 문장으로 진행한다.
        cout<<"try, catch를 이용한 exception handling"<<endl;
    }
----------------------------------------------------------------------
    #include <iostream>
    #include <string>
    using namespace std;
    
    void main()
    {
        string str; //string 객체를 생성해주어야 한다
        cout<<"문자열 입력:";
        getline(cin,str,'\n');
        try
        {
            if(str=="disk is full")
                throw str;
            //throw str="disk is full\n"; //string을 넘기는 경우
        }
        catch(string s)
        {
            cout<<s<<endl;
        }
    }
----------------------------------------------
    /*#include <iostream>
    using namespace std;
    
    void check(int);
    
    void main()
    {
        int n;
        
        try
        {
            cout<<"인원 입력:";
                cin>>n;
                check(n);
            cout<<n<<endl;
        }
        catch(int e)
        {
            cout<<"main에서 exception 처리"<<e<<endl;
        }
    }
    void check(int n)
    {
        if(n<0)
            throw n; //exception 전달
    }
    //exception을 throw했지만 catch 블락이 없어 run time error
------------------------------------------------------------------------
    #include <iostream>
    using namespace std;
    
    void check(int);
    
    void main()
    {
        int n;
        
        try
        {
            cout<<"인원 입력:";
            cin>>n;
            check(n);
            cout<<n<<endl;
        }
        catch(int e)
        {
            cout<<"main에서 exception 처리"<<e<<endl;
        }
    }
    void check(int n)
    {
        try
        {
            if(n<0)
                throw n; //함수에서 exception 처리
        }
        catch(int e)
        {
            cout<<"check에서 exception 처리"<<e<<endl;
            throw; //동일한 exception이 main에게도 전달함
        }
    }
    
--------------------------------------------------------------
    
    #include <iostream>
    #include <string>
    using namespace std;
    
    void check(int);
    
    void main()
    {
        int n;
        
        try
        {
            cout<<"인원 입력:";
                cin>>n;
                check(n);
            cout<<n<<endl;
        }
        catch(int e) //정수 매개변수
        {
            cout<<e<<endl;
        }
        catch(char *e) //문자열 매개변수
        {
            cout<<"char * exception"<<e<<endl;
        }
        catch(string e)
        {
            cout<<"string exception"<<e<<endl;
        }
    }
    void check(int n)
    {
        string str;
        if(n<0)
            throw n;
        if(n==100)
            throw "Too many people\n";
        if(n>100)
            throw str="Toooo many people\n";
        //문자열을 전달하는 경우 문자열의 시작 주소를
        //전달하기 때문에 char *로 받아야 한다
        //string을 전달하려면??
    }
    */
    
    #include <iostream>
    using namespace std;
    
    class MyApplicationException
    {
        int ex;
    public:
        MyApplicationException(int e=0)
        {
            ex=e;
        }
        int getEx()
        {
            return ex;
        }
    };
    void check(int) throw (MyApplicationException); //명시해줌
    
    void main()
    {
        int n;
    
        try
        {
            cout<<"인원 입력:";
            cin>>n;
            check(n);
            cout<<"인원이 "<<n<<endl;
        }
        catch(MyApplicationException e)
        {
            cout<<"인원이(Exception) "<<e.getEx()<<endl;
        }
    }
    void check(int n) throw(MyApplicationException)
    {
        if(n<0)
            throw MyApplicationException(n);
    }
    
---
    
    /*
    //function template(함수 템플릿) : generic programming
    #include <iostream>
    using namespace std;
    
    template <typename T>
    T get_max(T n1, T n2) //자료형을 T로 대치
    {
        return (n1>n2) ? n1 : n2;
    }
    void main()
    {
        cout<<get_max(10,20)<<endl; //자료형이 int로 바뀜
        cout<<get_max(10.0,20.0)<<endl; //자료형이 double로 바뀜
        cout<<get_max('a','z')<<endl; //자료형이 char로 바뀜
    }
    */
    
    //template specialization(템플릿 특수화)
    //특정 매개변수에 대해서는 다른 동작을 하도록 정의
    
    #include <iostream>
    using namespace std;
    
    template <typename T>
    T get_max(T n1,T n2)
    {
        return (n1>n2) ? n1 : n2;
    }
    //char 타입은 template 정의와 다르게 동작하도록 정의
    template <>
    char get_max(char c1,char c2)
    {
        return c2;
    }
    void main()
    {
        cout<<get_max(10,20)<<endl; 
        cout<<get_max(10.0,20.0)<<endl; 
        cout<<get_max('a','z')<<endl; 
    }
    
------------------------------------------------------------------------
    #include <iostream>
    using namespace std;
    
    //일반 변수에 대한 template
    template <typename T>
    void swap_values(T &n1, T &n2) //반드시 참조자 사용, 그냥 변수면 call-by-value
    {
        T temp;
    
        temp=n1;
        n1=n2;
        n2=temp;
    }
    
    //배열에 대한 template/
    template <typename T> 
    void swap_values(T v1[],T v2[]) //template도 오버로딩(중복)이 가능하다
    {
        T temp;
    
        for(int i=0;i<3;i++)
        {
            temp=v1[i];
            v1[i]=v2[i];
            v2[i]=temp;
        }
    }
    
    void main()
    {
        int n1=100, n2=200;
        double d1=10.0, d2=20.0;
        char c1='a',c2='z';
        int v1[3]={1,2,3};
        int v2[3]={4,5,6};
        char s1[3]={'a','b','c'};
        char s2[3]={'d','e','f'};
    
        swap_values(n1, n2);
        swap_values(d1, d2);
        swap_values(c1, c2);
        cout<<n1<<","<<n2<<endl;
        cout<<d1<<","<<d2<<endl;
        cout<<c1<<","<<c2<<endl;
    
        swap_values(v1,v2);
        swap_values(s1,s2);
        for(int i=0;i<3;i++)
            cout<<v1[i]<<","<<v2[i]<<endl;
        for(int i=0;i<3;i++)
            cout<<s1[i]<<","<<s2[i]<<endl;
    }
    
-------------------------------------------------------------------------------
    #include <iostream>
    using namespace std;
    
    template <typename T1,typename T2> //int, double인 경우
    T1 add(T1 n, T2 d)
    {
        return n+(T1)d; //형변환도 가능함(int로 반환하고 싶은 경우)
        //return (T2)n+d; //(double로 반환하고 싶은 경우)
    }
    /*
    T2 add(T1 n, T2 d) //double로 함수 반환형을 만들고 싶은 경우
    {
        return n+d;
    }
    */
    void main()
    {
        int n=10;
        double d=12.5;
    
        n=add(n,d);
        cout<<n<<endl;
        cout<<add(n,d)<<endl;
    }
    
----------------------------------------------------------------------------------
    /*
    #include <iostream>
    using namespace std;
    
    template <typename T>
    T abs(T num)
    {
        return (num>=0) ? num : -num;
    }
    void main()
    {
        int n=-100;
        double d=-1.0;
    
        cout<<abs(n)<<endl;
        cout<<abs(d)<<endl;
    }
    */
    /*
    #include <iostream>
    using namespace std;
    
    template <typename T>
    T add(T n1, T n2)
    {
        return n1+n2;
    }
    void main()
    {
        int n1=10, n2=20;
        double d1=-1.0, d2=3.0;
    
        cout<<add(n1,n2)<<endl;
        cout<<add(d1,d2)<<endl;
    }
    */
    #include <iostream>
    using namespace std;
    
    template <typename T>
    void displayArray(T n[],int s) //template 매개변수 안에 크기 변수도 같이 쓸 수 있다
    {
        for(int i=0;i<s;i++)
            cout<<n[i]<<endl;
    }
    void main()
    {
        int i[3]={1,2,3};
        double d[3]={1.0, 2.0, 3.0};
        char c[3]={'a','b','c'};
        
        displayArray(i,3);
        displayArray(d,3);
        displayArray(c,3);
    }
    
------------------------------------------------------------------------------
    #include <iostream>
    using namespace std;
    
    template <typename T>
    void print_array(T a[], int n) //T*a도 가능
    {
        for(int i=0;i<n;i++)
            cout<<a[i]<<endl;
    }
    template <> //다른 타입을 선언해주고 싶을 때
    void print_array(char a[], int n)
    {
        cout<<a[n-1]<<endl; //마지막 문자 출력
    }
    void main()
    {
        int num[5]={1,2,3,4,5};
        double dbl[3]={1.1,2.2,3.3};
        char ch[3]={'a','b','c'};
    
        print_array(num,5);
        print_array(dbl,3);
        print_array(ch,3);
    }
-----------------------------------------------------------------------
    //클래스 템플릿
    //사용자가 지정하는 타입을 써주어야함(함수 템플릿과의 차이점)
    
    #include <iostream>
    #include <string>
    using namespace std;
    
    template <typename T> //클래스 앞에 써준다
    class Box
    {
        T data;
    public:
        Box(T d) : data(d) {}
        void setData(T d) { data=d; }
        T getData() { return data; }
    };
    void main()
    {
        Box<int> box(100); //int를 타입으로 만들고 싶다하면!!
        cout<<box.getData()<<endl;
        box.setData(200);
        cout<<box.getData()<<endl;
    
        Box<double> b(10.5); //double를 타입으로 만들고 싶다하면!!
        cout<<b.getData()<<endl;
        b.setData(20.5);
        cout<<b.getData()<<endl;
    
        Box<char> ch='a'; //char를 타입으로 만들고 싶다하면!!
        cout<<ch.getData()<<endl;
        ch.setData('b');
        cout<<ch.getData()<<endl;
    
        Box<char*> c="abc"; //char*를 타입으로 만들고 싶다하면!!
        cout<<c.getData()<<endl;
        c.setData("bcd");
        cout<<c.getData()<<endl;
        
        Box<string> s("lee"); //string을 타입으로 만들고 싶다하면!!
        cout<<s.getData()<<endl;
        s.setData("kim");
        cout<<s.getData()<<endl;
    }
    
---------------------------------------------------------------------
    // 멤버함수를 외부에 정의하는 경우
    #include <iostream>
    using namespace std;
    
    template <typename T>
    class Box
    {
        T data;
    public:
        Box(T d);
        void setData(T d);
        T getData();
    };
    template <typename T> //template 선언문을 모든 함수 앞에 명시(다 따로 따로 써줘야함)
    Box<T>::Box(T d) { data=d; } //클래스 이름 뒤에 타입을 뜻하는 <T> 명시
    
    template <typename T>
    void Box<T>::setData(T d) { data=d; }
    
    template <typename T>
    T Box<T>::getData() { return data; }
    
    void main()
    {
        Box<int> box(100);
        cout<<box.getData()<<endl;
        box.setData(200);
        cout<<box.getData()<<endl;
    }
-------------------------------------------------------------------------
    // 여러개의 타입을 가지는 클래스 템플릿 + 외부 정의
    #include <iostream>
    using namespace std;
    
    template <typename T1, typename T2>
    class Box
    {
        T1 first;
        T2 second;
    public:
        Box(T1 f, T2 s) 
        {
            first=f;
            second=s;
        }
        T1 get_first() { return first; }
        T2 get_second() { return second; }
        void set_first(T1 f);
        void set_second(T2 s);
    };
    template <typename T1, typename T2> 
    void Box<T1,T2>::set_first(T1 f) { first=f; } //멤버함수가 T1꺼만이라고 T2에 대해 안쓰면 안된다
    
    template <typename T1, typename T2>
    void Box<T1,T2>::set_second(T2 s) { second=s; }
    
    void main()
    {
        Box<int, double> box(10,10.5); //선언한 개수만큼 타입을 적어야함
        box.set_first(200);
        box.set_second(100.5);
        cout<<box.get_first()<<endl;
        cout<<box.get_second()<<endl;
    
        Box<int, char> box1(10,'a');
        box1.set_first(200);
        box1.set_second('x');
        cout<<box.get_first()<<endl;
        cout<<box.get_second()<<endl;
    }