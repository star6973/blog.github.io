---
layout: post
title: SW Expert Academy [9480]
date: 2020-04-21 10:00:00-11:00:00
categories: ProblemSolving
tag: ProblemSolving
---

# [9480] 민정이와 광직이의 알파벳 공부
> [SW Expert Academy](https://swexpertacademy.com/main/main.do){: target="_blank"}

## my solution
```python
from itertools import combinations

for t in range(int(input())):
    testCase = []
    for n in range(int(input())):
        testCase.append(input())

    cnt = 0
    for i in range(1, len(testCase)+1):
        possible = list(combinations(testCase, i))
        for pos in possible:
            if ''.join(sorted(set(''.join(pos)))) == 'abcdefghijklmnopqrstuvwxyz':
                cnt += 1

    print('#{0} {1}'.format(t+1, cnt))
```
