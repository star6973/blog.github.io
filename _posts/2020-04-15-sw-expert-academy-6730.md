---
layout: post
title: SW Expert Academy [6730]
date: 2020-04-15 17:00:00-18:00:00
categories: ProblemSolving
tag: ProblemSolving
---

# [6730] 장애물 경주 난이도
> [SW Expert Academy](https://swexpertacademy.com/main/main.do){: target="_blank"}

## my solution
```python
for t in range(int(input())):
    N = int(input())
    block = list(map(int, input().split()))

    level = []
    for idx in range(len(block)-1):
        level.append(block[idx+1] - block[idx])

    upLevel = list(filter(lambda x: x >= 0, level))
    downlevel = list(filter(lambda x: x < 0, level))

    print('#{0} {1} {2}'.format(t+1, max(upLevel) if upLevel != [] else 0, abs(min(downlevel)) if downlevel != [] else 0))
```
