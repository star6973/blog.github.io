---
layout: post
title: SW Expert Academy [1229]
date: 2020-04-08 11:00:00-12:00:00
categories: ProblemSolving
tag: ProblemSolving
---

# [1229] 암호문2
> [SW Expert Academy](https://swexpertacademy.com/main/main.do){: target="_blank"}

## my solution
```python
for t in range(10):
    N1 = int(input())
    encryption = list(input().split())
    N2 = int(input())
    command = list(input().split())

    command_idx = [i for i in range(len(command)) if command[i] in ['I', 'D']]
    for idx in command_idx:
        if command[idx] == 'I':
            insert_loc = int(command[idx+1])
            insert_cnt = int(command[idx+2])
            insert_num = command[idx+3:idx+3+insert_cnt]

            for i_n in range(insert_cnt-1, -1, -1):
                encryption.insert(insert_loc, insert_num[i_n])

        if command[idx] == 'D':
            delete_loc = int(command[idx+1])
            delete_cnt = int(command[idx+2])

            for d_c in range(delete_cnt):
                del encryption[delete_loc]

    print("#{0} {1}".format(t + 1, " ".join(map(str, encryption[:10]))))
```
