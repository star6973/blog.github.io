---
layout: post
title: SW Expert Academy [1240]
date: 2020-04-08 15:00:00-16:00:00
categories: ProblemSolving
tag: ProblemSolving
---

# [1240] 단순 2진 암호코드
> [SW Expert Academy](https://swexpertacademy.com/main/main.do){: target="_blank"}

## my solution
```python
code = {'0001101': 0, '0011001': 1, '0010011': 2, '0111101': 3, '0100011': 4, '0110001': 5, '0101111': 6, '0111011': 7, '0110111': 8, '0001011': 9}

for t in range(int(input())):
    N, M = map(int, input().split())
    case = [input() for _ in [0] * N]

    # 추출해야 하는 암호코드는 0으로 시작해서 1로 끝나야 함
    crawlCase = ''
    for c in case:
        if '1' in c:
            idx = c.rfind('1')
            crawlCase = c[idx-55:idx+1]
            break
    # print(crawlCase)

    # 최종 암호코드
    info = []
    for i in range(0, len(crawlCase), 7):
        codeCase = crawlCase[i:i+7]
        # print(codeCase)
        info.append(code[codeCase])
    # print(info)

    # 암호코드 규칙 확인
    tot = (info[0]+info[2]+info[4]+info[6]) * 3 + (info[1]+info[3]+info[5]) + info[7]
    if tot % 10 == 0:
        print('#{0} {1}'.format(t+1, sum(info)))
    else:
        print('#{0} {1}'.format(t+1, 0))
```
