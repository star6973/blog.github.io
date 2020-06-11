---
layout: post
title: "Python Programming [Exercise 6]"
date: 2017-06-06 09:00:00-18:00:00
categories: Language
tag: Language
---

## [문제 1번] 
#### (*Fan* 클래스) 팬을 표현하는 *Fan* 클래스를 설계하시오. *Fan* 클래스는 다음을 포함한다.
> 팬의 속도를 나타내는 각각의 값이 1, 2, 3 인 세 상수 *SLOW*, *MEDIUM* 과 *FAST*
> 팬의 속도를 명시하는 private *int* 데이터 필드 *speed*
> 팬의 전원이 켜져 있는지 나타내는 private *bool* 데이터 필드 *on* (기본 값 *False*)
> 팬의 반지름(크기)을 나타내는 private *float* 데이터 필드 *radius*
> 팬의 색상을 나타내는 private *string* 데이터 필드 *color*
> 모든 데이터 필드에 대한 접근자와 변경자
> 특정 속도(기본값 *SLOW*), 반지름(기본값 *5*), 색상(기본값 *blue*), 그리고 전원(기본값 *False*)에 대한 선풍기를 생성하는 생성자

#### Fan 클래스에 대한 UML 다이어그램을 작성한 후 클래스를 구현하시오. 두 개의 Fan 객체를 생성하는 테스트 프로그램을 작성하시오. 첫 번째 객체에 대해 최대 속도, 크기 *10*, 색상 노란색 그리고 전원 켜짐을 할당하시오. 두 번째 객체에 대해 중간 속도, 크기 *5*, 색상 파란색 그리고 전원은 꺼짐을 할당하시오. 각 객체의 속도, 반지름, 색상 및 전원 속성을 출력하시오.
```python
# 팬의 속도를 나타내는 speed는 SLOW = 1, MEDIUM = 2, FAST = 3인 세 상수를 가지고 있으므로
# 이를 전역변수로 선언해준다
SLOW = 1 
MEDIUM = 2
FAST = 3

class Fan:
    global SLOW, MEDIUM, FAST # 클래스 내에서도 전역변수처럼 사용하기 위한 global 예약어 
    def __init__(self, speed = SLOW, radius = 5, color = "blue", on = False): 
        # 속도 기본값 SLOW, 반지름 기본값 5, 색상 기본값 blue, 전원 기본값 False인 디폴트 생성자  
        if speed == SLOW: # speed가 SLOW이면
            self.__speed = 1 # 값을 1을 반환
        elif speed == MEDIUM: # speed가 MEDIUM이면
            self.__speed = 2 # 값을 2를 반환
        elif speed == FAST: # speed가 FAST이면
            self.__speed = 3 # 값을 3을 반환
        self.__radius = radius 
        self.__color = color 
        self.__on = on 
    def setSpeed(self, speed): # speed 변경자
        self.__speed = speed 
    def getSpeed(self): # speed 접근자
        return self.__speed
    def setRadius(self, radius): # radius 변경자
        self.__radius = radius
    def getRadius(self): # radius 접근자
        return self.__radius
    def setColor(self, color): # color 변경자
        self.__color = color
    def getColor(self): # color 접근자
        return self.__color
    def setOn(self, on): # on 설정자
        self.__on = on
    def getOn(self): # on 접근자
        return self.__on
    def PrintInfo(self): # Fan 클래스의 속성 출력 메소드
        print("속도: ", self.__speed)
        print("반지름: ", format(self.__radius, ".1f"))
        print("색상: ", self.__color)
        print("전원 속성: ", self.__on)

        
Fan1 = Fan(FAST, 10, "yellow", True)
Fan2 = Fan(MEDIUM, 5, "blue", False)

Fan1.PrintInfo()
print()
Fan2.PrintInfo()
```

## [문제 2번] 
#### (스톱워치) *StopWatch* 클래스를 설계하시오. 클래스는 다음을 포함한다. 
> Private 데이터 필드 *startTime*과 *endTime* 및 get 메소드
> 현재시간으로 *startTime*을 초기화하는 생성자
> 현재시간으로 *startTime*을 재설정하는 *start()* 메소드
> 현재시간을 *endTime*으로 설정하는 *stop()* 메소드
> 스톱워치의 경과시간을 밀리초 단위로 반환하는 *getElapsedTime()* 메소드

#### StopWatch 클래스의 UML 다이어그램을 작성한 후 클래스를 구현하시오. 1부터 1,000,000 까지 숫자를 더하는 데 소요되는 실행 시간을 측정하는 테스트 프로그램을 작성하시오.  
```python
import time # time 함수를 사용하기 위해 import해준다

class StopWatch:
    def __init__(self): # startTime을 초기화하는 생성자
        self.__startTime = time.time()
    def start(self, startTime): # startTime을 재설정하는 start() 메소드
        self.__startTime = startTime
    def getStart(self): # startTime을 반환하는 get() 메소드
        return self.__startTime
    def stop(self, endTime): # endTime을 설정하는 stop() 메소드
        self.__endTime = endTime
    def getStop(self): # endTime을 반환하는 get() 메소드
        return self.__endTime
    def getElapsedTime(self): # 스톱워치의 경과시간을 밀리초 단위로 반환하는 메소드
        return (self.__endTime - self.__startTime) * 1000 # 밀리초이므로 1000을 곱해준다

watch = StopWatch() # StopWatch 클래스의 객체 생성
watch.start(time.time()) # 1부터 1,000,000 더하기 시작 
sum = 0
for i in range(1, 1000001):
    sum += i
watch.stop(time.time()) # 1부터 1,000,000 더하기 끝

print("경과시간:", watch.getElapsedTime())
```

## [문제 3번]
#### (*Time* 클래스) *Time* 라는 이름의 클래스를 설계하시오. 클래스는 다음을 포함한다.
> 시간을 표현하는 *hour, minute* 및 *second* private 데이터 필드
> 현재 시간을 사용하여 *hour, minute* 및 *second* 를 초기화하는 *Time* 객체를 생성하는 생성자 
> 각각의 *hour, minute* 및 *second* 데이터 필드에 대한 *get* 메소드
> 초 단위의 소요 시간을 사용하여 객체의 새로운 시간을 설정하는 *setTime(elapseTime)* 메소드, 예를 들어 소요 시간 *555550* 초는 *10* 시간, *19* 분, *10* 초이다.

#### *Time* 클래스에 대한 UML 다이어그램을 작성한 후 클래스를 구현하시오. *Time* 객체를 생성하는 테스트 프로그램을 작성하고 시, 분, 초를 출력하시오. 프로그램은 사용자로부터 소요 시간을 입력 받고 *Time* 객체에 입력 받은 소요 시간을 설정하고 시, 분, 초를 출력한다. 실행 예는 다음과 같다.

    현재 시간은 12:41:6 입니다.
    소요 시간을 입력하세요: 55550505 (enter)
    소요 시간의 시:분:초는 22:41:45 입니다.
    (힌트: time 모듈의 time() 함수는 1970년 1월 1일 자정 이후 현재까지 소요 시간을 초 단위의 float 데이터로 반환한다. 소요 시간을 시:분:초로 변환한 위의 예는 UTC (그리니치 표준시) 기준으로 표현된 것이다. 이를 위해 time 모듈에 정의된 함수를 활용할 수 있다.)

```python
import time

class Time:
    def __init__(self): # 현재 시간을 사용하여 초기화하는 생성자
        self.__hour = int(((time.time() / 60) / 60) % 24)
        self.__minute = int((time.time() / 60) % 60)
        self.__second = int(time.time() % 60)
    def getHour(self): # hour의 get() 메소드
        return self.__hour
    def getMinute(self): # minute의 get() 메소드
        return self.__minute
    def getSecond(self): # second의 get() 메소드
        return self.__second
    def setTime(self, elapseTime): # 초 단위의 소요 시간을 사용하여 새로운 시간 설정 메소드
        self.__hour += int((((elapseTime) // 60) // 60) % 24) 
        if self.__hour > 24:
            self.__hour %= 24
        self.__minute += int((elapseTime // 60) % 60)
        if self.__minute > 60:
            self.__minute %= 60
        self.__second += int(elapseTime % 60)
        if self.__second > 60:
            self.__second %= 60

t = Time()
print("현재 시간은 " + str(t.getHour()) + ":" + str(t.getMinute()) + ":" + str(t.getSecond()) + " 입니다.")
timer = eval(input("소요 시간을 입력하세요:"))
t.setTime(timer)
print("현재 시간은 " + str(t.getHour()) + ":" + str(t.getMinute()) + ":" + str(t.getSecond()) + " 입니다.")
```

## [문제 4번]
#### (대수학: 2차 방정식) 2차 방정식 𝑎𝑥2 + 𝑏𝑥 + 𝑐 = 0에 대한 *QuadraticEquation* 클래스를 설계하시오. 클래스는 다음과 같이 구성된다.
> 세 개의 계수를 표현하는 private 데이터 필드 *a, b, c*
> *a, b, c*에 대한 인자를 받는 생성자
> *a, b, c*에 대한 세 개의 *get* 메소드
> 판별식 𝑏2 − 4𝑎𝑐를 반환하는 *getDiscriminant()* 메소드 
> 다음 공식을 사용하여 방정식의 두 해를 반환하기 위한 *getRoot1()* 과 *getRoot2()* 메소드

#### 두 메소드는 오직 판별식이 음수가 아닐 때만 유효하다. 판별식이 음수이면 두 메소드는 0을 반환한다. 
#### QuadraticEquation 클래스의 UML 다이어그램을 작성한 후 클래스를 구현하시오. 
#### 사용자로부터 a, b와 c의 값을 입력받고 판별식에 기반하여 결과를 출력하는 테스트 프로그램을 작성하시오. 판별식이 양수이면 두 해를 출력하시오. 판별식이 0이면, 하나의 해를 출력하시오. 그렇지 않으면 "이 방정식은 해가 없습니다."를 출력하시오.
```python
import math # math.sqrt() 함수를 사용하기 위해 math 모듈 import

class QuadraticEquation:
    def __init__(self, a, b, c): # a,b,c를 매개변수로 받는 생성자
        self.__a = a
        self.__b = b
        self.__c = c
    def getA(self): # 데이터 필드 a 접근자
        return self.__a
    def getB(self): # 데이터 필드 b 접근자
        return self.__b
    def getC(self): # 데이터 필드 c 접근자
        return self.__c
    def getDiscriminant(self): # 판별식 반환 메소드
        return pow(self.__b, 2) - 4 * self.__a * self.__c
    def getRoot1(self, dis): # 두 해 중 하나의 해를 반환하는 메소드, dis라는 매개변수를 사용, 이 변수는 판별식이 들어온다
        r1 = (-self.__b + math.sqrt(dis)) / (2 * self.__a) # 공식 사용
        if dis < 0: # 판별식이 음수이면
            return 0 # 0으로 반환
        else: # 0 또는 양수이면 
            return r1 # 해를 반환
    def getRoot2(self, dis): # 두 해 중 나머지 하나의 해를 반환하는 메소드
        r2 = (-self.__b - math.sqrt(dis)) / (2 * self.__a) # 공식 사용
        if dis < 0: # 판별식이 음수이면 
            return 0 # 0으로 반환
        else: # 0 또는 양수이면
            return r2 # 해를 반환

a, b, c = eval(input("a,b,c 값 입력: "))
equation = QuadraticEquation(a, b, c)
discriminant = equation.getDiscriminant()
if discriminant > 0: # 만약 판별식이 양수이면 서로 다른 두 해를 출력
    print("서로 다른 두 해는:", equation.getRoot1(discriminant),",",equation.getRoot2(discriminant))
elif discriminant == 0: # 만약 판별식이 0이면 하나의 중근을 출력
    print("중근은:", equation.getRoot1(discriminant))
else: # 만약 판별식이 음수이면 해가 없음을 출력
    print("이 방정식은 해가 없습니다.")
```
