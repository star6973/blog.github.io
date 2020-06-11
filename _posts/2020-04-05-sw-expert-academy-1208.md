---
layout: post
title: SW Expert Academy [1208]
date: 2020-04-05 09:00:00-10:00:00
categories: ProblemSolving
tag: ProblemSolving
---

# [1208] Flatten
> [SW Expert Academy](https://swexpertacademy.com/main/main.do){: target="_blank"}

## my solution
```python
for t in range(10):
    dump = int(input())
    block = list(map(int, input().split()))

    while dump != 0:
        highIdx = block.index(max(block))
        lowIdx = block.index(min(block))

        block[highIdx] -= 1
        block[lowIdx] += 1

        dump -= 1

    highBlock = max(block)
    lowBlock = min(block)
    print('#{0} {1}'.format(t+1, highBlock - lowBlock))
```

## others solution
```python
for t in range(10):
    dump = int(input())
    block = sorted(list(map(int, input().split())))

    while dump != 0:
        block[-1] -= 1
        block[0] += 1
        block.sort()
        dump -= 1

    highBlock = max(block)
    lowBlock = min(block)
    print('#{0} {1}'.format(t + 1, highBlock - lowBlock))
```
