---
layout: post
title: SW Expert Academy [8104]
date: 2020-04-17 13:00:00-14:00:00
categories: ProblemSolving
tag: ProblemSolving
---

# [8104] 조 만들기
> [SW Expert Academy](https://swexpertacademy.com/main/main.do){: target="_blank"}

## my solution
```python
for t in range(int(input())):
    N, K = map(int, input().split())
    result = 0
    last = 0
    next = 1
    # N이 짝수인 경우(모두 같은 값이 출력)
    if N % 2 == 0:
        # 첫 번째 줄
        i = 0
        while next != N * K + 1:
            last = (4 * i) * K // 2
            result += last
            if last == N * K:
                break
            next = last + 1
            result += next
            i += 1

        print('#{}'.format(t+1), end=' ')
        for k in range(K):
            if k != K-1:
                print(result, end=' ')
            else:
                print(result)

    # N이 홀수인 경우(하나씩 증가해서 출력)
    else:
        # 첫 번째 줄
        for j in range(0, N, 2):
            last = K * j
            next = K * j + 1
            result += (last + next)

        print('#{}'.format(t+1), end=' ')
        for k in range(K):
            if k != K-1:
                print(result+k, end=' ')
            else:
                print(result+k)
```
