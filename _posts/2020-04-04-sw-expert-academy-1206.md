---
layout: post
title: SW Expert Academy [1206]
date: 2020-04-04 16:00:00-18:00:00
categories: ProblemSolving
tag: ProblemSolving
---

# [1206] View
> [SW Expert Academy](https://swexpertacademy.com/main/main.do){: target="_blank"}

## my solution
```python
for t in range(10):
    tc = int(input())
    building = list(map(int, input().split()))

    right = [0 for i in range(len(building))]
    for i in range(len(building)):
        if i == len(building)-2 and building[i] > building[i+1]:
            right[i] = building[i] - building[i+1]
        elif i == len(building)-1:
            right[i] = building[i]
        else:
            if building[i] > building[i+1] and building[i] > building[i+2]:
                if building[i+1] >= building[i+2]:
                    right[i] = building[i] - building[i+1]
                else:
                    right[i] = building[i] - building[i+2]

    left = [0 for i in range(len(building))]
    for i in range(len(building)-1, -1, -1):
        if i == 1 and building[i] > building[i-1]:
            left[i] = building[i] - building[i+1]
        elif i == 0:
            left[i] = building[i]
        else:
            if building[i] > building[i-1] and building[i] > building[i-2]:
                if building[i-1] >= building[i-2]:
                    left[i] = building[i] - building[i-1]
                else:
                    left[i] = building[i] - building[i-2]

    final = [0 for i in range(len(building))]
    idx = 0
    for r, l in zip(right, left):
        if r <= l:
            final[idx] = r
        else:
            final[idx] = l
        idx += 1

    print('#{0} {1}'.format(t+1, sum(final)))
```

## others solution
```python
for t in range(10):
    final = 0
    tc = int(input())
    building = list(map(int, input().split()))

    for i in range(2, tc-2):
        if building[i] > max([building[i-2], building[i-1], building[i+1], building[i+2]]):
            final += building[i] - max([building[i-2], building[i-1], building[i+1], building[i+2]])

    print('#{0} {1}'.format(t+1, final))
```
