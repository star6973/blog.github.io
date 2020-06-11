---
layout: post
title: "Python Programming [Exercise 7]"
date: 2017-06-07 09:00:00-18:00:00
categories: Language
tag: Language
---

## [문제 1번] 
#### (지정된 문자의 빈도수) 어떤 문자열에서 지정된 문자의 빈도수를 찾는 함수를 작성하시오. 함수 헤더는 다음과 같다.
> def count(s, ch): 

#### str 클래스는 count 메소드를 이미 가지고 있다. str 클래스의 count 메소드를 사용하지 않고 자신만의 메소드를 구현하여 사용하시오. 예를 들어, count("Welcome", 'e')는 2를 반환한다. 사용자로부터 하나의 문자열과 하나의 문자를 입력받고 그 문자열 내에 입력된 문자의 빈도수를 출력하는 테스트 프로그램을 작성하시오.
```python
# str 클래스는 count 메소드를 가지고 있다하였으므로 
# 나만의 count 메소드를 만든다

user_str = input("문자열을 입력:") # 사용자 넣고 싶은 문자열 입력
user_ch = input("문자를 입력:") # 빈도수를 보고싶은 문자를 지정

def count(s, ch):
    cnt = 0 # 지정된 문자의 빈도수를 세기 위한 변수 cnt
    for i in range(len(s)): # 문자열 s의 길이만큼 for문으로 반복
        if(s[i] == ch): # 리스트를 이용하여 문자열의 인덱스 값에 따른 문자 추출
            cnt += 1 # 지정된 문자와 같으면 cnt의 값을 1씩 증가
    return cnt # cnt를 반환한다

print("입력된 문자의 빈도수:", count(user_str, user_ch))
```

## [문제 2번] 
#### (최장 공통 접두어) 두 문자열의 최장 공통 접두어를 반환하는 메소드를 작성하시오. 예를 들어, distance와 disinfection 의 최장 공통 접두어는 dis 이다. 이 메소드의 헤더는 다음과 같다. 
> def prefix(s1, s2):

#### 두 문자열에 공통 접두어가 없다면, 빈 문자열을 반환한다.
#### 사용자로부터 두 문자열을 입력받고 이들의 최장 공통 접두어를 출력하는 main 메소드를 작성하시오.
```python
# 사용자로부터 두 개의 문자열을 입력받아서 두 문자열을 리스트를 이용하여 첫 문자부터 비교를 시작하며
# 만약 같은 문자가 있다면 s3라는 리스트에 문자를 추가해준다

user_str1 = input("첫 번째 문자열 입력:")
user_str2 = input("두 번째 문자열 입력:")

def prefix(s1, s2): 
    s3 = [] # s3라는 빈 리스트를 선언한다
    if len(s2) >= len(s1): # 만약 문자열의 길이가 s2가 s1보다 크다면 s1까지만 문자열을 비교하면 되므로
        for i in range(len(s1)): # s1의 문자열 길이만큼 for문을 반복한다
            if s1[i] == s2[i]: # 리스트를 이용하여 만약 s1에서 추출한 문자와 s2에서 추출한 문자와 같다면
                s3 += s1[i] # s3의 리스트에 s1의 문자를 추가해준다
        return s3 # 리스트를 반환해준다
    else: # 만약 문자열의 길이가 s1이 s2보다 크다면 s2까지만 문자열을 비교하면 되므로
        for i in range(len(s2)): # s2의 문자열 길이만큼 for문을 반복한다
            if s1[i] == s2[i]: # 마찬가지로 리스트를 이용하여 추출한 문자를 비교한다
                s3.append(s2[i]) # 리스트가 가지고 있는 메소드이다. 문자를 추가해주는 기능이 있다
        return s3

def main(): # main 메소드를 생성
    LongDistanceCommonString = prefix(user_str1, user_str2) # prefix 메소드에서 반환된 s3 리스트 값을 
                                                               # LongDistanceCommonString이라는 변수에 할당
    print("최장 공통 접두어:", end = '')
    for i in range(len(LongDistanceCommonString)): # LongDistanceCommonString의 길이만큼 for문을 이용하여반복
        print(LongDistanceCommonString[i], end = '') # LongDistanceCommonString의 문자를 띄어쓰기 없이 출력해준다

main() # main 메소드 호출
```

## [문제 3번]
#### (기업 업무:ISBN-13 검사) ISBN-13은 도서 식별을 위한 새로운 표준이다. 이 표준은 13자리 숫자, _d0d1d2d3d4d5d6d7d10d11d12d13_ 을 사용한다. 마지막 자리 숫자 d13은 체크섬이며, 다른 자리의 숫자들을 이용하여 다음의 수식에 의하여 계산된다.
#### 체크섬이 10이면, 마지막 자리를 0으로 설정한다. 입력값은 문자열로 읽어야 한다.
```python
user_d = input("ISBN-13의 첫 12자리를 문자열로 입력하세요: ") # 사용자로부터 ISBN의 12자리를 입력 받는다

odd_cnt = 0 # 인덱스 값의 홀수 번호를 세주는 변수
even_cnt = 0 # 인덱스 값의 짝수 번호를 세주는 변수
for x in range(len(user_d)): # 사용자로부터 입력받은 문자열의 길이의 범위만큼 반복한다
    if x % 2 != 0: # 만약 인덱스 번호가 홀수이면
        odd_cnt += 3 *int(user_d[x]) # 홀수 변수의 값에 입력받은 문자열의 리스트 원소의 값을 정수형으로 변환하여 3만큼 곱해서 더해준다
    else: # 만약 인덱스 번호가 짝수이면
        even_cnt += int(user_d[x]) # 짝수 변수의 값에 입력받은 문자열의 리스트 원소의 값을 정수형으로 변환하여 더해준다

checkSum = 10 - (even_cnt + odd_cnt) % 10 # 체크섬은 ISBN-13번이며, 수식에 의해 계산된 결과이다
if checkSum == 10: # 만약 체크섬이 10이면
    ISBN_13 = 0 # 마지막 자리는 0이고
else: # 만약 체크섬이 10이 아니면
    ISBN_13 = checkSum # 마지막 자리는 체크섬이다
user_d += str(ISBN_13) # ISBN_13의 값을 다시 문자열로 형변환하여 user_d의 문자열에 추가해준다
print("ISBN-13 번호는", eval(user_d), "입니다.") # 이를 다시, eval을 이용하여 숫자로 표현하여 출력한다
```

## [문제 4번]
#### (숫자의 빈도수) 1과 100 사이의 정수들을 입력받고 각 정수의 빈도수를 세는 프로그램을 작성하시오.
```python
num = input("1과 100 사이의 정수를 입력하세요:") # 사용자로부터 정수를 띄어쓰기해서 받는다. 이때, 문자열 형태로 받는다.
number = num.split() # split 메소드를 이용하면 문자열을 리스트로 분할해줄 수 있다
cnt = 101 * [0] # 1부터 100까지의 정수의 빈도수를 각각 세줄 100개의 리스트를 생성해준다.
                # 이때, 왜 101개를 했냐면, 크기가 n인 리스트의 인덱스는 0부터 시작하여 n-1까지이므로
                # 101을 곱해야 정수 100을 담을 수 있는 인덱스가 생성이 된다.
                # 가독성을 높이기 위해 인덱스가 1이면 정수 1의 빈도수의 값이라고 설정해주었다.

for i in range(1, 101): # 사용자로부터 받은 정수들을 비교해주기 위해 1부터 100까지 하나씩 비교해주기 위해
                        # for문을 이용하여 1부터 100까지 반복해준다.
    for j in range(len(number)): # for문 중첩을 이용하여 사용자로부터 받은 문자열을 리스트로 변환하여 그 리스트의 길이만큼 반복한다.
        if eval(number[j]) == i: # 만약, 숫자로 표현된 리스트의 값이 1부터 100까지 하나씩 비교해서 같다면
            cnt[i] += 1 # cnt의 인덱스에 일치하는 값을 넣어서 일치된 값의 cnt의 값을 1만큼 증가시켜준다.
    if cnt[i] == 0: # 만약 cnt의 값이 0이라면 
        continue # 계속해서 반복해주고
    else: # 만약 cnt의 값이 1이상이라면
        print(i,"-",cnt[i],"번 나타납니다.") # 문제의 예시와 같은 시그니처로 출력해준다.
```

## [문제 5번]
#### (가장 작은 원소의 인덱스 찾기) 정수 리스트에서 가장 작은 원소의 인덱스를 반환하는 함수를 작성하시오. 만약 그러한 원소가 두 개 이상이라면, 그 중에 가장 작은 인덱스를 반환한다. 다음의 헤더를 사용하라.
> def indexOfSmallestElement(lst):

#### 사용자로부터 정수 리스트를 입력받고, 위 함수를 호출하여 가장 작은 원소의 인덱스를 찾아서 그 인덱스를 출력하는 테스트 프로그램을 작성하시오.
```python
def indexOfSmallestElement(lst):
    for i in range(len(lst)):
        min_num = min(lst) # 리스트에 있는 min 메소드를 이용하여 가장 작은 원소를 구한다
    return lst.index(min_num) # 찾은 원소의 값을 가지고 있는 인덱스 값을 구하기 위해 index 메소드를 이용한다

user_str = input("정수 리스트 입력:") # 사용자로부터 리스트를 문자열의 형태로 입력 받는다
change_lst = user_str.split() # 입력 받은 문자열을 리스트로 변환해준다
change_integer_lst = list(map(int, change_lst)) # map 메소드를 이용하여 문자열 리스트를 정수형 리스트로 변환해준다

print("정수 리스트:", change_integer_lst)
print("가장 작은 원소:", min(change_integer_lst))
print("가장 작은 원소의 인덱스:", indexOfSmallestElement(change_integer_lst))
```

## [문제 6번]
#### (랜덤 숫자 선택기) random.shuffle(lst)를 사용하여 리스트 내의 숫자들을 랜덤하게 섞을 수 있다. random.shuffle(lst)를 사용하지 않고 리스트 내의 숫자들을 랜덤하게 섞고 그 리스트를 반환하는 자신만의 함수를 작성하시오. 다음의 함수 헤더를 사용하라.
> def shuffle(lst):

#### 사용자로부터 숫자 리스트를 입력받고, 위 함수를 이용하여 랜덤하게 섞여진 숫자들을 출력하는 테스트 프로그램을 작성하시오.
```python
# 리스트 안의 원소들을 랜덤하게 섞는 문제
# 인덱스의 위치를 랜덤하게 두 개를 잡고 리스트의 길이만큼 섞는다

import random # random 모듈을 import한다

def shuffle(lst):
    for i in range(len(lst)): # 입력받은 리스트의 길이만큼 반복한다
        idx1 = random.randrange(len(lst)) # 랜덤하게 리스트의 인덱스 중 하나를 뽑는다
        idx2 = random.randrange(len(lst)) # 랜덤하게 리스트의 인덱스 중 하나를 뽑는다
        if idx1 != idx2: # 인덱스가 동일하지 않을 때
            lst[idx1], lst[idx2] = lst[idx2], lst[idx1] # 리스트의 원소를 서로 바꾼다
    return lst        
    
user_lst = input("리스트를 입력:") # 사용자로부터 숫자 리스트를 문자열의 형태로 입력받는다
change_lst = user_lst.split() # split 메소드를 이용하여 문자열을 리스트의 형태로 바꾼다
change_integer_lst = list(map(int, change_lst)) # map 메소드를 이용하여 문자열 리스트를 정수형 리스트로 변환해준다

for i in range(5):
    print("랜덤 리스트", i + 1, shuffle(change_integer_lst))
```
