---
layout: post
title: SW Expert Academy [4676]
date: 2020-04-10 10:00:00-11:00:00
categories: ProblemSolving
tag: ProblemSolving
---

# [4676] 늘어지는 소리 만들기
> [SW Expert Academy](https://swexpertacademy.com/main/main.do){: target="_blank"}

## my solution
```python
for t in range(int(input())):
    string = input()
    H = int(input())
    hIdx = sorted(list(map(int, input().split())))
    dic = dict()
    for i in range(len(string)):
        dic[i] = string[i]

    for h in hIdx:
        if h == 0:
            dic[h] = '-' + dic[h]
        else:
            dic[h-1] = dic[h-1] + '-'

    print('#{0} {1}'.format(t+1, ''.join(dic.values())))
```

## others solution
```python
i = input
for tc in range(int(i())):
    a = list(i())
    i()
    h = list(sorted(map(int,i().split()), reverse=True))
    print(a)
    print(h)
    for x in h:
        a.insert(x, "-")
    print(f'#{tc+1}', ''.join(a))
```
