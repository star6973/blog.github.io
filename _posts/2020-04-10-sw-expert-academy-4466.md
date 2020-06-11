---
layout: post
title: SW Expert Academy [4466]
date: 2020-04-10 09:00:00-10:00:00
categories: ProblemSolving
tag: ProblemSolving
---

# [4466] 최대 성적표 만들기
> [SW Expert Academy](https://swexpertacademy.com/main/main.do){: target="_blank"}

## my solution
```python
T = int(input())
for t in range(T):
    N, K = map(int, input().split())
    score = sorted(list(map(int, input().split())))

    print('#{0} {1}'.format(t+1, sum(score[-K:])))
```
