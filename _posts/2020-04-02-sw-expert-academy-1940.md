---
layout: post
title: SW Expert Academy [1940]
date: 2020-04-02 10:00:00-11:00:00
categories: ProblemSolving
tag: ProblemSolving
---

# [1940] 가랏! RC카!
> [SW Expert Academy](https://swexpertacademy.com/main/main.do){: target="_blank"}

## my solution
```python
T = int(input())
for t in range(T):
    N = int(input())
    RC = []
    for n in range(N):
        command = list(map(int, input().split()))
        RC.append(command)

    v0 = 0
    time = 1
    s = 0
    for rc in RC:
        # 가속인 경우
        if rc[0] == 1:
            v = v0 + rc[1]
            v0 = v  # 초기속도는 현재 속도로 바뀜
        # 감속인 경우
        elif rc[0] == 2:
            # 현재 속도보다 감속할 속도가 더 큰 경우, 속도가 0m/s가 된다
            if v0 < abs(rc[1]):
                v = 0
            else:
                v = v0 - rc[1]
            v0 = v  # 초기속도는 현재 속도로 바뀜
        # 그대로인 경우
        else:
            v = v0

        # 거리 계산
        s += v * time

    print('#{} {}'.format(t+1, s))
```
