---
layout: post
title: SW Expert Academy [6958]
date: 2020-04-12 11:00:00-12:00:00
categories: ProblemSolving
tag: ProblemSolving
---

# [6958] 동철이의 프로그래밍 대회
> [SW Expert Academy](https://swexpertacademy.com/main/main.do){: target="_blank"}

## my solution
```python
for t in range(int(input())):
    N, M = map(int, input().split())
    problem = list(''.join(input().split()) for _ in [0] * N)

    maxScore = 0
    sumProblem = []
    for pro in problem:
        sumProblem.append(sum(list(map(int, pro))))
        if maxScore <= sum(list(map(int, pro))):
            maxScore = sum(list(map(int, pro)))

    print('#{0} {1} {2}'.format(t+1, sumProblem.count(maxScore), maxScore))
```
