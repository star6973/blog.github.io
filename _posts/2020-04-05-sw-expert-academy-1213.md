---
layout: post
title: SW Expert Academy [1213]
date: 2020-04-05 11:00:00-12:00:00
categories: ProblemSolving
tag: ProblemSolving
---

# [1213] String
> [SW Expert Academy](https://swexpertacademy.com/main/main.do){: target="_blank"}

## my solution
```python
for t in range(10):
    tc = int(input())
    find = input()
    str = input()

    # print('#{0} {1}'.format(tc, len(str.split(find)) - 1))
    print('#{0} {1}'.format(tc, str.count(find)))
```
