---
layout: post
title: SW Expert Academy [4751]
date: 2020-04-14 11:00:00-12:00:00
categories: ProblemSolving
tag: ProblemSolving
---

# [4751] 다솔이의 다이아몬드 장식
> [SW Expert Academy](https://swexpertacademy.com/main/main.do){: target="_blank"}

## my solution
```python
for t in range(int(input())):
    str = input()

    for s in str:
        print('..#.', end='')
    print('.')

    for s in str:
        print('.#.#', end='')
    print('.')

    for s in str:
        print('#.' + s + '.', end='')
    print('#')

    for s in str:
        print('.#.#', end='')
    print('.')

    for s in str:
        print('..#.', end='')
    print('.')
```

## others solution
```python
for t in range(int(input())):
    A = ['.']
    B = ['.']
    C = ['#']
    ur_word = list(input())
    for i in range(len(ur_word)):
        A.extend(['.', '#', '.', '.'])
        B.extend(['#', '.', '#', '.'])
        C.extend(['.', ur_word[i], '.', '#'])
        print(A)
        print(B)
        print(C)

    print(''.join(A))
    print(''.join(B))
    print(''.join(C))
    print(''.join(B))
    print(''.join(A))
```
