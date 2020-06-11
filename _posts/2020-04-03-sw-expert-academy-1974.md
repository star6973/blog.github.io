---
layout: post
title: SW Expert Academy [1974]
date: 2020-04-03 14:00:00-15:00:00
categories: ProblemSolving
tag: ProblemSolving
---

# [1974] 스도쿠 검증
> [SW Expert Academy](https://swexpertacademy.com/main/main.do){: target="_blank"}

## my solution
```python
right = [1, 2, 3, 4, 5, 6, 7, 8, 9]
T = int(input())
for t in range(T):
    sudoku = []
    for i in range(9):
        sudoku.append(list(map(int, input().split())))

    flag = True

    # 가로 확인
    for i in range(9):
        if sorted(sudoku[i]) != right:
            flag = False

    # 세로 확인
    for i in range(9):
        col = []
        for j in range(9):
            col.append(sudoku[j][i])

        if sorted(col) != right:
            flag = False

    # 3칸짜리 확인
    for i in range(9):
        tmp = []
        for j in range(9):
            if i in [0, 3, 6] and j in [0, 3, 6]:
                tmp = []
                for idx in range(i, i+3):
                    for jdx in range(j, j+3):
                        tmp.append(sudoku[idx][jdx])
                # print(tmp)
                if sorted(tmp) != right:
                    flag = False
        # print()

    if flag:
        print('#{} {}'.format(t+1, 1))
    else:
        print('#{} {}'.format(t+1, 0))
```
