---
layout: post
title: SW Expert Academy [6692]
date: 2020-04-15 16:00:00-17:00:00
categories: ProblemSolving
tag: ProblemSolving
---

# [6692] 다솔이의 월급 상자
> [SW Expert Academy](https://swexpertacademy.com/main/main.do){: target="_blank"}

## my solution
```python
for t in range(int(input())):
    tot = 0
    for n in range(int(input())):
        p, x = map(float, input().split())
        tot += p * x
    print('#{0} {1:.6f}'.format(t+1, tot))
```
