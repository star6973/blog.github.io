---
layout: post
title: SW Expert Academy [8556]
date: 2020-04-17 15:00:00-16:00:00
categories: ProblemSolving
tag: ProblemSolving
---

# [8556] 북북서
> [SW Expert Academy](https://swexpertacademy.com/main/main.do){: target="_blank"}

## my solution
```python
import re
from fractions import Fraction

for t in range(int(input())):
    direction = input()
    direction = re.findall(r'north|west', direction)
    direction = direction[::-1]

    tot = 0
    for i, dir in enumerate(direction):
        if i == 0 and dir == 'north':
            tot = 0

        elif i == 0 and dir == 'west':
            tot = 90

        elif dir == 'north':
            tot -= (90 / pow(2, i))
        elif dir == 'west':
            tot += (90 / pow(2, i))

    print('#{0} {1}'.format(t+1, Fraction(tot))) # Fraction: 기약 분수로 출력
```
