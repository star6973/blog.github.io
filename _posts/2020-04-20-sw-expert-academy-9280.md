---
layout: post
title: SW Expert Academy [9280]
date: 2020-04-20 14:00:00-18:00:00
categories: ProblemSolving
tag: ProblemSolving
---

# [9280] 진용이네 주차타워
> [SW Expert Academy](https://swexpertacademy.com/main/main.do){: target="_blank"}

## my solution
```python
for t in range(int(input())):
    N, M = map(int, input().split())
    R_i = dict()  # i번째 주차 공간의 단위 무게당 요금 R
    for n in range(N):
        R_i[n+1] = int(input())
    W_i = dict()  # 차량 i의 무게 W
    for m in range(M):
        W_i[m+1] = int(input())

    sequence = []
    for m in range(2*M):
        sequence.append(int(input()))

    parking = [0 for _ in range(N)]
    answer = 0
    seq = 0
    wait = []

    print(R_i)
    print(W_i)

    while True:

        if wait == [] and seq == len(sequence):
            break

        # 양수이면 출입(남은 자리로 주차 - 작은 번호 순)
        if sequence[seq] > 0:
            # 다음 주차 구역 가능한 곳
            for p in range(len(parking)):
                if parking[p] == 0:
                    print(sequence[seq], '차량', p+1, '번 주차 구역으로 진입')
                    parking[p] = sequence[seq]
                    answer += R_i[p+1] * W_i[sequence[seq]]
                    p = 0
                    break

            # 주차 구역이 가능한 곳이 남아있지 않으면 대기 구역에 진입
            if p == len(parking)-1:
                print(sequence[seq], '차량 대기 구역으로 진입')
                wait.append(sequence[seq])

        # 음수이면 출차
        elif sequence[seq] < 0:
            # 출차 후, 빈 곳으로 만들기
            for p in range(len(parking)):
                if parking[p] == -sequence[seq]:
                    print(-sequence[seq], '차량', p+1, '번 주차 구역에서 출차')
                    parking[p] = 0
                    break

            # 대기 구역에 차가 있으면 남은 자리로 진입
            if len(wait) >= 1:
                for p in range(len(parking)):
                    if parking[p] == 0:
                        print(wait[0], '차량 대기 구역에서 ', p+1, '번 주차 구역으로 진입')
                        parking[p] = wait[0]
                        answer += R_i[p+1] * W_i[wait[0]]
                        break

                wait.remove(wait[0])

        print(answer)
        seq += 1

    print('#{0} {1}'.format(t+1, answer))
```
