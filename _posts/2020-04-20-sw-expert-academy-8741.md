---
layout: post
title: SW Expert Academy [8741]
date: 2020-04-20 09:00:00-10:00:00
categories: ProblemSolving
tag: ProblemSolving
---

# [8741] 두문자어
> [SW Expert Academy](https://swexpertacademy.com/main/main.do){: target="_blank"}

## my solution
```python
T = int(input())
for t in range(T):
    string = input().split()
    print('#{0}'.format(t+1), end=' ')
    for s in string:
        print(s[0].upper(), end='')
    print()
```
