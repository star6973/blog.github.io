---
layout: post
title: "Java Programming [Day 2]"
date: 2017-10-11 09:00:00-18:00:00
categories: Language
tag: Language
use_math: true
---
        
# Java 예제 4일만에 뽀개기 - Day 2
    
    abstraction(추상화) 
    
    1. 속성 attribute -> field(멤버변수, instance변수)
    2. 행동(동장) behavior -> method(멤버함수)
    
    class : 새로운 data type 작성 = new 사용 객체 생성
    
    -설정자(mutator)
    -접근자(accessor)
    -지역변수 : 메소드 안에 선언, 메소드의 매개변수도 지역변수의 일종
    
    *객체를 비교할 때 객체를 비교할지 객체의 값을 비교할지 생각하자.
    -객체를 비교하는 것은 가리키는 객체가 같은지 다른지를 비교
    -객체의 값을 비교하는 것은 객체 내의 멤버 변수의 값이 같은지 다른지를 비교
    
    *값의 전달
    -객체를 전달할 때, 객체가 복사되어 같은 객체를 가리킨다.
    
---

    예시1) CarTest class
    
    class Car 
    {
        private String color;
        private int speed;
        private int gear;
        
        public String getColor() { return color; }
        public void setColor(String color) { this.color = color; }
        public int getSpeed() { return speed; }
        public void setSpeed(int speed) 
        {
            if(speed < 0)
                speed = 0;
            this.speed = speed;
        }
        public int getGear() { return gear; }
        public void setGear(int gear) { this.gear = gear; }
        
        //자바에서는 객체의 변수를 비교하기 위해서 일일이 다해주어야 되기에 equals함수를 생성하여 비교를 해준다.
        public boolean equals(Car c) 
        {
            boolean b;
            if(color == c.color && speed == c.speed && gear == c.gear)
                b = true;
            else
                b = false;
            
            c.speed = 10000;
            return b;
        }
    }
    
    public class CarTest
    {
        public static void main(String[] args)
        {
            Car myCar = new Car();
            
            myCar.setColor("red");
            myCar.setSpeed(1000);
            myCar.setGear(1);
            
            System.out.println("Color : " + myCar.getColor());
            System.out.println("Speed : " + myCar.getSpeed());
            System.out.println("Gear : " + myCar.getGear());
            
            Car yourCar = new Car();
            
            yourCar.setColor("red");
            yourCar.setSpeed(1000);
            yourCar.setGear(1);;
            
            if(myCar == yourCar) //참조하는 객체가 다르므로 false가 나옴
                System.out.println(true);
            else
                System.out.println(false);
            
            myCar = yourCar; //참조하는 객체가 같으므로 true가 나옴
    
            if(myCar == yourCar)
                System.out.println(true);
            else
                System.out.println(false);
            
            System.out.println(myCar == yourCar); //객체를 비교
            System.out.println(myCar.equals(yourCar)); //객체의 값을 비교
            System.out.println("Speed : " + yourCar.getSpeed());
        }
    }
    
---

    예시2) DiceTest class
    
    // 난수 발생 방법 2가지
    
    // 1번 Math.random() 함수 사용
    // 0.0 <= Math.random() < 1.0
    // 예를 들어 4,5,6,7,8을 난수로 발생하게 하자
    // (int)(Math.random() * 단계) + 시작숫자
    // 단계 = 4,5,6,7,8이므로 5단계
    // 시작숫자 = 4
    
    // 2번 Random class를 사용하는 방법
    // import java.util.Random 선언
    // Random 객체를 생성 ex)Random random = new Random();
    // rand.nextInt(6) + 1
    
    import java.util.Scanner;
    
    class DiceGame
    {
        private int diceFace; // 주사위를 굴려 나온 값
        private int userGuess; // 사용자 추측
        
        private void rollDice()
        {
            diceFace = (int)(Math.random() * 6) + 1; // 0.0 <= Math.random() < 1.0
        }
        
        private int getUserInput(String prompt)
        {
            System.out.print(prompt);
            Scanner s = new Scanner(System.in);
            
            return s.nextInt();
        }
        
        private void checkUserGuess() // 메소드를 private로 해도 무방하다(클래스 내에서만 사용되므로)
        {
            if(diceFace == userGuess)
                System.out.println("Correct");
            else
                System.out.println("Wrong");
            
            System.out.println(diceFace);
        }
        
        public void startPlaying()
        {
            userGuess = getUserInput("Enter your guess : ");
            rollDice(); // 동일 클래스 내부의 메소드 호출은 메소드 이름만 사용
            checkUserGuess();
        }
    }
    
    public class DiceTest 
    {
        public static void main(String[] args) 
        {
            DiceGame game = new DiceGame();
            game.startPlaying(); // 다른 클래스의 메소드 호출은 반드시 객체를 통해서 호출
        }
    }
    
---

    // 가변 길이 배열
    class Test
    {
        void sub(int... v) // v를 배열로 처리, int형으로 v에 값을 넣은 만큼의 크기의 배열 생성(가변 길이 배열)
        {
            System.out.println("Number of args : " + v.length);
            
            for(int i = 0; i < v.length; i++)
                System.out.println(v[i] + " ");
            System.out.println();
            
            for(int x : v) // Java5의 추가된 for each loop(배열에서 자세히)
                           // v배열에 있는 값들을 하나씩 끊어서 x에 넣어 준다.
                System.out.print(x + " ");
            System.out.println();
        }
    }
    
    public class ValArgsTest 
    {
        public static void main(String[] args)
        {
            Test c = new Test();
            c.sub(1);
            c.sub(2, 3, 4, 5, 6);
            c.sub();
        }
    
    }
    
---
 
    class Box
    {
        private int width, length, height;
    
        public int getWidth() {
            return width;
        }
    
        public void setWidth(int width) {
            this.width = width;
        }
    
        public int getLength() {
            return length;
        }
    
        public void setLength(int length) {
            this.length = length;
        }
    
        public int getHeight() {
            return height;
        }
    
        public void setHeigth(int height) {
            this.height = height;
        }
        
        public int getVolume()
        {
            return width * length * height;
        }
        
        public void print()
        {
            System.out.println("Width : " + width);
            System.out.println("Length : " + length);
            System.out.println("Height : " + height);
        }	
    }
    
    public class BoxTest 
    {
        public static void main(String[] args) 
        {
            Box box1;
            
            box1 = new Box();
            box1.setWidth(100);
            box1.setLength(100);
            box1.setHeigth(100);
            System.out.println(box1.getVolume());
            
            Box box2;
            
            box2 = new Box();
            box2.setWidth(200);
            box2.setLength(200);
            box2.setHeigth(200);
            System.out.println(box2.getVolume());
            
            System.out.println(box1 == box2 ? "same" : "different");
            box1 = box2; // box1이 box2를 가리키게 되므로 같은 객체가 됨
            System.out.println(box1 == box2 ? "same" : "different");
            System.out.println(box1.getHeight());
            System.out.println(box2.getHeight());
    
        }
    
    }
    
---

    // class의 생성자 method가 없으면 compiler는 자동으로 default 생성자를 삽입
    /*
    class Car
    {
        private String color;
        private int speed;
        private int gear;
        
        public Car() {     // 생성자 method는 반드시 public, return type 없음, class 이름과 동일
            color = "red"; // 생성자도 method, 따라서 overloading 가능
            speed = 0;     // 매개변수가 있는 생성자를 작성하면 컴파일러는 디폴트 생성자를 만들지 않는다
            gear = 1;      // 따라서 기본적으로  매개변수가 있는 생성자와 디폴트 생성자를 반드시 작성한다
        }
        
        public Car(String c, int s, int g) {
            color = c;
            speed = s;
            gear = g;
        }
        
        public void print()
        {
            System.out.println(color + ", " + speed + ", " + gear);
        }
    }
    
    public class CarTest {
    
        public static void main(String[] args) {
            Car c1 = new Car("blue", 100, 0);
            c1.print();
            Car c2 = new Car(); // 만약 디폴트 생성자를 작성하지 않았다면 오류
            c2.print();
        }
    
    }
    */
    
    // this를 이용하면 다른 생성자 호출 가능 ex)this( )
    // this를 이용하면 코드의 중복 회피 가능(자기 자신을 참조하는 키워드) ex)this.name
    
    class Car
    {
        private String color;
        private int speed;
        private int gear;
        
        public Car() {   // default 생성자
    //      System.out.println("default constructor"); // 오류됨
            this("red", 100, 2); // this를 사용한 생성자 호출은 반드시 첫 번째 문장이 되어야 한다
        }
        
        public Car(String c) { // 매개변수 1개 있는  생성자
            this(c, 100, 3);
        }
        
        public Car(String c, int s, int g) { // 매개변수 모두 있는 생성자
            color = c;
            speed = s;
            gear = g;
        }
        
        public void print()
        {
            System.out.println(color + ", " + speed + ", " + gear);
        }
    }
    
    public class CarTest {
    
        public static void main(String[] args) {
            Car c1 = new Car();
            c1.print();
            Car c2 = new Car("blue");
            c2.print();
            Car c3 = new Car("yellow", 130, 5);
            c3.print();
        }
    
    }

---

    class Date{
        private int year;
        private String month;
        private int day;
        
        public Date() {
            year = 2017;
            month = "October";
            day = 16;
        }
        
    //  public Date() { // 같은 방법
    //      setDate(2017, "October", 16);
    //  }
        
        public Date(int year) {
            setDate(year, "October", 16);
        }
        
        public Date(int year, String month, int day) {
            setDate(year, month, day);
        }
        
        public void setDate(int year, String month, int day) { // field의 매개변수 이름과 멤버 변수의 이름이 동일한 경우
            this.year = year; // this는 setDate를 호출한 객체
            this.month = month;
            this.day = day;
        }
        
        public void print() {
            System.out.println(year + ", " + month + ", " + day);
        }
    }
    
    public class DateTest {
    
        public static void main(String[] args) {
            Date d1 = new Date(2017, "March", 2);
            Date d2 = new Date(2017);
            Date d3 = new Date();
            d1.print();
            d2.print();
            d3.print();
    
        }
    
    }

---

    // class안에는 생성자, setter, getter, equals, toString 메소드를 가져야 한다 
    
    class Time {
        private int hour;
        private int min;
        private int sec;
        
        public Time() { 
            this(0, 0, 0); // setTime(0, 0, 0);
        }
        
        public Time(int h, int m, int s) {
            setTime(h, m, s);
        }
        
        public void setTime(int h, int m, int s) {
            hour = (h >= 0 && h < 24) ? h : 0; // field가 잘못된  값이 설정되는 것을 방지
            min = (m >= 0 && m < 60) ? m : 0;
            sec = (s >= 0 && s < 60) ? s : 0;
        }
        
        public String toString() { // field값들을 리턴하는 함수(반드시 작성)
            return String.format("%02d %02d %02d", hour, min, sec);
        //  return hour + " " + min + " " + sec;
        }
    }
    public class TimeTest {
    
        public static void main(String[] args) { 
            Time time1 = new Time();
            System.out.println(time1.toString());
            
            Time time2 = new Time(13, 27, 6);
            System.out.println(time2);
            // println로 매개변수로 객체를 넘기면 자동으로 toString 메소드 호출
            // 반드시 toString의 시그니처를 제대로 작성해야지 안그러면 못찾아온다.
            
            Time time3 = new Time(99, 66, 77);
            System.out.println(time3);
        }
    
    }

---

    // 클래스의 멤버로 다른 클래스로 객체를 포함하는 함수
    class Point {
        private int x, y;
        
        public Point(int a, int b) {
            x = a; 
            y = b;
        }
    }
    
    class Circle { // Has-A 관계(Circle has a Point)
        private int radius;
        private Point center; // private int x, y; 가능하지만 이미 Point 클래스가 있으면 사용
        
        public Circle(Point p, int r) {
            center = p;
            radius = r;
        }
    }
    
    public class CircleTest {
    
        public static void main(String[] args) {
            Point p = new Point(25, 70); // Point 객체 생성
            Circle c = new Circle(p, 10); // 생성된 Point 객체를 c의 객체로 전달
        }
    
    }

---

    인스턴스 변수(instance variable): 객체마다 하나씩 있는 변수
    정적 변수(static variable): 모든 객체를 통틀어서 하나만 있는 변수
    
    static 변수와 함수는 객체 생성 전에 이미 만들어져 있음
    일반 instance 변수와 함수는 객체 생성 후에 만들어짐
    
    *따라서 static 변수나 함수는 instance 변수나 함수를 호출할 수 없음(static 변수나 함수가 instance 변수와 함수보다 먼저 생성되 있으므로)
    *하지만 instance 변수나 함수는 static 변수나 함수를 호출할 수 있음(static 변수나 함수는 이미 생성되 있으므로) 
    
    비슷한 맥락으로 
    Scanner 클래스와 Math 클래스와 비교
    Scanner 클래스 - 객체 생성 ex)Scanner sc
    Math 클래스 - 클래스 생성 ex)Math.random() -> Math 클래스도 static으로 되어 있기 때문이다!

---

    // Carid 객체가 생성될때마다 고유번호를 추가하려고 한다
    // 현재까지 생성된 객체의 수를 알아야 가능
    
    class Carid
    {
        private String color;
        private int speed;
        private int gear;
        private int id; // Car 객체의 고유번호
        
        private static int numberOfCars = 0;
        
        public Carid(String c, int s, int g) { 
            color = c;
            speed = s;
            gear = g;
            
            id = ++numberOfCars;
        }
        
        public static int getNumberOfCars() {
    //      System.out.println(color); // static 함수는 일반 멤버 변수나 멤버 함수 호출 불가
            return numberOfCars;
        }
    }
    
    public class CarTestid {
    
        public static void main(String[] args) {
            System.out.println(Carid.getNumberOfCars()); // static 함수는 클래스 이름으로 호출
            Carid c1 = new Carid("blue", 100, 4);
            Carid c2 = new Carid("white", 0, 1);
            
            System.out.println(Carid.getNumberOfCars());
            System.out.println(c1.getNumberOfCars());
            // 객체를 이용하여 static 함수에 접근 가능하지만 반드시 class 이름을 사용한다
        }
    
    }

---

    class Employee {
        private String name;
        private double salary;
        
        private static int count = 0;
        
        public Employee(String n, double s) {
            name = n;
            salary = s;
            count++; // 객체의 개수 증가
        }
        
        protected void finalize() { // 객체 소멸시 자동 호출, 반드시 이와 같은 시그니처를 작성해야 된다.
            count--; // 객체의 개수 감소
            System.out.println("fianlize() : " + count);
        }
        
        public static int getCount() {
            return count;
        }
    }
    
    public class EmployeeTest {
    
        public static void main(String[] args) {
            System.out.println(Employee.getCount()); // static 함수는 클래스 이름으로 호출
            Employee e1, e2, e3;
            
            e1 = new Employee("Kim", 300);
            e2 = new Employee("Choi", 200);
            e3 = new Employee("Lee", 400);
            
            System.out.println(Employee.getCount()); // static 함수는 클래스 이름으로 호출
            e1 = null; // 객체 소멸시
            System.gc(); // garbate collector, 자동으로 finalize를 호출해준다(CPU가 한가할 때 지 맘대로 정해지므로 순서는 바뀔 수 있다)
            System.out.println("count : " + Employee.getCount());
        }
    
    }

---

    class Circle
    {
        private double radius;
        static final double PI = 3.141592;
        
        public Circle(double radius) {
            this.radius = radius;
        }
    
        public double getRadius() {
            return radius;
        }
    
        public void setRadius(double radius) {
            this.radius = radius;
        }
        
        private double square(double r) {
            return r * r;
        }
        
        private double getArea() {
            return square(radius) * PI;
        }
        
        public double getPerimeter() {
            return radius * 2 * PI;
        }
        
        public static double getPI() { // PI가 static변수 이므로
            return PI;
        }
        
    }
    
    public class LabCircleTest {
    
        public static void main(String[] args) {
            System.out.println(Circle.getPI());
            Circle c = new Circle(5.0);
            System.out.println(c.getRadius());
            c.setRadius(10.0);
    
        }
    
    }
    
---

    import java.util.Scanner;
    
    class Average
    {
        private final static int STUDENTS = 5;
        private int total;
        private int[] scores;
        
        public Average() // 생성자, 필요한 초기화 수행
        {
            total = 0;
            scores = new int[STUDENTS];
        }
        
        public void getScore()
        {
            Scanner sc = new Scanner(System.in);
            for(int i = 0; i < STUDENTS; i++)
            {
                System.out.print("Enter score : ");
                scores[i] = sc.nextInt();
            }
            sc.close();
        }
        
        public int getAverage()
        {
            for(int i = 0; i < scores.length; i++)
                total += scores[i];
            return total / scores.length;
        }
    }
    
    public class ArrayTest1 {
        public static void main(String[] args)
        {
            Average ave = new Average();
            ave.getScore();
            int average = ave.getAverage();
            System.out.println("Average : " + average);
        }
    
    }

---

    // for-each loop은 배열의 처음부터 끝까지 처리하는 경우 사용
    // 그러나 이것은 중간에 수정이 불가능함
    // 배열의 값을 바꾸거나 중간부분을 처리하는 경우 사용할 수 없다.
    
    public class ArrayTest2 {
    
        public static void main(String[] args) {
            int[] numbers = {10, 20, 30};
            
            for(int v : numbers) // 자료형 변수 : 배열이름
                System.out.println(v); // 반복 문장들
            
        }
    
    }

---

    import java.util.Scanner;
    
    // Java는 배열의 크기를 실행 시에 입력할 수 있다.
    // why? 자바에는 포인터가 없다. 따라서 포인터를 이용한 메모리 동적할당이 불가능함
    // 그래서 배열에 그러한 기능(동적할당)을 만들어 주었다.
    public class ArrayTest4 {
    
        public static void main(String[] args) {
            int total = 0, size;
            
            Scanner sc = new Scanner(System.in);
            System.out.print("Enter array size : ");
            size = sc.nextInt();
            int[] scores = new int[size]; // run time에 배열 크기 결정 가능
                                          // 메모리 동적할당 따름
            
            for(int i = 0; i < scores.length; i++)
            {
                System.out.print("Enter score : ");
                    scores[i] = sc.nextInt();
                total += scores[i];
            }
            
            System.out.println("Average : " + total /size);
            sc.close();
        }
    
    }

---

    // 배열을 함수에 전달하는 경우 배열에 대한 참조가 전달된다.
    // 따라서, 함수에서 배열을 변경하면 변경된 값이 저장된다.
    
    class Array1
    {
        public void changeArray(int array[])
        {
            for(int i = 0; i < array.length; i++)
                array[i] *= 10;
        }
    }
    
    public class ArrayTest5 {
    
        public static void main(String[] args) {
            int[] scores = { 10, 20, 30, 40, 50 };
            Array1 test = new Array1();
            
            test.changeArray(scores);
            for(int i = 0; i < scores.length; i++)
                System.out.println(scores[i]);
        }
    
    }

---

    import java.util.Scanner;
    
    public class ArrayTest6 {
    
        public static void main(String[] args) {
            int[][] array = { {10, 20}, {50, 60}, {90, 100} };
    //      int[][] array = new int[3][2];
    //      array = { {10, 20}, {50, 60}, {90, 100} };
            // 배열 선언 후 {}를 이용한 초기화는 Error
    //      for(int i = 0; i < array.length; i++) // 선언 후 초기화는 반드시 개별적으로 처리
    //          for(int j = 0; j < array[i].length; j++)
    //              array[i][j] = 100;
            
            // array.length는 행의 길이
            // array[0].length는 열의 길이
            
            for(int i = 0; i < array.length; i++)
                for(int j = 0; j < array[i].length; j++)
                    System.out.println(i + " " + j + " : " + array[i][j]);
            
            System.out.println(array.length + ", " + array[0].length);
        }
    
    }

---

    // 배열 원소 전달은 call by value
    // 배열 전달은 call by reference
    
    class Array2
    {
        public void changeValue(int num)
        {
            num *= 10;
        }
        
        public void changeArray(int[] a)
        {
            for(int i = 0; i < a.length; i++)
                a[i] *= 10;
        }
    }
    public class ArrayTest {
    
        public static void main(String[] args) {
            Array2 myArray = new Array2();
            int[] array = { 1, 2, 3, 4, 5 };
            
            for(int i = 0; i < array.length; i++)
                myArray.changeValue(array[i]); // 배열 원소 전달 -> 값이 바뀌지 않는다
            for(int i = 0; i < array.length; i++)
                System.out.println(array[i]);
            
            myArray.changeArray(array); // 배열 객체 전달 -> 값이 바뀐다
            for(int i = 0; i < array.length; i++)
                System.out.println(array[i]);
        }
    
    }

---

    // 객체 배열
    
    class Car
    {
        private int speed;
        private int gear;
        private String color;
        
        public Car()
        {
            speed = 0;
            gear = 1;
            color = "Red";
            System.out.println("Constructor call");
        }
        
        public void speedUp()
        {
            speed += 10;
        }
        
        public String toString()
        {
            return "Speed : " + speed + ", Gear : " + gear + ", Color : " + color;
        }
    }
    
    public class CarArrayTest {
    
        public static void main(String[] args) {
            Car[] cars = new Car[5]; // 5개의 Car 객체 저장 공간 할당
            
            for(int i = 0; i < cars.length; i++) // 배열에 객체 생성
                cars[i] = new Car();
            for(int i = 0; i < cars.length; i++)
                cars[i].speedUp();
            for(Car car : cars)
                System.out.println(car); // toString() 자동 호출
            
        }
    
    }

---

    public class Ex5 {
    
        public static void main(String[] args) {
            int[] a = {1, 2, 3, 4, 5};
            int[] b = new int[5];
            
            b = a; // 객체 참조가 변경된다.
            System.out.println(a == b);
            
            for(int i = 0; i < a.length; i++)
                System.out.println(a[i] + " : " + b[i]);
            for(int i = 0; i < a.length; i++)
                a[i] += 10;
            for(int i = 0; i < a.length; i++)
                System.out.println(a[i] + " : " + b[i]);
            for(int i = 0; i < b.length; i++)
                b[i] += 10;
            for(int i = 0; i < a.length; i++)
                System.out.println(a[i] + " : " + b[i]);
        }
    
    }

---
    
    class Car
    {
        private int speed;
        private int gear;
        private String color;
        
        public void speedUp(int increment)
        {
            speed += increment;
        }
        
        public void speedDown(int decrement)
        {
            speed -= decrement;
        }
        
        public String toString()
        {
            return speed + ", " + gear + ", " + color;
        }
    }
    
    class SportsCar extends Car
    {
        private boolean turbo;
        
        public void setTurbo(boolean newValue)
        {
            turbo = newValue;
        }
        
        public String toString() // method overriding
        {
            return super.toString() + ", " + turbo;
            // 만약 super를 빼먹으면 재귀 호출
        }
    }
    
    public class CarTest 
    {
        public static void main(String[] args) 
        {
            SportsCar c = new SportsCar();
            
            c.speedUp(100);
            c.speedDown(30);
            c.setTurbo(true);
            System.out.println(c);
            
            Car car = new Car();
    //      car.setTurbo(true);
            // 기반클래스 객체는 파생 클래스가 추가된 내용을 사용할 수 없다(뭐가 추가됬는지 알 수 없기에) -> 중요함
    
    //      System.out.pritnln(c.speed); // private 멤버는 파생 객체라 해도 사용 불가
    //		System.out.pritnln(c.gear); // protected 멤버는 파생 객체가 사용 가능
        }
    }

---

    class Employee
    {
        public String name;
        String address; // package 속성
        protected int salary; // 파생 클래스에스는 public, 다른 클래스에서는 private
        private int rrn;
        
        public String toString()
        {
            return name + ", " + address + ", " + rrn + ", " + salary;
        }
    }
    
    class Manager extends Employee
    {
        private int bonus;
        
        public void print()
        {
            System.out.println(name + ", " + address + ", " + (salary + bonus));
            // '+'연산자를 산술연산자로 사용하고 싶다면 ()를 사용한다
    //      System.out.println(rrn); // 상속 받았다해도 기반 클래스의 private 멤버에 접근 불가
        }
    }
    
    public class EmployeeTest 
    {
        public static void main(String[] args) 
        {
            Manager m = new Manager();
            m.print();
        }
    
    }

---

    // method overriding
    // 기반 클래스에 정의되어 있는 메소드를 파생 클래스에서 다시 필요하는 경우
    // 메소드를 재정의하려면 메소드의 이름, 반환형, 매개변수의 개수와 데이터 타입이 일치하여야 한다
    
    class Animal
    {
        public void sound1() {
            
        }
    }
    
    class Dog extends Animal
    {
        @Override // super class로 method를 overriding한다는 소리(컴파일러에게 알림 -> 안전장치)
        public void sound1()
        {
            System.out.println("멍멍");
        }
    }
    
    class Cat extends Animal
    {
        public void sound1()
        {
            System.out.println("야옹야옹");
        }
    }
    
    public class AnimalTest 
    {
        public static void main(String[] args) 
        {
            Dog d = new Dog();
            d.sound1();
            
            Cat c = new Cat();
            c.sound1();
        }
    }

---

    // super : 기반 클래스의 메소드나 변수 호출
    // 기반클래스와 파생 클래스의 변수 이름이 같은 경우 파생 클래스의 변수가 우선한다(shadow effect)
    // 따라서, 되도록 같은 이름을 사용하지 않는다
    
    class Parent
    {
        protected int date = 100; // protected를 이용하여 파생 클래스에서도 사용 가능
        
        public void print()
        {
            System.out.println("Super class print() method");
        }
    }
    
    class Child extends Parent
    {
        int date = 200;
        
        public void print() // method overriding
        {
            super.date = 200; // 기반 클래스의 멤버 변수(protected이므로 접근 가능)
            super.print(); // super class로 print 메소드 호출
            System.out.println("Child class print() method"); 
            System.out.println(date); // this.date
            System.out.println(super.date); // 기반 클래스의 date
        }
    }
    
    public class SuperTest 
    {
        public static void main(String[] args) 
        {
            Child obj = new Child();
            obj.print();
        }
    }

---
    
    // 파생 클래스 객체 생성시 기반 클래스의 생성자를 먼저 호출하며
    // 기반 클래스를 생성한 후 파생객체를 생성한다
    
    // 기반 클래스에서는 항상 디폴트 생성자 및 매개변수가 있는 생성자를 만들자
    // -> 그래야 파생 클래스에서 오버라이딩 할때, super를 사용하기가 껄끄러워지지 않는다
    // 만약 매개변수 있는 생성자가 없으면 super는 자동적으로 무조건 디폴트 생성자만 호출한다
    // -> 기반 클래스의 멤버변수가 초기화를 해줄 수 없다 
    
    class Shape
    {
        private String msg;
        
        public Shape()
        {
            System.out.println("Shape default 생성자");
        }
        
        public Shape(String m)
        {
            msg = m;
            System.out.println("매개변수 있는 Shape 생성자");
        }
        
        public String toString()
        {
            return msg;
        }
    }
    
    class Rectangle extends Shape
    {
        private int width, height;
        
        public Rectangle()
        {
            super(); // 반드시 첫 번째 문장이어야 한다(명시적 호출)
                     // 생략 가능(묵시적 호출) -> default 생성자만 호출
            System.out.println("Rectangle default 생성자");	
        }
        
        public Rectangle(String m, int w, int h)
        {
            super(m); // 기반 클래스의 매개변수 있는 생성자가 없으면 error
                      // 생략될 경우 기반 클래스의 default 생성자 호출 -> 생략하면 안됨
                      // -> 기반 클래스의 msg 초기화 불가능
                      // -> 항상 기반 클래스에 매개변수가 모두 있는 생성자 만들어준다
            width = w;
            height = h;
            System.out.println("매개변수 있는 Rectangle 생성자");
        }
        
        public String toString()
        {
            return super.toString() + ", " + width + ", " + height;
        }
    }
    
    public class ConstructorTest 
    {
        public static void main(String[] args) 
        {
            Rectangle r = new Rectangle(); // default 생성자 호출	
            Rectangle r1 = new Rectangle("Rectangle", 10, 20);
            System.out.println(r1);
        }
    }

---
    
    Object 클래스
    
    -clone() : 객체 자신을 복사본을 생성하여 반환
    -equals(Object obj) : obj가 이 객체과 같은지를 나타냄
    -finalize() : 가비지 콜렉터에 의해 호출
    -getClass() : 객체를 생성한 클래스 정보를 반환
    -hashCode() : 객체에 대한 해쉬 코드를 반환
    -toString() : 객체에서 문자열 표현을 반환

    class A extends Object
    {
        
    }
    
    class B extends A
    {
        
    }
    
    public class GetClassTest 
    {
        public static void main(String[] args) 
        {
            A obj1 = new A();
            System.out.println(obj1.getClass()); // class A 출력
            System.out.println(obj1.getClass().getName()); // A 출력
            
            B obj2 = new B();
            System.out.println(obj2.getClass()); // class B 출력
            System.out.println(obj2.getClass().getName()); // B 출력
            
            if(obj2.getClass().getName() == "A")
                System.out.println(true); 
            else
                System.out.println(false); // false 출력
            
            obj1 = obj2; // 기반 클래스의 객체 변수는 파생 객체를 참조할 수 있다(매우 중요한 개념) 
                                 // obj1은 obj2를 가리키게됨
            
                    System.out.println(obj1.getClass().getName()); // B출력
            // obj1의 기반 클래스 A의 변수이지만 파생 객체 obj2를 참조하고 있다
            // 이 경우 getClass() 함수는 실제로 참조하고 있는 객체의 클래스를 리턴한다
            // obj2 = obj1 // 역은 성립하지 않는다
        
            Object obj = new B(); // 최상위 클래스의 객체 변수는 어떠한 파생 객체도 참조 할 수 있다.
            System.out.println(obj instanceof Object); // obj객체는 Object 클래스와 동일 클래스의 객체인가? true 출력
            System.out.println(obj instanceof A); // obj객체는 A 클래스와 동일 클래스의 객체인가? true
            System.out.println(obj instanceof B); // obj객체는 B 클래스와 동일 클래스의 객체인가? true
    
                    // instanceof를 이용한 연산결과로 true를 얻었다는 것은 참조변수가 검사한 타입으로 형변환이 가능하다는 것을 뜻한다.
                    // 기반 클래스의 참조변수 instanceof 파생 클래스의 인스턴스 (무조건 true)
                    // Object 클래스의 참조변수 instanceof 파생 클래스의 인스턴스 (무조건 true)
            
        }
    
    }

---

    class Car extends Object
    {
        int speed;
        int gear;
        String color;
        
        public Car(int s, int g, String c)
        {
            speed = s;
            gear = g;
            color = c;
        }
        
        public boolean equals(Object obj) // equals method는 Object 객체를 매개변수로 받는다
        {                               
            if(obj instanceof Car) // 동일 클래스의 객체인가?
            {
                Car other = (Car)obj; // Object를 Car로 형변환
                if(speed == other.speed && gear == other.gear && color == other.color)
                    return true;
                else
                    return false;
            }
            else
                return false;
        }
        
        public String toString()
        {
            return speed + ", " + gear + ", " + color;
        }
    }
    
    class SportsCar extends Car
    {
        boolean turbo;
        
        public SportsCar(int s, int g, String c, boolean t)
        {
            super(s, g, c);
            turbo = t;
        }
        
        public boolean equals(Object obj)
        {
            if(obj instanceof SportsCar)
            {
                if(super.equals(obj)) // 기반 클래스의 equals 호출해서 비교
                {
                    SportsCar other = (SportsCar)obj;
                    if(turbo == other.turbo)
                        return true;
                        else
                        return false;
                    }
                    else 
                        return false;
                }
            else
                return false;
        }
        
        public String toString()
        {
            return super.toString() + ", " + turbo;
        }
    }
    
    public class EqualTest1 
    {
        public static void main(String[] args) 
        {
            SportsCar sc1 = new SportsCar(100, 3, "Red", true);
            SportsCar sc2 = new SportsCar(100, 3, "Red", true);
            
            System.out.println(sc1 == sc2); // 동일 객체를 참조하는지 여부
            System.out.println(sc1.equals(sc2)); // 객체들의 field가 같은 값을 갖는지 여부
            // Object 클래스에서 제공하는 equals 메소드는 ==를 사용한다
            // 따라서, 객체의 내용을 비교하면 equals 메소드를 overriding 해야  한다
            
            System.out.println(sc1); // toString() 호출
        }
    }

---

    class C // class가 final이면 상속이 불가
    {
        public static final double PI = 3.14; // final 상수 선언시 사용
        
            public void print() // method가 final이면 파생 클래스에서 overriding 불가
        {
                System.out.println("class C");
    //      PI = 3.15; // final 상수 변경 불가
        }
    }
    
    class D extends C
    {
        public void print()
        {
            System.out.println("classC");
        }
    }
    
    public class FinalTest 
    {
        public static void main(String[] args) 
        {
            D obj = new D();
        }
    }

---

    import java.util.Scanner;
    
    class E extends Object
    {
        Scanner sc;
        
        public E()
        {
            sc = new Scanner(System.in);		
        }
        
        protected void finalize() // Object 클래스의 finalize 메소드를 재정의하고
        {                         // resource 반환 후 정의 작업 실행
            System.out.println("class E finalize 호출");
            sc.close();
        }
    }
    
    public class finalizeTest 
    {
        public static void main(String[] args) 
        {
            E obj = new E();
            obj = null;
            System.gc(); // finalize() 호출 -> JVM이 원할 때 호출됨
        }
    
    }

---
 
    // abstract method(추상 메소드) : 몸체가 구현되지 않은 메소드, 반드시 ;로 끝난다
    // abstract class(추상 클래스) : 추상 메소드를 갖는 클래스
    
    abstract class Shape
    {
        private int x, y;
        
        public Shape(int x, int y) // 추상 클래스도 생성자를 가질 수 있다
        {
            this.x = x;
            this.y = y;
        }
        
        public void move(int ax, int ay) // 일반 메소드도 가질 수 있다
        {
            x = ax;
            y = ay;
        }
        
        public abstract int getArea(); // 추상 메소드가 있으면 반드시 추상 클래스가 된다
    }
    
    class Rectangle extends Shape
    {
        private int width, height;
        
        public Rectangle(int x, int y, int w, int h)
        {
            super(x, y);
            width = x;
            height = h;
        }
        
        public int getArea()          // 기반 클래스의 추상 메소드를 구현화 시켜주자                
        {                             // 만약 상속을 받지만 구현을 안해준다면 이 파생 클래스 
            return width * height;    // 역시나 abstract 클래스가 된다 
                                      // 하지만 마지막에 상속 받은 클래스가 abstract 메소드를 구현해주어야 한다
        }
    }
    
    class Circle extends Shape
    {
        private double radius;
        
        public Circle(int x, int y, double r)
        {
            super(x, y);
            radius = r;
        }
        
        public int getArea() // 기반 클래스의 추상 메소드의 시그니처(Function Header)가 같아야 한다(반환형도 조심)
        {
            return (int)(radius * radius * Math.PI);
        }
    }
    
    public class Test 
    {
        public static void main(String[] args) 
        {
            Rectangle r = new Rectangle(10, 20, 30, 40);
            System.out.println(r.getArea());
            
            Circle c = new Circle(10, 200, 10);
            System.out.println(c.getArea());
            
    //		Shape s = new Shape(10, 10); // 추상 클래스는 객체를 생성할 수 없다(불안정한 메소드를 가지고 있기 때문이다)
        }
    }
    
---

    // 일반 메소드와 추상 메소드의 차이는?
    // 일반 메소드로 정의한 경우 파생 클래스에서 반드시 재정의 하지 않아도 에러 발생하지 않지만
    // 추상 메소드로 정의한 경우 반드시 정의해야 하는 강제성을 갖는다
    /*
    class Animal 
    {
        public void sound() { } // 일반 메소드일 경우 
    }
    
    class Dog extends Animal
    {
    //	public void sound() { } // 재정의 하지 않아도 에러 발생하지 않는다
    }
    
    class Cat extends Animal
    {
        public void sound() 
        { 
            System.out.println("야옹야옹");
        }
    }
    */
    
    abstract class Animal
    {
        public abstract void sound(); // 추상 메소드일 경우
    }
    
    class Dog extends Animal
    {
    //	public void sound() // 메소드를 구현화하지 않으면 오류
    //	{
    //		System.out.println("멍멍");
    //	} 
        public void sound()
        {
            System.out.println("멍멍");
        }
    }
    
    class Cat extends Animal
    {
        public void sound()
        {
            System.out.println("야옹야옹");
        }
    }
    
    public class AnimalTest {
    
        public static void main(String[] args) 
        {
            Dog dog = new Dog();
            dog.sound();
            
            Cat cat = new Cat();
            cat.sound();
        }
    
    }

---

    인터페이스 : 추상 메소드들로만 이루어진다(극단적인 추상 클래스)
    인터페이스를 구현하는 클래스 -> implements 키워드 사용
    
    // interface는 추상 메소드로만 구성되고
    // interface를 구현하는 클래스는 모든 추상 메소드를 정의한다
    
    interface RemoteControl
    {
        public abstract void turnOn(); // public abstract 생략 가능(default)
        void turnOff();                // 되도록 생략하지 않는다
    }
    
    class TV implements RemoteControl        // implements 사용하여 구현
    {                                        
        public void turnOn()                 // interface를 구현하는 클래스는 interface에 선언된
        {                                    // 모든 메소드를 정의해야 한다
            System.out.println("tv 켜기");
        }
        
        public void turnOff()
        {
            System.out.println("tv 끄기");
        }
        
        public void volumeUp() // TV 클래스에서 추가된 메소드
        {
            System.out.println("volume up");
        }
    }
    
    class Radio implements RemoteControl
    {
        public void turnOn()
        {
            System.out.println("radio 켜기");
        }
        
        public void turnOff()
        {
            System.out.println("radio 끄기");
        }
    }
    
    public class InterfaceTest 
    {
        public static void main(String[] args) 
        {
            TV tv = new TV();
            Radio radio = new Radio();
            
            tv.turnOn();
            radio.turnOff();
            
    //		RemoteControl rc = new RemoteControl(); // interface는 객체를 생성할 수 없다
            RemoteControl rc = new TV(); // interface 참조변수는 interface를 구현하는 객체 참조 가능
                                         // 자신이 상속한 클래스들에 추상 메소드(기반 클래스가 가지고 있는 메소드)가 있는지 알고 있다
            rc.turnOff();  // interface에 선언된 메소드만 사용 가능
    //		rc.volumeUp(); // 참조 객체에 추가된 메소드 호출 불가
            
        }
    
    }

---

    // Comparable : java.lang package에 선언된 interface
    // Comparable interface 안에는 compareTo 메소드 하나만 있다
    // compareTo 메소드 : 비교를 통해 반환을 해준다
    // -> compareTo의 진가는 따로 있다!
    
    class Box implements Comparable
    {
        private double volume;
        
        public Box(double v)
        {
            volume = v;
        }
        
        public int compareTo(Object obj) // 매개변수는 반드시 Object 타입
        {
            Box other = (Box)obj;        // Object를 Box로 형변환
            if(volume > other.volume)    // 비교하고자 하는 멤버변수를 obj와 비교
                return 1;
            else if(volume < other.volume)
                return -1;
            else
                return 0;
    //		return (int)(volume - other.volume); // 이런 식으로 한 줄로도 가능
        }
    }
    
    public class BoxTest 
    {
        public static void main(String[] args) 
        {
            Box b1 = new Box(90);
            Box b2 = new Box(85);
            
            if(b1.compareTo(b2) > 0)
                System.out.println("b1 is bigger than b2");
            else
                System.out.println("b1 is smaller or same to b2");
    
        }
    
    }
    
---

    /*
    public class Student implements Comparable
    {
        private String name;
        private double gpa;
        
        public Student(String n, double g)
        {
            name = n;
            gpa = g;
        }
        
        public String getName()
        {
            return name;
        }
        
        public double getGPA()
        {
            return gpa;
        }
        
        public int compareTo(Object obj) 
        {
            Student other = (Student)obj;
            if(gpa > other.gpa) // gpa를 비교하고 싶다!
                return 1;
            else if(gpa < other.gpa)
                return -1;
            else
                return 0;
        }
    }
    */
    public class Student implements Comparable
    {
        private String name;
        private double gpa;
        
        public Student(String n, double g)
        {
            name = n;
            gpa = g;
        }
        
        public String getName()
        {
            return name;
        }
        
        public double getGPA()
        {
            return gpa;
        }
        
        public int compareTo(Object obj)
        {
            Student other = (Student)obj;
            if(name.compareTo(other.name) > 0) // name을 비교하고 싶다!(하지만 String 타입의 비교 연산자는 없다)
                return 1;                      // String 클래스에도 역시나 compareTo 메소드가 있다
            else if(name.compareTo(other.name) < 0)
                return -1;
            else
                return 0;
        }
    }
    
    import java.util.Arrays; // 배열을 쉽게 처리해줌, 여기서는 sort 사용(배열 정렬)
    // Comparable 클래스의 compareTo 메소드를 사용하는 이유 
    // Arrays 클래스의 sort 메소드(정렬)를 사용해 주기 위해 compareTo 메소드를 사용한다
    
    public class StudentTest 
    {
        public static void main(String[] args) 
        {
            Student[] students = new Student[3];
            
            students[0] = new Student("Hong", 3.39);
            students[1] = new Student("Lim", 4.21);
            students[2] = new Student("Hwang", 2.19);
            
            Arrays.sort(students);
            // sort 메소드는 static 메소드이므로 클래스 이름으로 정의
            // Comparable 인터페이스를 구현하는 어떠한 객체 배열이든지 sort 메소드로 정렬할 수 있다
            // sort 메소드가 자동 정렬해줌
            for(Student s : students)
                System.out.println("Name : " + s.getName() + ", gpa : " + s.getGPA());
            
            System.out.println(students[0] instanceof Comparable);
            // 파생 클래스의 객체가 기반 클래스의 객체가 될 수 있는 것과 같이
            // interface를 구현하는 객체는 interface의 객체가 될 수 있다
        }
    }
    
---

    // 1. 하나의 클래스 -> 다중 인터페이스 가능
    //    -> 인터페이스 안의 모든 메소드들을 재정의 해주어야 한다(구현화) 
    // 2. 인터페이스 상속하기 -> 생각하지 말자
    
    // interface에는 상수를 선언할 수 있지만 일반 변수는 선언할 수 없다
    // interface를 구현하는 모든 클래스는 상수를 공유할 수 있다
    
    interface Days
    {
    //	private int day; // interface는 instance 변수를 가질 수 없다 -> 딱 두개를 가질 수 있다
        public static final double PHI = 3.14; // 1. static final로 선언되는 상수는 가질 수 있다
        public abstract void print(String msg); // 2. abstract 메소드를 가질 수 있다
                                                // Days를 구현하는 모든 객체가 공유할 수 있다
    }
    
    class MyDays implements Days
    {
        public void area(double r)
        {
            System.out.println(r * r * PHI);
        }
        
        public void print(String msg)
        {
            System.out.println(msg);
        }
    }
    
    public class DayTest 
    {
        public static void main(String[] args) 
        {
            MyDays md = new MyDays();
            
            md.area(10.0);
            md.print("Interface example");
        }
    
    }
    // 자바는 다중 상속 지원하지 않음
    // 인터페이스를 이용하면 다중 상속의 효과를 낼 수 있다

---
    
    다형성이란 객체들의 타입이 다르면 똑같은 메시지가 전달되더라도 서로 다른 동작을 하는 것
    
    class Shape
    {
        int x, y; // 다형성을 보여주기 위해 일부러 private을 사용하지 않음
    }
    
    class Rectangle extends Shape
    {
        int width, height;
    }
    
    
    public class ShapeTest
    {
        public static void main(String[] args) 
        {
            Rectangle r= new Rectangle();
            
            Shape s = r; // super class의 객체 변수는 subclass
            s.x = 20; // 객체를 참조할 수 있다(상향 형변환 : up casting)
            s.y = 10;
            System.out.println(s.x + ", " + s.y);
            
                    // 그러나 사용할 수 있는 변수나 함수는 super class에 정의된 멤버들로 제한된다.
                    // s.width = 30; 
                    // s.height = 50;
            ((Rectangle)s).width = 100; // 하향 형변환 : down casting
            ((Rectangle)s).height = 200; // 이 방법은 일시적이다. s가 잠시동안 Rectangle객체가 된다
            
                    // r = s; // 반대는 성립하지 않는다
                    // subclass의 객체 변수는 super class의 객체를 참조할 수 없다
            
            System.out.println(s.getClass().getName()); // s는 기반 클래스의 변수임에도 불구하고 실제로 참조하고 있는 클래스의 이름이 나온다
            r = (Rectangle)s; // 하향 형변환 : down casting은 가능
            r.width = 1000; // 이 방법은 지속적인 방법이다
            r.height = 2000;
            System.out.println(r.width + ", " + r.height);
            
            System.out.println(((Rectangle)s).width + ", " + ((Rectangle)s).height);
        }
    
    }

---

    abstract class Shape
    {
        int x, y;
        
        public abstract void draw();
    }
    
    class Rectangle extends Shape
    {
        public int width, height;
        
        public void draw() // 상속받으므로 구체화를 시켜주어야 한다
        {
            System.out.println("Rectangle draw");
        }
    }
    
    class Triangle extends Shape
    {
        private int base, height;
        
        public void draw()
        {
            System.out.println("Triangle draw");
        }
    }
    
    class Circle extends Shape
    {
        private int radius;
        
        public void draw()
        {
            System.out.println("Circle draw");
        }
    }
    
    public class ShapeTest2 
    {
        public static void main(String[] args) 
        {
            Shape[] array = new Shape[3]; // 기반 클래스 Shape 배열에 파생 객체들을 저장할 수 있다
            
            array[0] = new Triangle();
            array[1] = new Rectangle();
            array[2] = new Circle();
            
            for(int i = 0; i < array.length; i++)
                array[i].draw(); // 실제 참조하는 객체의 draw() 호출
            
            Shape s = new Rectangle(); // super class 객체 변수는 subclass 객체를 참조할 수 있다
            System.out.println(s.x);
            System.out.println(((Rectangle)s).width);
            
            // 상속은 is-a 관계
            System.out.println(array[0] instanceof Shape); // 파생 클래스 객체는 기반 클래스의 객체이다 // True
            System.out.println(array[0] instanceof Triangle); // True
            System.out.println(array[1].getClass().getName() == "Rectangle"); // True
            System.out.println(array[2].getClass().getName()); // Circle
            for(int i = 0; i < array.length; i++)
                myDraw(array[i]); // static main 함수이므로 static 함수로 작성
        }
        public static void myDraw(Shape s) // 기반 클래스를 매개변수로 사용 
        {
            s.draw();
        }
        
    
    }

---

    내부 클래스
    inner class : 클래스 안에 클래스를 정의 --> private
    outer class : 클래스의 필드와 메소드 정의 --> public
    
    class OuterClass
    {
        private String secret = "Time is money";
        InnerClass obj1 = new InnerClass();
        
        public OuterClass()
        {
            System.out.println("Outer class constructor");
            InnerClass obj2 = new InnerClass();
            obj2.method();
        }
        
        private class InnerClass
        {
            public InnerClass()
            {
                System.out.println("Inner Class constructor");
            }
            
            public void method() // inner 클래스 메소드는 outer 클래스의 멤버를 사용할 수 있다
            {
                System.out.println(secret);
            }
        }
    }
    
    public class OuterClassTest 
    {
        public static void main(String[] args) 
        {
            OuterClass out = new OuterClass();
        }
    
    }