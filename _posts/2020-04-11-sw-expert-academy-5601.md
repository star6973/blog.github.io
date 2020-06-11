---
layout: post
title: SW Expert Academy [5601]
date: 2020-04-11 10:00:00-11:00:00
categories: ProblemSolving
tag: ProblemSolving
---

# [5601] 쥬스 나누기
> [SW Expert Academy](https://swexpertacademy.com/main/main.do){: target="_blank"}

## my solution
```python
for t in range(int(input())):
    N = int(input())
    print('#{}'.format(t+1), end=' ')
    for n in range(N):
        print(str(1) + '/' + str(N), end=' ')
    print()
```
