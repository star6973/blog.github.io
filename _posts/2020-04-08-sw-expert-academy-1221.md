---
layout: post
title: SW Expert Academy [1221]
date: 2020-04-08 09:00:00-10:00:00
categories: ProblemSolving
tag: ProblemSolving
---

# [1221] GNS
> [SW Expert Academy](https://swexpertacademy.com/main/main.do){: target="_blank"}

## my solution
```python
number = {"ZRO":0, "ONE":1, "TWO":2, "THR":3, "FOR":4, "FIV":5, "SIX":6, "SVN":7, "EGT":8, "NIN":9}

for t in range(int(input())):
    tc = input().split()

    testcase = list(map(str, input().split()))
    testcase = sorted([number[key] for key in testcase])
    answer = []
    for tC in testcase:
        answer.append(*[key for key, value in number.items() if tC==value])
    print(tc[0])
    print(*answer)
```
