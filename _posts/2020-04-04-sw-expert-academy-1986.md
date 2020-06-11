---
layout: post
title: SW Expert Academy [1986]
date: 2020-04-04 10:00:00-11:00:00
categories: ProblemSolving
tag: ProblemSolving
---

# [1986] 지그재그 숫자
> [SW Expert Academy](https://swexpertacademy.com/main/main.do){: target="_blank"}

## my solution
```python
T = int(input())
for t in range(T):
    N = int(input())

    tot = 0
    for n in range(1, N+1):
        if n % 2 == 0:
            tot -= n
        else:
            tot += n

    print('#{} {}'.format(t+1, tot))
```
