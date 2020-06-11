---
layout: post
title: SW Expert Academy [8658]
date: 2020-04-17 16:00:00-17:00:00
categories: ProblemSolving
tag: ProblemSolving
---

# [8658] Summation
> [SW Expert Academy](https://swexpertacademy.com/main/main.do){: target="_blank"}

## my solution
```python
for t in range(int(input())):
    tc = list(map(str, input().split()))
    print('#{0} {1} {2}'.format(t+1, max([sum(map(int, t)) for t in tc]), min([sum(map(int, t)) for t in tc])))
```
