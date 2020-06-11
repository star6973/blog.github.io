---
layout: post
title: SW Expert Academy [9700]
date: 2020-04-21 11:00:00-12:00:00
categories: ProblemSolving
tag: ProblemSolving
---

# [9700] USB 꽂기의 미스터리
> [SW Expert Academy](https://swexpertacademy.com/main/main.do){: target="_blank"}

## my solution
```python
# Thinking
# 1번 뒤집는 경우: 올바르지 않게 꽂아서, 뒤집은 뒤(1번) 다시 꽂았을 때 정확히 꽂히는 경우 -> (1-p) x q
# 2번 뒤집는 경우: 올바르게 꽂았지만 정확히 꽂히지 않았기에, 뒤집었지만(1번),
#                당연히 뒤집은 상태로 바뀌었기 때문에 다시 올바르게 뒤집어서(2번) 정확히 꽂히는 경우 -> p x (1-q) x q

for t in range(int(input())):
    p, q = map(float, input().split())

    s1 = (1-p) * q
    s2 = p * (1-q) * q

    if s1 < s2:
        print('#{0} {1}'.format(t+1, 'YES'))
    else:
        print('#{0} {1}'.format(t + 1, 'NO'))
```
