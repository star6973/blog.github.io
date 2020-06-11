---
layout: post
title: SW Expert Academy [1217]
date: 2020-04-07 16:00:00-17:00:00
categories: ProblemSolving
tag: ProblemSolving
---

# [1217] 거듭 제곱
> [SW Expert Academy](https://swexpertacademy.com/main/main.do){: target="_blank"}

## my solution
```python
def powNum(N, M):
    if M == 0:
        return 1

    return N * powNum(N, M-1)

for i in range(10):
    t = int(input())
    N, M = map(int, input().split())
    print('#{} {}'.format(t, powNum(N, M)))
```
