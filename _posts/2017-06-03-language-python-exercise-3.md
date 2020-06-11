---
layout: post
title: "Python Programming [Exercise 3]"
date: 2017-06-03 09:00:00-18:00:00
categories: Language
tag: Language
---

## [문제 1번] 
#### (게임: 덧셈 학습하기) 100 이하의 2 개 정수를 생성하고 이들 정수의 합을 사용자가 맞추도록 하는 프로그램을 작성하시오. 사용자의 입력 값이 맞는지 혹은 틀린지를 출력해야 한다. 이 프로그램은 AdditionQuiz.py 와 유사하다.
```python
# 난수 발생을 위한 함수
import random

# 두 개의 난수 생성
num1 = random.randint(0,100)
num2 = random.randint(0,100)

# 두 정수의 합
sum = num1 + num2

# 사용자로부터 예상되는 정수의 합 입력
user_input = eval(input("정수의 합은 얼마일까요?:"))

# while 문을 이용해 사용자 입력 값과 두 정수의 합 비교 
while True:
    if user_input == sum:
        print("맞습니다.\n")
        break
    elif user_input > sum: 
        print("입력된 값이 더 큽니다.\n")
        user_input = eval(input("정수의 합은 얼마일까요(재입력)?:"))
    else:
        print("입력된 값이 더 작습니다.\n")
        user_input = eval(input("정수의 합은 얼마일까요(재입력)?:"))
```

## [문제 2번] 
#### (미래의 요일 맞추기) 사용자가 오늘의 요일을 정수로 입력하는 프로그램을 작성하시오(예를 들어, 일요일은 0, 월요일은 1, ... , 토요일은 6). 또한 사용자로부터 오늘부터 경과한 일수를 입력하면 미래의 요일을 출력합니다.
```python
# 사용자로부터 오늘의 요일을 입력
days = eval(input("오늘의 요일을 입력하세요:"))

# 사용자로부터 오늘부터 경과한 일수를 입력
routing = eval(input("오늘부터 경과한 일수를 입력하세요:"))

# 미래의 요일 값 정하기
future = days + routing % 7

# 사용자로부터 받은 오늘의 요일을 문자열로 바꾸는 함수
def change_days(num):
    if num == 0:
        return "일요일"
    elif days == 1:
        return "월요일"
    elif num == 2:
        return "화요일"
    elif num == 3:
        return "수요일"
    elif num == 4:
        return "목요일"
    elif num == 5:
        return "금요일"
    elif num == 6:
        return "토요일"

# 미래의 요일 출력
print("오늘은 " + change_days(days) + "이고 미래의 요일은 " + change_days(future) + "입니다.." )
```

## [문제 3번]
#### (숫자 검사하기) 사용자로부터 하나의 정수를 입력받고, 그 정수가 5와 6 모두 나누어지는지, 5 또는 6으로 나누어지는지, 혹은 두 정수 모두로는 나누어지지 않지만 둘 중에 하나로만 나누어지는지를 검사하는 프로그램을 작성하시오.
```python
# 사용자로부터 하나의 정수를 입력
num = eval(input("하나의 정수를 입력하세요:"))

# 5와 6으로 모두 나누어지는지 검사하기
print(num, "은/는 5 와 6 으로 나누어집니까?" ,bool((num % 5 == 0) and (num % 6 == 0)))

# 5또는 6으로 나누어지는지 검사하기
print(num, "은/는 5 혹은 6 으로 나누어집니까?" ,bool((num % 5 == 0) or (num % 6 == 0)))
      
# 5와 6으로 모두 나누어지지 않지만 둘 중에 하나로는 나누어지는지 검사하기
print(num, "은/는 5 혹은 6으로 나누어지지만, 둘 모두로는 나누어지지 않습니까?" ,bool((num % 5 == 0) or (num % 6 == 0) and (num % 30 != 0)))  
```

## [문제 4번]
#### (대칭수) 사용자로부터 세 자리 정수를 입력받고 그 숫자가 대칭수(palindrome number)인지를 판별하는 프로그램을 작성하시오. 어떤 숫자를 오른쪽에서 왼쪽으로 읽으나 왼쪽에서 오른쪽으로 읽으나 서로 같다면, 그 숫자는 대칭수이다.
```python
# 사용자로부터 세 자리 정수 입력
num = input("세 자리 정수를 입력하세요:")

# 입력받은 정수가 대칭수인지 아닌지 구별해서 출력
if num[0] == num[2]:
    print(num, "은/는 대칭수입니다.")
else:
    print(num, "은/는 대칭수가 아닙니다.")
```

## [문제 5번]
#### (16진수를 10진수로 변환) 사용자로부터 16진수 문자를 입력받고, 입력 문자에 해당하는 10 진수 정수를 출력하는 프로그램을 작성하시오.
```python
# 사용자로부터 16진수 문자 입력
char = input("16진수 문자를 입력하세요:")

# 입력받은 16진수 문자를 10진수로 변환하여 출력
# A 또는 a라는 문자를 받은 경우 10을 출력
if char == 'A' or char == 'a':
    print("10진수 값은 10입니다")
# B 또는 b라는 문자를 받은 경우 11을 출력
elif char == 'B' or char == 'b':
    print("10진수 값은 11입니다")
# C 또는 c라는 문자를 받은 경우 12를 출력
elif char == 'C' or char == 'c':
    print("10진수 값은 12입니다")
# D 또는 d라는 문자를 받은 경우 13을 출력
elif char == 'D' or char == 'd':
    print("10진수 값은 13입니다")
# E 또는 e라는 문자를 받은 경우 14를 출력
elif char == 'E' or char == 'e':
    print("10진수 값은 14입니다")
# G 또는 g라는 문자를 받은 경우 15를 출력
elif char == 'F' or char == 'f':
    print("10진수 값은 15입니다")
# 0부터 9까지는 아스키코드 변환문자가 10진수와 동일하므로 ord함수를 사용하여 출력
elif ord(char) >= 48 and ord(char) <= 57:
    print("10진수 값은", char, "입니다")
# 그 이외의 문자가 입력된 경우 잘못된 입력이라고 출력
else:
    print("잘못된 입력입니다.")
```
