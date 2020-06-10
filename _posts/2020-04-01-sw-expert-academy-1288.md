---
layout: post
title: SW Expert Academy [1288]
date: 2020-04-01 13:00:00-14:00:00
categories: ProblemSolving
tag: ProblemSolving
---

# [1288] 새로운 불면증 치료
> [SW Expert Academy](https://swexpertacademy.com/main/main.do){: target="_blank"}

## my solution
```python
cnt_num = ['0', '1', '2', '3', '4', '5', '6', '7', '8', '9']

T = int(input())
for t in range(T):
    N = int(input())

    k = 1
    p = 0
    cnt_tmp = []
    while True:

        tmp = list(str(N * k))
        cnt_tmp.extend(tmp)
        # print(sorted(list(set(cnt_tmp))))

        if sorted(list(set(cnt_tmp))) == cnt_num:
            break

        k += 1

    print('#{} {}'.format(t+1, N * k))
```
