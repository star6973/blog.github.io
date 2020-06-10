---
layout: post
title: SW Expert Academy [1959]
date: 2020-04-02 17:00:00-18:00:00
categories: ProblemSolving
tag: ProblemSolving
---

# [1959] 두 개의 숫자열
> [SW Expert Academy](https://swexpertacademy.com/main/main.do){: target="_blank"}

## my solution
```python
T = int(input())
for t in range(T):
    N, M = map(int, input().split())
    A = list(map(int, input().split()))
    B = list(map(int, input().split()))

    # A보다 B가 더 긴 경우
    if len(A) <= len(B):
        possible_b = []
        for b in range(len(B)):
            if b+len(A) <= len(B):
                possible_b.append(B[b:b+len(A)])
        # print(possible_b)

        max_tot = 0
        for p_b in possible_b:
            tot = 0
            for a in range(len(A)):
                tot += p_b[a] * A[a]

            if max_tot <= tot:
                max_tot = tot

    else:
        possible_a = []
        for a in range(len(A)):
            if a+len(B) <= len(A):
                possible_a.append(A[a:a+len(B)])
        # print(possible_a)

        max_tot = 0
        for p_a in possible_a:
            tot = 0
            for b in range(len(B)):
                tot += p_a[b] * B[b]

            if max_tot <= tot:
                max_tot = tot

    print('#{} {}'.format(t+1, max_tot))
```
