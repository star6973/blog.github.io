---
layout: post
title: SW Expert Academy [1926]
date: 2020-04-02 09:00:00-10:00:00
categories: ProblemSolving
tag: ProblemSolving
---

# [1926] 간단한 369게임
> [SW Expert Academy](https://swexpertacademy.com/main/main.do){: target="_blank"}

## my solution
```python
clap = ['3', '6', '9']
N = int(input())
for n in range(1, N+1):
    c1 = list(str(n)).count(clap[0])
    c2 = list(str(n)).count(clap[1])
    c3 = list(str(n)).count(clap[2])

    if c1+c2+c3 != 0:
        for c in range(c1+c2+c3):
            print('-', end='')
        print(end=' ')
    else:
        print(str(n), end=' ')
```
