---
layout: post
title: SW Expert Academy [5356]
date: 2020-04-10 15:00:00-16:00:00
categories: ProblemSolving
tag: ProblemSolving
---

# [5356] 의석이의 세로로 말해요
> [SW Expert Academy](https://swexpertacademy.com/main/main.do){: target="_blank"}

## my solution
```python
from itertools import zip_longest

for t in range(int(input())):
    alpha = [''.join(map(str, input())) for _ in range(5)]
    # print(alpha)
    # print(*alpha)
    # print(list(zip(*alpha)))
    print(f'#{t+1}', end=' ')
    for i in zip_longest(*alpha, fillvalue=''):
        print(''.join(i), end='')
    print()
```

## others solution
```python
T = int(input())
for tc in range(1, T + 1):
    arr = [input() for _ in range(5)]
    l = [len(i) for i in arr]  # 각 문자열의 길이
    ml = max(l)  # 문자열 중 최대길이

    temp = ""
    for i in range(ml):
        for j in range(5):
            if l[j] > i:  # 자신의 문자열의 길이보다 열번호가 작으면 문자 추가하지 않음
                temp += arr[j][i]
    print("#%d %s" % (tc, temp))
```
