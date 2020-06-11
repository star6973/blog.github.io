---
layout: post
title: SW Expert Academy [8931]
date: 2020-04-20 11:00:00-12:00:00
categories: ProblemSolving
tag: ProblemSolving
---

# [8931] 제로
> [SW Expert Academy](https://swexpertacademy.com/main/main.do){: target="_blank"}

## my solution
```python
T = int(input())
for t in range(T):
    K = int(input())
    receipt = []
    for k in range(K):
        num = int(input())
        if num != 0:
            receipt.append(num)
        else:
            del receipt[-1]

    print('#{0} {1}'.format(t+1, sum(receipt)))
```
