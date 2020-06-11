---
layout: post
title: SW Expert Academy [1216]
date: 2020-04-07 14:00:00-16:00:00
categories: ProblemSolving
tag: ProblemSolving
---

# [1216] 회문2
> [SW Expert Academy](https://swexpertacademy.com/main/main.do){: target="_blank"}

## my solution
```python
def isPalindrome(str):
    if str == str[::-1]:
        return True
    else:
        return False

for tc in range(10):
    n = int(input())
    table = [str(''.join(input().split())) for _ in [0] * 100]
    table_transpose = list(zip(*table))

    # 가로 회문 확인
    rowMaxPalin = 0
    for t in table:
        for idx in range(len(t)):
            for jdx in range(len(t)+1):
                if isPalindrome(t[idx:jdx]):
                    if rowMaxPalin <= len(t[idx:jdx]):
                        rowMaxPalin = len(t[idx:jdx])

    # 세로 회문 확인
    colMaxPalin = 0
    for table in table_transpose:
        for idx in range(len(table)):
            for jdx in range(idx+1, len(table)+1):
                if isPalindrome(''.join(table[idx:jdx])):
                    if colMaxPalin <= len(''.join(table[idx:jdx])):
                        colMaxPalin = len(''.join(table[idx:jdx]))

    print('#{0} {1}'.format(n, rowMaxPalin if rowMaxPalin >= colMaxPalin else colMaxPalin))
```

## others solution
```python
def check(table):
    cnt = 0
    for t in table:
        for idx in range(len(t)):
            for jdx in range(len(t) + 1):
                s = t[idx:jdx]
                if s == s[::-1]:
                    cnt = max(cnt, len(t[idx:jdx]))
    return cnt

for tc in range(10):
    n = int(input())
    table = [str(''.join(input().split())) for _ in [0] * 100]
    table_transpose = list(zip(*table))

    # 가로 회문 확인
    rowMaxPalin = check(table)
    # 세로 회문 확인
    colMaxPalin = check(table_transpose)

    print('#{0} {1}'.format(n, max(rowMaxPalin, colMaxPalin)))
```

```python
def pal(a,n):
    for i in range(100):
        for j in range(101-n):
            for k in range(n//2):
                if a[i][j+k] != a[i][j+n-1-k]:
                    break
                elif k + 1 == n//2:
                    return n
            for k in range(n//2):
                if a[j+k][i] != a[j+n-1-k][i]:
                    break
                elif k + 1 == n//2:
                    return n
    return 0
for _ in range(10):
    print(f"#{input()}",end=' ')
    a = [input() for _ in range(100)]
    for i in range(30,0,-1):
        n = pal(a,i)
        if n:
            print(n)
            break
```
