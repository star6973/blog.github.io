---
layout: post
title: SW Expert Academy [9317]
date: 2020-04-21 09:00:00-10:00:00
categories: ProblemSolving
tag: ProblemSolving
---

# [9317] 석찬이의 받아쓰기
> [SW Expert Academy](https://swexpertacademy.com/main/main.do){: target="_blank"}

## my solution
```python
for t in range(int(input())):
    N = int(input())
    answer = input()
    write = input()

    cnt = 0
    for a, w in zip(answer, write):
        if a == w:
            cnt += 1

    print('#{0} {1}'.format(t+1, cnt))
```
