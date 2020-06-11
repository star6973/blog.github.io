---
layout: post
title: SW Expert Academy [3314]
date: 2020-04-10 14:00:00-15:00:00
categories: ProblemSolving
tag: ProblemSolving
---

# [3314] 보충학습과 평균
> [SW Expert Academy](https://swexpertacademy.com/main/main.do){: target="_blank"}

## my solution
```python
for t in range(int(input())):
    score = list(map(int, input().split()))
    for i, s in enumerate(score):
        if s < 40:
            score[i] = 40

    result = sum(score) // len(score)
    print('#{0} {1}'.format(t+1, result))
```

## others solution
```python
T = int(input())
res = []
for t in range(T):
    score = list(map(int, input().split()))

    total = 0
    for i in range(5):
        if score[i] < 40:
            score[i] = 40
        total += score[i]

    res.append('#{} {}'.format(t + 1, total // 5))
print('\n'.join(res))
```
