---
layout: post
title: SW Expert Academy [3233]
date: 2020-04-10 11:00:00-12:00:00
categories: ProblemSolving
tag: ProblemSolving
---

# [3233] 정삼각형 분할 놀이
> [SW Expert Academy](https://swexpertacademy.com/main/main.do){: target="_blank"}

## my solution
```python
for t in range(int(input())):
    A, B = map(int, input().split())
    print('#{0} {1}'.format(t+1, pow(A, 2) // pow(B, 2)))
```
