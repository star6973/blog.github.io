---
layout: post
title: SW Expert Academy [1984]
date: 2020-04-04 09:00:00-10:00:00
categories: ProblemSolving
tag: ProblemSolving
---

# [1984] 중간 평균값 구하기
> [SW Expert Academy](https://swexpertacademy.com/main/main.do){: target="_blank"}

## my solution
```python
T = int(input())
for t in range(T):
    num = list(map(int, input().split()))
    num = sorted(num)

    min_num, max_num = num[0], num[len(num)-1]
    print('#{} {}'.format(t+1, round((sum(num) - (min_num + max_num)) / (len(num) - 2)), 1))
```
