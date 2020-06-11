---
layout: post
title: SW Expert Academy [7732]
date: 2020-04-17 10:00:00-12:00:00
categories: ProblemSolving
tag: ProblemSolving
---

# [7732] 시간 개념
> [SW Expert Academy](https://swexpertacademy.com/main/main.do){: target="_blank"}

## my solution
```python
for t in range(int(input())):
    now = list(map(int, input().split(':')))
    promise = list(map(int, input().split(':')))

    diff_time = [p-n for p, n in zip(promise, now)]
    # print('diff_time', diff_time)

    # 초가 음수인 경우, 23:59:60 으로 설정
    if diff_time[-1] < 0:
        std = [23, 59, 60]

    # 분이 음수인 경우, 23:60:00 으로 설정
    elif diff_time[1] < 0:
        std = [23, 60, 0]

    # 시가 음수인 경우, 23:00:00 으로 설정
    elif diff_time[0] < 0:
        std = [24, 0, 0]

    else:
        std = [0, 0, 0]

    answer = [s+d for s, d in zip(std, diff_time)]
    # print('answer', answer)

    if answer[0] >= 24:
        answer[0] -= 24
    if answer[1] >= 60:
        answer[1] -= 60
    if answer[2] >= 60:
        answer[2] -= 60

    answer = [str(0) + str(a) if a < 10 else str(a) for a in answer]

    print('#{}'.format(t+1), end=' ')
    print(':'.join(answer))
```
