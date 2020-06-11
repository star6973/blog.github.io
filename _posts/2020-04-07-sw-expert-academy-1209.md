---
layout: post
title: SW Expert Academy [1209]
date: 2020-04-07 10:00:00-11:00:00
categories: ProblemSolving
tag: ProblemSolving
---

# [1209] Sum
> [SW Expert Academy](https://swexpertacademy.com/main/main.do){: target="_blank"}

## my solution
```python
for t in range(10):
    N = int(input())
    matrix = [list(map(int, input().split())) for _ in [0] * 100]

    # 가로 행
    rowSum = []
    for i in range(100):
        rowSum.append(sum(matrix[i]))

    # 세로 열
    colSum = []
    for j in range(100):
        tot = 0
        for i in range(100):
            tot += matrix[i][j]
        colSum.append(tot)

    # 오른쪽으로 내려가는 중앙 대각선
    diagonalRightSum = 0
    for i in range(100):
        diagonalRightSum += matrix[i][i]

    # 왼쪽으로 내려가는 중앙 대각선
    diagonalLeftSum = 0
    for i in range(100):
        diagonalLeftSum += matrix[i][99-i]

    print('#{0} {1}'.format(N, max(max(rowSum), max(colSum), diagonalRightSum, diagonalLeftSum)))
```
