---
layout: post
title: SW Expert Academy [1234]
date: 2020-04-06 14:00:00-15:00:00
categories: ProblemSolving
tag: ProblemSolving
---

# [1234] 비밀번호
> [SW Expert Academy](https://swexpertacademy.com/main/main.do){: target="_blank"}

## my solution
```python
for t in range(10):
    N, secret = map(str, input().split())
    i = 0
    while i != len(secret)-1:
        if secret[i] == secret[i+1]:
            secret = secret.replace(secret[i:i+2], '')
            i = 0
        else:
            i += 1

    print('#{0} {1}'.format(t+1, secret))


for q in range(10):
    n, d = input().split()
    while 1:
      a = len(d)
      for i in range(a-1):
        if d[i] == d[i+1]:
            d = d[:i] + d[i+2:]
            break
      if a == len(d):
          break

    print(f'#{q+1} {d}')
```
