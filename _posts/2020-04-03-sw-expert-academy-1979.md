---
layout: post
title: SW Expert Academy [1979]
date: 2020-04-03 16:00:00-17:00:00
categories: ProblemSolving
tag: ProblemSolving
---

# [1979] 어디에 단어가 들어갈 수 있을까
> [SW Expert Academy](https://swexpertacademy.com/main/main.do){: target="_blank"}

## my solution
```python
T = int(input())
for t in range(T):
    N, K = map(int, input().split())

    puzzle = []
    for n in range(N):
        puzzle.append(list(input().split()))

    # 가로
    row = 0
    for i in range(N):
        pz = ''.join(puzzle[i])
        pz = pz.split('0')
        pz = list(filter(None, pz))

        for p in pz:
            if len(p) == K:
                row += 1

    # 세로
    col = 0
    for j in range(N):
        pz = []
        for i in range(N):
            pz.append(puzzle[i][j])

        pz = ''.join(pz)
        pz = pz.split('0')
        pz = list(filter(None, pz))

        for p in pz:
            if len(p) == K:
                col += 1

    print('#{} {}'.format(t+1, row+col))
```

## others solution
```python
def isPossible(puzzle, i, j):
    if i < 0 or j < 0 or i >= len(puzzle) or j >= len(puzzle):
        return False
    if puzzle[i][j] == 1:
        return True


def setPuzzle(puzzle, N, K):
    ans = 0

    for i in range(N):
        for j in range(N - K + 1):
            if(j > 0 and puzzle[i][j-1] == 1):
                continue

            cnt = 0
            for k in range(K):
                if(puzzle[i][j+k] == 1): cnt += 1
                else: break

            if(cnt == K and puzzle[i][j+K] == 0):
                ans += 1

    for i in range(N - K + 1):
        for j in range(N):
            if (i > 0 and puzzle[i-1][j] == 1):
                continue

            cnt = 0
            for k in range(K):
                if (puzzle[i+k][j] == 1): cnt += 1
                else: break

            if (cnt == K and puzzle[i+K][j] == 0):
                ans += 1

    return ans


T = int(input())
for t in range(T):
    N, K = map(int, input().split())

    puzzle = []
    for n in range(N):
        puzzle.append(list(map(int, input().split())))
        puzzle[n].append(0)
    puzzle.append([0 for _ in range(N)])

    result = setPuzzle(puzzle, N, K)

    print(result)
```
