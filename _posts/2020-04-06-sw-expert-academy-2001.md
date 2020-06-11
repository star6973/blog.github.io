---
layout: post
title: SW Expert Academy [2001]
date: 2020-04-06 13:00:00-14:00:00
categories: ProblemSolving
tag: ProblemSolving
---

# [2001] 파리 퇴치
> [SW Expert Academy](https://swexpertacademy.com/main/main.do){: target="_blank"}

## my solution
```python
T = int(input())
for t in range(T):
    N, M = map(int, input().split())

    matrix = []
    for n in range(N):
        matrix.append(list(map(int, input().split())))

    result = 0
    for i in range(len(matrix)):
        for j in range(len(matrix[i])):
            x = i
            y = j
            x_range = i + M
            y_range = j + M

            if x < 0 or y < 0 or x > N - M or y > N - M:
                pass
            else:
                tot = 0
                # print('x, y 위치: {}, {}'.format(x, y))
                for p in range(x, x_range):
                    for q in range(y, y_range):
                        tot += matrix[p][q]

                if result <= tot:
                    result = tot

    print('#{} {}'.format(t+1, result))
```
