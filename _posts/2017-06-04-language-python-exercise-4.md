---
layout: post
title: "Python Programming [Exercise 4]"
date: 2017-06-04 09:00:00-18:00:00
categories: Language
tag: Language
---

## [문제 1번] 
#### (양수와 음수 개수 세기 및 평균 계산하기) 불특정 개수의 정수를 읽은 후, 양수와 음수가 몇 개씩 읽혔는지 결정하고, 입력값의 개수와 평균을 계산하는 프로그램을 작성하시오(0은 세지 않는다). 프로그램은 입력값 0으로 종료된다. 평균값을 부동소수점 숫자로 출력한다.
```python
sum = 0 # 총합을 나타내는 sum은 처음 0으로 할당
count = 0 # 정수의 개수를 나타내는 count는 처음 0으로 할당
plus_count = 0 # 양수의 개수를 나타내는 plus_count는 처음 0으로 할당  
minus_count = 0 # 음수의 개수를 나타내는 minus_count는 처음 0으로 할당

num = eval(input("정수를 입력하세요. 입력값이 0이면 종료됩니다:"))
while num != 0: # num의 값이 0이 되면 종료
    sum += num # sum은 num을 모두 합한 값
    count += 1 # num을 입력할 때마다 count의 값이 1씩 증가
    if num > 0: # 만약 num이 양수이면 
        plus_count += 1 # plus_count의 값이 1씩 증가
    else: # 만약 num이 음수이면 
        minus_count += 1 # minus_count의 값이 1씩 증가
    
    num = eval(input("정수를 입력하세요. 입력값이 0이면 종료됩니다:"))
        

print("양수의 개수는",plus_count,"개 입니다.")
print("음수의 개수는",minus_count,"개 입니다.")
print("총합은",sum,"입니다.") 
print("평균은", format(sum / count, ".2f"), "입니다.") # format을 이용하여 부동소수점 숫자로 출력
```

## [문제 2번] 
#### (최대공약수 계산하기) 예제 코드 lec05/GreatestCommonDivisor.py에서 최대공약수를 구하는 방법을 알아보았다. 두 정수 n1과 n2의 최대공약수를 찾는 또 다른 해결방법은 다음과 같다. 우선, n1과 n2 중 작은 수를 d라고 한 후, d, d-1, d-2, ... , 2, 1의 순서로 각각 d가 n1과 n2의 공약수인지 검사한다. 첫 번째로 나타난 공약수가 두 수 n1과 n2의 최대공약수이다. 이 방법으로 최대공약수를 구하는 프로그램을 작성하시오.
```python
n1 = eval(input("n1의 값을 입력: ")) # n1의 정수값 입력
n2 = eval(input("n2의 값을 입력: ")) # n2의 정수값 입력

if n1 > n2: # 만약 n1이 n2보다 크다면 
    d = n2  # d에 n2를 할당
else:       # 만약 n1이 n2보다 작다면 
    d = n1  # d에 n1을 할당

while d != 0: # d가 0이 될 때까지(0이 되면 안되므로)
    if n1 % d == 0 and n2 % d == 0: # d가 n1과 n2의 공약수일 때
        gcd = d # 최대공약수 gcd는 d의 값을 할당
        break   # 그 즉시 루프를 탈출한다
    d -= 1  # 만약 나머지의 경우 d를 1씩 줄여나간다    

print("최대공약수는",d,"입니다.")
```

## [문제 3번]
#### (윤년 출력하기) 21세기(2001년부터 2100년까지)의 모든 윤년을 한 행에 10개씩 출력하는 프로그램을 작성하시오. 연도는 단 공백 한 개로 구분된다.
```python
line = 10 # 한 행에 출력할 개수
count = 0 # 구해준 윤년의 개수

for year in range(2001,2100): # for문의 형식은 다음과 같다 for 변수 in range(초기값,종료값):
    if (year % 4 == 0 and year % 100 != 0) or year % 400 == 0: # 윤년은 연도가 4로 나누어지면서 100으로는 나누어지지 않거나 400으로 나누어진다 
        count += 1 # 윤년을 구할 때마다 count의 값을 1씩 증가시킨다
        print(year,end=' ') # 윤년을 출력해주는데 이때, end = ' '를 이용하면 공백으로 칸을 나누어준다 
        if count % line == 0: # 만약 count의 값이 10으로 나누어 떨어지면 
            print() # print()를 이용하여 한 행을 뛰어넘는다
```

## [문제 4번]
#### (가장 큰 수의 출현 빈도수) 정수를 읽은 후, 가장 큰 수를 찾고 그 수의 출현 빈도 수를 계산하는 프로그램을 작성하시오. 입력은 숫자 0일 때 종료된다고 가정한다. 3 5 2 5 5 5 0이 입력되었다고 가정하면 프로그램은 가장 큰 수 5와 5가 출현한 빈도수 4를 찾는다(힌트: 두 변수 max와 count를 사용한다. 변수 max는 현재의 최대 숫자를 저장하고 count는 그 수의 출현 빈도수를 센다. 초기에 첫 번째 숫자를 max에 1을 count에 할당한다. 각각의 뒤따르는 숫자를 max와 비교하라. 만약 그 숫자가 max보다 크면, 그 숫자를 max에 할당하고 count를 1로 재설정한다. 만약 숫자가 max와 같으면, count의 값을 1로 증가시킨다).
```python
num = eval(input("숫자를 입력하세요(0은 입력 종료):"))# 처음 num의 값 지정
max = num # 처음 num의 값을 max로 지정
count = 1 # 첫 번째 max의 값이 num일 때 count의 값이 1

while num != 0:
    num = eval(input("숫자를 입력하세요(0은 입력 종료):"))
    if max < num:  # 만약 max보다 큰 수가 입력되면
        max = num  # max에 num의 값을 할당
        count = 1  # count의 값을 1로 재설정
    elif max == num: # 만약 max와 같은 수가 입력되면
        count += 1   # max가 한 번 더 출현하므로 count의 값 1 증가
    else:         # 나머지의 경우
        continue  # 조건을 다시 반복해준다
        
print("가장 큰 수는",max,"입니다.")
print("가장 큰 수가 나타난 빈도수는",count,"번입니다.")
```

## [문제 5번]
#### (정수 자릿수 합산하기) 정수의 자릿수 합을 계산하는 함수를 작서아시오. 다음의 함수 헤더를 사용하라. 
#### def sumDigits(n):
#### 예를 들어, sumDigits(234)는 9(9=2+3+4이다.)를 반환한다(힌트: 자릿수 추출을 위해  % 연산자를 사용하라. 예를 들어, 234에서 4를 추출하기 위해 234%10(=4)을 사용한다. 234에서 4를 삭제하기 위해 234//10(=23)을 사용한다. 모든 자릿수가 추출될 때까지 각 자리마다 자릿수를 반복적으로 추출하고 삭제하기 위해 '루프'를 사용하라.)
#### 사용자로부터 하나의 정수를 입력 받고 이 정수의 모든 자릿수의 합을 출력하는 프로그램을 작성하시오.
```python
def sumDigits(n):
    sum = 0 # 처음 sum의 값을 0으로 할당
    digit_number = n % 10 # 처음 자릿수의 값은 10으로 나눈 나머지
    while digit_number != 0: # 자릿수의 값이 0이 되면 안되므로
        sum += digit_number # sum은 자릿수를 모두 더한 결과
        n = n // 10 # 뒤의 숫자를 없애기 위해 n에 //를 해준다
        digit_number = n % 10 # 다시 자릿수를 구하기 위해 n을 10으로 나눈 나머지를 구한다
    return sum
        
num = eval(input("하나의 정수를 입력하시오:"))
print(num,"의 모든 자릿수의 합은", sumDigits(num), "입니다.")
```

## [문제 6번]
#### (3개 숫자를 정렬하기) 3개 숫자를 오름차순으로 출력하는 다음의 함수를 작성하시오.
#### def displaySortedNumbers(num1, num2, num3):
#### 사용자로부터 3개 숫자를 입력 받고 이들 숫자를 오름차순으로 출력하기 위해 위 함수를 호출하는 테스트 프로그램을 작성하시오.
```python
def displaySortedNumbers(num1,num2,num3): # 크게 3 가지 경우로 나뉜다
    if (num1 > num2) and (num1 > num3): # num1이 최대값일 경우
        if num2 > num3: # 다시 num2가 num3보다 큰 경우
            print("정렬된 숫자는",num3,num2,num1,"입니다.")
        else: # num3가 num2보다 큰 경우
            print("정렬된 숫자는",num2,num3,num1,"입니다.")
    elif (num2 > num1) and (num2 > num3): # num2가 최대값일 경우
        if num3 > num1: # 다시 num3가 num1보다 큰 경우
            print("정렬된 숫자는",num1,num3,num2,"입니다.")
        else: # num1이 num3보다 큰 경우
            print("정렬된 숫자는",num3,num1,num2,"입니다.")
    else: # num3가 최대값일 경우
        if num1 > num2: # 다시 num1이 num2보다 큰 경우
            print("정렬된 숫자는",num2,num1,num3,"입니다.")
        else: # num2가 num1보다 큰 경우
            print("정렬된 숫자는",num1,num2,num3,"입니다.")

n1,n2,n3 = eval(input("세 수를 입력하세요:"))
displaySortedNumbers(n1,n2,n3)
```
