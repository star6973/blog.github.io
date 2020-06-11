---
layout: post
title: SW Expert Academy [5515]
date: 2020-04-10 17:00:00-18:00:00
categories: ProblemSolving
tag: ProblemSolving
---

# [5515] 2016년 요일 맞추기
> [SW Expert Academy](https://swexpertacademy.com/main/main.do){: target="_blank"}

## my solution
```python
month = [31, 60, 91, 121, 152, 182, 213, 244, 274, 305, 335]

T = int(input())
for t in range(T):
    M, D = map(int, input().split())
    dday = 0
    if M == 1:
        dday = D
    else:
        dday = month[M-2] + D

    print('#{0} {1}'.format(t+1, (dday % 7 + 3) % 7))
```
