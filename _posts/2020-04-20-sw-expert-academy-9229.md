---
layout: post
title: SW Expert Academy [9229]
date: 2020-04-20 13:00:00-14:00:00
categories: ProblemSolving
tag: ProblemSolving
---

# [9229] 한빈이와 Spot Mart
> [SW Expert Academy](https://swexpertacademy.com/main/main.do){: target="_blank"}

## my solution
```python
from itertools import combinations

for t in range(int(input())):
    N, M = map(int, input().split())
    weight = list(map(int, input().split()))

    max_weight = 0
    for combi in list(combinations(weight, 2)):
        if max_weight <= sum(combi) <= M:
            max_weight = sum(combi)

    if max_weight == 0:
        print('#{0} {1}'.format(t+1, -1))
    else:
        print('#{0} {1}'.format(t+1, max_weight))
```
