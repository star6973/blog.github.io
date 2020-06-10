---
layout: post
title: SW Expert Academy [1204]
date: 2020-03-09 09:00:00-10:00:00
categories: ProblemSolving
tag: ProblemSolving
---

# [1204] 최빈수 구하기
> [SW Expert Academy](https://swexpertacademy.com/main/main.do){: target="_blank"}

## my solution
```python
T = int(input())
for t in range(T):
    n = int(input())
    score = list(map(int, input().split()))
    score_unique = list(set(score))
    # print(score_unique)
    score_count = [score.count(s) for s in score_unique]
    # print(score_count)

    score_dict = {}
    for u, c in zip(score_unique, score_count):
        score_dict[u] = c
    # print(score_dict)

    resultList = sorted(score_dict.items(), key=lambda x: x[1])
    # print(resultList)
    print('#{} {}'.format(n, resultList[-1][0]))
```

## others solution
```python
T = int(input())
for t in range(T):
    n = int(input())
    score = list(map(int, input().split()))

    max_cnt = 0
    max_score = 101

    for s in score:
        cnt = score.count(s)
        if max_cnt <= cnt:
            max_cnt = cnt
            max_score = s

    print('#{} {}'.format(n, max_score))
```
