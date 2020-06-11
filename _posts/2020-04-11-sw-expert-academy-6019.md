---
layout: post
title: SW Expert Academy [6019]
date: 2020-04-11 15:00:00-16:00:00
categories: ProblemSolving
tag: ProblemSolving
---

# [6019] 기차 사이의 파리
> [SW Expert Academy](https://swexpertacademy.com/main/main.do){: target="_blank"}

## my solution
```python
for t in range(int(input())):
    D, A, B, F = map(int, input().split())

    x = D / (1 + B/A)
    s = x / A
    print('#{0} {1:f}'.format(t+1, F * s))
```
