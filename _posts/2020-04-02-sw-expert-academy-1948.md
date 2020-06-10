---
layout: post
title: SW Expert Academy [1948]
date: 2020-04-02 14:00:00-15:00:00
categories: ProblemSolving
tag: ProblemSolving
---

# [1948] 날짜 계산기
> [SW Expert Academy](https://swexpertacademy.com/main/main.do){: target="_blank"}

## my solution
```python
cal = {1:31, 2:28, 3:31, 4:30, 5:31, 6:30, 7:31, 8:31, 9:30, 10:31, 11:30, 12:31}

T = int(input())
for t in range(T):
    calander = list(map(int, input().split()))

    month1, day1, month2, day2 = calander[0], calander[1], calander[2], calander[3]

    # 달이 같으면 날짜만 세어주면 됨
    if month1 == month2:
        diff = day2 - day1 + 1

    # 달이 다르면 날짜랑 같이 세어줘야 함
    else:
        tot = 0
        for m in range(month1, month2+1):
            tot += cal[m]
        diff = tot - (day1-1) - (cal[m]-day2)

    print('#{} {}'.format(t+1, diff))
```
