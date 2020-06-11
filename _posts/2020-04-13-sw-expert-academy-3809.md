---
layout: post
title: SW Expert Academy [3809]
date: 2020-04-13 13:00:00-15:00:00
categories: ProblemSolving
tag: ProblemSolving
---

# [3809] 화섭이의 정수 나열
> [SW Expert Academy](https://swexpertacademy.com/main/main.do){: target="_blank"}

## my solution
```python
for t in range(int(input())):
    N = int(input())
    integer = []
    while True:
        if len(integer) == N:
            break
        else:
            input_data = input()
            integer.extend(input_data.split())

    flag = False
    answer = []
    for k in range(len(integer)):
        if flag == True:
            break
        for i in range(len(integer)-k):
            answer.append(int(''.join(integer[i:(i+1)+k])))
            answer = sorted(set(answer))
        for j in range(len(answer)-1):
            if answer[j] + 1 != answer[j+1]:
                flag = True
                break
    # print(answer)
    if answer[0] == 0:
        print('#{0} {1}'.format(t+1, answer[j]+1))
    else:
        print('#{0} {1}'.format(t+1, 0))
```

## others solution
```python
for T in range(int(input())):
    N = int(input())
    arr = ''
    while len(arr) != N:
        arr += input().replace(' ', '')

    num = 0
    while str(num) in arr:
        print(str(num))
        num += 1

    print('#{} {}'.format(T + 1, num))
```
