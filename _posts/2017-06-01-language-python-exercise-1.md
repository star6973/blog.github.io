---
layout: post
title: "Python Programming [Exercise 1]"
date: 2017-06-01 09:00:00-18:00:00
categories: Language
tag: Language
---

## [문제 1번] 파운드를 킬로그램으로 변환하기
```python
# 파운드의 변수를 선언 후 eval(input())을 이용하여 콘솔에서 값을 입력받는다.
pound = eval(input("파운드 값을 입력하세요:"))

#킬로그램은 파운드의 변수를 이용하여 구할 수 있다.
kilo = pound * 0.454

print(pound , "파운드는" , kilo , "킬로그램입니다")
```

## [문제 2번] 정수의 자릿수 합산하기
```python
# 0부터 100사이의 숫자를 입력 받기 위한 변수 num을 생성하여 eval(input)을 이용하여 값을 입력받는다.
num = eval(input("0과 1000 사이의 숫자를 입력하세요:"))

# 백의 자리는 num을 100으로 나눈 몫이다.
hundred = num // 100

# 십의 자리는 num을 100으로 나눈 나머지를 다시 10으로 나눈 몫이다.
ten = (num % 100) // 10

#일의 자리는 num을 num을 100으로 나눈 나머지를 다시 10으로 나눈 나머지이다.
one = (num % 100) % 10

#주어진 문제의 결과는 각 자릿수의 합이므로 백의 자리, 십의 자리, 일의 자리를 합한다.
result = hundred + ten + one

print("각 자릿수의 합은 ",result,"입니다")
```

## [문제 3번] 년과 일수 계산하기
```python
# 분을 년과 일수로 만들기에 minute이라는 '분'을 나타내는 변수를 생성하여 입력 받는다.
minute = eval(input("분에 대한 숫자를 입력하세요:"))

#분을 연으로 만들기 위해선 먼저 한 시간으로 만들고 이 한 시간을 하루로 만들며 다시 이 하루를 1년으로 만들어 준다.
year = ((minute // 60) // 24) // 365

#마찬가지로 분을 일로 만들기 위해선 위에처럼 하는데 이때 1년으로 나눈 나머지가 되어야 된다.  
day = ((minute // 60) // 24) % 365

print(minute,"분은 약",year,"년",day,"일입니다.")
```

## [문제 4번] 기하: 삼각형의 넓이
```python
# sqrt함수를 쓰기 위해 math라는 모듈을 import 시킨다.
import math

#세 꼭지점을 입력 받기 위해 변수를 (x1,y1),(x2,y2),(x3,y3) 로 받는다.
x1,y1,x2,y2,x3,y3 = eval(input("삼각형의 세 꼭지점을 입력하세요:"))

#세 변의 길이는 math 모듈의 sqrt함수를 이용해서 구할 수 있다. 세 변을 각각 a,b,c로 잡는다.
a = math.sqrt((x2 - x3) * (x2 - x3) + (y2 - y3) * (y2 - y3))
b = math.sqrt((x1 - x3) * (x1 - x3) + (y1 - y3) * (y1 - y3))
c = math.sqrt((x1 - x2) * (x1 - x2) + (y1 - y2) * (y1 - y2))

#삼각형의 넓이를 구하기 위해 s라는 변수를 생성하여 위에서 구한 a,b,c를 이용한다.
s = (a + b + c)/2

#삼각형의 넓이는 s를 이용하여 다음과 같은 공식을 사용하여 구한다.
area = math.sqrt(s * (s - a) * (s - b) * (s - c))

#출력할 때 삼각형의 넓이를 소수점 한 자리만 보여주기 위해서 format함수를 이용하였다.
print("삼각형의 넓이는",format(area,".1f"),"입니다.")
```

## [문제 5번] 현재 시간
```python
# time함수를 사용하기 위해 time 모듈을 이용한다.
import time

# 현재 시간
currentTime = time.time() 

# 1970년 1월 1일 자정 이후로의 전체 초 값
totalSeconds = int(currentTime)

# 현재 시간의 초 값
currentSecond = totalSeconds % 60

# 전체 분 값
totalMinutes = totalSeconds // 60

# 현재 시간의 분 값
currentMinute = totalMinutes % 60

# 전체 시 값
totalHours = totalMinutes // 60 

# 현재 시간의 시 값
currentHour = totalHours % 24

#GMT와 시간대 차이를 입력 받는다.
dis = eval(input("GMT 와 시간대 차이를 입력하세요:"))

#입력된 차이 시간대를 GMT와 더해서 현재 시간을 구한다. 
PresentHour = currentHour + dis

#차이는 시간만 계산한다.
print("현재 시간은 "+str(PresentHour)+":"+str(currentMinute)+":"+str(currentSecond)+" 입니다.")
```

## [문제 6번] 금융 어플리케이션: 이자 계산하기
```python
# 잔고와 연이율의 변수를 입력 받는다.
charge, percent = eval(input("잔고와 연이율을 입력하세요:"))

#이자는 다음과 같은 공식으로 구할 수 있다.
interest = charge * (percent / 1200)

#이자를 소수점 2자리까지 나타내기 위해 int형으로 설정해주고 100.00으로 나눈다.
print("이자는",int(interest * 100000) / 100.00,"입니다")
```
