---
layout: post
title: "Python Programming [Day 2]"
date: 2017-05-14 09:00:00-18:00:00
categories: Language
tag: Language
use_math: true
---

# Python 4일만에 뽀개기 - Day 2

```angular2
# 한줄 주석
"""
파이썬은 인터프리터 언어이다.
파이썬은 객체지향 언어이다.
"""
'''
# 출력문이란?
# print(값이나 수식)
# print(값1, 값2) : 쉼표를 이용해서 연결해서 출력하기
# 한행에 결과값 출력하기
# print(값1 , end='공백또는 대체문자)
print(10+100)
print("파이썬", end=' ')
print("파이썬")
print("파이썬", end=' // ')
print("파이썬")
print('홍길동', '20살', '남자')

# \n 줄바꿈, \t 들여쓰기 , \\ \ 표시, \인용부호  => 이스케이프 문자
print('철수야! \n\n \t\t\t놀자')
print('\\어머님은 말하셨지 \n \" 밥은 먹었니?  \"  ')

# 변수
num1 = 100
txt1 = 'Python'
print(num1, txt1)
# 변수명 정의 방식
# 스네이크 방식 예) num_1
# 카멜 방식 예) pythonWord

# 파이썬 예약어 출력하기 - PPT 20
import keyword
print(keyword.kwlist)
print('키워드 갯수는? ', len(keyword.kwlist))

# 변수 삭제하기
# del 변수명
num2 = 1000
print('num2 = ', num2)
del num2
# NameError
# print('num2 = ', num2)

# 쉼표를 이용한 파이썬 변수 할당 => 파이썬스럽다.
# user1 = '김철수'
# user2 = '이영희'
user1, user2 = '김철수', '이영희'
print(user1, user2)

# 입력문 문법
# 변수명 = input(메세지)
# answer = input('파이썬은 인터프리터언어이다. (O, X) ... ')
# print('답은? ', answer  )

# 파이썬의 자료형
# 기본 자료형 (숫자형[실수, 정수, 8진수0o숫자, 16진수0x숫자..], 문자열형, 논리형)
# 집합 자료형 ( 리스트, 튜플, 집합, 딕셔너리 ...)
# 자료형 출력하기 - type(값이나 변수)

print(100, type(100))
print(3.14, type(3.14))
# 8진수, 16진수로 표시하지만 출력은 10진수로 표시
print(0x11, type(0x11))
print(0o10, type(0o10))

print('홍길동', type('홍길동'))
print(True, type(True))
print(False, type(False))

# False 인 값
# False, 0, '', "", [], (), {}, None
# bool(값이나 수식) => 데이타가 참인지 거짓인지 표시
print(bool(0))
print(bool(1))
print(bool(''))
print(bool('홍길동'))
print(bool(100))


input1 = input('첫 번째 숫자를 입력하세요: ')
input2 = input('두 번째 숫자를 입력하세요: ')

print(input1, '+', input2, '=', int(input1) + int(input2))
print(input1, '-', input2, '=', int(input1) - int(input2))
print(input1, '*', input2, '=', int(input1) * int(input2))
print(input1, '/', input2, '=', int(input1) / int(input2))


# 산술 연산자
print(100//3)
print(100/3)
print(100%7)
print(2**3)

# 대입 연산자(+=, -=, *=, /=)
num4 = 0
num4 += 1
print(num4)
num4 -= 10
print(num4)

# 비교 연산자(==, !=, <=, >=, <, >)
x = 10
y = 100
print(x==y)
print(x!=y)
print(x<=y)
print(x>=y)
print(x<y)
print(x>y)


# 논리 연산자: 논리합, 논리곱, 부정
# 논리곱: and 연산자 // 하나라도 거짓이면 모두 거짓
# 논리합: or 연산자  // 하나만 진실이면 모두 진실
# 부정: ! 연산자
# True and True => True
# True and False => False
# True or False => True
# False or False => False
x = 10
y = 100
print('x =', x)
print('y =', y)
print((x>y) and (x==y))
print((x>y) or (x==y))
print((x>y) or (x!=y))

'''

# 다행 문자열 만들기
message = '''점심시간은 12~1시입니다.
1시부터 오후 수업시간입니다.
'''
print(message)

# 문자열 연산자: * 반복, + 연결
class1 = '파이썬'
class2 = '자바'
class3 = class1 + class2

print('class1 = ', class1)
print('class2 = ', class2)
print('class3 = ', class3)
print('-' * 20)

# 문자열 인덱싱
# 문자열 슬라이싱
a = 'Life is too short, You need Python'
print(a[0])
print(a[12])
print(a[-1])
print(a.split())
print(a.splitlines())
print(a.upper())
print(a.lower())
print(a.join('[]'))
print(a.partition('too'))
print(a.replace('Python', 'Java'))

# 문자열 슬라이싱
# 문자열 변수[start:end:step]
print(a[:4])
print(a[-6:])
```

---

```angular2
m1 = 'abcdefghij'
print(m1)
# 홀수번째 찍기
print(m1[::2])
# 짝수번째 찍기
print(m1[1::2])

# % 포맷팅
a = 100
b = '홍길동'
c = 3.14567
print(a, b, c)
print('a값은? %d' % a)
print('a값은? %.2f' % a)
print('b값은? % s' % b)
print('c값은? %15.2f' % c)
ans = 'y'
print('ans값은? %c' % ans)

# 8진수 0o숫자값 => 10진수로 출력
# 16진수 0x숫자값 => 10진수로 출력
# 10진수 => 8진수 %o, 16진수 %x
num2 = 17
print('10진수 => %d' % num2)
print('8진수 => %o' % num2)
print('16진수 => %x' % num2)

# % 출력시에는 %%로 사용
today = 20
print('오늘의 습도는 %d%%입니다.' % today)

# format() 함수를 이용한 출력문
# print(' {인덱스 숫자1} {인덱스 숫자2} ... '.format(변수1, 변수2, ...))
user1 = '김정수'
user2 = '고은비'
print('사용자1: {0} 사용자2: {1}'.format(user1, user2))
print('사용자1: {1} 사용자2: {0}'.format(user1, user2))

# f''을 이용한 출력
# print(f' {변수1}, {변수2} ...')
print(f'학생1은 {user1} 학생2는 {user2}')
pi = 3.145678
print(f'파이값은 {pi}')

# 문자열 함수
# 문자열변수.함수명(옵션)
# 문자열.count(문자)
# 문자열.find(문자)
# 문자열.index(문자)

# '구분자 문자'.join(문자열)
sampleTxt = '''
Lorem Ipsum is simply dummy text of the printing and typesetting industry. Lorem Ipsum has been the industry's standard dummy text ever since the 1500s, when an unknown printer took a galley of type and scrambled it to make a type specimen book. It has survived not only five centuries, but also the leap into electronic typesetting, remaining essentially unchanged. It was popularised in the 1960s with the release of Letraset sheets containing Lorem Ipsum passages, and more recently with desktop publishing software like Aldus PageMaker including versions of Lorem Ipsum.
'''

print('a의 글자 갯수 = {}'.format(sampleTxt.count('a')))
print('et의 글자 갯수 = {}'.format(sampleTxt.count('et')))

sampleTxt2 = 'abcdef'
print(','.join(sampleTxt2))

# 대소문자 바꾸기
# 문자열변수.upper()
# 문자열변수.lower()
print(sampleTxt[:20].upper())
print(sampleTxt[:20].lower())

# 공백 없애기
# 문자열변수.strip()
# 문자열변수.lstrip()
# 문자열변수.rstrip()
sampleTxt3 = "      Python      "
print('*', sampleTxt3, '*')
print('*', sampleTxt3.strip(), '*')
print('*', sampleTxt3.lstrip(), '*')
print('*', sampleTxt3.rstrip(), '*')

# 문자열 교체
# 문자열변수.replace(문자열A, 문자열B)
sampleTxt4 = sampleTxt
sampleTxt4 = sampleTxt4.replace('Ipsum', 'IpSSSSS')
print(sampleTxt4)

# 문자열 => 리스트화
# 문자열변수.split()        // 공백을 기준으로 분리
# 문자열변수.split('구분자') // 구분자 문자를 기준으로 분리
sampleTxt5 = '가 나 다 라 마'
sampleTxt6 = '가:나:다:라:마'
print(sampleTxt5.split())
print(sampleTxt6.split(':'))
```

---

```angular2
# 집합형 자료
# 리스트/튜플/집합/딕셔너리
# CRUD

# 리스트의 생성1
# 리스트변수 = [value1, value2 ...]  // 초기값 설정

# 리스트의 생성2
# 리스트변수 = []
# 리스트변수.append(value)

listA = [100, 400, 5, -100]
listB = ['포도', '사과', '배']
listC = ['홍길동', '자곡동', '서울', 15, True]

print('listA = ', listA)
print('listB = ', listB)
print('listC = ', listC)
print('리스트의 데이터형은?', type(listA))

listD = []
print('listD = ', listD)
# 값 추가하기
listD.append(True)
listD.append(500)
print('listD = ', listD)

# 리스트 인덱싱: 0부터 시작, -1은 반대
# 리스트 슬라이싱
# 리스트변수[start:end:step]
print(listA[0])
print(listA[-1])
print(listA[-2])
print(listB[:1])
print(listB[1:])
print(listC[::2])
print(listC[1::2])

# 리스트 원소 교체하기
# 리스트변수[인덱스] = 교체값
print('listA', listA)
listA[0] = 500
print('listA', listA)

# 리스트 원소 추가
list1 = [1, 2, 3, 4, 5]
print('list1', list1)
# 방법1: 리스트변수.append(값)
list1.append(100)
print('list1', list1)
# 방법2: 리스트변수.insert(위치, 값)
list1.insert(0, 400)
print('list1', list1)

# 리스트 원소 삭제
# 리스트변수.remove(값)
# 리스트변수.pop(): 마지막 요소가 삭제되면서 리턴
# 리스트변수.pop(인덱스 위치)
# 리스트변수.clear() : 값 모두 삭제하고 빈 리스트
# del 리스트변수: 메모리에서 삭제
list1.remove(3)
print('list1', list1)
list1.pop(0)
print('list1', list1)
list1.pop()
print('list1', list1)
list1.clear()
print('list1', list1)
try:
    del list1
    print('list1', list1)
except NameError:
    print('값이 존재하지 않음')

# 리스트 전체 길이 확인하기
# len(리스트 변수)
# 리스트 값 정렬 - 조건: 정렬시키고자 하는 리스트의 데이터 형이 같아야 한다.
# 리스트변수.sort()
# 리스트변수.reverse()
import random
list2 = ['word', '강아지', 'rose', '파이썬', '100']
random.shuffle(list2)
print(list2)
list2.sort()
print(list2)
list2.reverse()
print(list2)

# 리스트변수.count(값): 값에 해당하는 갯수 세기
# 리스트변수.index(값): 값에 해당하는 위치값 출력하기
list3 = [100, 5, 7, 10, 100, 100]
print('100의 갯수는?', list3.count(100))
# 중복값이 존재하는 경우 첫 번째 위치값만 표시
print('100의 위치는?', list3.index(100))

# 자료형 변환
# 리스트 => 문자열    str(리스트변수)
# 문자열 => 리스트    문자열변수.split()
print('list3', list3)
txt = str(list3)
print('txt: ', txt)
print(txt[-1], type(txt))
txt2 = 'a, b, c'
list4 = txt2.split(',')
print('list4: ', list4)
list5 = [str(_) for _ in list3]
print('/'.join(list5))

# 2차원 리스트
list2D_1 = [[1, 2, 3],
            [4, 5, 6],
            [7, 8, 9]]
print(list2D_1[0][0])
```

---

```angular2
# 튜플
# 값 추가는 되지만, 삭제와 교체 불가능
# 튜플변수 = (값1, 값2, ...)
# 튜플변수 = (값, )
t1 = (1, 5, 80, -80)
# 원소갯수가 하나인 경우에는 쉼표 필수(그래야 튜플로 인식, 안그러면 문자열로 인식)
t2 = ('가')
t3 = ('가', )
print(t1, type(t1))
print(t2, type(t2))
print(t3, type(t3))

# 인덱싱과 슬라이싱
print(t1[:3])
print(t1[-1])
```

---

```angular2
# 딕셔너리 = 연관배열, 사전형 자료구조
# 딕셔너리 변수 = { 키1:값1, 키2:값2, ... }
# 키는 중복값이 있으면 안됨
# 각 원소의 호출: 딕셔너리 변수[키값]
dict1 = {'a':'apart', 'b':'banana', 'c':'cat', 'd':'drama'}
print(dict1)

# 키값으로 호출하기
print(dict1['a'])
print(dict1['d'])

dict2 = {100:'백', 400:'사백', 1000:'천'}
print(dict2)
print(dict2[400])

# 딕셔너리 원소 추가
# 딕셔너리 변수[키값] = 값
dict3 = {}
dict3['가'] = '가로수'
dict3['나'] = '나비'
print(dict3)

# 딕셔너리 중복키값 테스트
dict3['나'] = '나혼자 산다'
print(dict3)

# 딕셔너리 원소의 삭제
# 딕셔너리 원소
# 딕셔너리 변수.clear():
# del 딕셔너리 변수: 메모리에서 삭제
del dict3['나']
print(dict3)
dict3.pop('가')
print(dict3)
print(dict1)
print(dict1.clear())
print(dict1)

# 딕셔너리 함수
# 딕셔너리 변수.keys() => dict_keys => list()
# 딕셔너리 변수.values() => dict_values => list()
# 딕셔너리 변수.items() => dict_items => list()
dict4 = {'a': 'apart', 'b': 'banana', 'c': 'cat', 'd': 'drama', 'f':'five'}
print(dict4.keys())
print(dict4.values())
print(dict4.items())

# 딕셔너리의 키값만 모아서 리스트 구조로 변경하기
result1 = dict4.keys()
print(result1)
result1 = list(result1)
print(result1)

# 딕셔너리 값만 모아서 튜플 구조로 변경하기
result2 = dict4.values()
print(result2)
result2 = tuple(result2)
print(result2)
```

---

```angular2
# 집합 자료형(set)
# 순서가 없는 자료형이기 때문에 인덱싱이 불가능
# 집합의 특징 - 중복값을 허용하지 않는다

# 집합의 생성1
# 집합변수 = { 값1, 값2, ... }

# 집합의 생성2
# 집합변수 = set(리스트)

set1 = {100, 200, 500, '가', 'python'}
set2 = set([100, 500, 100, 200, 100, 'python', '가'])

print(set1)
print(set2)
print(set1==set2)

set3 = {10, 56, 34, 100, 1}
set4 = {34, 1, 90, 2, -10}
print('set3: ', set3)
print('set4: ', set4)

# 합집합
print(set3.union(set4))
print(set3 | set4)

# 교집합
print(set3.intersection(set4))
print(set3 & set4)

# 차집합
print(set3.difference(set4))
print(set3 - set4)

# 집합요소의 추가/삭제 - 인덱스가 없기 때문에 값으로 삭제해야 함
# 집합변수.add(값)
# 집합변수.update([값1, 값2, ...])
# 집합변수.remove(값)
# 집합변수.discard(값)
# 집합변수.clear()
set5 = set()
for i in range(10):
    set5.add(i)
print(set5)

set5.remove(0) # 집합에 없는 원소를 삭제하려고 하면 에러 발생
set5.discard(8) # 집합에 없는 원소를 삭제하려고 하면 그냥 무시하고 넘어감
print(set5)
set5.clear()
print(set5)
```