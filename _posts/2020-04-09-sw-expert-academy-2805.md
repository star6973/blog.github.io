---
layout: post
title: SW Expert Academy [2805]
date: 2020-04-09 15:00:00-16:00:00
categories: ProblemSolving
tag: ProblemSolving
---

# [2805] 농작물 수확하기
> [SW Expert Academy](https://swexpertacademy.com/main/main.do){: target="_blank"}

## my solution
```python
for t in range(int(input())):
    N = int(input())
    value = [list(map(int, input())) for _ in [0] * N]
    revenue = 0
    k = 0
    m = 1
    for i in range(N):

        if i > N//2:
            for j in range(m, N-m):
                revenue += value[i][j]
                # print(j, end=' ')
            m += 1
            # print()
        else:
            for j in range(N//2 - k, N//2 + (k+1)):
                revenue += value[i][j]
                # print(j, end=' ')
            k += 1
            # print()

    print('#{0} {1}'.format(t+1, revenue))
```
