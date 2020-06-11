---
layout: post
title: SW Expert Academy [1983]
date: 2020-04-03 17:00:00-18:00:00
categories: ProblemSolving
tag: ProblemSolving
---

# [1983] 조교의 성적 매기기
> [SW Expert Academy](https://swexpertacademy.com/main/main.do){: target="_blank"}

## my solution
```python
T = int(input())
for t in range(T):
    N, K = map(int, input().split())

    tot_score = []
    for n in range(N):
        mid, term, ass = map(int, input().split())
        score_sum = mid * 0.35 + term * 0.45 + ass * 0.2
        tot_score.append((n+1, score_sum))

    cnt = 0
    for number, score in sorted(tot_score, key=lambda x: x[1], reverse=True):
        if number == K:
            break
        cnt += 1

    grade = ['A+', 'A0', 'A-', 'B+', 'B0', 'B-', 'C+', 'C0', 'C-', 'D0']
    grade_equal = N // 10
    print('#{} {}'.format(t+1, grade[cnt // grade_equal]))
```
