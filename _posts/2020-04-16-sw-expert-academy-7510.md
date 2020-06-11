---
layout: post
title: SW Expert Academy [7510]
date: 2020-04-16 15:00:00-17:00:00
categories: ProblemSolving
tag: ProblemSolving
---

# [7510] 상원이의 연속 합
> [SW Expert Academy](https://swexpertacademy.com/main/main.do){: target="_blank"}

## my solution
```python
for t in range(int(input())):
    result = 0
    num = 0
    temp = 1
    N = int(input())
    for i in range(1, N + 1):
        num += i
        # 만약 num이 N보다 커졌으면 앞에서부터 줄여 나간다
        while num > N:
            num -= temp
            temp += 1
        # print(i, temp, num)
        if num == N:
            result += 1

    print('#{0} {1}'.format(t+1, result))
```

## others solution
```python
# N = x + (x+1) + (x+2) + ... + (x+y)를 해결하면 됨
# x는 이어지는 자연수의 시작 위치, y는 연속된 숫자의 갯수
# 위의 수식을 풀어내면, x = (N - y * (y+1) / 2) / (y+1) 이 됨
# N은 문제에서 주어지고, y를 반복문으로 순회하며 임의로 정하며, 이로 인해 만들어진 x가 정수인지 파악
# 단, (N - y * (y+1) / 2) 가 0보다 커야 한다. 자연수이기 때문에
for t in range(int(input())):
    N = int(input())
    result = 1 # 자기 자신을 포함
    for y in range(1, N+1):
        chil = (N - y * (y + 1) / 2)
        pare = (y + 1)

        if chil <= 0:
            break
        result += (chil % pare == 0)

    print('#{0} {1}'.format(t+1, result))
```
