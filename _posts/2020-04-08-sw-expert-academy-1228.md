---
layout: post
title: SW Expert Academy [1228]
date: 2020-04-08 10:00:00-11:00:00
categories: ProblemSolving
tag: ProblemSolving
---

# [1228] 암호문1
> [SW Expert Academy](https://swexpertacademy.com/main/main.do){: target="_blank"}

## my solution
```python
for t in range(10):
    N = int(input())
    encryption = list(map(int, input().split()))
    I = int(input())
    command = list(map(str, input().split()))

    for i in range(len(command)):
        if command[i] == 'I':
            x = int(command[i+1])
            y = int(command[i+2])
            s = command[i+3:i+3+y]

            for j in range(len(s)-1, -1, -1):
                encryption.insert(x, int(s[j]))

    print('#{0}'.format(t+1), end=' ')
    print(*encryption[:10])
```
