---
layout: post
title: SW Expert Academy [1966]
date: 2020-04-03 11:00:00-12:00:00
categories: ProblemSolving
tag: ProblemSolving
---

# [1966] 숫자를 정렬하자
> [SW Expert Academy](https://swexpertacademy.com/main/main.do){: target="_blank"}

## my solution
```python
T = int(input())
for t in range(T):
    N = int(input())
    arr = list(map(int, input().split()))

    print('#{}'.format(t+1), end=' ')
    for s_a in sorted(arr):
        print(s_a, end=' ')
    print()
```
