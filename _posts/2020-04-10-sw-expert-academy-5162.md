---
layout: post
title: SW Expert Academy [5162]
date: 2020-04-10 14:00:00-15:00:00
categories: ProblemSolving
tag: ProblemSolving
---

# [5162] 두가지 빵의 딜레마
> [SW Expert Academy](https://swexpertacademy.com/main/main.do){: target="_blank"}

## my solution
```python
for t in range(int(input())):
    A, B, C = map(int, input().split())
    # 최대한 많은 개수의 빵을 사야 하기 때문에 가격이 적은 걸로 많이 사는 게 이득
    a, b = 0, 0
    if A <= B:
        a = C // A
        C = C % A
        b = C // B
    else:
        b = C // B
        C = C % B
        a = C // A

    print('#{0} {1}'.format(t+1, a+b))
```
