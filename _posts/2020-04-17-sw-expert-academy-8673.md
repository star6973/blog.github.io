---
layout: post
title: SW Expert Academy [8673]
date: 2020-04-17 17:00:00-18:00:00
categories: ProblemSolving
tag: ProblemSolving
---

# [8673] 코딩 토너먼트1
> [SW Expert Academy](https://swexpertacademy.com/main/main.do){: target="_blank"}

## my solution
```python
for t in range(int(input())):
    k = int(input())
    player = list(map(int, input().split()))
    n = pow(2, k) - 1
    tot = 0
    while True:
        winPlayer = []
        for i in range(0, len(player), 2):
            if player[i] <= player[i + 1]:
                winPlayer.append(player[i + 1])
            else:
                winPlayer.append(player[i])

            tot += abs(player[i+1] - player[i])

        player = winPlayer
        # print(winPlayer)
        n -= 1
        if len(player) == 1:
            break

    print('#{0} {1}'.format(t+1, tot))
```
