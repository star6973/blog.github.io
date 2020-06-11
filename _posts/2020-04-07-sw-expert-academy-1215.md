---
layout: post
title: SW Expert Academy [1215]
date: 2020-04-07 13:00:00-14:00:00
categories: ProblemSolving
tag: ProblemSolving
---

# [1215] 회문1
> [SW Expert Academy](https://swexpertacademy.com/main/main.do){: target="_blank"}

## my solution
```python
def isPalindrome(str):
    if str == str[::-1]:
        return True
    else:
        return False

for t in range(10):
    n = int(input())
    matrix = []
    for i in range(8):
        matrix.append(list(input()))

    row_palind = []
    for mat in matrix:
        for idx in range(len(mat)-(n-1)):
            str = ''.join(mat[idx:idx+n])
            if isPalindrome(str):
                row_palind.append(str)

    col_palind = []
    for i in range(len(matrix)-(n-1)):
        for j in range(len(matrix[i])):
            str = ''.join(matrix[i+k][j] for k in range(n))
            if isPalindrome(str):
                col_palind.append(str)

    result = row_palind + col_palind
    print('#{} {}'.format(t+1, len(result)))
```
