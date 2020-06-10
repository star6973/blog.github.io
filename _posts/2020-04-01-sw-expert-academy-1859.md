---
layout: post
title: SW Expert Academy [1859]
date: 2020-04-01 13:00:00-14:00:00
categories: ProblemSolving
tag: ProblemSolving
---

# [1859] 백만 장자 프로젝트
> [SW Expert Academy](https://swexpertacademy.com/main/main.do){: target="_blank"}

## my solution
```python
for t in range(int(input())):
    N = int(input())
    price = list(map(int, input().split()))

    maxPrice = price[-1]
    cnt = 0
    tot = 0
    minPrice = 0
    for i in range(len(price)-2, -1, -1):
        if maxPrice <= price[i]:
            tot += maxPrice * cnt - minPrice
            cnt = 0
            minPrice = 0
            maxPrice = price[i]
        else:
            minPrice += price[i]
            cnt += 1

    tot += maxPrice * cnt - minPrice

    print('#{} {}'.format(t+1, tot))
```

## others solution
```python
koko = int(input())
for qwer in range(koko):
    cc = int(input())
    price = list(map(int, input().split()))
    tot = 0
    minPrice = price[-1]
    for x in range(len(price) - 1, -1, -1):
        if price[x] <= minPrice:
            tot += minPrice - price[x]
        else:
            minPrice = price[x]
    print('#{} {}'.format(qwer + 1, tot))
```
