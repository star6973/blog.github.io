---
layout: post
title: SW Expert Academy [4789]
date: 2020-04-10 11:00:00-12:00:00
categories: ProblemSolving
tag: ProblemSolving
---

# [4789] 성공적인 공연 기획
> [SW Expert Academy](https://swexpertacademy.com/main/main.do){: target="_blank"}

## my solution
```python
for t in range(int(input())):
    clap = list(map(int, input()))

    tot = 0
    result = 0
    for i in range(len(clap)):
        need = i
        if clap[i] != 0:
            if tot < need:
                result += (need - tot)
                tot = (need + clap[i])
            else:
                tot += clap[i]

    print('#{0} {1}'.format(t+1, result))
```
