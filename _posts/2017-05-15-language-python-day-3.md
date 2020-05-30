---
layout: post
title: "Python Programming [Day 3]"
date: 2017-05-15 09:00:00-18:00:00
categories: Language
tag: Language
use_math: true
---

# Python 4일만에 뽀개기 - Day 3

```angular2
# 제어문
# 비교연산자(< > <= >= == !=) / 논리연산자(or and not)
# 참 조건 - True, 1, 공백이 아닌 문자열, [리스트], (튜플), {집합}
# 거짓 조건 - False, 0, "", '', None, [], (), {}
# 조건문 - if문 / if-else문 / if-elif-else / switch-case문이 없다.
# 반복문 - while / for

money = 1000
if money:
    print('택시를 타고')
print('가자')

# 두 변수의 크기 비교에 따라 분기하기
a = 10
b = 20
if a > b:
    print('a가 b보다 크다')
else:
    print('b가 a보다 크다')

# 홀수인지 짝수인지 명령 실행하기
# 숫자 % 2 = 0 -> 짝수
# 홀수 % 2 = 1 -> 홀수
myNum = int(input('숫자를 입력하시오...'))
if myNum % 2 == 0:
    print('짝수')
if myNum % 2 != 0:
    print('홀수')

# 두 조건 분기:
'''
if 조건:
    명령문 A
else:
    명령문 B
'''
# 3의 배수이면 메세지 출력
myNum = int(input('숫자를 입력하시오...'))
if myNum % 3 == 0:
    print('3의 배수')
if myNum % 3 != 0:
    print('3의 배수가 아님')

# 조건에 논리연산자 사용하기
# id, password 변수 지정
# id, password 둘 다 맞다면 메시지 출력
id = 'admin'
pwd = '1234'
if id == 'admin' and pwd == '1234':
    print('회원님 환영합니다')
else:
    print('잘못된 접근입니다')

# 특정 멤버가 리스트에 있는가?
bts = ['지민', 'RM', '뷔', '정국']
member = '정국'
if member in bts:
    print(f'{member}은(는) bts 멤버이다')
else:
    print(f'{member}은(는) bts 멤버가 아니다')

# 수강 과목이 튜플안에 있는가?
myClass = {'파이썬', 'C', 'C++'}
subject = 'Java'
if subject not in myClass:
    print('수강 과목이 아닙니다')
else:
    print('수강 과목이 맞습니다')
```

---

```angular2
# 조건문 다중 분기
'''
if 조건A:
    실행문 A
elif 조건B:
    실행문 B
elif 조건 C:
    실행문 C
else:
    실행문 X
'''

# 양수인지 음수인지 0인지 따라서 실행문 실행하기
num1 = 0
if num1 > 0:
    print('양수')
elif num1 < 0:
    print('음수')
else:
    print('0')

# 태어난 년도를 입력하면 해당하는 띠를 출력하기
# 태어난 년도 % 12 = 나머지 값에 따라서 리스트의 인덱스값으로 분기
# 예) 태어난 년도 = 2009
# 2009 % 12 = 5
animalList = ['원숭이띠', '닭띠', '개띠', '돼지띠', '쥐띠',
              '소띠', '범띠', '토끼띠', '용띠', '뱀띠', '말띠', '양띠']

birth = int(input('태어난 년도를 입력하세요...'))
animal = birth % 12
print(f'태어난 년도는 {birth}년이고, {animalList[animal]}입니다.')

# if.. elif.. else문 + in/not 연산자
# if 값 in 문자열/리스트/튜플/집합:
#   명령문1
# elif 값 in 문자열/리스트/튜플/집합:
#   명령문2
# else:
#   명령문3

fruit = ['바나나', '포도', '자두', '수박']
if '체리' in fruit:
    print('체리가 냉장고에 있다')
elif '포도' in fruit:
    print('포도가 냉장고에 있다')
else:
    print('냉장고에 비었다')

# pass 키워드
# 실행되지 않는 명령문
pocket = ['paper', 'money', 'cellphone']
if 'money1' in pocket:
    pass
elif 'cellphone1' in pocket:
    pass
else:
    print('카드를 사용해라')
```

---

```angular2
# while 조건:
#   실행문
# 조건이 True 인동안 실행문을 계속 반복하여라

# 초기값
# while 초기값을이용한조건:
#   초기값 증감
#   실행

# 1-10까지 출력하여라
count = 1
while count <= 10:
    print(f'count = {count}')
    count += 1

# 10-1까지 출력하여라
count = 10
while count > 0:
    print(f'count = {count}')
    count -= 1

# 1-100까지의 합은?
# 1+2+3+...+100 = ?
count = 1
# 누적합의 변수
result = 0
while count <= 100:
    count += 1
    result += count
print(f'1-100까지의 합은? {result}')

# while문 + 조건문 if 사용하기
# 1-50에서 짝수만 출력하기
cnt = 1
while cnt <= 50:
    cnt += 1
    if cnt % 2 == 0:
        print(cnt, end=' ')

# 1-10까지 출력하기
cnt = 1
while True:
    print('cnt = %d' % cnt)
    cnt += 1
    if cnt > 10:
        break
print('무한루프 테스트 끝')

# q를 입력할 때까지 리스트에 값을 추가하여라
myList = []
while True:
    indata = input('q를 입력하면 끝. 그렇지 않으면 리스트에 값 추가 => ')
    if indata == 'q':
        break
    else:
        myList.append(indata)
print(f'myList = {myList}')

# continue
# : 반복문을 탈출하지는 않고 그 다음 단계로 분기
# : continue문이 실행되면 반복문 안에서는 하단 명령은 실행되지 않는다.
# 1-10까지 출력하는 데 5라는 숫자는 스킵
cnt = 0
while cnt <= 10:
    cnt += 1
    if cnt == 5:
        continue
    elif cnt <= 10:
        print(f'cnt = {cnt}')
```

---

```angular2
# for 변수 in 문자열/리스트/튜플:
#   실행문
# 리스트의 값을 순차적으로 출력하여라
myList = [100, 5, 8, 45, -100]
for i in myList:
    print(f'** {i}')
print()

# 문자열에서 홀수번째만 찍기
myText = '도레미파솔라시'
cnt = 1
for i in myText:
    if cnt % 2 != 0:
        print(i, end=' ')
    cnt += 1
print('\n')

# 리스트의 평균과 총점 구하기
grade = [78, 55, 66, 34, 97]
print(f'총점은? {sum(grade)}')
print(f'평균은? {sum(grade)//len(grade)}')
print()

# 딕셔너리와 for문
# for key in 딕셔너리 변수:
#   key 명령문
myDict = {1:'q', 2:'w', 3:'e', 4:'r', 5:'t', 6:'y'}
for key, value in myDict.items():
    print('{0}: {1}'.format(key, value))
```

---

```angular2
# range(start, end, step)
# range 객체로 생성
# start - end - 1 까지 step 만큼
print(range(1, 11))
# 리스트화
print(list(range(1, 11)))
# 튜플화
print(tuple(range(1, 50, 2)))
# 집합화
print(set(range(1, 50, 2)))

# 0-50에서 3의 배수만 출력하기
for i in range(3, 50, 3):
    print(i, end=' ')
print()

# 특정 숫자의 구구단 출력하기
num1 = 3
for i in range(1, 10):
    print(f' {num1} x {i} = {i*i}')

# 1-100까지의 합 구하기
tot = 0
for i in range(1, 101):
    tot += i
print(tot)

# 구구단 모두 찍기
for i in range(1, 10):
    print(f' {i} 단')
    for j in range(1, 10):
        print(f' {i} x {j} = {i*j}')

# 퀴즈1: 1-25까지 한 줄에 5개씩 출력하기
for i in range(1, 26):
    if i % 5 != 0:
        print(i, end=' ')
    else:
        print(i)

# 퀴즈2: 1-27까지 5의 배수만 빼고 출력하기
for i in range(1, 28):
    if i % 5 != 0:
        print(i, end=' ')

# 2차원 리스트와 for문
list_2d = [[1, 2, 3], ['가', '나', '다'], ['python', 'c', 'c++']]
print(list_2d)
print(list_2d[0])
print(list_2d[2][0])

# 다중 for문과 이차원 리스트
for i in range(0, 3):
    for j in range(0, 3):
        print(list_2d[i][j], end=' ')
print('\n')

# 이차원 리스트(동일 값이 들어있는 경우)에 인덱스 변수 3개 사용하기
# for (변수1, 변수2, 변수3, ...) in 이중리스트
for (i, j, k) in list_2d:
    print(f'i = {i}, j = {j}, k = {k}')

# 퀴즈3: 학생 이름, 국어, 영어, 수학으로 구성된 2차원 리스트를 생성한다.
# 출력 형식은 아래와 같이 한다.
'''
학생이름   국어  영어  수학  합계  평균
김태희      30   40   100    ?    ?
'''

stGradeList = [['김태희', 30, 50, 55],
               ['신민아', 50, 90, 80],
               ['아이린', 50, 90, 40],
               ['한고은', 60, 50, 56],
               ['박보영', 90, 88, 66]]

print('학생이름    국어  영어  수학  합계    평균')
for std in stGradeList:
    print('{0:8s} {1:d} {2:4d} {3:5d} {4:5d} {5:8.2f}'.format(std[0], std[1], std[2], std[3], sum(std[1:]), sum(std[1:])//3))
print()
for (stName, i, j, k) in stGradeList:
    tot = i+j+k
    print('%s %7d %4d %5d %5d %8.2f' % (stName, i, j, k, tot, tot/3))

    # # 반올림 함수 round(값, 소숫점자릿수)
    # print(sum, round((tot / 3), 2))
    # print()
    # print('-' * 10)
```

---

```angular2
# 리스트 내포 - 한줄 for 리스트
# 리스트 변수 = [결과값이나 수식 for문]
# 1-10으로 구성된 리스트 만들기
# 빈 리스트를 만들고 1-10까지 값 추가
result1 = []
for i in range(1, 11):
    result1.append(i)
print(result1)

result2 = [i for i in range(1, 11)]
print(result2)

# 3단의 결과값으로 리스트를 만들어라
result3 = []
for i in range(1, 10):
    result3.append(i*3)
print(result3)

result4 = [i*3 for i in range(1, 10)]
print(result4)

# 3단의 결과값에서 -1 한 값으로 리스트를 만들어라.
result5 = []
for i in range(1, 10):
    result5.append(i*3-1)
print(result5)

result6 = [i*3-1 for i in range(1, 10)]
print(result6)

# 다음과 같은 형태로 리스트를 만들어라.
# ['*', '**', ..., '*********']
result7 = []
for i in range(1, 10):
    result7.append('*'*i)

print(result7)

result8 = ['*'*i for i in range(1, 10)]
print(result8)

# 이중 for문으로 리스트 만ㄷ르기 - 리스트 다중 for
# 리스트 변수 = [결과값이나 수식 for문 for문]
# 구구단의 결과값으로 리스트를 만들어라
result9 = []
for i in range(2, 10):
    for j in range(1, 10):
        print(f'{i} X {j} = {i * j}')
        result9.append(i*j)
print(result9)

result10 = [i*j for i in range(2, 10) for j in range(1, 10)]
print(result10)
```

---

```angular2
# 함수1 - 인자(Parameter, 전달값) x, 결과값(return) x
'''
# 함수 정의
def 함수명():
    명령문

# 함수 호출
함수명()
'''

# 특정 메시지를 출력하는 함수 정의하고 호출하기
def helloWord():
    print(f'*** Hello Word')

# 함수 호출
helloWord()

# 함수2 - 인자(Parameter, 전달값) O, 결과값(return) X
'''
# 함수 정의
def 함수명(인자):
    명령문
    
# 함수 호출
함수명(인자값)
'''

# 특정 메시지를 전달한 후 출력하는 함수 정의 및 호출하기
# 함수 정의
def helloWord2(m):
    print(f'*** {m}')

# 인자값을 지정해서 호출하기
helloWord2('안녕')

# 두 수의 합을 구하는 함수를 정의하고 호출하여라
def add(x, y):
    print(f'{x} + {y} = {x+y}')

add(10, 90)


# 함수3 -  인자(Parameter,전달값) X, 결과값(return) O
# 함수 정의
'''
def 함수명():
    return 값/수식

# 함수 호출
함수명()
'''
def helloWorld3():
    # print('-'*10)
    return '** Helloworld'

print(helloWorld3())


# 함수4 - 인자(Parameter, 전달값) O, 결과값(return) O
'''
# 함수 정의
def 함수명(인자):
    return 값/수식
# 함수 호출
함수명(인자)
'''

# 고객 이름을 인자로 전달해서 환영메시지를 리턴한다.
def message(userName):
    return userName + '님 환영합니다.'

print(message('홍길동'))

# 1부터 특정 숫자까지의 합을 리턴한다.
def totalN(n):
    tot = 0
    for i in range(1, n+1):
        tot += i
    return tot

print('1-100까지의 합은?', totalN(100))
print('1-1000까지의 합은?', totalN(1000))
print('1-10000까지의 합은?', totalN(10000))

# 함수5 - 인자(Parameter, 전달값) O, 결과값(return) 다중 O
# 리턴값이 다중인 경우에는 튜플로 저장된다.
'''
# 함수 정의
def 함수명(인자):
    return 값1/수식1, 값2/수식2, ...
# 함수 호출
함수명(인자)
'''

# 두 수를 인자로 전달한 후 합과 곱의 결과를 리턴한다.
def sumMul(x, y):
    return x+y, x*y

print(sumMul(10, 4))
print(sumMul(7, 89))

# 3수를 인자로 전달한 후 더하기/빼기/곱하기/나누기 한 값을 출력하는 함수를 정의하고 호출하여라.
def myFunc(n1, n2, n3):
    return n1+n2+n3, n1-n2-n3, n1*n2*n3, n1/n2/n3

print(myFunc(10, 5, 2))

# 함수6 - 인자값에 모두 초기값이 있는 경우
'''
# 함수 정의
def 함수명(인자=값):
    return 값/수식
# 함수 호출
함수명() -> 인자를 주지 않는 경우, 초기값으로 설정됨
함수명(인자)
'''
# 인자의 합을 구하는 함수 정의
def sum1(n1=0, n2=0, n3=0):
    return n1+n2+n3

print(sum1())
print(sum1(1))
print(sum1(1,2))
print(sum1(1,2,3))

# 함수7 - 인자의 일부만 초기값이 있는 경우
'''
# 함수 정의
def 함수명(인자1, 인자2, 인자3=값):
    return 값/수식
# 함수 호출
함수명(인자1, 인자2)
함수명(인자1, 인자2, 인자3)
'''
# 세 수의 곱을 리턴한다.
# 마지막 인자에만 초기값이 지정되어 있다.
def threeMul(n1, n2, n3=1):
    return n1*n2*n3

print(threeMul(3, 4))
print(threeMul(3, 4, 9))

# 함수8 - 가변인자 함수
'''
# 함수 정의
def 함수명(*args):
    명령문
    return 값/수식

# 함수 호출
함수명(인자1)
함수명(인자1, 인자2)
함수명(인자1, 인자2, 인자3)
'''
# 가변인자로 함수를 정의한 후 가변인자의 값을 출력한다.
def f1(*args):
    print(args)
    print(*args)
    print(type(args))

f1('*', '**', '***')

# 학생이름을 인자로 전달하고 출력한다.
def f2(*args):
    if len(args) == 0:
        print('학생이 없다')
    for i in args:
        print(f'학생 이름 = {i}')
    print()

f2()
f2('홍길동', '고은비')
f2('이몽룡', '성춘향', '김콩쥐')

# 함수9 - 인자 + 가변인자가 함께 전달하는 함수
'''
# 함수 정의
def 함수명(인자0, *args):
    명령문
    return 값/수식

# 함수 호출
함수명(인자0)
함수명(인자0, 인자1, 인자2, ...)
'''
def f3(num, *args):
    return num, args

print(f3(10))
print(f3(10, '강아지'))
print(f3(10, '고양이', '강아지', '펭귄'))

# 퀴즈: 계산기호 인자 값에 따라서 뒤의 가변인자값을 계산하여라.
def calChoice(operator, *args):
    if operator == '+':
        result = 0
        for arg in args:
            result += arg
    elif operator == '-':
        result = 0
        for arg in args:
            result -= arg
    elif operator == '*':
        result = 1
        for arg in args:
            result *= arg
    else:
        result = sum([arg for arg in args]) // len(args)

    return result

print(calChoice('*', 20, 30))
print(calChoice('+', 20, 30, 50))

# 함수10 -  딕셔너리 가변인자 **kwargs
# 함수 정의
'''
def 함수명(**kwargs):
    kwargs 명령문

# 함수 호출
함수명(키1변수=값1)
함수명(키1변수=값1, 키2변수=값2 ...)
'''

def dictCall1(**kwargs):
    return kwargs, type(kwargs)

print('-'*20)
print(dictCall1())
print(dictCall1(key1='apart', key2='banana'))
print(dictCall1(key1='apart', key2='banana', key3='flower'))

# for key in 딕셔너리 와 가변딕셔너리 함수 활용
def dictCall2(**kwargs):
    print('-'*20)
    for key in kwargs:
        print(key, ' => ', kwargs[key])

print('-'*20)
dictCall2(age=12, userName='홍길동', gender='남')
dictCall2(age=22, userName='장민호')
```

---

```angular2
# 람다함수
# 람다함수란? 한 줄짜리 함수, def 키워드가 필요없음. 대신 lambda 키워드 사용
# 람다변수 = lambda 인자: 명령문

# 두 수의 합을 리턴한다.
# 람다함수 정의
ff1 = lambda x, y: x + y
# 람다함수 호출
print(ff1(10, 20))

# 메시지 출력
ff2 = lambda x: x + '님 어서오세요'
print(ff2('홍길동'))

# 함수의 범위(Scope)
# 전역변수 - global 변수명. 함수 안에서도 사용 가능
# 지역변수 - local

x = 10
print('함수밖의 x', x)
def test():
    x = 20
    print('함수안의 x', x)

test()
print('함수밖의 x', x) # 지역변수 값은 변경되지 않음
print()

x = 10
print('함수밖의 x', x)
def test():
    # global로 선언하면, 전역변수로 정의됨
    global x
    x = 20
    print('함수안의 x', x)

test()
print('함수밖의 x', x)
```