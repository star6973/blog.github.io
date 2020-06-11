---
layout: post
title: SW Expert Academy [4299]
date: 2020-04-13 16:00:00-17:00:00
categories: ProblemSolving
tag: ProblemSolving
---

# [4299] 태혁이의 사랑은 타이밍
> [SW Expert Academy](https://swexpertacademy.com/main/main.do){: target="_blank"}

## my solution
```python
day = [2011, 11, 11, 11, 11]
for t in range(int(input())):
    D, H, M = map(int, input().split())

    d_D = (D - day[2]) * 24 * 60
    d_H = (H - day[3]) * 60
    d_M = M - day[4]

    answer = d_D + d_H + d_M
    if answer < 0:
        print('#{0} {1}'.format(t+1, -1))
    else:
        print('#{0} {1}'.format(t+1, answer))
```
