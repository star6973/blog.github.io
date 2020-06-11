---
layout: post
title: SW Expert Academy [7087]
date: 2020-04-16 13:00:00-14:00:00
categories: ProblemSolving
tag: ProblemSolving
---

# [7087] 문제 제목 붙이기
> [SW Expert Academy](https://swexpertacademy.com/main/main.do){: target="_blank"}

## my solution
```python
alpha = 'abcdefghijklmnopqrstuvwxyz'

for t in range(int(input())):
    problem = []
    for n in range(int(input())):
        problem.append(input()[0].lower())

    problem = list(set(problem))
    problem.sort()
    pro_alpha = ''.join(problem)
    pro_size = len(pro_alpha)

    cnt = 0
    for a, b in zip(pro_alpha, alpha[:pro_size]):
        if a == b:
            cnt += 1

    print('#{0} {1}'.format(t+1, cnt))
```
