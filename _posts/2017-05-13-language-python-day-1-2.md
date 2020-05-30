---
layout: post
title: "Python Programming [Day 1_2]"
date: 2017-05-13 13:00:00-18:00:00
categories: Language
tag: Language
use_math: true
---

# Python 4일만에 뽀개기 - Day 1_2

    1. 객체
       : 유일한 식별자, 상태 ,행동을 포함
       : 객체의 상태(data field) = 인스턴스 변수
       : 객체의 행동(method) = 인스턴스 메소드
    
    2. 클래스
       : 클래스 내부에는 인스턴스 변수와 메소드 정의 -> 클래스의 멤버
       : 인스턴스 변수 생성 - self.변수
       : self - 객체 자신을 가리키는 변수, 
                클래스 정의에 포함된 객체의 멤버에 접근 가능
                생성자, 변수, 함수 모두 다 써주자
    
    3. 생성자
       : 객체가 생성될 때 객체를 기본값으로 초기화하는 특수한 메소드 
       : __init__()
       : 파이썬에서 인스턴스 변수가 생성되는 곳
       : 객체가 생성될 때 생성자가 자동 호출
       : 파이썬에서는 클래스 당 하나의 생성자만 허용
       : 기본 인자값을 사용하는 매개변수를 이용할 수 있음
    
    4. UML 클래스 다이어그램
       : 데이터 필드(멤버 변수) - 데이터 필드 이름(데이터 필드 타입)
       : 생성자 - 클래스 이름(매개변수 이름 : 매개변수 타입)
       : 메소드 - 메소드 이름(매개변수 이름 : 매개변수 타입) : 반환 타입
       : 클래스를 이용하는 방법을 알려줌 
         - 객체를 어떻게 생성하고 메소드를 어떻게 호출하는지 설명
    
    5. 정보 은닉
       : 인스턴스 변수의 값이 올바르지 않게 변경됨, 클래스 관리하고 유지보수하기가 어려움
         -> 정보 은닉 필요
       : 외부에서 객체의 인스턴스 변수를 직접 접근할 수 없도록 하는 것
       : private('__' 추가, 클래스 내부에서만 접근 가능)
       : 본인이 작성한 프로그램에서만 내부적으로 사용되는 클래스의 경우,
         반드시 필요한 것은 아님
    
    6. 접근자와 설정자
       : 접근자 - 인스턴스 변수값을 반환하는 메소드
       : 설정자 - 인스턴스 변수값을 설정하는 메소드
       : 사용 이유
         - 나중에 클래스를 수정할 때 용이함
         - 설정자에서 매개변수를 통해서 잘못된 값이 넘어오는 경우, 
           이를 차단할 수 있음
         - 필요할 때마다 인스턴스 변수값을 계산하여 반환할 수 있음
         - 어떤 변수에 대해 접근자만 제공하면 자동으로 읽기만 가능한 인스턴스
           변수를 만드는 것이 됨
    
    7. 캡슐화
       : 데이터와 메소드를 하나의 객체로 묶고 사용자로부터 
         데이터 필드와 메소드 구현을 감추는 것
       : 사용자는 공개된 인터페이스(메소드)가 무엇이고 그것을 어떻게 사용하는지
         알면 내부 구현 내용을 알지 못해도 객체를 사용할 수 있음
    
    8. 객체지향 프로그래밍
       : 객체와 객체의 연산에 초점
       : 데이터와 행위(메소드)를 객체에 결합
       : 객체의 속성과 행동이 연결되어 있는 실세계를 반영하는 방식으로 프로그램을 구성
    
       <-> 절차적 프로그래밍 
           : 행동/행위 기반 방식 -> 함수 설계에 초점
           : 데이터와 행위가 분리
           : 데이터를 함수에 전달하도록 요구함
    
       => 객체의 사용은 소프트웨어의 재사용성을 높이고, 프로그램 개발과 유지를 쉽게 만듦
    
    ---------------------------------------------------------------------------------------------
    class BMI:
        def __init__(self, name, age, weight, height):
            self.__name = name
            self.__age = age
            self.__weight = weight
            self.__height = height
        
        def getBMI(self):
            KILOGRAMS_PER_POUND = 0.45359237
            METERS_PER_INCH = 0.0254
            bmi = self.__weight * KILOGRAMS_PER_POUND / \
              ((self.__height * METERS_PER_INCH) * \
               (self.__height * METERS_PER_INCH))
            return round(bmi * 100) / 100
        
        def getStatus(self):
            bmi = self.getBMI()
            if bmi < 18.5:
                return "Underweight"
            elif bmi < 25:
                return "Normal"
            elif bmi < 30:
                return "Overweight"
            else:
                return "Obese"
            
        def getName(self):
            return self.__name
        
        def getAge(self):
            return self.__age
        
        def getWeight(self):
            return self.__weight
        
        def getHeight(self):
            return self.__height
        
                                          
    def main():
        bmi1 = BMI("John Doe", 18, 145, 70)
        print("The BMI for", bmi1.getName(), "is", bmi1.getBMI(), bmi1.getStatus())
        
        bmi2 = BMI("Peter King", 50, 215, 70)
        print("The BMI for", bmi2.getName(), "is", bmi2.getBMI(), bmi2.getStatus())
        
    main()
    ---------------------------------------------------------------------------------------------
    9. str 클래스
       : 파이썬에서 문자열 처리를 위해 사용하는 클래스
       : 문자열은 str 클래스의 객체
       : s = "hello" # 문자열의 값을 직접 할당하는 방식
       : s = str("hello") # 문자열 객체 생성
       : 변경 불가능 객체 - 문자열 객체는 변경 불가능
         ex) s = "hello"
             s[0] = "t" (x)
       : 변경 불가능 객체(int, float, str, tuple)
    
    10. 문자열에 사용 가능한 내장 함수
        : len() - 문자열의 길이(문자열 내의 문자의 개수) 반환
        : max() - 문자열 내에서 가장 큰 문자(ASCII 코드 값 기준) 반환
        : min() - 문자열 내에서 가장 작은 문자(ASCII 코드 값 기준) 반환
    
    11. 인덱스 연산자
        : 음수 인덱스 - 문자열 끝을 기준으로 한 상대적 위치를 참조하는 인덱스
                      - 실제 위치 => 문자열 길이 + 음수 인덱스
                        ex) s[-1] = s[-1 + len(s)]
    
    12. 슬라이싱 연산자[start : end]
        : 문자열의 일부를 반환
        : start에서 end - 1까지 부분의 문자열
        : start나 end 인덱스는 생략 가능
        : start 기본값 - 0, end 기본값 - 마지막 인덱스
        : end가 start보다 작으면 빈 문자열
        : end > len(s)이면 end는 len(s)로 대치
          ex) s = "Programming"
              s[1:4] = 'rog'
              s[ :6] = 'Progra'
              s[1: ] = 'rogramming'
              s[4:2] = ''
              s[ :15] = 'Programming'
              s[-1:3] = '' # s[-1] = s[-1 + len(s)] = s[10]
    
    13. 슬라이싱 연산자[start : end : step]
        : 인덱스 start에서 end - 1까지 step 만큼 건너 뛰면서 문자 슬라이싱
        : step 기본값 1
          ex) s = "Programming"
              s[1:5:2] = 'rg'
              s[0:10:-1] = ''
              s[0:10:10] = 'P'
              s[10:0:-1] = 'gnimmargor'
    
    14. 연결 연산자(+), 반복 연산자(*)
        : 연결 연산자 - 두 개의 문자열을 연결
        : 반복 연산자 - 동일한 문자열을 여러 번 연결
    
    15. in, not in 연산자
        : 어떤 문자열이 다른 문자열 내에 있는지 검사
        : in 있으면 True
        : not in 없으면 True
     
    16. 문자열 비교하기
        : 비교 연산자를 이용하여 문자열 비교 가능
        : 문자열 내의 문자들을 하나씩 비교해 간다
    
    17. 문자열 반복하기
        : 문자열을 첫 인덱스부터 하나씩 접근하는 경우
          # for ch in s:
        : 문자열을 인덱스를 사용하여 순차적으로 접근하지 않는 경우
          # for i in range(0, len(s)):
    
    18. 문자열 검사하기
        : isalnum() - 문자열이 공백/특수 문자가 아닌 문자와 숫자로 되어 있고
                      최소한 하나의 문자가 있으면 True 반환
        : isalpha() - 문자열이 공백/특수 문자, 숫자가 아닌 문자로 되어 있고
                      최소한 하나의 문자가 있으면 True 반환
        : isdigit() - 문자열이 숫자 문자만 가지고 있으면 True 반환
        : islower() / isupper() - 문자열 내의 모든 문자가 소/대문자이고
                                  최소한 하나의 문자가 있으면 True 반환
                                - 숫자나 특수/한글 문자는 결과에 영향 없음
                                - 단, 숫자나 특수/한글 문자만 있으면 False
        : isspace() - 문자열이 공백 문자만 가지고 있으면 True 반환
                    - " ", "\n", "\t", "\r"
     
    19. 부분 문자열 검색하기
        : endswith(s1) - 어떤 문자열이 문자열 s1으로 끝나면 True 반환
        : startswith(s2) - 어떤 문자열이 문자열 s2로 시작하면 True 반환
        : find(s3) - 어떤 문자열 내에 문자열 s3가 있으면 s3가 있는 위치의 가장 작은 인덱스
                   - 왼쪽에서부터 검색
        : rfind(s4) - find와 동일한데, 오른쪽에서부터 검색
                    - 문자열 s4가 위치하는 가장 큰 인덱스
        : count(s5) - 어떤 문자열 내에 문자열 s5의 빈도수 반환
    
    20. 문자열 변환하기
        : capitalize() - 첫 문자만 대문자로 변환된 문자열의 복사본 반환
        : title() - 각 단어의 첫 문자가 대문자로 변환된 문자열의 복사본 반환
        : lower() / upper() - 모든 문자가 소/대문자로 변환된 문자열의 복사본 반환
        : swapcase() - 소문자는 대문자로 대문자는 소문자로 변환된 문자열의 복사본 반환
        : replace(old, new) - 부분 문자열 old를 new로 대체한 새로운 문자열 반환
    
    21. 공백 문자 제거하기
        : lstrip() - 왼쪽 공백 문자가 제거된 문자열 반환
        : rstrip() - 오른쪽 공백 문자가 제거된 문자열 반환
        : strip() - 왼쪽/오른쪽 모두 공백 문자가 제거된 문자열 반환
    
    22. 문자열 서식화하기
        : center(width) - 주어진 폭 영역에서 가운데 정렬된 문자열 반환
        : ljust(width) / rjust(width) - 주어진 폭 영역에서 왼쪽/오른쪽 정렬된 문자열 반환
        
    ---------------------------------------------------------------------------------------------
    def isPalindrome1(s):
        low = 0
        high = len(s) - 1
        
        while low < high:
            # 회문인지 아닌지 판별
            if s[low] != s[high]:
                return False
            
            low += 1
            high -= 1
        
        return True
    
    
    def isPalindrome2(s):
        if s == s[::-1]:
            return True
        else:
            return False
    ---------------------------------------------------------------------------------------------
    23. 연산자 오버로딩과 특수 메소드
        : 연산자에 대한 메소드를 정의하는 것
        : C++, C#, Python은 가능 / C, Java에서는 불가능
        : 시작과 끝에 두 개의 밑줄이 붙여짐
          ex) s1.__add__(s2) -> s1 + s2
              s1.__sub__(s2) -> s1 - s2
              s1.__mul__(s2) -> s1 * s2
              s1.__div__(s2) -> s1 / s2
              s1.__mod__(s2) -> s1 % s2
              s1.__It__(s2) -> s1 < s2
              s1.__le__(s2) -> s1 <= s2
              s1.__eq__(s2) -> s1 == s2
              s1.__ne__(s2) -> s1 != s2
              s1.__gt__(s2) -> s1 > s2
              s1.__ge__(s2) -> s1 >= s2
              s1.__getitem__(0) -> s1[0]
              s1.__contains__(value) -> value in s1
              s1.__len__() -> len(s1)
    
    24. list 클래스
        : 파이썬에서 연속적인 데이터를 저장하기 위해 사용하는 클래스
        : 가변 크기
        : 서로 다른 타입의 데이터를 한 리스트에 담을 수 있음
        : 리스트 생성
          ex) list1 = list() # 빈 리스트 생성
              list2 = list([2, 3, 4])
              list3 = list(range(3, 6))
              list4 = [] # 빈 리스트 생성
    
    25. list - 시퀀스 타입
        : str, list, tuple
        : 시퀀스 타입에 제공되는 공통 연산 기능
          -list 슬라이싱[start : end : step]
          -연결 연산자(+), 반복 연산자(*), in / not in
          -len, min, max, sum, random.shuffle
          -for loop(참조하는 인덱스가 범위를 넘어가지 않도록 해야함)
          -<, <=, >, >=, ==, !=(둘 모두 동일한 타입의 데이터이어야 함)
        : 리스트는 변경 가능
          cf) 변경 불가능 시퀀스 타입 : str, tuple
    
    26. list comprehension
        : 리스트 안에 표현식을 내포하여 그 표현식의 결과로 리스트를 만드는 것
        : [expression for item1 in iterable1 if condition1
                      for item2 in iterable2 if condition2
                                       ....               
                      for itemN in iterableN if conditionN]
    
    27. list 메소드
        : append(x) - 원소 x를 리스트의 끝에 추가
        : count(x) - 리스트 내에서 원소 x의 개수 반환
        : extend(list) - 리스트 lis의 모든 원소를 리스트에 추가
        : index(x) - 리스트에서 원소 x가 첫 번째로 나온 위치의 인덱스 값 반환
        : insert(ind, x) - ind에 해당하는 인덱스 위치에 원소 x를 삽입
        : pop(i) - 인덱스 위치 i에 있는 원소를 제거하고 그 원소를 반환, i가 없으면 마지막 원소
        : remove(x) - 리스트에서 원소 x를 제거, 두 개 이상 있다면 첫 번째로 나오는 원소 x를 제거
        : reverse() - 리스트 내의 원소를 역순으로 만듦
        : sort() - 리스트 내의 원소를 오름차순으로 정렬
    
    28. 문자열을 리스트로 분할 
        : str 클래스의 split() 메소드 이용
        : split() 메소드에 구분자를 넣어 줄 수 있음
          ex) nlist = names,split('/')
        ex) s = input("5개 숫자 입력:")
            items = s.split()
            lst = [eval(x) for x in items]
            lst = list(map(int, items))
    
    29. 리스트 복사
        ex1) list1 = [1, 2]
             list2 = list1
             list2 # [1, 2]
     
        ex2) list1 = list(range(1, 10, 2))
             list2 = list1
             list1[0] = 111
             print(list1, list2) # [111, 10, 2] [111, 10, 2]
    
        ex3) 다른 객체 만들기
             list1 = list(range(1, 10, 2))
             list2 = [] + list1
             list1[0] = 111
             print(list1, list2) # [111, 10, 2] [1, 10, 2]
    
    30. 함수에 리스트 전달하기
        ex) def main():
                x = 1
                y = [1, 2, 3]
        
                m(x, y)
        
                print("x is", x)
                print("y[0] is", y[0])
        
            def m(number, numbers):
                number = 1001
                numbers[0] = 5555
        
            main() # x is 1
                   # y[0] is 5555
    
    
    31. 리스트를 기본 인자로 사용
        ex) def add(x, lst = []):
                if x not in lst:
                    lst.append(x)
            
                return lst
    
            def main():
                list1 = add(1)
                print(list1) # [1]
        
                list2 = add(2)
                print(list2) # [1, 2]
        
                list3 = add(3, [11, 12, 13, 14]) # 새로운 리스트 할당
                print(list3) # [11, 12, 13, 14, 3]
        
                list4 = add(4)
                print(list4) # [1, 2, 4]
        
            main()
    
    32. 함수에서 리스트 반환하기 - 역순으로 출력하기
        ex) def reverse(lst):
                result = []
        
                for x in lst:
                    result.insert(0, x) # 인덱스 idx위치의 값을 뒤로 밀고 그 자리에 값을 넣는다
        
                return result
    
             list1 = [1, 2, 3, 4, 5, 6]
             list2 = [] + list1
             list2.reverse()
     
             print("list1: ", list1) # list1: [1, 2, 3, 4, 5, 6]
             print("reverse of list1: ", list2) # reverse of list1: [6, 5, 4, 3, 2, 1]
    ---------------------------------------------------------------------------------------------
    33. 문자 빈도수 세기
    
    # 0으로 초기화된 26개의 정수 리스트를 생성한다
    counts = 26 * [0]
    
    *********************************************************************************************
    #RandomCharacter
    
    from random import randint # randint를 임포트한다.
    
    # ch1과 ch2 사이의 랜덤 문자를 생성한다.
    def getRandomCharacter(ch1, ch2):
        return chr(randint(ord(ch1), ord(ch2)))
    
    # 랜덤 소문자를 생성한다.
    def getRandomLowerCaseLetter():
        return getRandomCharacter('a', 'z')
    
    # 랜덤 대문자를 생성한다.
    def getRandomUpperCaseLetter():
        return getRandomCharacter('A', 'Z')
    
    # 랜덤 숫자를 생성한다.
    def getRandomDigitCharacter():
        return getRandomCharacter('0', '9')
    
    # 랜덤 문자를 생성한다.
    def getRandomASCIICharacter():
        return getRandomCharacter(chr(0), chr(127))
    
    *********************************************************************************************
    import RandomCharacter
    
    def main():
        # 문자 리스트를 생성한다.
        chars = createList()
    
        # 리스트를 출력한다.
        print("소문자:")
        displayList(chars)
    
        # 각 문자의 빈도수를 센다.
        counts = countLetters(chars)
    
        # 빈도수를 출력한다.
        print("각 문자의 빈도수는:")
        displayCounts(counts)
    
    # 문자 리스트를 생성한다.
    def createList():
        # 빈 리스트를 생성한다.
        chars = []
    
        # 소문자를 랜덤하게 생성하고 리스트에 추가한다.
        for i in range(100):
            chars.append(RandomCharacter.getRandomLowerCaseLetter())
    
        # 리스트를 반환한다.
        return chars
    
    # 문자 리스트를 출력한다.
    def displayList(chars):
        # 리스트에 포함된 문자를 한 행에 20개씩 출력한다.
        for i in range(len(chars)):
            if (i + 1) % 20 == 0:
                print(chars[i])
            else:
                print(chars[i], end = ' ')
    
    # 각 문자의 빈도수를 센다.
    def countLetters(chars):
        # 0으로 초기화된 26개의 정수 리스트를 생성한다.
        counts = 26 * [0]
    
        # 리스트의 각 소문자를 센다.
        for i in range(len(chars)):
            counts[ord(chars[i]) - ord('a')] += 1
    
        return counts
    
    # 빈도수를 출력한다.
    def displayCounts(counts):
        for i in range(len(counts)):
            if (i + 1) % 10 == 0:
                print(counts[i], chr(i + ord('a')))
            else:
                print(counts[i], chr(i + ord('a')), end = ' ')
    
    main() # main 함수를 호출한다.
    ---------------------------------------------------------------------------------------------
    34. 리스트 검색하기
        : 선형 검색 - 찾고자 하는 원소(key)를 리스트의 각 원소와 순차적으로 비교한다
                    - key와 일치하는 원소를 리스트에서 찾을 때까지 혹은 일치된 원소가 
                      없이 리스트의 모든 원소를 전부 비교할 때까지 반복
                    - 일치하는 원소를 찾으면, 이 원소의 인덱스 반환
                    - 찾지 못하면 -1 반환
                    - 평균적으로 리스트 절반을 검색해야 함
                    - 검색 실행 시간은 리스트 원소의 개수가 증가할 수록 선형적으로 증가
                    - 큰 리스트의 경우 비효율적임
    
          ex) def linearSearch(lst, key):
                  for i in range(len(lst)):
                      if key == lst[i]:
                          return i
                         
                  return -1
     
        : 이진 검색 - 한 번에 리스트의 반씩 쪼개어 가면서 검색해야 하는 부분을 줄임
                    - 리스트의 원소가 정렬이 되어 있어야 함
         
          ex) def binarySearch(lst, key):
                  low = 0
                  high = len(lst) - 1
     
                  while high >= low:
                      mid = (low + high) // 2
                      if key < lst[mid]:
                          high = mid - 1
                      elif key == lst[mid]:
                          return mid
                      else:
                          low = mid + 1
                         
                  return low - 1 # 현재 high < low이므로, 키는 발견되지 않음       
     
    35. 리스트 정렬하기
        : 선택 정렬, 삽입 정렬, 오름차순, 내림차순
        : sort() 메소드 - 리스트 내의 원소를 오름차순 정렬
    
        : 선택 정렬 - 리스트 내에서 가장 작은 원소를 찾고 그것을 첫 번째 원소와 교환
                    - 남은 원소 중에서 가장 작은 원소를 찾고 그것을 남은 원소들 중에 
                      첫 번째 원소와 교환
                    - 이 과정을 한 개의 원소가 남을 때까지 반복
          ex) # 원소를 오름차순으로 정렬하기 위한 함수
              def selectionSort(lst):
                  for i in range(len(lst) - 1):
                  # lst[i : len(lst)]에서 가장 작은 원소를 찾는다.
                      currentMin, currentMinIndex = lst[i], i
                      for j in range(i + 1, len(lst)):
                          if currentMin > lst[j]:
                              currentMin, currentMinIndex = lst[j], j
                     
                          # 필요한 경우, lst[i]와 lst[currentMinIndex] 를 교환한다.
                          if currentMinIndex != i:
                              lst[currentMinIndex], lst[i] = lst[i], lst[currentMinIndex]
    
    
    36. 2차원 리스트 
        : 리스트의 원소가 또 다른 리스트로 이루어진 리스트
        : 랜덤 값으로 리스트 초기화
        ex) import random
            matrix = []
        
            numberOfRows = eval(input("행의 개수를 입력:"))
            numberOfCols = eval(input("열의 개수를 입력:"))
            for row in range(numberOfRows):
                matrix.append([])
                for col in range(numberOfCols):
                    matrix[row].append(random.randrange(0, 100))
              
            print(matrix)
         
        :리스트 출력하기
        ex) matrix = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
            for row in range(len(matrix)):
                for col in range(len(matrix[row]):
                    print(matrix[row][col], end = ' ')
                print()
    
    37. 모든 원소 합계 구하기
        ex1) matrix = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
             total = 0
             for row in range(len(matrix)):
                 for col in range(len(matrix[row]):
                     total += matrix[row][col]
    
        ex2) for row in range(len(matrix)):
                 total += sum(matrix[row])
    
    38. 합계가 가장 큰 행 찾기
        ex) for row in range(1, len(matrix)):
            if maxRow < sum(matrix[row]):
            indexOfMaxRow = row
            maxRow = sum(matrix[row])
    
    
    39. 파일 입출력
        : 파일 사용 - 프로그램에서 생성된 데이터를 영구적으로 저장하기 위함(비휘발성)
                    - 나중에 다른 프로그램에서 읽을 수 있음
        : 파일
          1. 텍스트 파일
             : 메모장
             : 텍스트 파일의 문자는 ASCII나 유니코드 같은 문자 인코딩 체계를 이용하여 인코딩
             : 파이썬 소스 프로그램은 텍스트 파일로 저장
          2. 바이너리 파일
             : 텍스트 파일이 아닌 모든 파일
             : MS 워드 파일은 바이너리 파일로 저장, MS 워드 프로그램에 의해 처리
    
    40. 파일 열기
        : open() 함수 이용
        : 파일 변수 = open(파일명, 모드)
          1. "r" : 파일을 읽기 용도로 연다.
          2. "w" : 새로운 파일을 쓰기 용도로 연다. 이미 파일이 존재하면 이전 내용은 모두 파괴된다.
          3. "a" : 파일의 끝에 데이터를 덧붙이기 용도로 연다.
          4. "rb" : 바이너리 데이터를 읽기 용도로 파일을 연다.
          5. "wb" : 바이너리 데이터를 쓰기 용도로 파일을 연다.
    
        : 파일 이름을 디렉토리 저장 위치를 나타내도 됨.
          open("C:\\pybook\\Scores.txt","r") 
          -> 이스케이프 시퀀스를 사용
          -> \\ : 문자열에서 역슬래시를 나타내는 이스케이프 시퀀스
    
        : 파일명을 나타내는 문자열 앞에 r을 붙여주면 문자열이 raw string(미가공 문자열)임을 
          표시하여 \가 이스케이프 시퀀스가 아닌 문자 그대로 역슬래시로 취급하도록 함
          open(r"C:\pybook\Scores.txt", "r")
    
        : open 함수 호출 결과
          -> _io.TextIOWrapper 클래스의 인스턴스인 파일 객체 생성
          -> 제공 메소드
             1. read([number:int]):str
             2. readline():str
             3. readlines():list
             4. write(s:str):int
             5. close():None
    
         : 데이터 쓰기 - write
           -> 파일이 이미 존재하면 덮어 쓰기가 된다
         : 파일 포인터 - 파일이 열리면 파일 포인터라는 특수 표시자가 파일 내부에 위치
                       - 이 포인터가 가리키는 위치에서 읽기와 쓰기 작업이 수행
                       - 파일이 열리면 파일 포인터는 파일의 시작 부분을 가리킴
         : 파일 존재 여부 검사
           -> os.path 모듈 사용
           -> os.path 모듈의 isfile() 함수
           -> import os.path
              if os.path.isfile("temp.txt"):
                  print("temp.txt 파일이 존재합니다")
          
         : 데이터 읽기
           1. read() : 특정 개수의 문자를 읽거나 파일의 모든 문자를 읽고 문자열로 반환
           2. readline() : 한 줄을 읽고 문자열로 반환
           3. readlines() : 모든 줄을 읽고 리스트 형태 반환
            
           *repr() 함수 : 미가공 문자열을 반환(줄바꿈 문자나 특수 문자 등등 같이 나옴)
           *파일에서 모든 데이터 읽기
            -> read() 메소드 사용 : 모든 데이터를 읽고 하나의 문자열로 반환
            -> readlines() 메소드 사용 : 모든 데이터를 읽고 문자열의 리스트로 반환
            -> 파일의 크기가 매우 커서 모든 내용이 메모리에 저장될 수 없다면?
               ex) countLines = 0
                   infile = open("Lectures.txt", "r")
                   for line in infile:
                       countLines += 1
                       print(line, end = "")
                   
         : 파일 복사하기
           ex) import os.path
               import sys
                  
               def main():
                   f1 = input("원본 파일을 입력하세요:").strip()
                   f2 = input("대상 파일을 입력하세요:").strip()
                          
                   if os.path.isfile(f2):
                       print(f2 + " 는 이미 존재하는 파일입니다")
                       sys.exit(1)
                      
                   infile = open(f1, "r")
                   outfile = open(f2, "w")
                             
                   countLines = countChars = 0
                             
                   for line in infile:
                       countLines += 1
                       countChars += len(line)
                       outfile.write(line)
                              
                   print(countLines, "개 행과", countChars, "개 문자가 복사")
                      
               infile.close()
               outfile.close()
                    
           main()
            
         : 데이터 추가하기 - "a"
                           - 기존 파일에 뒤쪽에 데이터를 추가
         : 숫자 데이터 쓰고 읽기
           -읽기 : 숫자를 문자열로 변환 후 파일에 쓴다
           -쓰기 : 문자열을 읽은 후에 eval() 함수를 통해 숫자로 변환한다
           ex) from random import randint
    
               def main():
                   outfile = open("Numbers.txt", "w")
                   for i in range(10):
                       outfile.write(str(randint(0, 9)) + " ")
                          
                   outfile.close()
    
                   # infile = open("Numbers.txt", "r")
                   # s = infile.read()
                   # numbers = [eval(x) for x in s.split()]
    
                   infile = open("Numbers.txt", "r")
                   numbers = list(map(int, infile.read().split()))
        
                   for number in numbers:
                       print(number, end  = " ")
                    
                   infile.close()
                     
               main()
               
         : 파일의 각 문자 별 문자 개수 세기
           ex) def main():
                   filename = input("파일명을 입력하세요:").strip()
                   infile = open(filename, "r")
    
                   counts = 26 * [0]
                   for line in infile:
                       countLetters(line.lower(), counts)
    
                   for i in range(len(counts)):
                       if counts[i] != 0:
                           print(chr(ord('a') + i) + ": " + str(counts[i])
                               + "번 나타납니다.")
                       
                   infile.close()
                       
               def countLetters(line, counts):
                   for ch in line:
                       if ch.isalpha():
                           counts[ord(ch) - ord('a')] += 1
                                                                                               
    
               main()
         
         : 웹에서 데이터 획득하기
           -urlopen() 함수 이용
            infile = urllib.request.urlopen("http://www.naver.com/index.html")
           -보통 HTTP를 이용하여 웹 서버에서 데이터를 읽어오는데 사용
           -파일과 유사한 객체를 반환함
           -read(), readline() : 파일을 open하고 read, readline 메소드를 호출하는 것과 비슷
            -> 차이점 1. 바이트 형식의 데이터를 반환(bytes 객체)
                      2. 일반 텍스트 문자열이 아닌 바이너리 문자열
           -info() : HTTP 응답 헤더와 같은 메타 정보를 반환
           -getcode() : HTTP 응답 코드를 반환
           -encode() / decode() 
            : decode()가 있어야 파일을 읽을 때, bytes형식의 데이터를 str형식의 데이터로 변환
            : decoding을 할 때는 encoding을 할 때 사용한 방법을 적용해야 함
            : 기본값은 utf-8 # 한글
           -str데이터 --------(encode)--------> bytes데이터
            str데이터 <--------(decode)-------- bytes데이터
         
    41. 예외 처리
        : try ~ except 문을 이용
        : 복수 예외 처리 - 여러 개의 예외 타입을 처리 할 수 있도록 한 개 이상의 except절 가능
        : else절 - 예외가 발생하지 않을 경우 실행되는 명령문
        : finally절 - 모든 상황에서 수행되는 마무리 기능을 정의하는 용도
    
    42. 예외 발생시키기
        : 예외 발생
          -함수가 오류를 감지하면, 함수는 적절한 예외 클래스로부터 객체를 생성하고 
           함수 호출자에게 예외를 전달
          ex) import math
          
              class Circle():
                  def __init__(self, radius):
                      super().__init__()
                      self.setRadius(radius)
    
                  def getRadius(self):
                      return self.__radius
     
                  def setRadius(self, radius):
                      if radius < 0:
                          raise RuntimeError("반지름이 음수")
     
                      else:
                          self.__radius = radius
    
                  def getArea(self):
                      return self.__radius * self.__radius * math.pi
    
                
              from CircleWithException import Circle
             
              try:
                  c1 = Circle(5)
                  print("c1의 넓이는", c1.getArea(), "입니다")
                  c2 = Circle(-5)
                  print("c2의 넓이는", c2.getArea(), "입니다")
    
              except RuntimeError as ex:
                  print("유효하지 않는 반지름", ex)
                  # ex는 RuntimeError 예외 객체를 ex라는 이름으로 참조하겠다는 의미
    
    43. 튜플
        : 리스트와 유사 - 시퀀스 타입 공통 연산 사용 가능
        : 차이점 - 튜플은 변경 불가능, 튜플은 ()로 표현
        : 튜플 생성
          t1 = ()
          t2 = (1, ) # 1개의 원소만 있을 때 원소 뒤에 콤마를 반드시 붙여야 함
          t3 = (1,2,3)
          t4 = 1,2,3 # 괄호를 생략해도 무방
          t5 = ('a','b',('ab','cd'))
          t6 = tuple([1,2,3]) # 리스트로부터 튜플 생성
          t7 = tuple([2 * x for x in range(1,5)])
          t8 = tuple("abcd")
        : 인덱스 연산은 가능
          ex) t3[0] = 1
    
    44. 세트
        : 집합과 관련된 처리를 쉽게 할 수 있다
          1. 중복을 허용하지 않는다
          2. 순서가 없다 -> 인덱싱 연산이 지원되지 않는다
             -> 리스트나 튜플은 순서가 있어서 인덱스를 통해 원소의 값을 얻는다
             -> 세트 자료형에 저장된 값을 인덱싱으로 접근하려면 리스트나 튜플로 변환 후 가능
        : {} 중괄호 이용
        : 교집합, 합집합, 차집합 구하기
        : 중복되는 데이터를 제거하기 위한 필터 역할로 사용 가능
        : 세트 생성
          s1 = set() # 빈 세트
          s2 = {1,3,5} 
          s3 = set((1,3,5)) # 튜플로부터 세트 생성
          s4 = set([x * 2 for x in range(1, 10)]) # 리스트로부터 세트를 생성
          s5 = set("abac") # 문자열로부터 세트 생성 s5 = {'a','b','c'}
        : 세트 다루기와 접근하기
          1. add/remove - 원소 추가/삭제
          2. update - 여러 개의 원소 추가
          3. len, min, max, sum
          4. for loop
          5. in, not in
        : 부분 세트와 상위 세트
          - 부분 세트 : s1의 모든 원소가 s2에 있으면 s1은 s2의 subset
            ex) s1 = {1, 2, 3}, s2 = {1, 2, 3, 4}
                s1.issubset(s2) # True
          - 상위 세트 : s1의 모든 원소가 s2에 있으면 s2는 s1의 superset
            ex) s1 = {1, 2, 3}, s2 = {1, 2, 3, 4}
                s2.issuperset(s1) # True
        : 동등 검사('==', '!=')
        : 비교 연산자('>', '>=', '<=', '<')
        : 합집합(union, |)
          ex) s1.union(s2) / s1 | s2
        : 교집합(intersection, &)
          ex) s1.intersection(s2) / s1 & s2
        : 차집합(difference, -)
          ex) s1.difference(s2) / s1 - s2
        : 대칭차(symmetric_difference, ^) - 공통 원소를 제외한 모든 원소를 포함하는 집합
          ex) s1.symmetric_difference(s2) / s1 ^ s2
        : 세트와 리스트의 성능 비교
          -> in, not in 연산자, remove 연산을 수행하는 데는 세트가 리스트보다 효율적
    ---------------------------------------------------------------------------------------------    
    **키워드 세기**
    
    import os.path
    import sys
    
    def main():
        keyWords = {"and", "as", "assert", "break", "class",
                    "continue", "def", "del", "elif", "else",
                    "except", "False", "finally", "for", "from",
                    "global", "if", "import", "in", "is", "lambda",
                    "None", "nonlocal", "not", "or", "pass", "raise",
                    "return", "True", "try", "while", "with", "yield"}
    
        filename = input("파이썬 소스 코드 파일명을 입력하세요: ").strip()
    
        if not os.path.isfile(filename):  # 파일이 존재하는지 검사한다.
            print("파일", filename, "이 존재하지 않습니다.")
            sys.exit()
     
        infile = open(filename, "r", encoding="utf-8") # 파일을 오픈한다.
     
        text = infile.read().split() # 파일을 읽고 단어 단위로 분리한다.
    
        count = 0
        for word in text:
            if word in keyWords:
                count += 1
     
        print(filename, "에", count, "개의 키워드가 포함되어 있습니다.")
     
    main()
    ---------------------------------------------------------------------------------------------
    45. 딕셔너리
        : 키/값의 쌍으로 이루어진 데이터를 저장하는 용도로 사용
          - 키는 인덱스 연산자와 유사
          - 각 키는 하나의 값에 매핑된다
          - 중복되는 키를 가질 수 없다
          - 키는 변하지 않는 값만 사용할 수 있음(리스트는 키로 사용 불가)
        : 딕셔너리 생성
          student = {} # 빈 딕셔너리 생성
          student = {"111-222" : "경석", "123-345" : "우성"}
          cf) {}은 빈 딕셔너리, set()는 빈 세트
        : 각 항목은 키, 콜론, 값의 순서
        : 항목은 콤마로 구분
        : 항목 추가/수정, 삭제
          ex) dic[key] = value 
              1. 존재하지 않는 key인 경우 추가
              2. 존재하는 key인 경우 수정
               
              del dic[key]
              1. 존재하는 key인 경우 해당 항목 삭제
              2. 존재하지 않는 key인 경우 KeyError 예외 발생
        : 항목 순회
          for key in dicA:
              print(key + ":" + str(dicA[key]))
        : len - 딕셔너리에 저장된 항목 수
        : in, not in - 딕셔너리에 특정 키가 존재하는지 검사
        : ==. != - 동일한 항목을 가지고 있는지 검사
          cf) 딕셔너리 내의 항목은 순서화 되어 있지 않기 때문에 비교 연산자 사용 불가능
              -> 인덱스로 접근하는 것이 아니라 key값으로 접근
              -> dicA[0] (x), dicA[key]
        : 딕셔너리 메소드
          1. keys() : tuple - 일련의 키들을 반환한다
          2. values() : tuple - 일련의 값들을 반환한다
          3. items() : tuple - 일련의 키/값 항목들을 반환한다
          4. clear() : None - 모든 항목들을 삭제한다
          5. get(key) : value - 키에 대한 값을 반환한다
          6. pop(key) : value - 키에 대한 항목을 삭제하고 그 항목의 값을 반환한다
          7. popitem() : tuple - 랜덤하게 선택된 키/값 쌍을 튜플 형태로 반환하고 선택된
                                 항목을 삭제한다.
    ---------------------------------------------------------------- 
    **단어의 빈도수 세기**
    
    def main():
        # 사용자로부터 파일명을 입력받는다.
        filename = input("파일명을 입력하세요: ").strip()
        infile = open(filename, "r", encoding='utf-8') # 파일을 연다.
    
        wordCounts = {} # 단어의 빈도수를 저장하기 위한 딕셔너리를 생성한다.
        for line in infile:
            processLine(line.lower(), wordCounts)
    
        pairs = list(wordCounts.items()) # 딕셔너리에서 쌍들을 얻어온다.
    
        items = [[x, y] for (y, x) in pairs] # 리스트 내의 쌍들을 역순으로 만든다.
    
        items.sort() # 항목을 정렬한다.
    
        for i in range(len(items) - 1, len(items) - 11, -1):
            print(items[i][1] + "\t" + str(items[i][0]))
     
    # 행 단위로 각각의 단어를 센다.
    def processLine(line, wordCounts):
        line = replacePunctuations(line) # 구두점을 공백으로 바꾼다.
        words = line.split() # 한 행에서 단어들을 가져온다.
        for word in words:
            if word in wordCounts:
                wordCounts[word] += 1
            else:
                wordCounts[word] = 1
    
    # 한 행에 있는 구두점을 공백으로 바꾼다.
    def replacePunctuations(line):
        for ch in line:
            if ch in "~@#$%^&*()_-+=~<>?/,.;:!{}[]|'\"":
                line = line.replace(ch, " ")
    
        return line
    
    main() # main 함수를 호출한다.
    ---------------------------------------------------------------------------------------------
    46. 상속
        : 객체지향 프로그래밍의 주요 특징
          1. 캡슐화 2. 상속 3. 다형성
        : 상속 - 기존 클래스의 속성과 행위를 이어 받는 새로운 클래스를 정의하는 것
               - 클래스 : 동일한 유현의 객체를 모델링 하기 위해 사용
                 1. 일반 클래스(슈퍼 클래스)
                    : 공통적인 속성과 행위를 하나의 클래스로 일반화하여 정의
                 2. 상세 클래스(서브 클래스)
                    : 이를 다른 클래스가 공유
               - 재사용성을 높임
        : 상속 문법
          ex) class Circle(GeometricObject): # class 서브클래스 이름(슈퍼클래스 이름):
        : 다중 상속 허용
          ex) class 서브 클래스 이름(슈퍼클래스1, 슈퍼클래스2, ...)
        : 상속시 슈퍼 클래스의 생성자 호출
          - 서브 클래스의 객체를 생성할 때 서브 클래스의 인스턴스 변수들은 서브 클래스의
            생성자에서 정의되고 초기화 됨
          - 서브 클래스의 init() 메소드에서 슈퍼 클래스의 init() 메소드를 호출해야 한다
            ex) super().__init__()
    47. 메소드 오버라이딩
        : 상속 받은 슈퍼 클래스의 메소드를 재정의하는 것
          - 메소드 이름, 매개 변수, 반환형은 동일해야 함
    48. Object 클래스
        : 파이썬에서 모든 클래스의 슈퍼 클래스가 되는 클래스
          - 클래스를 정의할 때 상속 관계를 명시하지 않으면 기본적으로 Object 클래스를 
            슈퍼클래스로 갖는다
        : Object 클래스의 메소드
          1. __new__() : 객체가 생성될 때 자동으로 호출됨
                       : 객체를 생성한 이후, 객체를 초기화 하기 위해 __init__() 메소드가 호출됨
          2. __init__() : 일반적으로 새로운 클래스에 정의된 데이터 필드를 초기화하기 위해 
                          이 메소드만 오버라이드 하면 된다
          3. __str__() : 객체에 대한 설명을 문자열로 반환
                       : 기본적으로 객체의 클래스 이름과 객체의 메모리 주소(16진수)로 
                         구성된 문자열
          4. __eq__() : 두 객체가 서로 동일하다면 True 반환
             ex) x.__eq__(y) # x와 y가 동일한 내용을 갖고 있어도 서로 다른 객체이므로 False
                 x.__eq__(x) # True
