---
layout: post
title: SW Expert Academy [1946]
date: 2020-04-02 13:00:00-14:00:00
categories: ProblemSolving
tag: ProblemSolving
---

# [1946] 간단한 압축 풀기
> [SW Expert Academy](https://swexpertacademy.com/main/main.do){: target="_blank"}

## my solution
```python
T = int(input())
for t in range(T):
    N = int(input())

    alpha = []
    cnt = []
    tot = 0
    for n in range(N):
        a, c = input().split()
        alpha.append(a)
        cnt.append(int(c))

        tot += int(c)

    i = 0
    j = 0
    p = 0

    print('#{}'.format(t+1), end='')
    for t in range(tot):
        if p == cnt[j]:
            i += 1
            j += 1
            p = 0

        if t % 10 == 0:
            print()

        p += 1
        print(alpha[i], end='')
    print()
```
