---
layout: post
title: "Python Programming [Exercise 9]"
date: 2017-06-09 09:00:00-18:00:00
categories: Language
tag: Language
---

## [문제 1번] 
#### (텍스트 제거) 텍스트 파일로부터 특정 문자열이 나타난 모든 부분을 제거하는 프로그램을 작성하시오. 프로그램은 사용자로부터 파일명과 제거할 문자열을 입력받는다.
```python
# 파일 입출력 예외 처리
try:
    fileName = input("파일이름을 입력하세요:").strip()
    inFile = open(fileName, "r")
except IOError: # 파일 입출력 예외 발생시
    print("file open fail!")
else: # 예외가 발생되지 않을시
    print("file open success!")
finally: # 예외가 발생하건 안하건 실행되는 finally절
    print("finally절이 실행되었습니다.")
    
# 사용자가 입력해준 파일을 불러서 하나의 문자열에 저장하기
copyFileString = inFile.read()

# 파일 닫기
inFile.close()
    
# 저장된 문자열에서 제거할 문자열 제거하기(replace를 이용)
removeString = input("제거할 문자열을 입력하세요:")
copyFileString = copyFileString.replace(removeString, "")
    
# 다시 쓰기 모드로 열기(이전의 내용이 사라짐)
inFile = open(fileName, "w")

# 내용 쓰기(저장된 문자열을 저장)
inFile.write(copyFileString)

# 파일 닫기
inFile.close()
    
# 저장된 내용을 다시 읽기 모드로 불러오기
modifyFile = open(fileName, "r")
modifyString = modifyFile.read()

print()

# 내용 불러오기
print(modifyString)
        
print("완료")
```

## [문제 2번] 
#### (파일에 문자, 단어 및 행의 개수 세기) 파일에 포함된 문자, 단어 및 행의 개수를 세는 프로그램을 작성하시오. 단어는 공백으로 분리된다. 프로그램은 사용자로부터 파일이름을 입력받는다. 
```python
fileName = input("파일이름을 입력하세요:")
infile = open(fileName, "r")

countChar = 0    # 문자 개수
countStr = 0     # 단어 개수
countLines = 0   # 행 개수

for line in infile:
    # 행의 개수 세기
    countLines += 1
    
    # 문자 개수 세기(특수문자, 공백문자 포함)
    countChar += len(line) # 한 행의 문자열의 길이는 문자의 개수
    
    # 단어 개수 세기(공백의 개수를 세기)
    for string in line:
        if string.isspace():
            countStr += 1

# 파일 닫기
infile.close()

print("문자", countChar, "개")
# 두 개의 단어이므로 공백의 개수에서 1을 더해준다
print("단어", countStr + 1, "개")  
print("행", countLines + 1, "개") # 마지막 행은 빈 문자열이므로 세주지 않았기 때문에 1을 더해준다
```

## [문제 3번]
#### 문자열 대체하기) 파일에 문자열을 대체하는 프로그램을 작성하시오.
#### 프로그램은 반드시 사용자로부터 파일명, 이전 문자열과 새로운 문자열을 입력받아야 한다.
```python
# 1번 문제와 비슷한 맥락으로 코드를 짠다
# 파일 입출력 예외 처리
try:
    fileName = input("파일이름을 입력하세요:").strip()
    outFile = open(fileName, "r", encoding = 'UTF8') # 한글 파일을 읽기 위해 UTF-8로 인코딩 해준다
except IOError: # 파일 입출력 예외 발생시
    print("file open fail!")
else: # 예외가 발생되지 않을시
    print("file open success!")
finally: # 예외가 발생하건 안하건 실행되는 finally절
    print("finally절이 실행되었습니다.")
    
# 사용자가 입력해준 파일을 불러서 하나의 문자열에 저장하기
copyString = outFile.read()

# 파일 닫기
outFile.close()
    
# 저장된 문자열에서 제거할 문자열 제거하기(replace를 이용)
beforeString = input("교체될 이전 문자열을 입력하세요:")
afterString = input("이전 문자열을 대체할 새로운 문자열을 입력하세요:")
copyString = copyString.replace(beforeString, afterString)
    
# 다시 쓰기 모드로 열기(이전의 내용이 사라짐)
outFile = open(fileName, "w")

# 내용 쓰기(저장된 문자열을 저장)
outFile.write(copyString)

# 파일 닫기
outFile.close()
    
# 저장된 내용을 다시 읽기 모드로 불러오기
replaceFile = open(fileName, "r")
replaceString = replaceFile.read()

print()

# 내용 불러오기
print(replaceString)
        
print("완료되었습니다")
```

## [문제 4번]
#### (단어 개수 세기) map을 이용하여 단어 빈도수를 센다. 하지만 우리는 map을 배우지 않았기에 다른 방법도 사용 가능하다.
```python
def main():
    # 파일 열기(쓰기 모드)
    userFile = open("userFile.txt", "w")
    while True:
        userFile.write(input("입력:"))
        finish = eval(input("입력을 마치시겠습니까(1 = Yes, 0 = No)"))
        # 반복문 탈출 조건
        if finish == 1:
            break
        else: 
            continue
            
    # 파일 닫기
    userFile.close()
    
    # 파일 열기(읽기 모드)
    openFile = open("userFile.txt", "r")
    # 파일의 내용 저장(문자열)
    openStr = openFile.read()
    # 문자열 내의 공백문자, 특수문자 처리
    Str = replacePunctuations(openStr)

    # 단어 딕셔너리 생성
    countWords = {}
    
    # 문자열을 공백을 기준으로 잘라서 행으로 만들기
    line = Str.split()
    
    # 공백을 기준으로 자른 행안에서
    for word in line:
        # 만약 단어를 저장하는 딕셔너리에 단어가 포함되어 있다면(중복되면)
        if word in countWords:
            # 개수를 하나 더 추가
            countWords[word] += 1
        # 만약 단어가 포함되어 있지 않다면(처음이라면)
        else:
            # 개수를 1로 만든다
            countWords[word] = 1
            
    print()
        
    # 단어 딕셔너리 출력
    for key in countWords:
        print(key + ":" + str(countWords[key]))

# 공백문자, 특수문자 등등을 공백문자로 통일해주는 메소드
def replacePunctuations(modifyStr):
    for ch in modifyStr:
        # 만약 문자가 다음 안에 포함되어 있다면
        if ch in "~@#$%^&*()_-+=~<>?/,.;:!{}[]|'\" ":
            # 공백문자로 대체하기
            modifyStr = modifyStr.replace(ch, " ")
    return modifyStr

main()
```

## [문제 5번]
#### (원소의 합 구하기) 숫자 n이 주어지면, 처음 n 개의 자연수로 구성된 집합의 가능한 모든 하위 집합에서 모든 원소의 합을 찾아야한다.
```python
# 부분집합의 모든 원소의 합 구하기

# 기본 풀이
# 1. 특정한 원소가 포함된 부분집합의 개수를 찾기
# 2. 원소와 부분집합의 개수를 곱하기
# 3. 1, 2에서 한 방법을 모든 원소에 적용하기
# 4. 구한 값을 모두 더하기

# 응용 풀이
# 모든 부분집합의 원소의 합에서 각각의 원소의 개수는 똑같다
# 특정한 원소 1개를 포함한 부분집합의 개수이기 때문에
# 집합 A
# 모든 부분집합의 원소의 합 = (A의 모든 원소의 합) x (특정한 원소 1개가 포함된 부분집합의 개수)
# 특정한 원소 1개가 포함된 부분집합의 개수 = 2 ^ (n - (특정 원소))

import math

n = eval(input("n의 값을 입력하시오:"))
print()

# 빈 리스트 생성(집합)
lst = []
# 1 ~ n 까지의 숫자 할당해주기
for i in range(1, n + 1):
    lst.append(i)

# lst의 모든 원소의 합
elementTot = sum(lst)
print("모든 원소의 합:", elementTot)

# 특정한 원소 1개가 포함된 부분집합의 개수(특정한 원소는 아무 원소나 상관없음)
uniqueElementCount = pow(2, (n - lst[0]))
print("특정 원소 1개가 포함된 부분집합의 개수:", uniqueElementCount)

# 모든 부분집합의 원소의 합
subsetTot = elementTot * uniqueElementCount

print("모든 부분집합의 원소의 합:", subsetTot)
```
