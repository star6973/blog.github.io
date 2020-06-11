---
layout: post
title: SW Expert Academy [3499]
date: 2020-04-13 11:00:00-12:00:00
categories: ProblemSolving
tag: ProblemSolving
---

# [3499] 퍼펙트 셔플
> [SW Expert Academy](https://swexpertacademy.com/main/main.do){: target="_blank"}

## my solution
```python
for t in range(int(input())):
    N = int(input())
    card = list(input().split())
    # print(card)
    mid_card = ''
    # 카드 개수가 홀수인 경우
    if len(card) % 2 != 0:
        # 가장 가운데 카드
        mid_card = card[len(card)//2]
        card.remove(mid_card)

    left = card[:len(card)//2]
    right = card[len(card)//2:]

    result = []
    for l, r in zip(left, right):
        result.append(l)
        result.append(r)

    if mid_card != '':
        result.append(mid_card)

    print('#{}'.format(t+1), end=' ')
    print(*result)
```
