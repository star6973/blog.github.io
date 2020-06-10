---
layout: post
title: "Java Programming [Day 4]"
date: 2017-10-13 09:00:00-18:00:00
categories: Language
tag: Language
use_math: true
---
        
# Java 예제 4일만에 뽀개기 - Day 4
    
    예외 : 잘못된 코드, 부정확한 데이터, 예외적인 상황에 의하여 발생하는 오류
    
    import java.util.Scanner;
    
    public class DivideByZero {
    
        public static void main(String[] args) {
            Scanner sc = new Scanner(System.in);
            
            System.out.print("분자 : ");
            int x = sc.nextInt();
            System.out.print("분모 : ");
            int y = sc.nextInt();
            int result = x / y;
            // y에 0을 넣어주면 위의 문장이 실행이 되지 않는다
            System.out.println("Result : " + result);
            
            sc.close();
        }
    
    }

---
    
    // 분모가 0인 경우 ArithmeticException이 발생한다
    // Exception이 발생할 수 있는 코드를 try 블락 안에 넣는다.
    // catch 블락이 exception의 종류에 따라 처리할 코드를 넣는다.
    // exception 발생하건 안하건 finally block 실행
    
    import java.util.Scanner;
    
    public class DivideByZero {
        public static void main(String[] args) {
            int result = 0;
            Scanner sc = new Scanner(System.in);
            
            System.out.print("분자 : ");
            int x = sc.nextInt();
            System.out.print("분모 : ");
            int y = sc.nextInt();
            
            try {
                result = x / y;
            } catch(ArithmeticException e) {
                System.out.println("Can not divide by 0");
            } finally {
                System.out.println("finally 블락은 option");
                sc.close();
            }
            System.out.println("Program reaches here");
            System.out.println("Result : " + result);
        }
    }

---

    /*
    public class ArrayException {
        public static void main(String[] args) {
            int[] array = { 1, 2, 3, 4, 5 };
            
            for(int i = 0; i <= array.length; i++) 
                System.out.println(array[i] + " ");
            
            System.out.println("Program ends\n");
        }
    
    }
    */
    
    public class ArrayException {
        public static void main(String[] args) {
            int[] array = { 1, 2, 3, 4, 5 };
            
            try {
                for(int i = 0; i <= array.length; i++) 
                    System.out.println(array[i] + " ");
            } catch(ArrayIndexOutOfBoundsException e) {
                System.out.println("Array index out of bounds exception");
                
                System.out.println(e.getMessage()); // 이 두 라인을 적으면(catch안에 꼭 적어주자)
                e.printStackTrace(); // 어느 위치에서 어떤 예외가 발생했는지 알려준다
            } finally {
                System.out.println("Finally block");
            }
            System.out.println("Program ends\n");
        }
    
    }

---

    // IOException을 처리하지 않아 compile error 발생
    import java.io.PrintWriter;
    import java.io.FileWriter;
    
    public class FileException {
        private int[] list;
        private static final int SIZE = 10;
        
        public FileException() {
            list = new int[SIZE];
            
            for(int i =  0; i < SIZE; i++)
                list[i] = i;
            writeList();
        }
        
        public void writeList() {
            PrintWriter out = null;
            
            out = new PrintWriter(new FileWriter("outfile.txt"));
            for(int i = 0; i < SIZE; i++)
                out.println("Array element " + i + " = " + list[i]);
                
        }
        
        public static void main(String[] args) {
            new FileExeption();
        }
    }
    
---

    import java.io.PrintWriter;
    import java.io.FileWriter;
    import java.io.IOException; // IOException도 하나의 클래스이므로
    
    public class FileException {
        private int[] list;
        private static final int SIZE = 10;
        
        public FileException() {
            list = new int[SIZE];
            
            for(int i =  0; i < SIZE; i++)
                list[i] = i;
            writeList();
        }
        
        public void writeList() {
            PrintWriter out = null;
            
            try {
                out = new PrintWriter(new FileWriter("outfile.txt"));
                for(int i = 0; i < SIZE; i++)
                    out.println("Array element " + i + " = " + list[i]);
            } catch(IOException e) {
                System.out.println("IOException");
            } finally {
                if(out != null)
                    out.close();
            }
        }
        
        public static void main(String[] args) {
            new FileException(); // outFile이라는 텍스트가 생성
        }
    }

---

    예외의 종류
    1. Error
       : 너무 심각해서 할 수 있는 범이 없음 -> 통과
    
    2. RuntimeException
       : 프로그래밍 버그이므로 스스로 고쳐야함(처리 안해도 컴파일 가능) -> 통과
    
    3. Error나 RuntimeException이 아닌 예외 // Checked Exception -> try ~ catch 사용
       : 반드시 처리해야함(안하면 컴파일이 안됨) -> 검사

    - RuntimeException의 종류
        1. ArithmeticException : 어떤 수를 0으로 나눌 때 발생한다.
        2. NullPointerException : 널 객체를 참조할 때 발생한다.
        3. ClassCastException : 적절치 못하게 클래스를 형변환하는 경우 발생한다.
        4. NegativeArraySizeException : 배열의 크기가 음수인 경우 발생한다.
        5. OutOfMemoryException : 사용 가능한 메모리가 없는 경우 발생한다.
        6. NoClassDefFoundException : 원하는 클래스를 찾지 못하였을 경우 발생한다.
        7. ArrayIndexOutOfBoundsException : 배열을 참조하는 인덱스가 잘못된 경우 발생한다.

    NumberException(상위 클래스) > TooSmallException(하위 클래스)
    TooSmallException이 먼저 나오고 NumberException이 나와야지 그렇지 않으면 NumberException이 에러를 다 잡아버린다                                                                                                                                                                                             
    
    public class ExceptionHierarchy {
    
        public static void main(String[] args) {
            int[] array = { 1, 2, 3, 4, 5 };
            
            try {
                for(int i = 0; i <= array.length; i++)
                    System.out.println(array[i] + " ");
            } catch(ArrayIndexOutOfBoundsException e) {
                System.out.println("Exception");
            } catch(Exception e) {
                System.out.println("ArrayIndexOfBoundsException");
            } /*catch(ArrayIndexOutOfBoundsException e) {
                System.out.println("Exception");
            }*/ // 밑에 있으면 위의 Exception이 모든 예외를 처리한다
        }
    }

---

    예외를 처리하는 방법
    1. try~catch
    2. throws
    
    // Exception을 함수에서 직접 처리하는 방법
    
    public class Test {
    
        public static void main(String[] args) {
            int[] array = { 1, 2, 3, 4, 5 };
            
            method(array);
        }
        
        public static void method(int[] array) {
            try {
                for(int i = 0; i <= array.length; i++) 
                    System.out.println(array[i] + " ");
                } catch(ArrayIndexOutOfBoundsException e) {
                    System.out.println("Array index out of bounds exception");
                    System.out.println(e.getMessage());
                    e.printStackTrace();
                }
        }
    }
    
    // Exception을 호출 함수에 전달하여 처리하는 방법
    
    public class Test {
        public static void main(String[] args) {
            int[] array = { 1, 2, 3, 4, 5 };
            
            try {
                method(array);
            } catch(ArrayIndexOutOfBoundsException e) {
                System.out.println("Array index out of bounds exception");
                System.out.println(e.getMessage());
                e.printStackTrace();
            }
        }
        
        public static void method(int[] array) throws ArrayIndexOutOfBoundsException 	{
            for(int i = 0; i <= array.length; i++)
                System.out.println(array[i] + " ");
        }
    }
    
    // Exception 객체를 생성하고 호출 함수에 전달, 처리하는 방법
    
    public class Test {
        public static void main(String[] args) {
            int[] array = { 1, 2, 3, 4, 5 };
            
            try {
                method(array);
            } catch(ArrayIndexOutOfBoundsException e) {
                System.out.println("Array index out of bounds exception");
                System.out.println(e.getMessage());
                e.printStackTrace();
            }
        }
        // 함수 뒤에다 이 Exception이 발생할 수 있다를 표현하기 위해서는 throws를 사용한다
        public static void method(int[] array) throws ArrayIndexOutOfBoundsException {
            for(int i = 0; i <= array.length; i++) {
                if(i == array.length)
                    // 실제 이 부분에 대해서 Exception이 발생할 수 있다를 표현하기 위해서는 throw를 사용한다
                    throw new ArrayIndexOutOfBoundsException(); // Exception 객체 생성하여 전달
                System.out.println(array[i] + " ");
            }
        }
    }
    
    // RuntimeErrorException의 경우 throws의 구문을 사용할 수 있다

---
    
    // 사용자 정의 exception class
    // 함수에서 exception을 처리하는 방법
    
    import java.util.Scanner;
    
    class MyException extends Exception {
        public MyException() {
            super("This is my exception");
        }
    }
    
    public class MyExceptionTest {
    
        public static void main(String[] args) {
            Scanner sc = new Scanner(System.in);
            
            System.out.print("Enter numerator : ");
            int x = sc.nextInt();
            System.out.print("Enter denominator : ");
            int y = sc.nextInt();
            
            divide(x, y);
        }
        
        public static void divide(int x, int y) {
            int result = 0;
            
            try {
                if(y == 0)
                    // 자신이 만든 exception이므로 throw를 하여 객체를 생성해주어야 한다
                    throw new MyException();
                result = x / y;
            } catch(MyException e) {
                System.out.println("분모가 0입니다.\n");
                System.out.println(e.getMessage());
                e.printStackTrace();
            }
            System.out.println("Result is " + result);
        }
    
    }
    
    // Exception을 호출 함수에 전달하여 처리하는 방법
    
    import java.util.Scanner;
    
    class MyException extends Exception {
        public MyException() {
            super("This is my exception");
        }
    }
    
    public class MyExceptionTest {
    
        public static void main(String[] args) {
            Scanner sc = new Scanner(System.in);
            
            System.out.print("Enter numerator : ");
            int x = sc.nextInt();
            System.out.print("Enter denominator : ");
            int y = sc.nextInt();
            try {
                divide(x, y);
            } catch(MyException e) {
                System.out.println(e.getMessage());
                e.printStackTrace();
            }
        }
        
        public static void divide(int x, int y) throws MyException {
            if(y == 0)
                throw new MyException();
            else
                System.out.println(x / y);
        }
    
    }

---

    제네릭 프로그래밍
    - 다양한 타입의 객체를 동일한 클래스로 사용한다
    - 첫 번째 방법 : 일반적인 객체를 처리하려면 Object 참조 변수를 사용
                 : Object 참조 변수는 어떤 객체이던지 참조할 수 있다
    
    public class Box<T> { // T : 타입 매개변수(객체 생성시 타입 지정)
        private T data;
        
        public void set(T d) {
            data = d;
        }
        
        public T get() {
            return data;
        }
    
        public static void main(String[] args) {
    //		Box<String> b = new Box<String>(); // String 타입으로 변수 선언
            Box<String> b = new Box<>(); // Java 7부터는 생성자 호출이 타입인수 생략 가능, diamond
            b.set("Hello World"); // b는 String을 가질 수 있는 변수가 되었다
            System.out.println(b.get()); 
            String str = b.get(); // type T가 String으로 형변환 불필요
            
            Box<Integer> i = new Box<>();
            i.set(100); // i.set(new Integer(100)); (auto boxing)
            System.out.println(i.get());Integer num = i.get();
            int n = i.get(); // auto unboxing(Integer 객체를 int로 변환)
            System.out.println(n);
    //		i.set("String"); // type이 다를 경우 error message
        }
    }

---

    public class Point<T> {
        private T x, y;
        
        public Point(T x, T y) {
            this.x = x;
            this.y = y;
        }
        
        public void set(T o1, T o2) {
            x = o1; 
            y = o2;
        }
        
        public T getX() {
            return x;
        }
        
        public T getY() {
            return y;
        }
    
        public static void main(String[] args) {
            Point<Integer> g1 = new Point<>(1, 2);
            g1.set(10, 20);
            System.out.println(g1.getX() + ", " + g1.getY());
            
            Point<Double> g2 = new Point<>(1.1, 2.2);
            g2.set(1.2, 3.4);
            System.out.println(g2.getX() + ", " + g2.getY());
        }
    
    }

---

    - 타입 매개 변수의 표기
        1. K - Key
        2. N - Number
        3. T - Type(대체로 많이 씀)
        4. V - Value
        5. E - Element
    
    - 다중 타입 매개 변수
    
    // 타입 매개변수는 여러개를 지정할 수 있다
    
    class OrderedPair<K, V> { // Key, Value
        private K key;
        private V value;
    
        public OrderedPair(K k, V v) {
            key = k;
            value = v;
        }
    
        public K getKey() {
            return key;
        }
    
        public V getValue() {
            return value;
        }
    }
    
    public class OrderedPairTest {
        public static void main(String[] args) {
            OrderedPair<String, Integer> p1 = new OrderedPair<String, Integer>("Even", 8); // 파라미터 지정
            OrderedPair<String, String> p2 = new OrderedPair<>("hello", "world"); // 파라미터 생략
    
            System.out.println(p1.getKey());
            System.out.println(p1.getValue());
    
            System.out.println(p2.getKey());
            System.out.println(p2.getValue());
    
            // generic class의 객체 생성에서 파라미터를 생략하면 모든 파라미터를
            // Object로 간주한다.
            // Java 5 이전에 작성했던 프로그램들과의 호환성을 유지하기 위함
            // 이것을 raw type이라고 한다
    
            OrderedPair p3 = new OrderedPair("Hello", 10);
            System.out.println(p3.getKey());
            System.out.println(p3.getValue());
        }
    
    }

---

    제네릭 메소드
    - 메소드에서도 타입 매개 변수를 사용하여서 제네릭 메소드를 정의할 수 있다.
    - 타입 매개 변수의 범위가 메소드 내부로 제한된다.
    - Comparable까지는 몰라도 됨
    
    // 일반 클래스의 메소드를 generic 메소드로 작성
    
    class GenericMethod {
        public <E> void printArray(E[] array) { // 타입 매개변수는 반드시 수식자와 반환형 사이에 기술
            for(int i = 0; i < array.length; i++) 
                System.out.print(array[i] + " ");
            System.out.println("\n");
        }
    }
    
    public class GenericMethodTest {
        public static void main(String[] args) {
            Integer[] intArray = { 1, 2, 3, 4, 5 };
            Double[] dArray = { 1.1, 2.2, 3.3, 4.4, 5.5 };
            Character[] cArray = { 'H', 'E', 'L', 'L', 'O' };
            String[] sArray = { "Lee", "Kim", "Choi" }; // 새로운 타입 추가 가능
            
            GenericMethod gm = new GenericMethod();
            gm.<Integer>printArray(intArray); // 타입 생략 가능
            gm.printArray(dArray);
            gm.printArray(cArray);
            gm.printArray(sArray);
        }
    
    }

---

    컬렉션
    - 자바에서 자료 구조를 구현한 클래스
    - 리스트, 스택, 큐, 집합, 해쉬 테이블
    
    1. Vector 클래스
    
    // Vector 객체는 자동으로 크기 조절
    // 어떤 타입도 저장 가능하지만(raw type) generic class의 특성을 사용하기 위하여 타입을 정의한다.
    
    import java.util.Vector;
    
    public class VectorTest {
    
        public static void main(String[] args) {
            Vector vc = new Vector(); // 모든 종류의 객체 저장 가능(raw type)
            
            System.out.println(vc.size() + ", " + vc.capacity() + "\n");
            vc.add("Hello World"); // 0번방에 "Hello World"라는 string을 집어넣는다(배열의 형태로 이루어진다)
            vc.add(new Integer(10)); // 1번방에 정수형 10을 집어넣는다.
            vc.add(20); // auto boxing // 2번방에 정수 20을 집어넣는다.
            vc.add(3.14); // 3번방에 실수 3.14를 집어넣는다.
            vc.add(new Double(10.5)); // 4번방에 실수형 10.5를 집어넣는다.
            vc.add(1, "Java"); // 1번방에 "Java"라는 string을 집어넣는다(기존에 있던 box의 구성원들은 뒤로 하나씩 밀린다)
            vc.insertElementAt("new string", 1); // 1번방에 "new string"을 집어넣는다(기존에 있던 box의 구성원들은 뒤로 하나씩 밀린다)
            for(int i = 0; i < vc.size(); i++) 
                System.out.println(i + " : " + vc.get(i));
            
            System.out.println(vc.size() + ", " + vc.capacity());
            System.out.println(vc.contains(20)); // 원소 중에 20이 있는가? True or False
            System.out.println(vc.lastElement());
            System.out.println(vc);
        }
    }
    
    import java.util.Vector;
    
    public class VectorTest {
        public static void main(String[] args) {
            Vector<String> str = new Vector<>(); // String 객체만 저장 가능
            str.add("My");
            str.add("Vector");
            str.add("Test");
            for(String s : str) 
                System.out.println(s);
            str.remove(0);
            str.add(0, "Your");
            str.set(1, "vector"); // 기존에 "Test" 문자열 삭제
            str.insertElementAt("string", 1);
            str.add("!!!"); // 인덱스를 사용하지 않으면 마지막에 들어가진다.
            for(String s : str)
                System.out.println(s);
            
            Vector<Integer> array = new Vector<>(); // Integer 객체만 저장 가능
            for(int i = 0; i < 11; i++) {
                array.add(i);
                System.out.println(array.get(i));
            }
            System.out.println(array.size() + ", " + array.capacity());
            System.out.println(array.contains(20)); // True or False
            System.out.println(array.lastElement());
            System.out.println(array);
        }
    }

---

    자바에서의 컬렉션
    1. 인터페이스 - Collection, Set, List, Map, Queue
    2. 클래스

    - ArrayList
    // 가변 길이의 배열 구현
    // 장점 : list 원소에 점자를 이용한 빠른 접근
    // 단점 : list 중간에 삽입, 삭제할 경우 list 전체 변경
    
    import java.util.*;
    
    public class ArrayListTest {
    
        public static void main(String[] args) {
            ArrayList<String> list = new ArrayList<>();
            
            list.add("Milk");
            list.add("Bread");
            list.add("Butter");
            list.add(1, "Apple");
            list.set(2, "Grape");
            list.remove(3);
            list.add("Apple");
            
            for(int i = 0; i < list.size(); i++) // for loop을 이용한 출력
                System.out.println(list.get(i));
            System.out.println();
            
            Iterator<String> e = list.iterator(); // Iterator(반복자)를 이용한 출력
            String s;
            while(e.hasNext()) { // Iterator의 e 다음 원소가 있다면
                s = (String)e.next(); // Object를 리턴하기 때문에 형변환 필요
                System.out.println(s);
            }
            System.out.println();
            
            for(String str : list) // Iterator interface를 개선한 for-each loop
                System.out.println(str); // 하나씩 꺼내서 뭘 하고 싶다면 for-each loop 사용
            System.out.println();
            
            System.out.println(list); // println을 사용한 출력 방법
        }
    
    }
    
---

    - LinkedList : 중간에 삽입, 삭제를 하고 싶다면
    
    import java.util.*;
    
    public class ArrayListTest {
    
        public static void main(String[] args) {
            LinkedList<String> list = new LinkedList<>();
            
            list.add("Milk");
            list.add("Bread");
            list.add("Butter");
            list.add(1, "Apple");
            list.set(2, "Grape");
            list.remove(3);
            list.add("Apple");
            
            for(int i = 0; i < list.size(); i++) // for loop을 이용한 출력
                System.out.println(list.get(i));
            System.out.println();
            
            Iterator<String> e = list.iterator(); // Iterator(반복자)를 이용한 출력
            String s;
                    // hashNext() : 아직 방문하지 않은 원소가 있으면 true 반환
                    // next() : 다음 원소를 반환
            while(e.hasNext()) { // Iterator의 e 다음 원소가 있다면
                s = (String)e.next(); // Object를 리턴하기 때문에 형변환 필요
                System.out.println(s);
            }
            System.out.println();
            
            for(String str : list) // Iterator interface를 개선한 for-each loop
                System.out.println(str); // 하나씩 꺼내서 뭘 하고 싶다면 for-each loop 사용
            System.out.println();
            
            System.out.println(list); // println을 사용한 출력 방법
        }
    
    }

---

    - Set : 원소의 중복을 허용하지 않는다
    
    1. HashSet
       : 해쉬 테이블에 원소를 저장하기 때문에 성능에서 가장 우수하다. 하지만 원소들의 순서가 일정하지 않는다.
    2. TreeSet
       : 레드 블랙 트리에 원소를 저장한다. 따라서 값에 따라서 순서가 결정되지만 느리다.(알파벳 순서)
    3. LinkedHashSet
       : 해쉬 테이블과 연결 리스트를 합한 것으로 원소들의 순서는 삽입되었던 순서와 같다.
    
    
    // Set은 list와 유사하지만 순서는 상관없고 동일한 원소를 가질 수 없다
    
    import java.util.HashSet; // hashCode()에 의한 순서
    import java.util.TreeSet; // alphabet 순서
    import java.util.LinkedHashSet; // 입력순서
    
    public class SetTest {
        public static void main(String[] args) {
            LinkedHashSet<String> set = new LinkedHashSet<>(); // 입력 순서에 의한 순서
            
            set.add("Milk");
            set.add("Bread");
            set.add("Butter");
            set.add("Cheese");
            set.add("Ham");
            set.add("Ham"); // 모든 set은 중복을 허용하지 않는다
            set.remove("Cheese");
            System.out.println(set);
            System.out.println(set.size());
            
            String[] sample = { "Java", "is", "fun", "Is", "Java", "easy?" };
            
            for(int i = 0; i < sample.length; i++)
                if(!set.add(sample[i])) // 중복되는 데이터는 false 리턴
                    System.out.println(sample[i] + " is already in set.");
            System.out.println(set);
            
            System.out.println();
            for(String str : set)
                System.out.println(str);
        }
    
    }

---

    import java.util.*;
    
    public class Lotto {
    
        public static void main(String[] args) {
            Random rand = new Random();
            HashSet<Integer> set = new HashSet<>();
            int count = 0;
            int num;
            
            while(count < 6) {
                num = rand.nextInt(45) + 1;
                if(!set.add(num))
                    continue;
                count++;
            }
            System.out.println(set);
        }
    
    }

---

    import java.util.*;
    
    public class SetTest1 {
    
        public static void main(String[] args) {
            HashSet<String> s1 = new HashSet<>();
            HashSet<String> s2 = new HashSet<>();
            
            s1.add("A");
            s1.add("B");
            s1.add("C");
            
            s2.add("A");
            s2.add("D");
            // 원집합이 파괴되면 안되므로 union이라는 복사본 집합을 생성
            HashSet<String> union = new HashSet<>(s1); // s1과 같은 HashSet 객체 생성
            union.addAll(s2); // s1과 s2의 합집합
            System.out.println("합집합 : " + union);
            
            HashSet<String> intersection = new HashSet<>(s1); 
            intersection.retainAll(s2); // s1과 s2의 공통집합
            System.out.println("공통집합: " + intersection);
            
            System.out.println(s1.containsAll(s2)); // s2가 s1의 부분집합이면 true, 아니면 false
            s1.removeAll(s2); // s1과 s2의 차집합                                                                       
            System.out.println("차집합: " + s1);
        }
    
    }

---

    큐는 먼저 들어온 데이터가 먼저 나가는 구조
    
    // LinkedList 클래스는 Queue 인터페이스를 구현한다
    // 따라서 Queue는 LinkedList를 사용하여 작성할 수 있다
    
    import java.util.LinkedList;
    
    public class QueueTest {
    
        public static void main(String[] args) {
            LinkedList<Integer> queue = new LinkedList<>();
            
            for(int i = 0; i <= 10; i++)
                queue.add(i);
            System.out.println(queue);
            
            queue.offer(11); // add와 동일
            System.out.println(queue);
            
            System.out.println(queue.element()); // 첫 번째 원소 리턴, 삭제하지 않는다
            System.out.println(queue);
            
            System.out.println(queue.peek()); // 첫 번째 원소 리턴, 삭제하지 않는다
            System.out.println(queue);
            
            System.out.println(queue.remove()); // 첫 번째 원소 리턴, 삭제한다
            System.out.println(queue);
            
            System.out.println(queue.poll()); // 첫 번째 원소 리턴, 삭제한다
            System.out.println(queue);
            
            System.out.println(queue.get(2)); // 번호에 있는 원소 리턴, 삭제하지 않늗다
            System.out.println("size : " + queue.size());
        }
    }
    
    //           Exception 발생     null, true, false 리턴
    // insert  :  add(e)            off(e)
    // examine :  element()         peek()
    // remove  :  remove()          poll()

---

    - 우선순위 큐
    
    import java.util.PriorityQueue;
    
    // PriorityQueue는 크기 순서로 저장되지는 않지만
    // 가장 작은 원소를 맨 앞으로 보내 작은 순서대로 remove
    public class PriorityQueueTest {
    
        public static void main(String[] args) {
            PriorityQueue<Integer> pq = new PriorityQueue<>();
            
            pq.add(80);
            pq.add(30);
            pq.add(50);
            pq.add(20);
            System.out.println(pq); // 가장 작은 원소를 처음으로 보낸다
            
            System.out.println(pq.remove());
            System.out.println(pq); // 왜 순서가 바뀌는건가?? 뒤에 있는 원소들은 순서 상관 없다
            
            while(!pq.isEmpty())
                System.out.println(pq.poll());
        }
    }

---

    import java.util.*;
    
    class Prisoner implements Comparable<Prisoner> {
        String name;
        int year;
        public Prisoner(String n, int y) {
            name = n;
            year = y;
        }
        
        public int compareTo(Prisoner p) {
            if(year > p.year)
                return 1;
            else if(year < p.year)
                return -1;
            else
                return 0;
        }
    }
    
    public class PriorityQueueTest {
    
        public static void main(String[] args) {
            PriorityQueue<Prisoner> q = new PriorityQueue<>();
            // 우선순위가 결정되는 것은 사용자가 만든 compareTo에 따라 정할 수 있다
            q.add(new Prisoner("james", 5));
            q.add(new Prisoner("schofield", 99));
            q.add(new Prisoner("alex", 14));
            q.add(new Prisoner("silvia", 10));
            q.add(new Prisoner("thomson", 1));
            
            System.out.println("==== 출옥 순서");
            while(!q.isEmpty()) {
                Prisoner p = q.poll();
                System.out.println(p.name);
            }
        }
    }

---

    - map
     : 키에 값이 매핑된다
    
    // Map 인터페이스를 구현한 클래스는 HashMap, TreeMap, LinkedHashMap이 있다
    // Map은 Key와 value를 갖는다
    
    import java.util.*;
    
    public class MapTest {
    
        public static void main(String[] args) {
            HashMap<Integer, String> map = new HashMap<>(); // hashcode에 의한 정렬
            
            map.put(2, "two");
            map.put(3, "three");
            map.put(1, "one");
            map.put(4, "four");
            map.put(1, "하나"); // key값이 같으면 overwrite, 이전에 있던 키 값의 value가 사라짐
            System.out.println(map.size());
            System.out.println(map.isEmpty());
            System.out.println(map.containsKey(1));
            System.out.println(map);
            
            map.remove(2);
            System.out.println(map);
            map.put(5, "five");
            System.out.println(map);
            System.out.println(map.get(3)); // value return, 없으면 null return
            
            for(Integer key : map.keySet()) { // key값에 대한 set 작성
                System.out.println(key + ", " + map.get(key));
            }
            // Map.Entry : key와 value의 값을 따로 따로 가지고 오고 싶을 때 -> getKey(), getValue()
            for(Map.Entry<Integer, String> s : map.entrySet()) { // key, value의 set
                Integer key = s.getKey();
                String value = s.getValue();
                System.out.println("Key = " + key + ", Value = " + value);
            }
        }
    
    }

---

    Collections 클래스
    
    1. max(), min() : 리스트에서 최대값과 최소값
    2. reverse() : 리스트의 원소들의 순서를 반대로
    3. fill() : 지정된 값으로 리스트를 채운다
    4. copy() : 목적 리스트와 소스 리스트를 받아서 소스를 목적지로 복사한다
    5. swap() : 리스트의 지정된 위치의 원소들을 서로 바꾼다
    6. addAll() : 컬렉션 안의 지정된 모든 원소들을 추가한다
    7. frequency() : 지정된 컬렉션에서 지정된 원소가 얼마나 많이 등장하는 지를 반환한다
    8. disjoint() : 두 개의 컬렉션이 겹치지 않는지를 검사한다

    import java.util.ArrayList;
    import java.util.Random;
    
    public class RandomList<T> {
        private T data;
        private ArrayList<T> alist = new ArrayList<>();
        private Random random = new Random();
        
        public void add(T item) {
            alist.add(item);
        }
        
        public T select() {
            int num = random.nextInt(alist.size());
            return alist.get(num);
        }
        
        public ArrayList<T> getList() {
            return alist;
        }
        
        public static void main(String[] args) {
            RandomList<Integer> list = new RandomList<>();
            for(int i = 0; i < 10; i++)
                list.add(i);
            System.out.println(list.getList());
            for(int i = 0; i < 10; i++)
                System.out.println(list.select());
        }
    }

---
    
    import java.util.ArrayList;
    import java.util.Collections;
    import java.util.Random;
    
    public class ArrayListTest {
        public static void main(String[] args) {
            ArrayList<Double> list = new ArrayList<>();
            Random rand = new Random();
            for(int i = 0; i < 10; i++)
    //			list.add(rand.nextDouble() * 10);
                list.add((double)(rand.nextInt(11)));
            
            for(Double d : list)
                System.out.println(d);
            
            Collections.sort(list);
            list.remove(0); // 제일 최하위 인덱스
            list.remove(8); // 제일 최상위 인덱스 why? 8인가.. 앞에서 0번방을 없애주었기 때문에
            System.out.println(list);
            
            double sum = 0.0;
            for(int i = 0; i < list.size(); i++)
                sum += list.get(i);
            System.out.printf("%.2f\n", sum);
            
            System.out.println(Collections.max(list));
            System.out.println(Collections.min(list));
        }
    }

---
    
    import java.util.HashMap;
    
    class MyPair<K, V> {
        private HashMap<K, V> hm;
        
        public MyPair() {
            hm = new HashMap<>();
        }
        
        public void put(K k, V v) {
            hm.put(k, v);
        }
        
        public V get(K i) {
            return hm.get(i);
        }
        
        public String toString() {
            return hm.toString();
        }
    }
    
    public class MyPairTest {
        public static void main(String[] args) {
            MyPair<Integer, String> pair = new MyPair<>();
            pair.put(1, "one");
            pair.put(2, "Two");
            System.out.println(pair.get(2));
            System.out.println(pair);
        }
    }
    
---

    - 스트림은 순서가 있는 데이터의 연속적인 흐름이다.
    - 스트림들은 연결될 수 있다.
    - 입출력 스트림:  1. 바이트 스트림 - 입/출력(8비트) (InputStream/OutputStream)
                    2. 문자 스트림 - 입/출력 (Reader/Writer)
    - 바이트 스트림
    
     // File 입출력을 바이트 단위로 출력하는 경우
    /*
    import java.io.FileInputStream;
    import java.io.FileOutputStream;
    import java.io.IOException;
    import java.io.FileNotFoundException;
    // import java.io.*; // 앞으로 이런식으로 쓰자
    
    public class FileCopy1 {
        public static void main(String[] args) throws IOException { // main에 IOException을 해주지 않으면 각 줄마다 예외처리를 해야함
            FileInputStream in = null;
            FileOutputStream out = null;
            
            System.out.println(System.getProperty("file.encoding")); // 텍스트 encoding 방식
                                                                     // 내 PC가 어떤 코딩 방식을 쓰는지 출력
            try {                                                    // MS949(한글 확장판)
                in = new FileInputStream("input.txt"); // FileNotFoundException
                out = new FileOutputStream("output.txt"); // append mode 설정은?(true)
                int c; // 하위 8비트만 사용
                while((c = in.read()) != -1) {      // read()는 int return, eof면 -1 반환
                    out.write(c);                   // read(), write()은 IOException
                    System.out.print((char)c);      // int(4바이트)를 char로 반환(안그러면 나머지 3바이트가 쓰레기값이 들어가짐)
                }
            } catch(FileNotFoundException e) {
                System.out.println("File open error");
            } finally {
                if(in != null)
                    in.close();
                if(out != null)  // IOException
                    out.close();
            }
        }
    }
    
    // 입력 파일은 없으면 IOException이 발생
    // 출력 파일은 없으면 자동 생성
    // 한글을 입력하면 콘솔 창에서 깨진다 -> byte 배열을 이용하여 해결하자
    */
    
    // byte 배열을 사용한 한글 입출력
    import java.io.*; 
    
    public class FileCopy1 {
        public static void main(String[] args) throws IOException { 
            FileInputStream in = null;
            FileOutputStream out = null;
            
            System.out.println(System.getProperty("file.encoding"));
                                                             
            try {                                            
                in = new FileInputStream("input.txt"); 
                out = new FileOutputStream("output.txt"); 
                int c, i = 0; 
                            byte[] buffer = new byte[100]; // byte라는 배열을 만들어서 모든 string을 집어넣자
                
                while((c = in.read()) != -1) {      
                    out.write(c);           
                    buffer[i++] = (byte)c;
                }
            } catch(FileNotFoundException e) {
                System.out.println("File open error");
            } finally {
                if(in != null)
                    in.close();
                if(out != null)  // IOException
                    out.close();
            }
        }
    }

---

    - 문자 스트림
    
    // File 입출력을 문자 스트림으로 처리하는 경우
    
    import java.io.*; // File I/O 문자 스트림
    
    public class FileCopy2 {
        public static void main(String[] args) throws IOException { 
            FileReader in = null;
            FileWriter out = null;
                                                             
            try {                                            
                in = new FileReader("input.txt"); // FileNotFoundException
                out = new FileWriter("output.txt"); 
                int c; // 하위 16 bit만 사용
                
                while((c = in.read()) != -1) {  // read()는 int return, 하위 16 bit 사용
                    out.write(c);               // read(), write() 는  IOException
                    System.out.print((char)c);     // 형변환 필요
                }
            } catch(FileNotFoundException e) {
                System.out.println(e.getMessage());
            } finally {
                if(in != null)
                    in.close();
                if(out != null)  // IOException
                    out.close();
            }
        }
    }

---

    - 스트림은 연결될 수 있다
    - 바이트 스트림과 문자 스트림을 연결하는 방법
    
    // 조합 stream
    import java.io.*;
    
    public class InputStreamReaderTest {
    
        public static void main(String[] args) throws IOException {
            InputStreamReader in = null;
            OutputStreamWriter out = null;
                                                             
            try {            
                // byte stream은 문자 스트림으로 변환하는 객체 생성(생성자에 byte stream 객체)
                // 문자 스트림들을 byte stream으로 변환하는 객체 생성(생성자에 byte stream 객체)
                in = new InputStreamReader(new FileInputStream("input.txt")); 
                out = new OutputStreamWriter(new FileOutputStream("output.txt")); 
                int c; 
                
                while((c = in.read()) != -1) {  
                    out.write(c);              
                    System.out.print((char)c);    
                }
            } catch(FileNotFoundException e) {
                System.out.println(e.getMessage());
            } finally {
                if(in != null)
                    in.close();
                if(out != null)  
                    out.close();
            }
    
        }
    
    }

---

    - 줄단위의 입출력
    
    // 문자 stream을 BufferedReader, BufferedWriter와 같이 사용
    // Buffering을 이용해 입출력 효울을 높인다.
    // 버퍼가 없는 스트림을 버퍼가 있는 스트림으로 변경
    // -> new BufferedReader(new FileReader())
    // -> new BufferedWriter(new FileWriter())
    
    import java.io.*;
    
    public class CopyLines {
    
        public static void main(String[] args) throws IOException {
            BufferedReader in = null;
            BufferedWriter out = null;
            
            try {            
                in = new BufferedReader(new FileReader("input.txt"));
                out = new BufferedWriter(new FileWriter("output.txt")); 
                String str;
                
                while((str = in.readLine()) != null) {  // line 단위 입력, 줄바꿈 기호는 포함하지 않는다
                    out.write(str + "\n");            
                    System.out.println(str);
                }
            } catch(FileNotFoundException e) {
                System.out.println(e.getMessage());
            } finally {
                if(in != null)
                    in.close();
                if(out != null)  // IOException
                    out.close();
            }
    
        }
    
    }

---

    - 토큰 분리
    
    // 파일에서 문자열을 읽고 token으로 분리
    // Scanner sc = new Scanner(System.in); 표준 입력
    // Scanner 클래스는 공백 문자(공백, 탭, 줄바꿈 문자 등)을 이용하여 각각의 토큰을 분리한다
    
    import java.io.*;
    import java.util.Scanner;
    
    public class ScanTest {
        public static void main(String[] args) throws IOException {
            Scanner sc = null;
            BufferedWriter out = null;
            String token;
            
            try {
                sc = new Scanner(new BufferedReader(new FileReader("input.txt")));
                out = new BufferedWriter(new FileWriter("output.txt"));
                while(sc.hasNext()) {
                    token = sc.next();
                    System.out.println(token);
                    out.write(token +  "\n");
                }
            } catch(FileNotFoundException e) {
                System.out.println(e.getMessage());
            } finally {
                if(sc != null)
                    sc.close();
                if(out != null)
                    out.close();
            }
        }
    
    }

---

    // Scanner 클래스보다 Tokenizer 클래스가 더 효율적임
    
    import java.io.*;
    import java.util.Scanner;
    import java.util.StringTokenizer;
    
    public class ScanTest {
        public static void main(String[] args) throws IOException {
            StringTokenizer t = new StringTokenizer(null);
            BufferedReader r = null;
            BufferedWriter w = null;
            String str, token;
            
            try {
                r = new BufferedReader(new FileReader("input.txt"));
                w = new BufferedWriter(new FileWriter("output.txt"));
                
                while((str = r.readLine()) != null) {
                        while(t.hasMoreTokens()) {
                            token = t.nextToken();
                            System.out.println(token);
                            w.write(token +  "\n");
                    }
                }
            } catch(FileNotFoundException e) {
                System.out.println(e.getMessage());
            } finally {
                if(r != null)
                    r.close();
                if(w != null)
                    w.close();
            }
        }
    }

---

    - Arguments
    - 외부에서 입력 가능
    
    public class CommandLineArgument {
    
        public static void main(String[] args) {
            for(int i = 0; i < args.length; i++) {
                System.out.println(args[i]);
            }
            
            int n1 = Integer.parseInt(args[0]); // String -> int
            int n2 = Integer.parseInt(args[1]);
            String str = String.valueOf(n1 + n2); // int -> String
            
            // 상단 메뉴 -> Run -> Run Configuration -> Arguments 탭 -> 10 20 입력(',' 사용 안됨 / 공백으로 사용)
            System.out.println(args[0] + " + " + args[1] + " = " + str);
        }
    }

---

    - DataStream
    
    // DataInputStream, DataOutputStream은 byte stream만 지원
    // byte 단위 io가 아니고 data type 단위의 io
    // 출력 순서대로 입력해야함
    
    import java.io.*;
    
    public class DataStreamTest {
    
        public static void main(String[] args) {
            DataInputStream in = null; 
            DataOutputStream out = null;
            
            try {
                out = new DataOutputStream(new BufferedOutputStream(new FileOutputStream("data.bin")));
                // data.bin 파일에 바이트 단위 파일 출력 스트림 개설 ->
                // 바이트 단위 출력 스트림을 버퍼링 ->
                // 바이트 단위 출력을 데이터 형 단위 출력으로 변환
                
                // type에 따라 write하는 메소드가 다르다
                out.writeDouble(3.14);
                out.writeInt(100);
                out.writeChar('c');
                out.writeBoolean(true);
                out.writeUTF("test data");
                System.out.println(out.size()); // 현재까지 출력된 데이터의 바이트 수
                out.flush(); // buffer에 남아 있는 데이터 강제 출력 // C에서 fflush()와 같음
                
                in = new DataInputStream(new BufferedInputStream(new FileInputStream("data.bin")));
                // 읽어오는 순서는 파일에 쓴 순서대로 적어줘야 한다
                // 일일이 써준다는 단점이 있지만 타입별로 적고 싶다면 이것이 가장 효율적이다
                System.out.println(in.readDouble());
                System.out.println(in.readInt());
                System.out.println(in.readChar());
                System.out.println(in.readBoolean());
                System.out.println(in.readUTF());
                if(out != null)
                    out.close();
                if(in != null)
                    in.close();
            } catch(IOException e) { // IOException을 써주어야 한다 -> main에서 throw를 이용해도 가능
                System.out.println(e.getMessage());
            }
        }
    
    }

---

    // 객체 단위 입출력을 하기 위한
    // 클래스는 Serializable interface를  구현해야 한다
    // file에 출력하는 경우를 직렬화(serialization)
    // (객체의 상태를 바이트 스트림으로 변환하는 것)
    // file에서 읽는 경우를 역직렬화(deserialization)
    // 따라서 byte stream 객체를 생성해야 한다
    
    import java.io.*;
    import java.util.*;
    
    class Student implements Serializable {
        // serialVersionUID를 쓰지 않으면 클래스 옆에 오류가 나와서 generate serial를 눌러주면 생성된다
        // default serial은 1의 값이 생성된다
        private static final long serialVersionUID = 6012177974279663492L;
        private int id;
        private String name;
        
        public Student(int id, String name) {
            this.id = id;
            this.name = name;
        }
        
        public String toString() {
            return id + ", " + name;
        }
    }
    
    public class ObjectStream {
        public static void main(String[] args) throws IOException {
            ObjectInputStream in = null;
            ObjectOutputStream out = null;
            
            try {
                // serialization(직렬화)
                out = new ObjectOutputStream(new FileOutputStream("object.dat"));
                out.writeObject(new Student(1, "Hong")); // IOException
                out.writeObject(new Student(2, "Kim"));
                out.writeObject(new Student(3, "Choi"));
                out.flush();
                
                // deserialization(역직렬화)
                in = new ObjectInputStream(new FileInputStream("object.dat"));
                System.out.println((Student)in.readObject());
                System.out.println((Student)in.readObject());
                System.out.println((Student)in.readObject());
                if(in != null)
                    in.close();
                if(out != null)
                    out.close();
            } catch(ClassNotFoundException e) { // 구제척인 예외는 대처할 수 있는 방법을 써주지만
                                                // IOException은 불확실하기 때문에 운영체제에 넘긴다
                System.out.println("ClassNotFoundException");
            }
        }
    }

---

    // 실전용
    // 여러 개의 객체를 읽고 싶을 때
    
    import java.io.*;
    import java.util.*;
    
    class Student implements Serializable {
        private static final long serialVersionUID = 6012177974279663492L;
        private int id;
        private String name;
        
        public Student(int id, String name) {
            this.id = id;
            this.name = name;
        }
        
        public String toString() {
            return id + ", " + name;
        }
    }
    
    public class ObjectStream2 {
        public static void main(String[] args) throws IOException {
            ObjectInputStream in = null;
            ObjectOutputStream out = null;
            
            try {
                out = new ObjectOutputStream(new FileOutputStream("object.dat"));
                
                ArrayList<Student> list = new ArrayList<>();
                
                list.add(new Student(1, "일지매"));
                list.add(new Student(2, "이지매"));
                list.add(new Student(3, "삼지매"));
                
                out.writeObject(list);
                out.flush();
                
                in = new ObjectInputStream(new FileInputStream("object.dat"));
                
                ArrayList<Student> list2 = (ArrayList<Student>)in.readObject();
                // ArrayList에 있는 모든 것을 화면에 출력하는 반복문
                for(Student s : list2)
                    System.out.println(s);
                
                if(in != null)
                    in.close();
                if(out != null)
                    out.close();
            } catch(ClassNotFoundException e) { 
                System.out.println("ClassNotFoundException");
            }
        }
    }

---

    import java.io.*;
    import java.util.*;
    
    public class FileTest {
        public static void main(String[] args) throws IOException {
            File file = new File("object.dat");
            System.out.println("Name : " + file.getName());
            System.out.println("Parent : " + file.getParent());
            System.out.println("Absoute path : " + file.getAbsolutePath());
            System.out.println("Is directory : " + file.isDirectory());
            System.out.println("Ins file : " + file.isFile());
            System.out.println("Length : " + file.length());
            file.deleteOnExit();
        }
    }

---

    - 임의 접근 파일
    
    // 지금까지는 순차 접근 파일(Sequential access file)
    // 임의 접근 파일(Random access file) : file pointer를 사용하여
    // 파일 임의 위치에 접근 가능
    
    import java.io.*;
    
    public class RandomAccessFileTest {
        public static void main(String[] args) throws IOException {
            RandomAccessFile inout;
            
            try {
                inout = new RandomAccessFile("inout.bin", "rw"); // 읽고 쓰기 모드
                 
                inout.setLength(0); // file 길이 설정
                for(int i = 0; i < 10; i++)
                    inout.writeInt(i); // 타입별로 쓸 수 있음(int는 4바이트이므로 4바이트씩 증가)
                                       // 파일 포인터의 마지막 위치는 40번지
                System.out.println("File length : " + inout.length()); // 파일 길이
                System.out.println(inout.getFilePointer()); // 현재 파일 포인터 위치
                
                inout.seek(0); // 파일 포인터 처음으로 이동
                // 파일 포인터가 4바이트의 메모리를 읽고 나면
                // 4바이트 뒤에 위치하게 된다
                // 따라서 처음 위치에서 4바이트 이동하게 된다
                System.out.println("number : " + inout.readInt()); 
                System.out.println(inout.getFilePointer()); // 현재 파일 포인터 위치
    
                inout.skipBytes(4); // 4 byte를 이동(number = 1 skip)
                System.out.println("number : " + inout.readInt());
                System.out.println(inout.getFilePointer()); // 현재 파일 포인터 위치
                
                inout.seek(5 * 4); // 6번째로 이동
                System.out.println(inout.getFilePointer()); // 현재 파일 포인터 위치
                System.out.println("number : " + inout.readInt());
                
                inout.writeInt(555); // 7번째 변경
                inout.seek(inout.length()); // 파일 끝으로 이동
                
                inout.writeInt(999); // 마지막에 추가
                System.out.println("New length : " + inout.length()); // 길이 변경 확인
                
                inout.seek(10 * 4);
                System.out.println("number : " + inout.readInt());
                
                inout.seek(0);
                for(int i = 0; i < inout.length(); i += 4)
                    System.out.println(inout.readInt());
                inout.close();
            } catch(FileNotFoundException e) {
                System.out.println(e.getMessage());
            }
        }
    }

---

    - zip 파일은 스킵!
    
    import java.io.*;
    import java.util.*;
    
    public class FileSort {
        public static void main(String[] args) {
            int value;
            ArrayList<Integer> list = new ArrayList<>();
            BufferedReader reader; // 입력
            PrintWriter writer; // 출력
            
            try { // 원래는 클래스를 만들어서 각각의 메소드를 설정해주어야 한다(reader, writer)
                reader = new BufferedReader(new FileReader("input.txt"));
                String line = reader.readLine();
                while(line != null) {
                    value = Integer.parseInt(line);
                    list.add(value);
                    line = reader.readLine();
                }
                Collections.sort(list);
                
                writer = new PrintWriter(new FileWriter("output.txt"));
                int num;
                for(int i = 0; i < list.size(); i++) {
                    num = list.get(i);
                    writer.write(num + "\n");
                    System.out.println(num);
                }
                reader.close();
                writer.close();
            } catch(IOException e) {
                System.out.println(e.getMessage());
            }
        }
    
    }
