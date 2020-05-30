---
layout: post
title: "Python Programming [Day 4]"
date: 2017-05-16 09:00:00-18:00:00
categories: Language
tag: Language
use_math: true
---

# Python 4일만에 뽀개기 - Day 4

```angular2
# 파이썬 함수 종류
'''
1) 내장함수 - 파이썬 설치시 자동으로 설치
    range() / len() / type() ...
2) 외장함수 : import 명령 필요
    - 파이썬 설치시 자동으로 설치
    - pip install 설치해야되는 함수
3) 사용자정의 함수
'''

# 수학 관련 내장함수
# abs(숫자형 자료형) : 절대값 함수
# min(문자열/리스트/튜플) : 최소값 반환
# max(문자열/리스트/튜플) : 최대값 반환
# divmod(숫자1, 숫자2) : 몫과 나머지값 반환. 튜플형태

print(abs(-100))
listA = [100, 5, 34, -90, 45]
txtA = '가12abcy화'
# 숫자 알파벳 한글
print(min(listA))
print(min(txtA))
print(max(listA))
print(max(txtA))
print(divmod(12, 5))
print('몫 = ', divmod(12, 5)[0])
print('나머지 = ', divmod(12, 5)[1])

# eval(문자열 계산식) => 계산값
# 입력받은 계산식의 값을 구하여라.
inData = input('계산식을 입력하세요...')
print(inData, '=', eval(inData))

# 유효성 검사와 관련된 내장함수 - 문자열변수/값.유효성 검사 함수()
# 문자열변수.isalpha() : 모두 문자(알파벳,한글..)인가? 숫자문자제외 , True/Fasle
# 문자열변수.isdigit() , 문자열변수.isnumeric() : 모두 숫자문자인가?  , True/Fasle
# 문자열변수.isalnum() : 문자열과 숫자로만 구성되어 있는가?
# islower(), isupper() : 대문자/ 소문자 검사
# isdecimal() : 모두 10진수인가?

txt1 = '1093725'
txt2 = '가나다word'
txt3 = 'an dndh'
print(txt1.isnumeric())
print(txt1.isalpha())
print(txt2.isalpha())
print(txt2.isdecimal())
print(txt3.isalnum()) # 공백이 포함되어 있기에 False
print(txt3.isupper())
print(txt3.islower())

# 문자열에서 숫자와 숫자가 아닌 문자의 갯수를 출력하여라.
txtB = 'Py00th1on2도777레100미'
# 숫자의 갯수를 저장하는 변수
digitCnt = 0
for i in txtB:
    if i.isdigit():
        digitCnt += 1
print('숫자문자의 갯수는? ', digitCnt)
print('숫자문자가 아닌 문자의 갯수는?', len(txtB) - digitCnt)

# 사용자정의 함수로 정의
def countText(word):
    cnt = 0
    for i in word:
        if i.isdigit():
            cnt += 1
        print('숫자문자의 갯수는? ', cnt)
        print('숫자문자가 아닌 문자의 갯수는?', len(word) - cnt)

countText('아름55다운34우리나라')
countText('파12이33썬ang은쉽5678다')

# filter()
# 리스트/튜플/집합 등에서 조건에 맞는 원소값만 리턴할 때 사용
# filter(사용자정의 함수명/람다함수, 리스트/튜플)
# => filter 객체 => 리스트화 list(filter 객체)
# 리스트에서 양수값만 추출
listB = [10, -90, 100, 5, 123, 0, -34]

# 일반적인 함수 이용
def positive1(myList):
    result = []
    for i in myList:
        if i > 0:
            result.append(i)
    return result
print(positive1(listB))

# filter 함수 이용
def positive2(n):
    return n > 0
print(list(filter(positive2, listB)))

# filter + lambda 함수 이용
print(list(filter(lambda x: x > 0, listB)))

# map()
# 리스트/튜플/집합 등에서 각 원소에 함수가 적용
# map(사용자정의 함수/람다함수, 리스트/튜플...)
# => map 객체 => 리스트화 list(map 객체)
# 리스트에서 3배 곱한값에서 1을 뺀 값으로 리스트를 새로 생성하여라.
listC = [5, 78, 23, 1, 89]

# 일반적인 함수 이용
def mapTest1(myList):
    result = []
    for m in myList:
        result.append(m*3-1)
    return result
print(mapTest1(listC))

# map 함수 이용
def mapTest2(x):
    return x*3-1
print(list(map(mapTest2, listC)))

# map + lambda 함수 이용
print(list(map(lambda x: 3*x-1, listC)))
```

---

```angular2
# 현재의 작업폴더 위치 알아보기
import os
print(os.getcwd())

# 파일 쓰기
'''
파일변수 = open(파일경로, 'w', encoding='utf-8'/'cp949')
파일변수.write(데이터)
파일변수.close()
'''

# 빈파일 만들기
f = open('data/test.txt', 'w', encoding='utf-8')
f.close()

# 파일에 데이터 쓰기
f = open('data/test.txt', 'w', encoding='utf-8')
f.write('파이썬 입출력 테스트')
# 1-10까지 쓰기
for i in range(1, 11):
    f.write('\n' + str(i) + '행 입니다.')
f.close()

# 기존 파일에 덮어쓰기가 되버림
# 리스트 요소를 파일에 쓰기
f = open('data/test.txt', 'w', encoding='utf-8')
f.write('파이썬 입출력 테스트2\n\n')
listA = ['사과', '수박', '귤', '오렌지', '바나나']

for i in listA:
    f.write('\t' + i)
f.close()


# 파일 내용 추가하기 - 기존 파일에 내용 추가
'''
파일변수 = open(파일경로, 'a', encoding='utf-8'/'cp949')
파일변수.write(데이터)
파일변수.close()
'''
f = open('data/test.txt', 'a', encoding='utf-8')
# 구구단 내용 추가하기
f.write('\n')
for i in range(2, 10):
    f.write(f'\n 3 x {i} = {3*i}')
f.close()

# 퀴즈 :구구단 전체를 test.txt에 내용 추가하여라.
f = open('data/test.txt', 'a', encoding='utf-8')
f.write('\n\n ----------------')
for i in range(2,10):
    f.write('-'*20)
    f.write('\n'+str(i)+'단 출력\n')
    for j in range(1,10):
        temp = '\n' + str(i) + ' X ' + str(j) + ' = ' + str(i*j)
        f.write(temp)
    f.write('\n')
f.close()
```

---

```angular2
# 파일 읽기
'''
파일변수 = open(파일경로, 'r', encoding='utf-8'/'cp949')
파일변수.read() => 문자열, 전체
파일변수.readline() => 문자열, 첫 행만
파일변수.readlines() => 리스트화
파일변수.close()
'''

f = open('data/coding.txt', 'r', encoding='utf-8')

# # 전체 읽기(문자열)
# data = f.read()
# print(data)

# # 첫 행 읽기(문자열)
# data = f.readline()
# print(data)

# 리스트로 읽기(문자열이 담긴 리스트)
data = f.readlines()
print(data[:2])

f.close()

# 파일을 읽은 후 데이터의 합과 평균 구하기
# data_eng.txt, data_kor.txt

def totAvr(url, encoding):
    f = open(url, 'r', encoding=encoding)
    dataList = f.readlines()
    dataList = list(map(lambda x: x.strip(), dataList)) # 개행 제거
    print(dataList)
    tot = 0
    for i in dataList:
        tot += int(i)

    print('총점 = ', tot)
    print('평균 = {:.2f}'.format(tot/len(dataList)))
    f.close()

totAvr('data/data_kor.txt', 'utf-8')
totAvr('data/data_eng.txt', 'utf-8')


# with문을 이용한 파일 입출력
# close()가 필요없음
'''
with open(url, 'w/r/a', encoding='utf-8/cp949') as 파일변수:
    파일변수.write(데이터)
    데이터변수 = 파일변수.read()
    데이터변수 = 파일변수.readline()
    데이터변수 = 파일변수.readlines()
'''

# 홀수만 쓰기
with open('data/text2.txt', 'w', encoding='utf-8') as f:
    for i in range(1, 50, 2):
        f.write(str(i) + '\n')

# 짝수만 내용추가
with open('data/text2.txt', 'a', encoding='utf-8') as f:
    f.write('-'*20 + '\n')
    for i in range(100, 151, 2):
        f.write(str(i) + '\n')

print('\n')
# 파일읽기
with open('data/color.txt', 'r', encoding='utf-8') as f:
    data = f.readlines()
    for i in data[:10]:
        print(i)

import os
# 파일 삭제하기
# os.remove(url)
ans = input('test.txt 파일을 삭제하시겠습니까? (y/n)')
if ans == 'y':
    os.remove('data/test.txt')
    print('파일 삭제 완료!')
```

---

```angular2
# 클래스 => 설계도
# 인스턴스 => 실제 사용
# 클래스 생성 문법
'''
class 클래스 이름:
    명령문

pass : 실행은 아니지만 명령문이 필요할 때 사용
'''

# 빈 클래스 만들기
class Test:
    pass

print(Test, type(Test))

# 인스턴스 생성하기
# 인스턴스명은 첫글자는 소문자로 지정
# 인스턴스변수 = 클래스명()
test1 = Test()
print(test1, type(test1))
test2 = 10

# 클래스와 인스턴스의 관계 확인하기
# isinstance(인스턴스명, 클래스명) => True / False
print(isinstance(test1, Test))
print(isinstance(test2, Test))

# 클래스 속성
# 예) 사각형 클래스 속성 - 가로, 세로, 색상, 크기
# 예) 붕어빵 속성 - 재료, 칼로리, 크기
# 생성자 함수를 이용한 클래스 속성 지정
'''
class 클래스명:
    def __init__(self, 인자):
        self.인자 = 인자값

# 속성이 있는 인스턴스 생성
인스턴스명 = 클래스(인자값, ...)
'''

# 사각형 클래스 정의
class Square:
    # 생성자 함수 => 속성을 정의하는 함수
    def __init__(self, width, height):
        self.width = width
        self.height = height
        self.color = 'red'
        self.pattern = ['도트', '줄무늬', '스트라이프']

    # 사각형의 정보를 출력하는 메소드 정의
    def info(self):
        print(f'가로 크기 {self.width}, 세로 크기 {self.height}, 색상 {self.color}')
        print('-' * 20)

    # 사각형의 넓이를 출력하는 메소드 정의
    def area(self):
        return self.width * self.height

# 인스턴스화
square1 = Square(100, 100)
square2 = Square(50, 200)

# 인스턴스가 해당 클래스에 의해 생성되었는지 확인
print(isinstance(square1, Square))
print(isinstance(square2, Square))

print(square1)
print(square2)
print()

# 인스턴스의 속성 접근
# 인스턴스명.속성
print(f'square1의 가로 크기 {square1.width}, 세로 크기 {square1.height}, '
      f'색상 {square1.color}, 무늬 {square1.pattern[0]}')
print(f'square2의 가로 크기 {square2.width}, 세로 크기 {square2.height}, 색상 {square2.color}')

# 인스턴스.메소드()로 호출하기
square1.info()
print(f'square1의 넓이는?', square1.area())

# 원의 이름, 색상, 반지름으로 구성된 클래스 정의
# 원의 정보와 원의 넓이를 구하는 메소드
class Circle:
    def __init__(self, cName, color, radius):
        self.cName = cName
        self.color = color
        self.radius = radius

    # 정보를 출력하는 메소드
    def info(self):
        print('원의 이름', self.cName)
        print('원의 색상', self.color)
        print('원의 반지름', self.radius)
        print('-' * 20)

    # 원의 넓이를 출력하는 메소드
    def area(self):
        print('원의 넓이는?', self.radius*self.radius*3.14)
        print('-' * 20)

# Circle 클래스를 이용하여 반지름 5, 10의 c1, c2 인스턴스 생성 후, 원의 정보와 넓이를 출력하여라.
c1 = Circle('c1', 'red', 5)
c2 = Circle('c2', 'blue', 10)

c1.info()
c1.area()
c2.info()
c2.area()

# 클래스 변수
'''
class 클래스명:
    # 공통 변수 = 클래스 변수
    변수명 = 값
    
    def __init__(self, ...):
        명령문
'''

# Bread 클래스 - 빵 종류, 가격, 칼로리, 브랜드(클래스 변수로 지정)
class Bread:
    # 클래스 변수
    brand = '파리바게뜨'

    # 생성자 함수
    def __init__(self, kind, price, kcal):
        self.kind = kind
        self.price = price
        self.kcal = kcal

    # 메소드
    def info(self):
        print(' 빵 종류 : {}'.format(self.kind))
        print(' 가격 : {}'.format(self.price))
        print(' 칼로리 : {}'.format(self.kcal))
        print('-'*20)

# 인스턴스화
b1 = Bread('소보루빵', 1000, 80)
b2 = Bread('크림빵', 1300, 100)
b1.info()
b2.info()

# 클래스변수 출력하기
print(f' 빵 브랜드는? {Bread.brand}')
print('\n')

# 상속
# Animal 클래스 - 부모
# Dog 클래스 - Animal 클래스 상속받음
# Cat 클래스 - Animal 클래스 상속받음
class Animal:
    def __init__(self, name, gender, age):
        self.name = name
        self.gender = gender
        self.age = age

    def run(self):
        print(f'{self.name}가 신나게 달리고 있다')

    def eat(self):
        print(f'{self.name}가 사료를 먹고 있다')

# 상속받는 자식 클래스
# class 클래스명(상속자의 클래스명, ...)
# 부모의 속성을 사용하려면?
# 부모클래스.__init__(self, 속성)
class Dog(Animal):
    def __init__(self, name, gender, age, kind):
        Animal.__init__(self, name, gender, age)
        self.kind = kind

class Cat(Animal):
    def __init__(self, name, gender, age, kind):
        Animal.__init__(self, name, gender, age)
        self.kind = kind

# 인스턴스화
dog1 = Dog('노루', '남자', 2, '똥강아지')
cat1 = Cat('루미', '여자', 2, '길냥이')

# 상속받은 메소드 사용하기
dog1.eat()
cat1.run()
print()

# 다중 상속
# class 클래스명(부모클래스1, 부모클래스2, ...)
# 부모 클래스의 메소드명이 같은 경우에는 처음 상속받은 부모클래스1의 메소드만 사용가능
class Papa:
    def __init__(self):
        pass
    def printHave1(self):
        print('상속 => 아파트')

class Mama:
    def __init__(self):
        pass
    def printHave2(self):
        print('상속 => 자동차, 오피스텔')

# 상속 받기
class Child(Papa, Mama):
    def __init__(self):
        pass
    def printHave3(self):
        print('내 자산 => 노트북, 아이패드')

# 인스턴스화
child1 = Child()
child1.printHave1()
child1.printHave2()
child1.printHave3()

# 부모 클래스와 자식 클래스의 상속 관계 확인
# issubclass(자식 클래스명, 부모 클래스명) => True / False
print('자식이 아버지한테 상속 받고 있는가? ', isinstance(child1, Papa))
print('자식이 어머니한테 상속 받고 있는가? ', isinstance(child1, Mama))

# 상속받은 부모 클래스의 정보 표시
print(Child.__bases__)

# dir(객체명): 사용할 수 있는 속성과 메소드 출력
print(dir(child1))
```

---

```angular2
# 외장 모듈 사용하기
# 수학함수를 제공하는 math 모듈 사용하기
# import 모듈명
# import 모듈명 as 별칭
# from 모듈명 import 함수

# 임포트한 모듈 내의 함수 호출
# 모듈명.함수명(옵션)
# 별칭.함수명(옵션)

# 외장모듈 호출 방식1
import math
print(math.pi)
print(math.cos(10))

# 외장모듈 호출 방식2
import math as mt
print(mt.pi)
print(mt.sin(10))

# 외장모듈 호출 방식3 - 사용하고자하는 함수이름을 알고 있어야 한다.
from math import pi, sin, cos
print(pi)
print(sin(10))
print(cos(10))
print()

# 난수 발생 모듈 이용하기
import random as rand

# 실수 난수 발생
print(rand.random())
# 정수 난수 발생 randint(start, end)
print(rand.randint(1, 50))
# 리스트에서 뽑기
eventList = ['꽝', '금2돈', '스타벅스 10만원 상품권', '연필']
print(rand.choice(eventList))
print(rand.choice(eventList))
print()

# 퀴즈 - 로또번호 생성하기
# 조건: 1-45의 숫자 중에서 6개로 구성
# 숫자는 중복되면 안된다.
# 로또리스트는 빈 리스트로 생성
# 반복문에서
#   : 번호의 갯수가 6개라면 반복문 탈출
#   : 1-45까지 번호 중에서 1개 생성
#   : 리스트 안에 이미 번호가 있다면 다시 번호 생성
#   : 중복 번호가 아니라면 리스트에 추가
# 로또 리스트를 출력한다.

lottoList = []
while len(lottoList) != 6:

    lottoNum = rand.randint(1, 45)
    if lottoNum not in lottoList:
        lottoList.append(lottoNum)

print(sorted(lottoList))


# 시계열 데이터 다루기
import time

# time.time() : 1970-01-01-00-00-00를 기준으로 현재 시간까지를 초단위로 리턴
print(time.time())
# time.localtime() : time.time() 값을 년월일시분초로 표시
print(time.localtime(time.time()))

todayList = list(time.localtime(time.time()))
# 현재 년 월 일 표시 => 2020년 5월 13일
print(f'{todayList[0]}년 {todayList[1]}월 {todayList[2]}일')
# 영어권 표시 스타일
print(time.ctime())
# 여러가지 포맷 이용하기
# time.strtime(포맷코드, time.localtime(time.time()))
# 출력할 형식 포맷코드
# %a / %A  : 요일
# %b / %B  :  달
# %c : 날짜와 시간
# %d : 날(day)
# %H : 24시간 기준의 시간
# %I : 12시간 기준의 시간
# %j : 1년중 누적 날짜 표시
# %x : 지역 기반 날짜 출력
# %X : 지역 기반 시간 출력
# %w : 숫자로된 요일 출력. 0은 일요일
# %Y : 년도 출력
# %z : 시간대 출력
# %p : AM/PM

print('-'*30)
print('%c를 사용', time.strftime('%c', time.localtime(time.time())))
print('%d를 사용', time.strftime('%d', time.localtime(time.time())))
print('%H를 사용', time.strftime('%H', time.localtime(time.time())))
print('%I를 사용', time.strftime('%I', time.localtime(time.time())))
print('%j를 사용', time.strftime('%j', time.localtime(time.time())))
print('%x를 사용', time.strftime('%x', time.localtime(time.time())))
print('%X를 사용', time.strftime('%X', time.localtime(time.time())))
print('%w를 사용', time.strftime('%w', time.localtime(time.time())))
print('%Y를 사용', time.strftime('%Y', time.localtime(time.time())))
print('%z를 사용', time.strftime('%z', time.localtime(time.time())))
print()

# datetime 모듈 사용하기
import datetime
# 날짜변수 = datetime.datetime.now()
# 날짜변수.year / 날짜변수.month / 날짜변수.day / 날짜변수.hour ...
now = datetime.datetime.now()
print(now)
print(f'오늘은 {now.year}년 {now.month}월 {now.day}일 입니다')

# calender 모듈 사용하기
import calendar

print(calendar.calendar(2020))
print(calendar.month(2020, 5))
print(calendar.monthcalendar(2020, 5))
```

---

```angular2
# 사용자정의 모듈
# 모듈 = 다른 파일

# cal.py의 calResult 함수 사용하기
import cal

cal.calResult(1, 2)

# 패키지 - 폴더단위, 모듈파일의 저장장소
# 패키지 폴더 안에는 __init__.py 파일이 존재
# 파이참에서 패키지 폴더 만들기
'''
1) [Project] 폴더에 맨위 루트폴더 마우스 우측 버튼 클릭
2) [New]-[Python Package]를 눌러서 패키지 폴더 생성
'''
# 패키지안의 모듈 임포트
# import 패키지명.모듈
# import 패키지명.모듈 as 별칭
# from 패키지명.모듈 import 함수명
from example.hello import message

message('Hello Python')
```

---

```angular2
# 오류와 예외처리
# SyntaxError(구문 오류)
# EOFError(파일의 끝일 경우: 읽을 내용이 없을 떄)
# FileNotNoundError(파일이 없을 때)
# ZeroDivisionError(숫자를 0으로 나누었을 때)
# IndexError(리스트에서 얻어올 수 없는 값일 경우)

# 0으로 나눌 때 발생하는 오류
# 예외처리1
# try:
#   명령 ...
# except 오류종류 as e:
#   명령 ...
#   오류 무시의 경우에는 pass
try:
    print(12 / 0)
except ZeroDivisionError as e:
    print(e)
    print('0으로 나눌 수는 없습니다')
print()

# 예외처리2
# 오류의 종류를 몰라도 된다.
# try:
#   명령 ...
# except Exception:
#   명령
user = [1, 2, 3, 4, 5]
try:
    print(user[5])
except Exception as e:
    print(e)
print()

# 예외처리3
# try ~ except ~ else(없어도 됨) ~ finally(없어도 됨)
# try:
#   명령 ...
# except Exception:
#   오류 처리에 대한 명령
# else:
#   오류가 발생하지 않은 경우 명령
# finally:
#   무조건 실행되는 명령

# 파일 입출력 오류
try:
    # f = open('아무거나.txt', 'r', encoding='utf-8')
    f = open('data/black.txt', 'r', encoding='utf-8')
except Exception as e:
    print(e)
else:
    print('파일 읽기 완료')
finally:
    print('예외처리 테스트 완료')
```

---

```angular2
# csv 파일 읽기
# 파일변수 = open(url, 'r', encoding='utf-8/cp949')
# csv 변수 = csv.reader(파일변수)
# 내장 모듈 임포트
import csv

f = open('data/train.csv', 'r', encoding='utf-8')
csv_data = csv.reader(f)

for data in csv_data:
    print(data)
print()

# data 폴더에서 csv 파일 확인
# population.csv / data.csv /
# 한국교통안전공단_대중교통 이용자유형별 이용인원(2017년).csv
# => koreaTraffic.csv

# 공공데이타포탈
# https://www.data.go.kr/

# CSV란?
# Comma Seperate Value
# 콤마(,)로 데이타가 분리된 파일

# 파일변수
#  = open('csv파일경로', 'r', encoding='utf-8/cp949')
# csv객체변수 = csv.reader(파일변수)
f = open('data/data.csv', 'r', encoding='utf-8')
csv_data = csv.reader(f)
print(csv_data, type(csv_data));
# <_csv.reader object at 0x010ED4F8>
# <class '_csv.reader'>
# csv 객체 출력 : csv파일에서 각행이 리스트로 생성
# for i in csv_data:
#     print(i, type(i))
# # 리스트안에 리스트구조로 변경
# # 주의 사항 : csv.reader(f) 는 한번만 읽어서 메모리에 로딩된다.
data_list = []
for i in csv_data:
    data_list.append(i)
print(data_list)
print(f' 전체 행의 갯수 크기 : {len(data_list)}') # 13

for i in data_list:
    print(i, '컬럼갯수', len(i))

# 첫번째 행은 ? => 컬럼 제목
print(data_list[0])
# 1행1열 출력
print(data_list[0][0])
# 마지막행의 마지막열 출력
print(data_list[-1][-1])
# kor, eng, mat, bio 과목의 데이터형 확인하기
print(type(data_list[-1][-1]))
# <class 'str'>

# 'kor' 데이타만 정수형 리스트로 생성하여라
#  1행은 제외 : 1행은 컬럼제목
korList = []
for i in range(1, len(data_list)):
    korList.append(int(data_list[i][2]))

# kor 데이타 리스트 확인
print(korList)

# kor 과목의 총합과 평균을 구해라
print(f'kor 과목의 총점: {sum(korList)}')
print(f'kor 과목의 평균: {sum(korList)/len(korList)}')
print(f'kor 과목의 최고점: {max(korList)}')
print(f'kor 과목의 최하점: {min(korList)}')

# 퀴즈 :
# 4과목('kor', 'eng', 'mat', 'bio')의 총점 구하기
# 전체 평균 구하기

totalList = []
for i in range(1, len(data_list)):
    totalList.append(int(data_list[i][2])+int(data_list[i][3])+int(data_list[i][4])+int(data_list[i][5]))
print(totalList)
print(f'4과목의 총점은? {sum(totalList)}')
# round(값, 소숫점이하자릿수)
print('전체 평균은?',
      round((sum(totalList)/(4*len(totalList))),2))
# 4과목의 총점은? 3748
# 전체 평균은? 78.08

# 국어 점수의 최고점을 받은 학생의 이름은?
# 알고리즘
# 국어점수의 최고점을 구한다. max()
# 최고점의 인덱스를 구한다. 리스트이름.index()
# 인덱스를 학생이름인덱스에 삽입한다.
#  - 전체리스트[?]
#  - data_list[?][1]

print(f'kor 과목의 최고점: {max(korList)}')
print(f'kor 과목의 최고점의 인덱스는? {korList.index(max(korList))}')
num = korList.index(max(korList))
print(f'kor 과목의 최고점의 학생이름은? {data_list[num+1][1]}')
# kor 과목의 최고점: 100
# kor 과목의 최고점의 인덱스는? 8
# kor 과목의 최고점의 학생이름은? oscar

# 전체 총점이 가장 높은 학생의 이름은?
print(f'총점의 최고점: {max(totalList)}')
print(f'총점의 최고점의 인덱스는? {totalList.index(max(totalList))}')
num2 = totalList.index(max(totalList))
print(f'전체 총점이 가장 높은 학생이름은? {data_list[num2+1][1]}')
# 총점의 최고점: 362
# # 총점의 최고점의 인덱스는? 9
# # 전체 총점이 가장 높은 학생이름은? martin

# 파일닫기
f.close()




# with문으로 csv파일 읽기
with open('data/data.csv', 'r', encoding='utf-8') as f:
    csv_data2 = csv.reader(f)

    for i in csv_data2:
        print(i)


with open('data/population.csv', 'r') as f:
    csv_data = csv.reader(f)
    print(csv_data, type(csv_data));

    # 행단위로 출력하기
    # for row in csv_data:
    #     print(row)

    # 리스트 구조로 변경하기
    result_list = []
    for i in csv_data:
        result_list.append(i)

    print(result_list)
    print(len(result_list)) # 행수 - 52
    print(len(result_list[0])) # 열수 - 2

    # 1행은 컬럼 제목이므로 삭제한 후 다른 리스트에 복사
    result_list2 = result_list[1:]
    print(len(result_list2)) # 행수 - 52

    # 1컬럼은 미국 주 데이타 => state_list
    # state_list를 정렬시켜라
    state_list = []
    for i in result_list2:
        state_list.append(i[0])

    print(state_list)
    # ['Alabama', 'Alaska', 'Arizona',
    # 'Arkansas', 'California', 'Colorado',
    # 'Connecticut', 'Delaware', 'District of Columbia',
    # 'Florida', 'Georgia', 'Hawaii', 'Idaho',
    # 'Illinois', 'Indiana', 'Iowa', 'Kansas',
    # 'Kentucky', 'Louisiana', 'Maine', 'Maryland',
    # 'Massachusetts', 'Michigan', 'Minnesota',
    # 'Mississippi', 'Missouri', 'Montana', 'Nebraska',
    # 'Nevada', 'New Hampshire', 'New Jersey',
    # 'New Mexico', 'New York', 'North Carolina',
    # 'North Dakota', 'Ohio', 'Oklahoma', 'Oregon',
    # 'Pennsylvania', 'Rhode Island', 'South Carolina',
    # 'South Dakota', 'Tennessee', 'Texas', 'Utah',
    # 'Vermont', 'Virginia', 'Washington',
    # 'West Virginia', 'Wisconsin', 'Wyoming']

    # 역순 정렬
    state_list.reverse()
    print(state_list)

    # 퀴즈0. 인구컬럼만 정수리스트로 새로 만들어라
    # '4,780,131' => 4780131
    # 콤마(,) 삭제한다. replace(',','')
    # int()

    population_list = []
    for i in result_list2:
        # population_list.append(i[1])
        # population_list.append(i[1].replace(',',''))
        population_list.append(int(i[1].replace(',','')))

    # print(population_list)
    # for i in range(0, len(population_list)):
    #     temp = population_list[i].replace(',','')
    #     population_list[i] = temp
    print(population_list)

    print(type(population_list[0])) # <class 'int'>

    # 퀴즈1 . 가장 많은 인구수는? max()
    print(max(population_list)) # 37254522
    # 퀴즈2 . 가장 작은 인구수는? min()
    print(min(population_list)) # 563767
    # 퀴즈3. 가장 인구가 많은 주(State)는?

    # 가장많은인구수의 인덱스 구하기
    max_pop = population_list.index(max(population_list))
    # state_list.sort()
    # print(state_list[population_list.index(max(population_list))])
    print(result_list2[max_pop][0])
    # California
    # 퀴즈4. 가장 인구가 적은 주(State)는?
    # 가장많은인구수의 인덱스 구하기
    min_pop = population_list.index(min(population_list))
    # print(state_list[population_list.index(min(population_list))])
    print(result_list2[min_pop][0])
    # Wyoming

# ordered dict 형태로 데이터 불러오기

# 파일변수 = open('csv파일경로', 'r', encoding='utf-8/cp949')
# csv객체변수 = csv.DictReader(파일변수)

# with open('csv파일경로', 'r', encoding='utf-8/cp949') as 파일변수:
#   csv객체변수 = csv.DictReader(파일변수)

with open('data/koreaTraffic.csv', 'r') as f:
    # csv_data = csv.reader(f)
    csv_data = csv.DictReader(f)
    print(csv_data, type(csv_data));
    # <csv.DictReader object at 0x02D90190>
    # <class 'csv.DictReader'>

    # 행별 구조는 딕셔너리
    # for row in csv_data:
    #     print(row, type(row))

    # {'구분': '서울', '일반인': '3711168', '어린이': '36098', '청소년': '259128', '기타': '417539'}
    # <class 'dict'>

    # 컬럼명 = 키값 으로 접근하기
    # for row in csv_data:
    #     print(row['구분'],' / ', row['일반인'], ' / ',
    #           type(row['일반인']))

    # 일반인 사용자의 가장 작은 값은?
    list1 = []
    list2 = []
    for row in csv_data:
        list1.append(int(row['일반인']))
        list2.append(row['구분'])
    print(list1)
    print(type(list1[0]))
    print(min(list1))

    # 일반인 사용자가 가장작은 값의 인덱스는?
    print(list1.index(min(list1)))
    # 7

    # 일반인 사용자가 가장 적은 도시는?
    print(list2)
    print(list2[list1.index(min(list1))])
    # 세종

# 일반인+청소년+어린이 합해서 지역별 사용자 구하기
with open('data/koreaTraffic.csv', 'r') as f:
    csv_data = csv.DictReader(f)
    list_city = []
    list_user = []
    for row in csv_data:
        list_city.append(row['구분'])
        list_user.append(int(row['일반인'])+
                         int(row['어린이'])+
                         int(row['청소년']))

    print(list_city)
    print(list_user)

    # 제주 사용자의 총 데이타값 출력 ?
    print('제주 인덱스 =>', list_city.index('제주'))
    print('제주의 사용자수 =>',
          list_user[list_city.index('제주')])

    # 딕셔너리 구조로 변경하기
    # {도시명:인구사용자총수, ...}
    # 빈 딕셔너리 만들기
    korea_traffic_dict = {}
    for i in range(0, len(list_city)):
        # 딕셔너리변수[키] = 값
        korea_traffic_dict[list_city[i]] = list_user[i]
    print(korea_traffic_dict)
    # {'서울': 4006394, '부산': 952394,
    # '대구': 547517, '인천': 716825,
    # '광주': 242717, '대전': 262872,
    # '울산': 152289, '세종': 22677,
    # '경기': 2816335, '강원': 84879,
    # '충북': 117371, '충남': 148303,
    # '전북': 118447, '전남': 120123, '경북': 165815,
    # '경남': 253772, '제주': 63008}

    # 강원 사용자의 총 데이타값 출력?
    print(korea_traffic_dict['강원'])

    # 퀴즈 1) 시도대학별기업규모에따른취업자수.csv
    #         파일 읽기
    #         day7 번 클라우드 참조
    # 퀴즈 2) 1000명 이상인 회사에 들어간 취업자수가
    #        가장 많은 도시는?
    # 퀴즈 3)
    #        딕셔너리로 만들기
    #        {'지역명':'분석대상자수'...}
```