---
layout: post
title: SW Expert Academy [1989]
date: 2020-04-04 11:00:00-12:00:00
categories: ProblemSolving
tag: ProblemSolving
---

# [1989] 초심자의 회문 검사
> [SW Expert Academy](https://swexpertacademy.com/main/main.do){: target="_blank"}

## my solution
```python
T = int(input())
for t in range(T):
    str = input()

    if str == str[::-1]:
        print('#{} {}'.format(t+1, 1))
    else:
        print('#{} {}'.format(t+1, 0))
```
