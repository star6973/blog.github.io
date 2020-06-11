---
layout: post
title: SW Expert Academy [7532]
date: 2020-04-16 17:00:00-18:00:00
categories: ProblemSolving
tag: ProblemSolving
---

# [7532] 세영이의 SEM력 연도
> [SW Expert Academy](https://swexpertacademy.com/main/main.do){: target="_blank"}

## my solution
```python
# E가 가장 작은 수로, 기준을 잡는다.
# E를 24씩 증가시키면서 365로 나눈 나머지 값이 S와 일치하고, 29로 나눈 나머지 값이 m이랑 일치할 때의 값이 정답

for t in range(int(input())):
    S, E, M = map(int, input().split())
    S -= 1
    E -= 1
    M -= 1
    while True:
        if E % 365 == S and E % 29 == M:
            print('#{0} {1}'.format(t + 1, E+1))
            break
        E += 24

```
