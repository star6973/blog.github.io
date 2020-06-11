---
layout: post
title: SW Expert Academy [4406]
date: 2020-04-09 17:00:00-18:00:00
categories: ProblemSolving
tag: ProblemSolving
---

# [4406] 모음이 보이지 않는 사람
> [SW Expert Academy](https://swexpertacademy.com/main/main.do){: target="_blank"}

## my solution
```python
vowel = ['a', 'e', 'i', 'o', 'u']
for t in range(int(input())):
    alpha = input()

    print('#{}'.format(t+1), end=' ')
    for a in alpha:
        if a not in vowel:
            print(a, end='')
    print()

# filter + lambda로 푸는 방식
for t in range(int(input())):
    print('#{} {}'.format(t+1, ''.join(filter(lambda x: x not in 'aeiou', input()))))

# 정규표현식으로 푸는 방식
import re
for t in range(int(input())):
    print(re.sub('a|e|i|o|u', '', input()))
```
