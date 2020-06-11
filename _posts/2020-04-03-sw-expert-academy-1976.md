---
layout: post
title: SW Expert Academy [1976]
date: 2020-04-03 15:00:00-16:00:00
categories: ProblemSolving
tag: ProblemSolving
---

# [1976] 시각 덧셈
> [SW Expert Academy](https://swexpertacademy.com/main/main.do){: target="_blank"}

## my solution
```python
T = int(input())
for t in range(T):
    time = list(map(int, input().split()))

    hour = time[0] + time[2]
    min = time[1] + time[3]

    if min >= 60:
        hour += min // 60
        min %= 60
    if hour >= 13:
        hour %= 12
    if hour == 0:
        hour = 12

    print('#{} {} {}'.format(t+1, hour, min))
```
