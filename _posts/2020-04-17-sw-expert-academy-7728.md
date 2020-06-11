---
layout: post
title: SW Expert Academy [7728]
date: 2020-04-17 09:00:00-10:00:00
categories: ProblemSolving
tag: ProblemSolving
---

# [7728] 다양성 측정
> [SW Expert Academy](https://swexpertacademy.com/main/main.do){: target="_blank"}

## my solution
```python
for t in range(int(input())):
    X = input()
    print('#{0} {1}'.format(t+1, len(list(set(X)))))
```
