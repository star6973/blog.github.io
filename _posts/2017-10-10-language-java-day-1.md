---
layout: post
title: "Java Programming [Day 1]"
date: 2017-10-10 09:00:00-18:00:00
categories: Language
tag: Language
use_math: true
---
    
# Java 예제 4일만에 뽀개기 - Day 1
    
    Camel Lotation
     
    1. 클래스의 이름은 대문자로 시작, 이어지는 문장도 대문자로 시작하자.
    2. 변수나 함수의 이름은 소문자로 시작하자.
    
    /**  */ : 이 안에 뭔가를 집어넣으면 JDK안에 javadoc의 프로그램이 이 안의 내용을 가지고 주석을 추출하여 자동으로 HTML문서를 만든다.                    
    
    - 자바 프로그램의 구조 -
    
    1. 소스파일의 이름과 클래스의 이름이 같아야한다.
    
    2. public static void main(String[] arg) --> public : 누구나 사용 가능하다.
                                             --> String : 대문자로 시작하니 클래스구나!
                                             --> args : 변수 이름이구나!
                                             --> [] : 배열이구나! 위치는 타입이어도 되고 
    
    3. System.out.println("Hello World"); --> System : 대문자로 시작하니 클래스구나!
                                          --> " " : " "문자열 출력 
    4. 문장은 순차적으로 진행
    
    ** 프로젝트 안에 클래스는 한 개만! **
    
---
    
    public class Add {
        public static void main(String[] args) {
            int x;
            int y;
            int sum;
            //int x,y,sum;
            
            x=100;
            y=200;
            sum=x+y;
            
            System.out.println(sum);
            System.out.println("Sum : "+sum);
        }
    } 

---

    import java.util.Scanner; //입력을 위한 라이브러리
    
    public class Add2 {
        public static void main(String[] args) {
            Scanner input = new Scanner(System.in); //Scanner 객체 생성
            int x, y, sum;
            
            System.out.println("Enter first number : ");
            x = input.nextInt(); //정수 입력
            System.out.print("Enter second number : ");
            y = input.nextInt();
            
            sum = x + y;
            System.out.println(sum);
            System.out.println(x + " + " + y + " = " + sum);
        }
    }
    
    println : 줄바꿈 기능 o
    print : 줄바꿈 기능 x 

---
  
    import java.util.Scanner;
    
    public class Salary {
        public static void main(String[] args) {
            Scanner input = new Scanner(System.in);
            
            int salary;
            int deposit;
            
            System.out.print("Enter salary : ");
            salary = input.nextInt();
            deposit = 10 * 12 * salary;
            
            System.out.println("10년 동안의 저축액 : " + deposit);
            System.out.printf("10년 동안의 저축액 : %d\n", deposit); //printf 사용가능(자바에서 기능이 훨씬 좋음)
        }
    }

---

    import java.util.Scanner;
    
    public class CircleArea {
        public static void main(String[] args)
        {
            double radius;
            double area;
            
            Scanner in = new Scanner(System.in);
            
            System.out.print("Enter radius : ");
            radius = in.nextDouble(); //실수 입력일 때는 nextDouble을 사용한다
            
            area = 3.14 * radius * radius;
            System.out.println(area);
            System.out.printf("%f\n",area); //자바에서는 %f만 사용가능하다
        }
    }

## 환전하기
    
    import java.util.Scanner;
    
    public class ChangeMoney {
        public static void main(String[] args) {
            Scanner in = new Scanner(System.in);
            
            double rate = 1392.83;
            double dollar;
            int won;
            
            System.out.print("input won : ");
            won = in.nextInt();
            
            dollar = won / rate;
            System.out.println("dollar : " + dollar);
            System.out.printf("%f\n", dollar); //실수는 %f를 사용한다. %lf를 사용하면 안된다.
        }
    }

---
    
    import java.util.Scanner;
    
    public class Box {
        public static void main(String[] args) {
            Scanner in = new Scanner(System.in);
            
            double w;
            double h;
            double area;
            double perimeter;
            
            System.out.print("input w and h: "); //한번에 입력 가능하다.
            w = in.nextDouble(); //띄고 다음 줄에서 높이 입력
            h = in.nextDouble();
            
            area = w * h;
            perimeter = 2 * (w + h);
            
            System.out.println("area : " + area);
            System.out.println("perimeter : " + perimeter);
            
            System.out.printf("%.2f, %f\n", area, perimeter); //소수점 두자리까지만 사용한다.
        }
    
    }

---

    import java.util.Scanner;
    
    public class ChangeMile {
    
        public static void main(String[] args) {
            Scanner in = new Scanner(System.in);
            
                    final double FACTOR = 1.609; //자바에서 상수 선언은 final을 사용한다.
            int mile;
            double kilo;
            
            System.out.print("input mile : ");
            mile = in.nextInt();
            
            kilo = mile * FACTOR;
            System.out.printf("%d mile is %.2f kilo", mile, kilo);
                      
                    in.close(); //메모리 누수를 막기 위해 프로그램을 닫아준다.
        }
        
    }

---

    정수 / 정수 = 0 이므로 5 / 9 이런 것은 안되고 5.0 / 9나 5 / 9.0으로 만들어 주어야 한다.

    public class Light {
    
        public static void main(String[] args) {
            long lightSpeed, distance; //8 byte
            
            lightSpeed = 300000;
            distance = lightSpeed * 365 * 24 * 60 * 60;
            System.out.println(distance + "km");
            
            int test1 = 012;    //8진수
            int test2 = 0x1e;   //16진수
            int test3 = 0b1010; //2진수
            System.out.println(test1 + ", " + test2 + ", " + test3);
            
            int test4 = 123_456_789; //숫자에 _ 사용 가능
            double test5 = 123_456_789.0; 
            System.out.println(test4 + ", " + test5);
            
            System.out.println(10.2 % 2.1); //실수에 대한 mod 연산은 되도록 사용하지 않는다.
        }
    
    }

---

    public class BooleanTest {
    
        public static void main(String[] args) {
            // TODO Auto-generated method stub
            boolean b;
            final double PI = 3.141592; //상수를 선언과 초기화 분리 가능하지만 선언과 동시에 초기화 한다.
            
            //PI = 3.141592;
            
            b = true;
            System.out.println("b : " + b);
            b = 1 > 2;
            System.out.println("b : " + b);
            System.out.println("PI  : " + PI);
            
            //PI = 3.14; //변경 불가
        }
    
    }

---

    public class TypeConversion {
    
        public static void main(String[] args) {
            // TODO Auto-generated method stub
            int i;
            double f;
            
            f = 5 / 4;
            System.out.println(f);
            f = (double)5 / 4; //실수 연산을 원하면 분모나 분자 중 하나 또는 모두를 실수로 형변환을 한다.
            System.out.println(f);
            f = 5 / (double)4;
            System.out.println(f);
            f = (double)5 / (double)4;
            System.out.println(f);
            i = (int)1.3 + (int)1.8;
            System.out.println(i);
            //i = 1.3 + 1.8; //type mismatch error
        }
    
    }

---

    import java.util.Scanner;
    
    public class Lab {
        static void main(String[] args)
        {
            boolean isCapital;
            int citizen, riches;
            
            Scanner sc = new Scanner(System.in);
            //System.out.print("수도입니까?(1,0) : "); //정수 입력하는 경우
            //isCapital = (sc.nextInt() == 1) ? true : false;
            
            System.out.print("인구(단위 : 만) : ");
            citizen= sc.nextInt();
            System.out.print("부자의 수(단위") : ")"; 
            riches = sc.nextInt();
            
            boolean isMetro = (isCapital && citizens > 100) || (riches > 50);
            System.out.println("메트로폴리스 여부  : " +isMetro);
            
            //int x = y =0; error
            //int x = 0 , y = 0;
            int x, y;
            x = y = 0;
        }
    
    }

---

    import java.util.Scanner;
    
    public class LeapYear {
    
        public static void main(String[] args) {
            Scanner in = new Scanner(System.in);
            int year;
            boolean isLeapYear;
            
            System.out.println("input year : ");
            year = in.nextInt();
            
            if(((year % 4 == 0) && (year % 100 != 0)) || year % 400 == 0)
                isLeapYear = true;
            else
                isLeapYear = false;
            System.out.println(isLeapYear);
        }
    
    }
 
---
   
    import java.util.Scanner;
    
    public class Pay {
    
        public static void main(String[] args) {
            int pay, hours;
            final int RATE = 7000;
            
            Scanner input = new Scanner(System.in);
            System.out.print("Enter hours : ");
            hours = input.nextInt();
            
            if(hours > 0)
                pay = RATE * 8 + (int)(1.5 * RATE * (hours - 8)); //형변환 필요
            else
                pay = RATE * hours;
            
            System.out.printf("임금은 %d원 입니다.\n", pay);
            System.out.println("임금은 " + pay + "원 입니다.");
    
        }
    
    }

---

    import java.util.Scanner;
    
    public class Tax {
    
        public static void main(String[] args) {
            int income, tax;
            
            Scanner in = new Scanner(System.in);
            System.out.print("Enter income : ");
            income = in.nextInt();
            
            if(income <= 1000)
                tax = (int)(0.09 * income);
            else if(income <= 4000)
                tax = (int)(0.18 * income);
            else if(income < 8000)
                tax = (int)(0.27 * income);
            else
                tax = (int)(0.36 * income);
            
            System.out.println("소득세는 " + tax + "입니다.");
        }
    
    }

---

    import java.util.Scanner;
    
    public class SwitchExample {
    
        public static void main(String[] args) {
            int number;
            
            Scanner in = new Scanner(System.in);
            System.out.print("Enter number : ");
            number = in.nextInt();
            
            switch(number) //( )안에는 무조건 정수형만(int나 char)
            {
            case 0 : 
                System.out.println("없음");
                break;
            case 1 :
                System.out.println("하나");
                break;
            case 2 :
                System.out.println("둘");
                break;
            default : 
                System.out.println("많음");
                break;
            //if else문과 같다
            }
        }
    
    }

---
    
    // switch문의 조건식에서 string을 사용할 수 있다.(JAVA에서만 가능, C나 C++에서는 사용 불가능)
    
    import java.util.Scanner;
    public class StringSwitch {
    
        public static void main(String[] args) {
            String month;
            int monthNumber;
            
            Scanner sc = new Scanner(System.in);
            System.out.print("Enter month : ");
            month = sc.next(); //token 입력
            
            switch(month)
            {
            case "January" : 
                monthNumber = 1;
                break;
            case "Feburary" :
                monthNumber = 2;
                break;
            case "March" :
                monthNumber = 3;
                break;
            default :
                monthNumber = 0;
                break;	
            }
            
            System.out.println(monthNumber);
    
        }
    
    }

---

    //break 생략하는 경우
    import java.util.Scanner;
    
    public class DaysInMonth {
        public static void main(String[] args)
        {
            int month, year = 2017, days = 0; //days의 default값을 정해주어야지 안그러면 
                                              //switch문의 default에서 days의 값이 없음으로 
                                              //간주하여 오류가 생긴다.
            
            Scanner scan = new Scanner(System.in);
            System.out.print("Enter month : ");
            month = scan.nextInt();
            
            switch(month)
            {
            case 1:
            case 3:
            case 5:
            case 7:
            case 10:
            case 12:
                days = 31;
                break;
            case 4:
            case 6:
            case 9:
            case 11:
                days = 30;
                break;
            case 2:
                if(((year % 4 == 0) && (year % 100 != 0)) || (year % 400 == 0))
                    days = 29;
                else
                    days = 28;
                break;
            default :
                System.out.println("Wrong input : ");
                break;	
            }
            System.out.println("Days : " + days); //days에 초기값이 없는 경우 에러가 생김
        }
    
    }

---

    import java.util.Scanner;
    //import java.lang.Math; //자동 import
    
    public class Programming4 {
    
        public static void main(String[] args) {
            int n1, n2, n3, min;
            System.out.print("input n1, n2, n3 : ");
            
            Scanner sc = new Scanner(System.in);
            n1 = sc.nextInt();
            n2 = sc.nextInt();
            n3 = sc.nextInt();
            
            if(n1 > n2)
            {
                if(n2 > n3)
                    System.out.printf("가장 작은 수는 %d\n", n3);
                else
                    System.out.printf("가장 작은 수는 %d\n", n2);
            }
            else
            {
                if(n1 > n3)
                    System.out.printf("가장 작은 수는 %d\n", n3);
                else
                    System.out.printf("가장 작은 수는 %d\n", n1);
            }
            
            min = (n1 < n2) ? ((n1 < n3) ? n1 : n3) : ((n2 < n3) ? n2 : n3);
            System.out.printf("가장 작은 수는 %d\n", min);
            
            min = Math.min(n2, n3);
            System.out.printf("가장 작은 수는 %d\n", min);
        }
    
    }

---

    import java.util.Scanner;
    
    public class Prog6_3 {
    
        public static void main(String[] args) {
            double height, weight, default_weight;
            
            Scanner sc = new Scanner(System.in);
            System.out.print("input height : ");
            height = sc.nextDouble();
            System.out.print("input weight : ");
            weight = sc.nextDouble();
            
            default_weight = (height - 100) * 0.9;
            
            if(default_weight > weight)
                System.out.println("과체중");
            else if(default_weight == weight)
                System.out.println("표쥰체중");
            else
                System.out.println("저체중");
        }
    
    }

---

    import java.util.Scanner;
    
    public class Prog6_5 {
        public static void main(String[] args)
        {
            double x, fx;
            
            Scanner sc = new Scanner(System.in);
            System.out.print("input x : ");
            x = sc.nextDouble();
            
            if(x <= 0)
                fx = (x * x) - (9 * x) + 2;
            else
                fx = (7 * x) + 2;
              //fx = Math.pow(x, 3) - 9 * x  + 2;
            System.out.println("fx : " + fx);
        }
    
    }

---
    
    //반복문 while, do~while, for문을 이용한 구구단 출력
    
    import java.util.Scanner;
    
    public class LoopExanmple2 {
    
        public static void main(String[] args) {
            int n, i;
            
            Scanner sc = new Scanner(System.in);
            System.out.println("단을 입력하시오 : ");
            n = sc.nextInt();
            
            i = 1;
            while(i <= 9)
            {
                System.out.println(n + " * " + i + " = " + (n * i)); //연산식은 () 사용
                i++;
            }
            
            System.out.println("\n");
            
            System.out.println("단을 입력하시오 : ");
            n = sc.nextInt();
            
            i = 1;
            do //조건문이 아래에 있기 때문에 무조건 한 번은 실행한다
            {
                System.out.println(n + " * " + i + " = " + (n * i)); 
                i++;
            }while(i <= 9);
            
            System.out.println("\n");
            
            System.out.println("단을 입력하시오 : ");
            n = sc.nextInt();
    
            for(i = 1; i<=9; i++) //(초기화; 조건식; 증감식)
                System.out.println(n + " * " + i + " = " + (n * i)); 
    
        }
    
    }

## 최대공약수(Greatest Common Divisor) 구하기
    
    import java.util.Scanner;
    
    public class Gcd {
    
        public static void main(String[] args) {
            int x, y, r;
            
            Scanner sc = new Scanner(System.in);
            System.out.println("Enter 2 numbers(larger, smaller) : ");
            x = sc.nextInt();
            y = sc.nextInt();
            
            while(y != 0)
            {
                r = x % y;
                x = y;
                y = r;
            }
            
            System.out.println("Gcd is " + x);
        }
    
    }

## 난수발생(Random Number Generator)
    
    import java.util.Scanner;
    import java.util.Random;
    
    //import java.util.*; //java.util의 패키지 클래스를 모두 끌어다 가져옴
    //--> 자바는 Dinamic Linking이라는 기능이 있는데 이것은 util.*을 쓸 때
    //처음부터 다 부르는 것이 아니라 코드상에서 필요한 부분에서만 필요한
    //클래스를 쓰는 것이기 때문에 크기가 커지는 불안함은 없어도 된다.
    
    public class LetterGame {
    
        public static void main(String[] args) {
            Random rand = new Random(); //Randomclass 객체 생성
            
            for(int i=0; i<10; i++)
                System.out.println(rand.nextInt(10) + "\t" + rand.nextDouble());
            //                             0 <= 난수 < 10        0.0 <= 난수 < 1.0
            //nextInt는 정수값이 필요      //nextDouble은 매개변수 있으면 안됨
            
            System.out.println("\n");
            
            //주사위 게임
            System.out.println("주사위를 세 번 던지시오.");
            for(int i=0; i<3; i++)
                System.out.println((rand.nextInt(6) + 1) + "\t" + ((int)(rand.nextDouble() * 6) + 1));
            //nextInt ()안에 6을 넣으면 0부터 5까지이므로 여기서 1을 더해준다.
            //nextDouble ()안에 아무것도 넣으면 안되고, 6을 곱해주면 0부터 5.99999가 되므로 int형으로 형변환 후 1을 더해준다.
            
            
            int answer, guess, tries = 0;
            
            answer  = rand.nextInt(50); //0부터 50사이의 난수 발생
            Scanner in = new Scanner(System.in);
            
            do
            {
                System.out.println("Enter number : ");
                guess = in.nextInt();
                tries++;
                
                if(guess > answer)
                    System.out.println("Too High");
                else if(guess < answer)
                    System.out.println("Too Low");
            }while(guess != answer);
            
            System.out.println("Congratulation! # of try : " + tries);
        }
    
    }

## 반복문 - Factorial
    
    import java.util.Scanner;
    
    public class Factorial {
    
        public static void main(String[] args) {
            Scanner sc = new Scanner(System.in);
            double fact = 1; //반드시 초기화
            int n;
            
            System.out.print("Enter number : ");
            n = sc.nextInt();
            
            for(int i=1; i<=n; i++)
                fact *= i;
            
            System.out.printf("%d! : %.2f",n,fact);
        }
    
    }

## 반복문 - break
    
    import java.util.Scanner;
    
    public class BreakTest {
    
        public static void main(String[] args) {
            int total = 0, count = 0, grade;
            
            Scanner in = new Scanner(System.in);
            while(true) //입력 데이터의 수를 모를 때, 음수가 입력될때까지 실행
            {
                System.out.println("Enter score : ");
                grade = in.nextInt();
                if(grade < 0)
                    break; //가장 가까운 반복문을 빠져 나간다.
                total += grade;
                count++;
            }
           
            System.out.println("Average : " + (total / count));
        }
    
    }

## 반복문 - continue
    
    public class ContinueTest {
    
        public static void main(String[] args) {
            String s = "no name is good name";
            int n = 0;
            
            System.out.println(s.length());
            for(int i=0; i<s.length(); i++) //String의 길이
            {
                if(s.charAt(i) != 'n') //s 문자열의 i번째 문자
                    continue;
                n++;
            }
            System.out.println("Number of n : " + n);
        }
    
    }

## 반복문 - PI
    
    import java.util.Scanner;
    
    public class Triangle {
    
        public static void main(String[] args) {
            double divisor, dividend, sum;
            int count;
            
            Scanner sc = new Scanner(System.in);
            divisor = 1.0;
            dividend = 4.0;
            sum = 0.0;
            
            System.out.print("Enter count : ");
            count = sc.nextInt();
            
            while(count > 0)
            {
                sum = sum + dividend / divisor;
                dividend *= -1.0;
                divisor += 2;
                count--;
            }
            
            System.out.println("PI = " + sum);
        }
    
    }

## 반복문 - 소수
    
    //입력 숫자가 소수인지 판별
    
    import java.util.Scanner;
    
    public class Prog6_6 {
    
        public static void main(String[] args) {
            /*
            int number;
            boolean isPrime = true;
            
            Scanner sc = new Scanner(System.in);
            System.out.print("Enter number : ");
            number = sc.nextInt();
            
            for(int i=2; i<number; i++)
            {
                if(number % i == 0)
                {
                    isPrime = false;
                    break;
                }
            }
            
            if(isPrime)
                System.out.println(number + " is Prime");
            else
                System.out.println(number + " is not Prime");
            */
            
            /*
            int count;
            int number = 0;
            
            for(int i=2; i<100; i++)
            {
                count = 0;
                for(int j=1; j<=i; j++)
                {
                    if(i % j == 0)
                        count++;
                }
                
                if(count == 2)
                {
                    number++;
                    System.out.print(i + " ");
                }
            }
            System.out.println("");
            System.out.printf("소수는 총 %d개 이다", number);
            */
        
            boolean isPrime;
            
            for(int i=2; i<100; i++)
            {
                isPrime = true;
                for(int j=2; j<i; j++)
                {
                    if(i % j == 0)
                    {
                        isPrime = false;
                        break;
                    }
                }
                
                if(isPrime)
                    System.out.print(i + " ");
            }
            
        }
    
    }
    

---
    
    class Car
    {
        String color;
        int speed;
        int gear;
        
        void print()
        {
            System.out.println("(" + color + ", " + speed + ", " + gear + ")");
        }
        
    }
    
    //main이 없는 클래스는 public이 들어가면 안됨
    //파일 이름도 main 함수가 있는 클래스와 같아야 한다.
    public class CarTest {
    
        public static void main(String[] args) {
            Car myCar = new Car(); //객체 생성은 반드시 new 연산자 사용
            
            myCar.print(); //instance 변수(멤버변수, 필드)는 자동 초기화된다
            myCar.color = "red";
            myCar.speed = 0;
            myCar.gear = 1;
            myCar.print();
            
            Car yourCar = new Car();
            
            yourCar.color = "blue";
            yourCar.speed = 60;
            yourCar.gear = 3;
            yourCar.print();
            
            //int num = 100;
            //System.out.println(num); //지역변수는 자동 초기화 되지 않는다
            
            yourCar = myCar; //2개의 객체변수가 동일한 객체 참조
            myCar.print();
            yourCar.print();
            
            myCar = null; //객체 소멸
            //myCar.print(); 
            yourCar.print();
            
        }
    
    }

---

    public class StringTest {
    
        public static void main(String[] args) {
            String proverb = "A barking dog";
            //String proverb = new String("A barking dog");
            String s1, s2, s3, s4;
            
            System.out.println(proverb.length());
            s1 = proverb.concat("never bites!");
            //s1 = proverb + " never bites!";
            s2 = proverb.replace('b', 'B'); //proverb는 변하지 않는다
            s3 = proverb.substring(2,  5); //5번째는 포함되지 않는다
            s4 = proverb.toUpperCase(); //모두 대문자로 변환
            
            System.out.println("s1 : " + s1);
            System.out.println("s2 : " + s2);
            System.out.println("s3 : " + s3);
            System.out.println("s4 : " + s4);
            System.out.println(proverb); //proverb는 변하지 않는다
            
            System.out.println(proverb.equals(s2)); //값 비교
            System.out.println(proverb.compareTo(s2)); //b : 98, B : 66 (코드 값의 차이 반환)
            System.out.println("A".compareTo("C")); //A : 65, C : 67
            
            int num = Integer.parseInt("123"); //숫자 형태의 문자열을 int로 형변환 *문자 형태로 들어가면 에러*
            System.out.println(num);
            double d = Double.parseDouble("3.141592");
            System.out.println(d);
            
            for(int i = 0; i < proverb.length(); i++)
                System.out.println(proverb.charAt(i)); //i번째 문자
            
            String str1 = new String("abc");
            String str2 = new String("abc");
            System.out.println(str1 == str2); //객체 비교, 같은 객체?
            System.out.println(str1.equals(str2)); //내용 비교
            
            String str3 = "abc"; //new로 선언하면 분명히 객체를 생성하지만 new를 안하면 같은 객체로 취급된다.
            String str4 = "abc"; //str4 = str3로 동작, new를 이용해서 만드는 것이 훨씬 더 안전하다.
            System.out.println(str3 == str4); //객체 비교
            System.out.println(str3.equals(str4)); //내용 비교
            
        }
    
    }
    
 
---
    
    public class WrapperTest {
    
        public static void main(String[] args) {
            Integer num1 = new Integer(100);
            Integer num2 = 100; //int를 Integer 객체로 자동 변환
                                //auto boxing
            System.out.println(num1 + ", " + num2);
            
            System.out.println(num1 == num2); //객체 비교 --> false
            System.out.println(num1.equals(num2)); //값 비교 --> true
            
            int num3 = 200;
            int num4 = 200;
            System.out.println(num3 == num4); //값 비교 --> true
            
            //기본 data type에서는 == 연산자는 값을 비교
            //객체 data type에서는 == 연산자는 객체를 비교
            
            int n1 = Integer.parseInt("123"); //정수 형태의 String을 int로 리턴
            Integer n2 = Integer.valueOf("123"); //정수 형태의 String을 Integer 객체로 변환
            String str = Integer.toString(123); //int를 String 객체로 변환
            int n3 = n2; //Integer 객체를 int로 자동 변환(auto unboxing), n2는 Integer 객체, n3는 기본 변수
            System.out.println(n1 + ", " + n2 + ", " + str + ", " + n3);
            
        }
    }
    
    //Wrapper class의 필요성
    //정수형 변수를 int num; 으로 선언하면 num 변수에 정수값을 저장하는 기능뿐이 없지만
    //wrapper class를 사용하여 Integer num; 으로 선언하면
    //Integer class에서 제공하는 많은 method들을 사용할 수 있다
    

---

    import java.util.Scanner;
    
    class BankAccount //public으로 선언하지 않는 이유는 main 클래스가 아니므로
    {                 //만약 public으로 선언하면 오류가 생긴다. --> 독립적인 파일을 생성하라.
        int balance; //잔액, 접근지시제어자를 안쓰면 패키지 파일로 선언되어 같은 패키지 안에서 사용 가능
        final double INTEREST_RATE = 0.075; //이자율
        
        public void deposit(int amount) //예금 기능
        {
            this.balance += amount;
        }
        
        public void withdraw(int amount) //출금 기능
        {
            if(balance < amount)
                System.out.println("잔액이 부족합니다.");
            this.balance -= amount;
        }
        
        public int getBalance() 
        {
            return balance;
        }
        
        public void addInterest()
        {
            balance += (int)(balance * INTEREST_RATE);
        }
    }
    
    public class BankAccountTest 
    {
        public static void main(String[] args) 
        {
            Scanner sc = new Scanner(System.in);
            BankAccount account = new BankAccount();
            
            System.out.print("얼마를 예금하시겠습니까?");
            int Input_money = sc.nextInt();
            account.deposit(Input_money);
            System.out.println("잔액 : " + account.getBalance());
            
            System.out.print("얼마를 출금하시겠습니까?");
            int Output_money = sc.nextInt();
            account.withdraw(Output_money);
            System.out.println("잔액 : " + account.getBalance());
            
            account.addInterest();
            System.out.println("이자를 더한 잔액 : " + account.getBalance());
        }
    }

---

    class NumberBox
    {
        public int ivalue;
        public double dvalue;
        
        public void print()
        {
            System.out.println(ivalue + "\n" + dvalue);
        }
    }
    
    public class NumberBoxTest 
    {
        public static void main(String[] args) 
        {
            NumberBox nb = new NumberBox();
            
            nb.ivalue = 10;
            nb.dvalue = 1.2345;
            
            System.out.println(nb.ivalue + ", " + nb.dvalue);
            
            nb.print();
    
        }
    
    }

---

    import java.util.Scanner;
    
    class Date
    {
        int year, month, day;
        
        public void setDate(int y, int m, int d)
        {
            year = y;
            month = m;
            day = d;
        }
        
        public void print()
        {
            System.out.println(year + "년 " + month + "월 " + day + "일입니다.");
        }
    }
    
    public class DateTest 
    {
        public static void main(String[] args) 
        {
            Scanner sc = new Scanner(System.in);
            Date date = new Date();
            
            date.print(); //instance 변수는 자동으로 초기화 된다.(0년 0월 0일)
            
            System.out.print("몇 년도 입니까?");
            int year = sc.nextInt();
            
            System.out.print("몇 월 입니까?");
            int month = sc.nextInt();
            
            System.out.print("몇 일 입니까?");
            int day = sc.nextInt();
            
            date.setDate(year, month, day);
            date.print();
        }
    
    }

---
    
    public class Exercise4 {
    
        public static void main(String[] args) {
            String verb = "현실이 된다"; 
            System.out.println("생각이 " + verb); //"생각이"라는 것이 객체 자체이다.
            System.out.println("생각이".concat(verb)); 
            
            String s = "1234567";
            for(int i = 0; i < s.length(); i++)
                System.out.println(s.charAt(i));
            
            s = "ABCDEFG";
            s.toLowerCase(); //s 자체가 변하지 않늗다
            System.out.println(s);
            String str = s.toLowerCase();
            System.out.println(str);
            
            System.out.println(2+3); //()안에 String이 없는 경우 산술연산 수행
            System.out.println("2 + 3 = " + 2 + 3); //String이 있는 경우 문자열로 취급하여 문자열로 연결해준다.
            System.out.println("2 + 3 = " + (2 + 3));  //()안에는 산술연산 수행
        }
    
    }