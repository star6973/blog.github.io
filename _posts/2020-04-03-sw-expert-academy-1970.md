---
layout: post
title: SW Expert Academy [1970]
date: 2020-04-03 13:00:00-14:00:00
categories: ProblemSolving
tag: ProblemSolving
---

# [1970] 쉬운 거스름돈
> [SW Expert Academy](https://swexpertacademy.com/main/main.do){: target="_blank"}

## my solution
```python
money = [50000, 10000, 5000, 1000, 500, 100, 50, 10]
T = int(input())
for t in range(T):
    coin = int(input())

    money_cnt = [0, 0, 0, 0, 0, 0, 0, 0]
    for idx, m in enumerate(money):
        money_cnt[idx] = coin // m
        coin %= m

    print('#{}'.format(t+1))
    for m in money_cnt:
        print(m, end=' ')
    print()
```
