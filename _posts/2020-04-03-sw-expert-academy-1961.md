---
layout: post
title: SW Expert Academy [1961]
date: 2020-04-03 09:00:00-11:00:00
categories: ProblemSolving
tag: ProblemSolving
---

# [1961] 숫자 배열 회전
> [SW Expert Academy](https://swexpertacademy.com/main/main.do){: target="_blank"}

## my solution
```python
T = int(input())
for t in range(T):
    N = int(input())

    arr = []
    for i in range(N):
        arr.append(list(input().split()))

    '''
         1  2  3  4  (0, 0) (0, 1) (0, 2) (0, 3)
         5  6  7  8  (1, 0) (1, 1) (1, 2) (1, 3)
         9 10 11 12  (2, 0) (2, 1) (2, 2) (2, 3)
        13 14 15 16  (3, 0) (3, 1) (3, 2) (3, 3)
        
        =============== 90도 회전 ===============
        13  9  5  1  (3, 0) (2, 0) (1, 0) (0, 0)
        14 10  6  2  (3, 1) (2, 1) (1, 1) (0, 1)
        15 11  7  3  (3, 2) (2, 2) (1, 2) (0, 2)
        16 12  8  4  (3, 3) (2, 3) (1, 3) (0, 3)
        
        ============== 180도 회전 ===============
        16 15 14 13  (3, 3) (3, 2) (3, 1) (3, 0)
        12 11 10  9  (2, 3) (2, 2) (2, 1) (2, 0)
         8  7  7  6  (1, 3) (1, 2) (1, 1) (1, 0)
         4  3  2  1  (0, 3) (0, 2) (0, 1) (0, 0)
         
        ============== 270도 회전 ===============
         4  8 12 16  (0, 3) (1, 3) (2, 3) (3, 3)
         3  7 11 15  (0, 2) (1, 2) (2, 2) (3, 2)
         2  6 10 14  (0, 1) (1, 1) (2, 1) (3, 1)
         1  5  9 13  (0, 0) (1, 0) (2, 0) (3, 0)
        
    '''

    rotate_90 = []
    # 90도 회전
    for j in range(N):
        tmp = []
        for i in range(N - 1, -1, -1):
            tmp.append(arr[i][j])
            # print(arr[i][j], end='')
        rotate_90.append(tmp)
        # print()

    rotate_180 = []
    # 180도 회전
    for i in range(N - 1, -1, -1):
        tmp = []
        for j in range(N-1, -1, -1):
            tmp.append(arr[i][j])
            # print(arr[i][j], end='')
        rotate_180.append(tmp)
        # print()

    rotate_270 = []
    # 270도 회전
    for j in range(N - 1, -1, -1):
        tmp = []
        for i in range(N):
            tmp.append(arr[i][j])
            # print(arr[i][j], end='')
        rotate_270.append(tmp)
        # print()


    result = []
    for i in range(N):
        tmp = [rotate_90[i], rotate_180[i], rotate_270[i]]
        result.append(tmp)

    print('#{}'.format(t+1))
    for i in range(N):
        for j in range(3):
            print("".join(result[i][j]), end=' ')
        print()
```
