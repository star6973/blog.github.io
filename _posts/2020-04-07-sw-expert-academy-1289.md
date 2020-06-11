---
layout: post
title: SW Expert Academy [1289]
date: 2020-04-07 09:00:00-10:00:00
categories: ProblemSolving
tag: ProblemSolving
---

# [1289] 원재의 메모리 복구하기
> [SW Expert Academy](https://swexpertacademy.com/main/main.do){: target="_blank"}

## my solution
```python
for t in range(int(input())):
    bit = list(input())
    new_bit = []
    for idx in range(len(bit)-1):
        if bit[idx] != bit[idx+1]:
            new_bit.append(bit[idx])

    new_bit.append(bit[idx+1])
    if new_bit[0] == '0':
        del new_bit[0]

    print('#{} {}'.format(t+1, len(new_bit)))
```

## others solution
```python
for i in range(int(input())):
    a = input()
    r = int(a[0])
    for j in range(len(a)-1):
        if a[j:j+2] in ('01', '10'):
            r += 1
    print(f'#{i+1} {r}')
```
