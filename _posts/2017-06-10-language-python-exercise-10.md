---
layout: post
title: "Python Programming [Exercise 10]"
date: 2017-06-10 09:00:00-18:00:00
categories: Language
tag: Language
---

## [문제 1번] 
#### (숫자 빈도수 세기) 사용자가 입력하는 정수를 읽고 이 중에 가장 많이 나온 숫자를 찾아 출력하는 프로그램을 작성하시오. 예를 들어, 2 3 40 5 4 -3 3 3 2 0을 입력하면, 숫자 3이 가장 많이 나온 숫자이다. 사용자는 한 행에 모든 숫자들을 입력하며 입력하는 정수의 개수에 제한은 없다. 가장 많이 나온 숫자의 개수가 여러 개일 경우, 이들 숫자 모두를 찾아야 한다. 예를 들어, 9 30 3 9 3 2 4를 입력했을 경우 9와 3이 두 번씩 가장 많이 나온다. 이 경우, 9와 3을 모두 찾아 결과로 출력해야만 한다.
```python
user_lst = list(map(int, input("숫자 입력:").split())) # 입력받은 문자열을 split() 메소드와 map 메소드를 이용하여 정수형 리스트 형태로 바꿔줌
user_dic = {} # 빈 딕셔너리 생성

for digit in user_lst: 
    # 만약 단어를 저장하는 딕셔너리에 단어가 포함되어 있다면(중복되면)
    if digit in user_dic:
        # 개수를 하나 더 추가
        user_dic[digit] += 1
    # 만약 단어가 포함되어 있지 않다면(처음이라면)
    else:
        # 개수를 1로 만든다
        user_dic[digit] = 1
        
maxNum = [] # 최대 빈도수를 가진 key값을 넣어줄 리스트

maxCnt = max(user_dic.values()) # 가장 큰 빈도수는 딕셔너리의 값 중 가장 큰 수

for key in user_dic: # 딕셔너리 원소 중 key값에서 
    if maxCnt == user_dic[key]: # maxCnt와 같은 값이면
        maxNum.append(key) # 최대 빈도수 리스트에 추가
        
print("가장 많은 빈도수를 가진 숫자:", end = ' ')
for x in maxNum:
    print(x, end = ' ')
```

## [문제 2번] 
#### (자음과 모음 수 세기) 사용자로부터 텍스트 파일명을 입력받고 이 파일 내에 있는 영문자 자음과 모음의 수를 각각 출력하는 프로그램을 작성하시오. 모음 A, E, I, O, U를 저장하기 위해 세트를 사용하라. 
```python
# 예외처리를 이용하여 파일 입출력 확인
try:
    fileName = input("파일 이름 입력:").strip()
    openFile = open(fileName, "r")
except IOError:
    print("File open fail")
else:
    print("File open success")
finally:
    print("finally절이 실행되었습니다")
    
copyString = openFile.read().upper() # 모든 문자열 대문자로 변환

StringList = list(copyString.split()) # 문자열 리스트 생성

StringSet = set() # 문자열 리스트를 세트로 변환해주기 위한 빈 세트 생성

collection = set(['A', 'E', 'I', 'O', 'U']) # 모음 집합

StringCollectionCnt = 0  # 모음의 개수를 세주는 변수

for word in StringList: # 문자열 리스트의 원소 중에서
    for char in word: # 그 원소의 문자 중에서
        StringSet.add(char) # 세트의 원소에 문자를 추가
        if char == 'A' or char == 'E' or char == 'I' or char == 'O' or char == 'U': # 만약 문자가 모음이면
            collectionCnt += 1 # 모음의 개수 1씩 증가
            
print("모음의 수:", collectionCnt)
print("모음 집합:", StringSet.intersection(collection)) # 파일 내에 있는 모음의 집합
```

## [문제 3번]
#### (인접 문자 출력) 문자열을 지정하면 한 문자 또는 두 개의 인접 문자 다음에 문자열을 분할하고 한 문자 또는 두 문자로만 구성된 문자열을 만들 수 있습니다. 가능한 모든 결과를 출력하시오.
```python
stringList = list(input(""))

def func1(stringLst):
    lst1 = []
    lst2 = []
    
    for i in range(len(stringList)):
        for j in range(len(stringList)):
            lst2 = []
            if i == j:
                continue
            else:
                if j == i + 1 and j <= len(stringList):
                    lst2.append(stringList[i] + stringList[j])
                    for i in range(len(stringList)):
                        lst2.append(stringList[i])
                else:
                    continue
            lst1.append(lst2)    
    return lst1

func1(stringList)
```
