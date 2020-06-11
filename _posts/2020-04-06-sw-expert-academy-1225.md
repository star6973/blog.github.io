---
layout: post
title: SW Expert Academy [1225]
date: 2020-04-06 16:00:00-18:00:00
categories: ProblemSolving
tag: ProblemSolving
---

# [1225] 암호생성기
> [SW Expert Academy](https://swexpertacademy.com/main/main.do){: target="_blank"}

## my solution
```python
for tc in range(10):
    t = int(input())
    encrpytion = list(map(int, input().split()))

    k = 1
    # 마지막 숫자가 0으로 변환되면 프로그램 종료
    while encrpytion[-1] != 0:
        # 한 번의 사이클은 5번 숫자 감소
        if k == 6:
            k = 1
        encrpytion[0] -= k
        if encrpytion[0] <= 0:
            encrpytion[0] = 0

        # encrpytion.append(encrpytion[0])
        # del encrpytion[0]

        encrpytion = encrpytion[1:] + encrpytion[:1]
        k += 1

    print('#{0} {1}'.format(t, ' '.join(map(str, encrpytion))))
```

## others solution
```python
input()
L = [*map(int, input().split())]
print(L)
c = 0
while (c % 5) + 1 < L[0]:
    L.append(L.pop(0)-(c % 5)-1)
    c += 1
    L = L[1:] + [0]
    print(*L)
```
