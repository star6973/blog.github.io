---
layout: post
title: "Python Programming [Exercise 8]"
date: 2017-06-08 09:00:00-18:00:00
categories: Language
tag: Language
---

## [문제 1번] 
#### (중복값 제거하기) 주어진 리스트에 대해서 중복값이 제거된 새로운 리스트를 반환하는 함수를 작성하시오. 다음의 함수 헤더를 사용하라.
> def eliminateDuplicates(lst):

#### 사용자로부터 정수 리스트를 입력받고, 위 함수를 호출하여 얻은 결과를 출력하는 테스트 프로그램을 작성하시오.
```python
def eliminateDuplicates(lst):
    eliminatelst = [] # 중복을 제거한 리스트를 담기 위한 리스트
    for i in lst: # i의 값은 매개변수로 받은 리스트의 원소 
        if i not in eliminatelst: # 만약 eliminatelst에 i의 값이 없다면
            eliminatelst.append(i) #  eliminatelst에 i의 값을 추가
    for i in range(len(eliminatelst)): # eliminatelst를 출력
        print(eliminatelst[i], end = " ")

user_lst = input("10개의 숫자를 입력하세요:");
change_user_string_lst = user_lst.split() # 입력된 문자열을 split으로 문자열 리스트로 변환
change_user_digit_lst = list(map(int, change_user_string_lst)) # 문자열 리스트를 정수형 리스트로 변환

print("중복을 제거한 고유한 숫자:", end = "")
eliminateDuplicates(change_user_digit_lst)
```

## [문제 2번] 
#### (시뮬레이션: 쿠폰 수집가 문제) 쿠폰 수집가 문제는 다양한 실전 응용을 가진 고전적인 통계 문제이다. 이 문제는 객체 집합에서 반복적으로 객체를 뽑고, 모든 객체가 적어도 한 번씩 선택되기 위해 얼마나 많은 시도가 필요한지를 알아내는 것이다. 이 문제를 52장의 뒤섞여진 카드로 구성된 카드팩에서 반복해서 카드를 뽑고 모든 종류의 카드가 선택되기까지 얼마나 많은 시도가 필요한지를 알아내는 문제로 변형해 보자. 뽑은 카드는 카드팩에 다시 넣는다고 가정한다. 4장의 카드를 얻기 위해 시도한 횟수를 시뮬레이션하며 뽑은 4장의 카드를 출력하는 프로그램을 작성하시오(1장의 카드가 두 번 뽑힐 수도 있다). 
```python
import random

NUM = 10 # 1 ~ 10
TYPE = ["Spade", "Clover", "Heart", "Diamond"] 
PICTURE = ["King", "Queen", "Jack"] 

# 트럼프 카드 만들기
# 트럼프의 개수는 총 52장
Trump = []

# NUM * TYPE의 형태로 40장, TYPE * PICTURE의 형태로 12장
NUM_TYPE = []
TYPE_PICTURE = []

for type_item in TYPE: # Spade(0), Clover(1), Heart(2), Diamond(3)
    for j in range(NUM): # 1 ~ 10(0 ~ 9)
        NUM_TYPE.append([type_item, j + 1]) # 리스트에 리스트를 추가하여 2차원 배열 생성, NUM_TYPE 형태의 카드 40장 생성
    for pic_item in PICTURE: # King(1), Queen(2), Jack(3) 
        TYPE_PICTURE.append([type_item, pic_item]) # TYPE_PICTURE 형태의 카드 12장 생성

Trump = NUM_TYPE + TYPE_PICTURE # 트럼프 카드는 NUM_TYPE 카드와 TYPE_PICTURE 카드를 합친 것

# 카드 셔플하기
random.shuffle(Trump)



# 다시 생각해본 코드
# 각각의 모양에 대해 True와 False의 값이 담긴 리스트를 생성
# 카드를 뽑을 때 선택된 모양이 나올 경우 True
# 선택된 모양이 나오지 않을 경우 다시 뽑기
selectCard = [] # 선택된 카드가 담기는 리스트
boolSelect = [False, False, False, False] # 각각 Spade, Clover, Heart, Diamond
bool_count =  4 * [0] # boolSelect의 변환 횟수
total_count = 0 # 전체 시행 횟수

while True: # 무한 루프를 돌린다(조건은 boolSelect의 원소들이 모두 True일때 탈출한다)
    randomSelect = random.randrange(len(Trump)) # 랜덤으로 카드를 하나 뽑는다
    total_count += 1 # 전체 시행 횟수를 한 번 증가한다(이 위치에 있으면 카드를 뽑을 때마다 한 번씩 카운트 된다)
    
    # while문 탈출 조건
    if boolSelect[0] == True and boolSelect[1] == True and boolSelect[2] == True and boolSelect[3] == True:
        for i in range(len(selectCard)):
            print(selectCard[i][0], end = " ") 
            print(selectCard[i][1]) # 뽑힌 카드를 출력
        print("뽑은 횟수:", total_count) # 전체 시행 횟수를 출력
        break # 반복문 탈출
        
    if Trump[randomSelect][0] == "Spade": # 만약 모양이 Spade가 나온다면
        if boolSelect[0] == True: # boolSelect의 첫 번째 값(Spade일 때만)이 이미 True이면
            continue # 카드를 다시 뽑는다
        else: # 첫 번째 값이 False이면
            boolSelect[0] = True # boolSelect의 첫 번째 값을 True로 변환
            if bool_count[0] == 0: # 만약 bool_count의 첫 번째 값(Spade일 때만)이 0이라면
                selectCard.append(Trump[randomSelect]) # 선택된 카드를 준비된 리스트에 추가해준다
                bool_count[0] += 1 # bool_count의 첫 번째 값이 1로 증가한다
            else: # bool_count의 첫 번째 값이 0이 아니라면
                continue # 카드를 다시 뽑는다
    # 마찬가지로 Clover부터해서 Diamond까지 같은 방법을 이용해준다
    elif Trump[randomSelect][0] == "Clover":
        if boolSelect[1] == True:
            continue
        else:
            boolSelect[1] = True
            if bool_count[1] == 0:
                selectCard.append(Trump[randomSelect])
                bool_count[1] += 1
            else:
                continue
    elif Trump[randomSelect][0] == "Heart":
        if boolSelect[2] == True:
            continue
        else:
            boolSelect[2] = True
            if bool_count[2] == 0:
                selectCard.append(Trump[randomSelect])
                bool_count[2] += 1
            else:
                continue
    elif Trump[randomSelect][0] == "Diamond":
        if boolSelect[3] == True:
            continue    
        else:
            boolSelect[3] = True
            if bool_count[3] == 0:
                selectCard.append(Trump[randomSelect])
                bool_count[3] += 1
            else:
                continue
```

## [문제 3번]
#### (패턴 인식: 연속된 네 개의 동일 숫자) 어떤 리스트가 동일한 네 숫자를 연속으로 갖고 있는지를 검사하는 함수를 작성하시오. 함수 헤더는 다음과 같다.
> def isConsecutiveFour(values):

#### 사용자로부터 일련의 정수들을 입력받고, 동일한 네 숫자를 연속으로 갖고 있는지를 검사하는 테스트 프로그램을 작성하시오.
```python
def isConsecutiveFour(values):
    cnt = 0 # 연속된 숫자를 확인하기 위해 세주는 변수
    for x in range(len(values)): # values 리스트 길이만큼
        if x+1 == len(values): # 연속된 숫자를 확인하기 때문에 리스트 길이가 초과되면 안되므로
                break          # 만약 뒤에 숫자가 초과하면 함수 종료
        if values[x] == values[x + 1]: # 만약 연속된 숫자가 동일하다면
            cnt += 1                 # cnt의 값을 1 증가
 
    if cnt == 3: # 만약 연속된 숫자가 4개라면 두 개씩 비교하므로 3이 카운트 된다
        print("연속입니다")
    else:
        print("불연속입니다")
        
user_lst = input("10개의 숫자를 입력하세요:");
change_user_string_lst = user_lst.split() # 입력된 문자열을 split으로 문자열 리스트로 변환
change_user_digit_lst = list(map(int, change_user_string_lst)) # 문자열 리스트를 정수형 리스트로 변환

isConsecutiveFour(change_user_digit_lst)
```

## [문제 4번]
#### (행렬의 주대각선 합하기) 다음 헤더를 사용하여 정수로 이루어진 n x n 행렬의 주대각선에 포함된 모든 숫자의 합을 구하는 함수를 작성하시오.
> def sumMajorDiagonal(m):

#### 주대각선이란 정방행렬의 왼쪽 위 모서리에서 오른쪽 아래 모서리를 가로지르는 대각선을 말한다. 4 x 4 행렬을 읽고 주대각선에 있는 모든 원소의 합계를 출력하는 프로그램을 작성하시오.
```python
matrix = [] # n x n 행렬

# 사용자로부터 n의 값을 입력
n = eval(input("n의 값을 입력하세요:"))

# n x n 리스트의 원소 할당
for i in range(n):
    print(n, "x", n, "행렬의", i + 1, "번의 행에 대한 원소를 입력하세요:", end = "")
    row = list(map(float, input().split())) # 입력받은 문자열을 실수형 리스트로 바꾼다
    matrix.append(row) # matrix에 원소를 하나씩 추가해준다
    
# 주대각선에 있는 모든 원소의 합계 출력
def sumMajorDiagonal(m):
    totalDiagonal = 0 # 주대각선에 포함된 원소의 합계
    for i in range(len(m)):
        totalDiagonal += m[i][i] # 주대각선의 원소만 더하므로
        
    print("주대각선에 포함된 원소의 합계는", totalDiagonal, "입니다.")

sumMajorDiagonal(matrix)
```

## [문제 5번]
#### (대수학: 행렬의 곱) 두 행렬을 곱하는 함수를 작성하시오. 함수의 헤더는 다음과 같다.
> def multiplyMatrix(a, b):

#### 행렬 a와 행렬 b를 곱하기 위해서, 행렬 a의 열의 개수와 행렬 b의 행의 개수가 같아야 하고 두 행렬의 원소는 동일한 또는 호환 가능한 데이터 타입이어야 한다. c를 행렬 곱의 결과라고 하자. 행렬 a의 열의 크기는 n이라고 가정한다. 각 원소 cij는 ai1 * b1j + ai2 * b2j + ... + ain * bnj 이다. 예를 들어 3 x 3 행렬 a와 b에 대한 c는 다음과 같다.
#### 단, cij = ai1 * b1j + ai2 * b2j + ai3 * b3j 이다.
#### 사용자로부터 두 행렬을 입력받고 두 행렬의 곱을 출력하는 예제 프로그램을 작성하시오.
```python
# 몇 차원 리스트를 만들지 결정하기
n = eval(input("n의 값을 입력하세요:"))

# 행렬 1의 리스트 원소 할당하기 전에 일차원 정수형 리스트 만들기
print("행렬 1을 입력하세요:", end = "")
rowFirst = list(map(float, input().split()))
        
# 행렬 1의 리스트 원소 할당하기
# 먼저 빈 리스트 만들기(n x n 행렬)
matrixFirst = []
for row in range(n):
    matrixFirst.append([])
    for col in range(n):
        matrixFirst[row].append([]) 

k = 0 # rowFirst의 인덱스 변수
# matrixFirst의 빈 리스트 값 할당하기
for i in range(n):
    for j in range(n):
        if matrixFirst[i][j] == []:
            matrixFirst[i][j] = rowFirst[k]
        k += 1

# 행렬 2의 리스트 원소 할당하기 전에 일차원 정수형 리스트 만들기
print("행렬 2을 입력하세요:", end = "")
rowSecond = list(map(float, input().split()))
        
# 행렬 2의 리스트 원소 할당하기
# 먼저 빈 리스트 만들기(n x n 행렬)
matrixSecond = []
for row in range(n):
    matrixSecond.append([])
    for col in range(n):
        matrixSecond[row].append([]) 
    
k = 0 # rowSecond의 인덱스 변수
# matrixSecond의 빈 리스트 값 할당하기
for i in range(n):
    for j in range(n):
        if matrixSecond[i][j] == []:
            matrixSecond[i][j] = rowSecond[k]
        k += 1

print()
# 두 행렬의 곱을 나타내는 함수
def multiplyMatrix(a, b):
    matrixThird = [] # 계산 결과를 나타내는 리스트
    tot = 0 # 원소끼리의 곱의 합(각 행마다의 원소를 지정)
    # matrixThird의 빈 리스트 값 할당하기
    for row in range(n): # 행은 n의 길이만큼
        matrixThird.append([]) # 각 행마다 빈 리스트 추가
        for col in range(n): # 열은 n의 길이만큼
            # 문제에서 나오는 공식 사용
            for i in range(n): # i가 n의 값에 따라서 증가함
                tot += a[row][i] * b[i][col]
            matrixThird[row].append(format(tot, ".1f")) # 각 행마다 tot의 값을 추가, 소수점 첫 째자리까지만
            tot = 0 # 다시 tot을 초기화해야 다음 열의 값을 넣어 줄 수 있다
            
    # matrixThird 리스트 출력하기
    print("결과 리스트")
    for row in range(len(matrixThird)):
        print("(", end = " ")
        for col in range(len(matrixThird)):
            print(matrixThird[row][col], end = " ")
        print(")")

multiplyMatrix(matrixFirst, matrixSecond)
```
