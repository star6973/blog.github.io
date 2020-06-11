---
layout: post
title: "Python Programming [Exercise 5]"
date: 2017-06-05 09:00:00-18:00:00
categories: Language
tag: Language
---

## [문제 1번] 
#### (0과 1의 행렬 출력하기) n * n 행렬을 출력하는 함수를 작성하시오. 함수 헤더는 다음과 같다.
#### def printMatrix(n):
#### 각 원소는 0이나 1의 값을 가지며 랜덤하게 생성된다. 사용자로부터 n을 입력받고 n * n 행렬을 출력하는 테스트 프로그램을 작성하시오.
```python
import random # 행렬의 값을 랜덤하게 생성해주어야 하므로 

n = eval(input("n을 입력하세요:")) # 행렬의 n값을 입력받는다
print() # 실행 예와 같게 해주기 위한 한 행을 띄어쓰기

def printMatrix(n):  
    for i in range(n): # 행렬의 n행만큼의 개수 출력
        for j in range(n): # 행렬의 n열만큼의 개수 출력
            num = random.randint(0,1) # 0이나 1의 값을 출력하기 위해 num을 randint()를 이용하여 난수 발생해준다
            print(num, end =' ') # 다음 행으로 바로 넘어가지않고 띄어쓰기를 해주고 열의 값이 끝나면 넘어간다
        print('\n') # 행을 구분하기 위한 띄어쓰기

printMatrix(n) # 함수 호출
```

## [문제 2번] 
#### (수소) 수소(emirp, 영문 prime의 역순)는 비대칭 소수(nonpalindromic prime number)이고 이 소수의 역순도 소수이다. 
#### 예를 들어, 17과 71은 소수인 동시에 수소이다. 최초 100개의 수소를 출력하는 프로그램을 작성하시오. 한 행당 10개의 수소를 출력하고 다음과 같이 적절히 정렬하시오.
```python
PerLine = 0 # 개행을 위해 수소의 개수를 세주는 변수
printPerLine = 10 # 10개씩 수소를 출력해야 하므로 수소의 개수를 세주는 변수를 나눠주어 10의 배수로 출력하는 역할

# 소수를 판별해주는 함수
def Prime(n): 
    cnt = 0 # 1부터 자기자신까지 나눠줄 때 나누어 떨어지는 개수를 세주는 변수 
    for j in range(1,n+1): # n을 1부터 자기자신까지 나누어준다 
        if n % j == 0: # 만약 나누었을 때 나누어 떨어진다면
            cnt += 1 # cnt의 값을 1만큼 증가시켜준다
            
    if cnt == 2: # 소수는 1과 자기자신일 때만 나뉘어 떨어지므로 cnt는 2가 되어야한다
        return True # cnt가 2이므로 True를 반환
    else: # 이외의 값은 False를 반환
        return False

# num의 값을 문자열 형태로 만들어서 거꾸로 바꿔주는 함수    
def Check1(num): 
    return str(num)[ : :-1] # 리스트의 [ : : -1]을 이용하여 역순으로 반환해준다

# num의 값이 그대로도 소수이고 거꾸로해도 소수인지 판별해주는 함수(feat. Prime함수, Check함수)
def Check2(num):
    if Prime(num) and Prime(int(Check1(num))): # 만약 Prime함수에 num을 넣었을 때 True이며
        return True                            # Check1함수에 num을 넣어 숫자를 문자열 형태로 거꾸로 출력하고 
    else:                                      # 다시 숫자 형태로 바꿔준 숫자를 Prime함수에 넣었을 때 True인 경우
        return False
    
# num이 회문인지 아닌지 판별해주는 함수
def Check3(num):     
    if str(num) == str(num)[ : :-1]: # 문자열이 그대로인 것과 역순으로 만들어준 것이 같다면
        return False # 회문이므로 False를 반환
    else:
        return True # 회문이 아니므로 True를 반환
        
# i의 값이 10부터 10000이하의 수 일 경우(수소인 소수는 일의 자리 수는 성립되지 않기 때문에 십의 자리 수부터 시작한다)
for i in range(10, 10000):
    if Check2(i) and Check3(i): # Check2함수와 Check3함수를 동시에 만족한다면
        PerLine += 1 # PerLine의 값을 1만큼 증가시킨다
        print(format(i, ">5d"), end = ' ') # 가독성을 위해 format을 이용해 정렬하고, end를 이용하여 칸을 띄어주었다 
        if (PerLine % printPerLine == 0): # 만약 PerLine이 printPerLine으로 나누어 떨어지면 10의 배수이므로
            if PerLine == 100: # 만약 PerLine이 100이면 
                break # 100개의 수소만 출력해준다
            print(end = "\n\n") # 한 행을 띄어준다
```

## [문제 3번]
#### (Rectangle 클래스) Circle 클래스 예제를 참조하여 사각형을 표현하는 Rectangle이라는 클래스를 구현하시오. Rectangle 클래스는 다음을 포함한다.
> 두 데이터 필드 : width와 height

#### 특정 폭(width)과 높이(height)를 갖는 사각형을 생성하는 생성자, 폭과 높이의 기본값은 각각 1과 2이다.

> 사각형의 넓이를 반환하는 getArea() 메소드
  사각형의 둘레를 반환하는 getPerimeter() 메소드

#### 하나는 폭 4, 높이 10, 다른 하나는 폭 3.5, 높이 35.7인 두 사각형을 생성하는 테스트 프로그램을 작성하시오. 각 사각형의 폭, 높이, 넓이 및 둘레 순으로 출력하시오.
```python
class Rectangle: # Rectangle 클래스
    def __init__(self, width = 1, height = 2): # Rectangle 클래스의 생성자
                                               # 디폴트 값으로 width = 1, height = 2
        self.width = width # self.width의 self는 이 클래스의 객체를 가리킴
        self.height = height
    def getWidth(self): # 폭을 호출하는 메소드
        return self.width
    def getHeight(self): # 높이를 호출하는 메소드
        return self.height
    def getArea(self): # 넓이를 호출하는 메소드
        return format(self.width * self.height, ".2f") # format함수를 이용하여 부동소수점 두 자리까지만 호출
    def getPerimeter(self): # 둘레를 호출하는 메소드
        return format(2 * self.width * self.height, ".2f") # 마찬가지로 format함수를 이용하여 부동소수점 두 자리까지만 호출
    def printRectangleInfo(self): # 생성된 객체의 폭, 높이, 넓이, 둘레의 정보를 보여주는 메소드
        print("폭:", self.width)
        print("높이:", self.height)
        print("넓이:", self.getArea())
        print("둘레:", self.getPerimeter())

rec1 = Rectangle(4, 10) # rec1이라는 객체 생성(폭은 4, 높이는 10)
rec2 = Rectangle(3.5, 35.7) # rec2라는 객체 생성(폭은 3.5, 높이는 35.7)

rec1.printRectangleInfo() # rec1 객체의 정보 호출
print() # 가독성을 위한 띄어쓰기
rec2.printRectangleInfo() # rec2 객체의 정보 호출
```
