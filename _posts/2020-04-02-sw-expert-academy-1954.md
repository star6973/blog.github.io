---
layout: post
title: SW Expert Academy [1954]
date: 2020-04-02 15:00:00-17:00:00
categories: ProblemSolving
tag: ProblemSolving
---

# [1954] 달팽이 숫자
> [SW Expert Academy](https://swexpertacademy.com/main/main.do){: target="_blank"}

## my solution
```python
def printArr(arr):
    for i in range(len(arr)):
        for j in range(len(arr[i])):
            print('{}'.format(arr[i][j]), end=' ')
        print()

T = int(input())
for t in range(T):
    N = int(input())
    M = 1

    arr = [[0 for i in range(N)] for j in range(N)]

    up, down, left, right = 0, N-1, N-1, 0
    while M != N * N:

        if M >= N*N:
            break

        # 오른쪽
        for j in range(right, (N-1)-right):
            arr[right][j] = M
            M += 1

        right += 1
        # printArr(arr)

        # 아래쪽
        for i in range((N-1)-down, down):
            arr[i][down] = M
            M += 1

        down -= 1
        # printArr(arr)

        # 왼쪽
        for j in range(left, (N-1)-left, -1):
            arr[left][j] = M
            M += 1

        left -= 1
        # printArr(arr)

        # 위쪽
        for i in range((N-1)-up, up, -1):
            arr[i][up] = M
            M += 1

        up += 1
        # printArr(arr)

    # 홀수인 경우 가운데 채우기
    if N % 2 != 0:
        arr[N//2][N//2] = M

    print('#{}'.format(t+1))
    printArr(arr)
```
