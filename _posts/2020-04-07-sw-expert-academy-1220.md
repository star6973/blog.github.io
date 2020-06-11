---
layout: post
title: SW Expert Academy [1220]
date: 2020-04-07 17:00:00-19:00:00
categories: ProblemSolving
tag: ProblemSolving
---

# [1220] Magnetic
> [SW Expert Academy](https://swexpertacademy.com/main/main.do){: target="_blank"}

## my solution
```python
for t in range(10):

    n = int(input())

    table = []
    for i in range(n):
        table.append(list(input().split()))

    col_table = []
    for j in range(len(table)):
        tmp = []
        for i in range(len(table[j])):
            tmp.append(table[i][j])
        col_table.append(tmp)

    # 왼쪽이 N극, 오른쪽이 S극
    # 1인 경우, 오른쪽에 2가 있을 때까지 계속 오른쪽으로 이동
    # 2인 경우, 왼쪽에 1이 있을 때까지 계속 왼쪽으로 이동
    cnt = 0
    for col in col_table:
        # print('변환 전', col)
        for i in range(len(col)-1):
            if col[i] == '1' and col[i+1] == '0':
                col[i], col[i+1] = col[i+1], col[i]
            if col[len(col)-(i+1)] == '2' and col[len(col)-(i+2)] == '0':
                col[len(col)-(i+1)], col[len(col)-(i+2)] = col[len(col)-(i+2)], col[len(col)-(i+1)]

        # 양 끝에 각각 1이나 2가 있으면 0으로 변환
        if col[0] == '2':
            col[0] = '0'
        if col[len(col)-1] == '1':
            col[len(col)-1] = '0'

        # print('변환 후', col)
        col = ''.join(col)
        # print(col)
        # print(col.split('12'))

        if len(col.split('12')) == 1:
            pass
        else:
            cnt += len(col.split('12')) - 1

    print('#{} {}'.format(t+1, cnt))
```

## others solution
```python
# 문제를 어렵지 않게 생각팔 필요가 있음.
# 결국, 12가 붙어있는 경우만 세면 되기 됨.
for t in range(10):
    n = int(input())
    a = [str(''.join(input().split())) for _ in [0] * n]
    # zip(*)을 이용하여 전치행렬로 변환할 수 있음
    b = list(''.join(i) for i in zip(*a))
    c = 0
    for d in b:
        c += d.replace('0', '').count('12')

    print('#{} {}'.format(t+1, c))
```
