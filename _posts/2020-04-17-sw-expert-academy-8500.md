---
layout: post
title: SW Expert Academy [8500]
date: 2020-04-17 14:00:00-15:00:00
categories: ProblemSolving
tag: ProblemSolving
---

# [8500] 극장 좌석
> [SW Expert Academy](https://swexpertacademy.com/main/main.do){: target="_blank"}

## my solution
```python
for t in range(int(input())):
    N = int(input())
    seat = sorted(list(map(int, input().split())), reverse=True)
    # print(seat)

    tot = seat[0]
    for i in range(len(seat)-1):
        if seat[i] >= seat[i+1]:
            tot += seat[i]
        else:
            tot += seat[i+1]
    tot += seat[-1]
    print('#{0} {1}'.format(t+1, tot + N))
```
