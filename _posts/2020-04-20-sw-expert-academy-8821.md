---
layout: post
title: SW Expert Academy [8821]
date: 2020-04-20 10:00:00-11:00:00
categories: ProblemSolving
tag: ProblemSolving
---

# [8821] 적고 지우기
> [SW Expert Academy](https://swexpertacademy.com/main/main.do){: target="_blank"}

## my solution
```python
for t in range(int(input())):
    study = list(map(int, input()))
    write = []

    for s in study:
        if s not in write:
            write.append(s)
        else:
            write.remove(s)

    print('#{0} {1}'.format(t+1, len(write)))
```
