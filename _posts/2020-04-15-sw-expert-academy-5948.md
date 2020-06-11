---
layout: post
title: SW Expert Academy [5948]
date: 2020-04-15 14:00:00-15:00:00
categories: ProblemSolving
tag: ProblemSolving
---

# [5948] 새샘이의 7-3-5 게임
> [SW Expert Academy](https://swexpertacademy.com/main/main.do){: target="_blank"}

## my solution
```python
from itertools import combinations

for t in range(int(input())):
    tc = list(map(int, input().split()))

    answer = []
    for combi in list(combinations(tc, 3)):
        answer.append(sum(combi))

    answer = sorted(set(answer), reverse=True)
    print('#{0} {1}'.format(t+1, answer[4]))
```
