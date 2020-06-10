---
layout: post
title: SW Expert Academy [1285]
date: 2020-04-01 11:00:00-12:00:00
categories: ProblemSolving
tag: ProblemSolving
---

# [1285] 아름이의 돌 던지기
> [SW Expert Academy](https://swexpertacademy.com/main/main.do){: target="_blank"}

## my solution
```python
T = int(input())
for t in range(T):
    N = int(input())
    location = list(map(int, input().split()))

    location = [abs(loc) for loc in location]
    location = sorted(location)

    min_dis = location[0]
    min_cnt = location.count(min_dis)

    print('#{} {} {}'.format(t+1, min_dis, min_cnt))
```
