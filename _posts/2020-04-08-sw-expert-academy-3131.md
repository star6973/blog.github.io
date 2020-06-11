---
layout: post
title: SW Expert Academy [3131]
date: 2020-04-08 09:00:00-10:00:00
categories: ProblemSolving
tag: ProblemSolving
---

# [3131] 100만 이하의 모든 소수
> [SW Expert Academy](https://swexpertacademy.com/main/main.do){: target="_blank"}

## my solution
```python
# 에라토스테네스의 체 : n = 120인 경우, n의 루트값인 10.9544..가 나오는데, 이는 11보다 작으므로 11이하의 배수만 체크하면 됨

import math

num = 1000001
prime = [1] * num
max_length = math.ceil(math.sqrt(num))

for i in range(2, max_length):
    if prime[i]:
        # i 번째는 그대로 1로 두고, i+i번째(i의 배수)부터 계산
        # i 이후 i의 배수들을 0으로
        for j in range(i+i, num, i): # 이 부분이 핵심
            prime[j] = 0

answer = [i for i in range(2, num) if prime[i] != 0]
print(*answer)
```
