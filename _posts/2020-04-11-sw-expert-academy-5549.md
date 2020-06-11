---
layout: post
title: SW Expert Academy [5549]
date: 2020-04-11 09:00:00-10:00:00
categories: ProblemSolving
tag: ProblemSolving
---

# [5549] 홀수일까 짝수일까
> [SW Expert Academy](https://swexpertacademy.com/main/main.do){: target="_blank"}

## my solution
```python
T = int(input())
for t in range(T):
    tc = int(input())

    if tc % 2 == 0:
        print('#{0} {1}'.format(t+1, 'Even'))
    else:
        print('#{0} {1}'.format(t+1, 'Odd'))
```
