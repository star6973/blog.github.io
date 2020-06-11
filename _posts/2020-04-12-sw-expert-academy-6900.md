---
layout: post
title: SW Expert Academy [6900]
date: 2020-04-12 09:00:00-11:00:00
categories: ProblemSolving
tag: ProblemSolving
---

# [6900] 주혁이의 복권 당첨
> [SW Expert Academy](https://swexpertacademy.com/main/main.do){: target="_blank"}

## my solution
```python
for t in range(int(input())):
    N, M = map(int, input().split())

    winning = dict()
    for n in range(N):
        number, coin = map(str, input().split())
        winning[number] = int(coin)

    buy = []
    for m in range(M):
        buy.append(input())

    answer = 0
    for key, value in winning.items():
        # key에서 '*'의 위치 찾기
        price = 0
        key_idx = [i for i in range(len(key)) if key[i] == '*']

        for num in buy:
            num2 = list(num)
            for idx in key_idx:
                num2[idx] = '*'
            num2 = ''.join(num2)
            try:
                price += winning[num2]
            except Exception:
                pass

        answer += price

    print('#{0} {1}'.format(t+1, answer))
```
