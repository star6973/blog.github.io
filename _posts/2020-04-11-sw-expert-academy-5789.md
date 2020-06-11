---
layout: post
title: SW Expert Academy [5789]
date: 2020-04-11 13:00:00-14:00:00
categories: ProblemSolving
tag: ProblemSolving
---

# [5789] 현주의 상자 바꾸기
> [SW Expert Academy](https://swexpertacademy.com/main/main.do){: target="_blank"}

## my solution
```python
for t in range(int(input())):
    N, Q = map(int, input().split())

    box = [0 for _ in range(N)]

    for q in range(Q):
        L, R = map(int, input().split())
        for i in range(L-1, R):
            box[i] = q+1

    print(f'#{t+1}', end=' ')
    print(*box)
```
