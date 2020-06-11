---
layout: post
title: SW Expert Academy [3456]
date: 2020-04-09 10:00:00-11:00:00
categories: ProblemSolving
tag: ProblemSolving
---

# [3456] 직사각형 길이 찾기
> [SW Expert Academy](https://swexpertacademy.com/main/main.do){: target="_blank"}

## my solution
```python
for t in range(int(input())):
    case = list(map(int, input().split()))
    print('#{0} {1}'.format(t+1, sorted(case, key=lambda x: case.count(x))[0]))
```
