---
layout: post
title: SW Expert Academy [1945]
date: 2020-04-02 11:00:00-12:00:00
categories: ProblemSolving
tag: ProblemSolving
---

# [1945] 간단한 소인수분해
> [SW Expert Academy](https://swexpertacademy.com/main/main.do){: target="_blank"}

## my solution
```python
T = int(input())
for t in range(T):
    N = int(input())
    cnt = [0, 0, 0, 0, 0]

    while N % 2 == 0:
        cnt[0] += 1
        N = N // 2

    while N % 3 == 0:
        cnt[1] += 1
        N = N //3

    while N % 5 == 0:
        cnt[2] += 1
        N = N // 5

    while N % 7 == 0:
        cnt[3] += 1
        N = N // 7

    while N % 11 == 0:
        cnt[4] += 1
        N = N // 11

    print('#{}'.format(t+1), end=' ')
    for c in cnt:
        print(c, end=' ')
    print()
```
