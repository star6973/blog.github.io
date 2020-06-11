---
layout: post
title: SW Expert Academy [5603]
date: 2020-04-11 11:00:00-12:00:00
categories: ProblemSolving
tag: ProblemSolving
---

# [5603] 건초더미
> [SW Expert Academy](https://swexpertacademy.com/main/main.do){: target="_blank"}

## my solution
```python
for t in range(int(input())):
    dummy = []
    for p in range(int(input())):
        dummy.append(int(input()))

    dummy = sorted(dummy)

    mid = sum(dummy) // len(dummy)

    answer = list(map(lambda x: abs(x-mid), dummy))

    print('#{0} {1}'.format(t+1, sum(answer)//2))
```
