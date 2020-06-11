---
layout: post
title: SW Expert Academy [3431]
date: 2020-04-09 09:00:00-10:00:00
categories: ProblemSolving
tag: ProblemSolving
---

# [3431] 준환이의 운동관리
> [SW Expert Academy](https://swexpertacademy.com/main/main.do){: target="_blank"}

## my solution
```python
T = int(input())
for t in range(T):
    L, U, X = map(int, input().split())

    if X > U:
        print('#{} {}'.format(t+1, -1))
    elif X < L:
        print('#{} {}'.format(t+1, L-X))
    else:
        print('#{} {}'.format(t+1, 0))
```
