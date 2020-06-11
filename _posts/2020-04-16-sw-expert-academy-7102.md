---
layout: post
title: SW Expert Academy [7102]
date: 2020-04-16 14:00:00-15:00:00
categories: ProblemSolving
tag: ProblemSolving
---

# [7102] 준홍이의 카드놀이
> [SW Expert Academy](https://swexpertacademy.com/main/main.do){: target="_blank"}

## my solution
```python
import itertools

for t in range(int(input())):
    N, M = map(int, input().split())
    n = [i for i in range(1, N+1)]
    m = [j for j in range(1, M+1)]

    sumList = [0 for i in range(N+M+1)]
    print(sumList)
    for prod in list(itertools.product(n, m)):
        sumList[sum(prod)] += 1
    print(sumList)

    maxSum = max(sumList)
    print('#{}'.format(t+1), end=' ')
    for i, s in enumerate(sumList):
        if maxSum == s:
            print(i, end=' ')
```
