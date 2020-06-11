---
layout: post
title: SW Expert Academy [2005]
date: 2020-04-04 14:00:00-15:00:00
categories: ProblemSolving
tag: ProblemSolving
---

# [2005] 파스칼의 삼각형
> [SW Expert Academy](https://swexpertacademy.com/main/main.do){: target="_blank"}

## my solution
```python
T = int(input())
for t in range(T):
    N = int(input())

    tot = []
    prev = [1, 0]
    tot.append([prev[0]])
    for n in range(N):
        result = []
        result.append(1)
        for p in range(len(prev)-1):
            result.append(prev[p] + prev[p+1])
        result.append(0)
        tot.append(result[:len(result)-1])
        prev = result

    tot.remove(tot[len(tot)-1])

    print('#{}'.format(t+1))
    for to in tot:
        for t in to:
            print(t, end=' ')
        print()
```
