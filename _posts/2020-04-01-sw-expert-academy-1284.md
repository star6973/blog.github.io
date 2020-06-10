---
layout: post
title: SW Expert Academy [1284]
date: 2020-04-01 10:00:00-11:00:00
categories: ProblemSolving
tag: ProblemSolving
---

# [1284] 수도 요금 경쟁
> [SW Expert Academy](https://swexpertacademy.com/main/main.do){: target="_blank"}

## my solution
```python
T = int(input())
for t in range(T):
    P, Q, R, S, W = map(int, input().split())

    # A 회사
    company_A = P * W

    # B 회사
    company_B = 0
    # B 회사의 기준 리터보다 이하인 경우
    if W <= R:
        # 기본 요금 지불
        company_B = Q
    else:
        company_B = Q + S * (W-R)

    print('#{} {}'.format(t+1, company_A if company_A < company_B else company_B))
```
