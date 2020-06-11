---
layout: post
title: SW Expert Academy [4047]
date: 2020-04-09 15:00:00-16:00:00
categories: ProblemSolving
tag: ProblemSolving
---

# [4047] 영준이의 카드 카운팅
> [SW Expert Academy](https://swexpertacademy.com/main/main.do){: target="_blank"}

## my solution
```python
for t in range(int(input())):
    card = ['S', 'D', 'H', 'C'] * 13
    card.sort()

    tc = input()
    split_tc = []
    for i in range(0, len(tc), 3):
        split_tc.append(tc[i:i+3])

    # print(split_tc)

    if len(set(split_tc)) != len(split_tc):
        print('#{0} {1}'.format(t+1, 'ERROR'))
    else:
        for st in split_tc:
            card.remove(st[0])
        print('#{0} {1} {2} {3} {4}'.format(t+1, card.count('S'), card.count('D'), card.count('H'), card.count('C')))
```
