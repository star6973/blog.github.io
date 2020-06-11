---
layout: post
title: SW Expert Academy [9778]
date: 2020-04-21 13:00:00-15:00:00
categories: ProblemSolving
tag: ProblemSolving
---

# [9778] 카드 게임
> [SW Expert Academy](https://swexpertacademy.com/main/main.do){: target="_blank"}

## my solution
```python
card = {'2': 4, '3': 4, '4': 4, '5': 4, '6': 4, '7': 4, '8': 4, '9': 4, '10': 16, '11': 4}
for t in range(int(input())):
    N = int(input())
    select = [input() for _ in [0] * N]
    tot = sum([int(s) for s in select])

    for s in select:
        card[s] -= 1


    lowCnt = 0
    highCnt = 0
    for key, value in card.items():
        for v in range(value):
            # 카드를 하나 더 뽑았을 때 가치의 합이 21 이하로 만드는 경우
            if tot + int(key) <= 21:
                lowCnt += 1
            # 카드를 하나 더 뽑았을 때 가치의 합이 21을 넘는 경우
            else:
                highCnt += 1

    print('#{0} {1}'.format(t+1, 'STOP' if lowCnt <= highCnt else 'GAZUA'))
```
