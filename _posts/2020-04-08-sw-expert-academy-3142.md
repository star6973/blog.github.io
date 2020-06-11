---
layout: post
title: SW Expert Academy [3142]
date: 2020-04-08 10:00:00-11:00:00
categories: ProblemSolving
tag: ProblemSolving
---

# [3142] 영준이와 신비한 뿔의 숲
> [SW Expert Academy](https://swexpertacademy.com/main/main.do){: target="_blank"}

## my solution
```python
T = int(input())
for t in range(T):
    N, M = map(int, input().split())

    for m in range(M+1):
        # m마리는 유니콘, (M-m)마리는 트윈혼
        y = M - m

        if m*1 + y*2 == N:
            print('#{0} {1} {2}'.format(t+1, m, y))
```
