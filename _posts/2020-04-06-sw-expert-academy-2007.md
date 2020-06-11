---
layout: post
title: SW Expert Academy [2007]
date: 2020-04-06 15:00:00-16:00:00
categories: ProblemSolving
tag: ProblemSolving
---

# [2007] 패턴 마디의 길이
> [SW Expert Academy](https://swexpertacademy.com/main/main.do){: target="_blank"}

## my solution
```python
T = int(input())
for t in range(T):
    testcase = input()

    for i in range(1, len(testcase)):
        string = testcase[:i]
        # print(string, end=' ')
        # print(testcase[i:i+len(string)])
        if string == testcase[i:i+len(string)]:
            dup = len(string)
            break
    print('#{} {}'.format(t+1, dup))
```
