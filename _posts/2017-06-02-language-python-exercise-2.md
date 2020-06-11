---
layout: post
title: "Python Programming [Exercise 2]"
date: 2017-06-02 09:00:00-18:00:00
categories: Language
tag: Language
---

## [문제 1번] 기하학: 오각형의 넓이
```python
import math

r = eval(input("중심에서 꼭짓점까지의 길이를 입력하세요."))
s = 2 * r * math.sin(math.pi / 5)
Area = (3 * math.sqrt(3) * s * s)/2
print("오각형의 넓이는",format(Area,"0.2f"),"입니다.")
```

## [문제 2번] 기하학: 정다각형의 넓이
```python
import math

n = eval(input("변의 개수를 입력하세요:"))
s = eval(input("변의 길이를 입력하세요:"))
Area = (n * s * s) / (4 * math.tan(math.pi / n))
print("다각형의 넓이는",Area,"입니다.")
```

## [문제 3번] ASCII 코드의 문자 찾기
```python
ch = eval(input("ASCII 코드를 입력하세요:"))

print("문자는",chr(ch),"입니다.")
```

## [문제 4번] 금융 어플리케이션: 급여
```python
name = input("사원이름을 입력하세요:")
hour = eval(input("주당 근무시간을 입력하세요:"))
pay = eval(input("시간당 급여를 입력하세요:"))
origin_tax_rate = eval(input("원천징수세율을 입력하세요:"))
residence_tax_rate = eval(input("주민세율을 입력하세요:"))

#총 급여(근무시간 * 임금)
salary = hour * pay

#원천징수세(20.0%)
origin_tax = salary * 0.2

#주민세(9.0%)
residence_tax = salary * 0.09

#총 공제(원천징수세 + 주민세)
total_tax = origin_tax + residence_tax

#공제 후 급여(총 급여 - 총 공제)
final_salary = salary - total_tax

print("\n")
print("사원 이름:" + name)
print("주당 근무시간:",hour)
print("임금:",pay)
print("총 급여:",salary)
print("공제:")
print("    원천징수세(20.0%):",int(origin_tax))
print("    주민세(9.0%):",int(residence_tax))
print("    총 공제:",int(total_tax))
print("공제 후 급여:",int(final_salary))
```

## [문제 5번] 자동판매기 프로그램
```python
#물건값
things = eval(input("물건값을 입력하시오:"))

#1000원 지폐개수
thounsand_count = eval(input("1000원 지폐개수:"))

#500원 동전개수
fivehundred_count = eval(input("500원 동전개수:"))

#100원 동전개수
onehundred_count = eval(input("100원 동전개수:"))

#가지고 있는 돈
money = (thounsand_count * 1000) + (fivehundred_count * 500) + (onehundred_count * 100)

#거스름돈
charge = money - things

#거스름돈에서 500원 동전개수
fivehundred_charge_count = int(charge / 500)

#거스름돈에서 100원 동전 개수
onehundred_charge_count = int((charge % 500) / 100)

#거스름돈에서 50원 동전 개수
fifty_charge_count = int((((charge % 500) % 100)) / 50)

#거스름돈에서 10원 동전 개수
ten_charge_count = int((((charge % 500) % 100)) / 10)

#거스름돈에서 1원 동전 개수
one_charge_count = int(((((charge % 500) % 100)) % 10))

print("500원 =",fivehundred_charge_count,"100원 =",onehundred_charge_count,"10원 =",ten_charge_count,"1원 =",one_charge_count)
```
