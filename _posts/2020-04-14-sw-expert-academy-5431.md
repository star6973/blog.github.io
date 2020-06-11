---
layout: post
title: SW Expert Academy [5431]
date: 2020-04-14 16:00:00-17:00:00
categories: ProblemSolving
tag: ProblemSolving
---

# [5431] 민석이의 과제 체크하기
> [SW Expert Academy](https://swexpertacademy.com/main/main.do){: target="_blank"}

## my solution
```python
for t in range(int(input())):
    N, K = map(int, input().split())
    submit = set(map(int, input().split()))

    n = set(_ for _ in range(1, N+1))
    print('#{0}'.format(t+1), end=' ')
    print(*list(n - submit))
```
