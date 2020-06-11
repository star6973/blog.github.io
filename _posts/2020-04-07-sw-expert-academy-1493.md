---
layout: post
title: SW Expert Academy [1493]
date: 2020-04-07 10:00:00-12:00:00
categories: ProblemSolving
tag: ProblemSolving
---

# [1493] 수의 새로운 연산
> [SW Expert Academy](https://swexpertacademy.com/main/main.do){: target="_blank"}

## my solution
```python
def calFunc(a, b):
    a1 = 1
    tmp = 0
    for i in range(a):
        tmp += (a1 + i)

    tmp2 = 0
    for j in range(b-1):
        tmp2 += (a+j)

    p = tmp + tmp2

    return p

def calFunc2(p):
    k = 1
    flag = False
    while flag != True:
        tmp = []
        for i in range(1, k+1):
            tmp.append((i, (k+1) - i))

        for (a, b) in tmp:
            result = calFunc(a, b)
            if result == p:
                flag = True
                break
        k += 1

    return a, b

for t in range(int(input())):
    p, q = map(int, input().split())
    p1, p2 = calFunc2(p)
    q1, q2 = calFunc2(q)

    print('#{0} {1}'.format(t+1, calFunc(p1+q1, p2+q2)))
```

## others solution
```python
def get_grid(num):
    m = num
    n = 1
    while m > 0:
        m -= n
        n += 1
    x = 1
    y = n - 1
    diff = num - (sum(range(1, n - 1)) + 1)
    for _ in range(diff):
        x += 1
        y += -1
    return (x, y)


def get_num(x, y):
    start = sum(range(1, y + (x - 1))) + 1
    return start + (x - 1)


t = int(input())
ans = ""

for tc in range(1, t + 1):
    p, q = map(int, input().split())

    x1, y1 = get_grid(p)
    x2, y2 = get_grid(q)
    x_new = x1 + x2
    y_new = y1 + y2
    res = get_num(x_new, y_new)

    ans += "#{} {}\n".format(tc, res)

print(ans)
```
