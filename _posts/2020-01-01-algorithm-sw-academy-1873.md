---
layout: post
title: SW Expert Academy [1873]
date: 2020-01-01 09:00:00-12:00:00
categories: ProblemSolving
tag: ProblemSolving
---

# [1873] 상호의 배틀필드
> [SW Expert Academy](https://swexpertacademy.com/main/main.do){: target="_blank"}

## 단순 무식 :(
```python
def isLoc(i, j, game):
    if i < 0 or j < 0 or i >= len(game) or j >= len(game[i]):
        return False
    else:
        return True

for t in range(int(input())):
    H, W = map(int, input().split())
    game = [input() for _ in range(H)]
    N = int(input())
    command = input()

    # print(game)
    # print(command)

    here = ''
    dir = ''
    for i in range(len(game)):
        for j in range(len(game[i])):
            if game[i][j] == '<':
                here = [i, j]
                dir = '<'
            elif game[i][j] == '>':
                here = [i, j]
                dir = '>'
            elif game[i][j] == '^':
                here = [i, j]
                dir = '^'
            elif game[i][j] == 'v':
                here = [i, j]
                dir = 'v'

    # print('start', here)
    # print('loc', dir)

    game = [list(g) for g in game]

    for com in command:
        # 일반벽('*')이면 부시고, 강철벽('#')이면 못 부심
        if com == 'S':
            # 좌측을 보고있는 경우, 벽의 변환 및 방향은 그대로
            if dir == '<':
                temp = here.copy()
                # 벽을 만날 때까지의 위치
                while True:
                    # 다음 위치가 올바른 경우
                    if isLoc(here[0], here[1]-1, game):
                        # 다음 위치의 지형 상태
                        ground = game[here[0]][here[1]-1]
                        if ground == '*':
                            # print([here[0], here[1]-1], '에 슈팅')
                            game[here[0]][here[1]-1] = '.'
                            break
                        elif ground == '#':
                            # print('강철벽이다!!')
                            break
                        else:
                            here = [here[0], here[1]-1]
                    else:
                        # print('범위를 벗어났습니다')
                        break

                # 슈팅하고 다시 원위치
                here = temp

            # 우측을 보고있는 경우, 벽의 변환 및 방향은 그대로
            elif dir == '>':
                temp = here.copy()
                # 벽을 만날 때까지의 위치
                while True:
                    # 다음 위치가 올바른 경우
                    if isLoc(here[0], here[1]+1, game):
                        ground = game[here[0]][here[1]+1]
                        if ground == '*':
                            # print([here[0], here[1]+1], '에 슈팅')
                            game[here[0]][here[1]+1] = '.'
                            break
                        elif ground == '#':
                            # print('강철벽이다!!')
                            break
                        else:
                            here = [here[0], here[1]+1]
                    else:
                        # print('범위를 벗어났습니다')
                        break

                # 슈팅하고 다시 원위치
                here = temp

            # 위측을 보고있는 경우, 벽의 변환 및 방향은 그대로
            elif dir == '^':
                temp = here.copy()
                # 벽을 만날 때까지의 위치
                while True:
                    # 다음 위치가 올바른 경우
                    if isLoc(here[0]-1, here[1], game):
                        ground = game[here[0]-1][here[1]]
                        if ground == '*':
                            # print([here[0]-1, here[1]], '에 슈팅')
                            game[here[0]-1][here[1]] = '.'
                            break
                        elif ground == '#':
                            # print('강철벽이다!!')
                            break
                        else:
                            here = [here[0]-1, here[1]]
                    else:
                        # print('범위를 벗어났습니다')
                        break

                # 슈팅하고 다시 원위치
                here = temp

            # 아래측을 보고있는 경우, 벽의 변환 및 방향은 그대로
            elif dir == 'v':
                temp = here.copy()
                # 벽을 만날 때까지의 위치
                while True:
                    # 다음 위치가 올바른 경우
                    if isLoc(here[0]+1, here[1], game):
                        ground = game[here[0]+1][here[1]]
                        if ground == '*':
                            # print([here[0]+1, here[1]], '에 슈팅')
                            game[here[0]+1][here[1]] = '.'
                            break
                        elif ground == '#':
                            # print('강철벽이다!!')
                            break
                        else:
                            here = [here[0]+1, here[1]]
                    else:
                        # print('범위를 벗어났습니다')
                        break

                # 슈팅하고 다시 원위치
                here = temp

        # 우측으로 방향 전환 뒤, 평지('.')이면 이동
        elif com == 'R':
            dir = '>'
            game[here[0]][here[1]] = dir
            # 다음 위치가 올바른 경우
            if isLoc(here[0], here[1]+1, game):
                ground = game[here[0]][here[1]+1]
                if ground == '.':
                    game[here[0]][here[1]] = '.'
                    # print([here[0], here[1]+1], '으로 이동')
                    game[here[0]][here[1]+1] = dir
                    here = [here[0], here[1]+1]
                elif ground == '-':
                    # print('물이 있어서 이동이 불가능합니다')
                    game[here[0]][here[1]] = dir
                else:
                    # print('벽이 있습니다!')
                    game[here[0]][here[1]] = dir

        # 좌측으로 방향 전환 뒤, 평지('.')이면 이동
        elif com == 'L':
            dir = '<'
            game[here[0]][here[1]] = dir
            # 다음 위치가 올바른 경우
            if isLoc(here[0], here[1]-1, game):
                ground = game[here[0]][here[1]-1]
                if ground == '.':
                    game[here[0]][here[1]] = '.'
                    # print([here[0], here[1]-1], '으로 이동')
                    game[here[0]][here[1]-1] = dir
                    here = [here[0], here[1]-1]
                elif ground == '-':
                    # print('물이 있어서 이동이 불가능합니다')
                    game[here[0]][here[1]] = dir
                else:
                    # print('벽이 있습니다!')
                    game[here[0]][here[1]] = dir

        # 위측으로 방향 전환 뒤, 평지('.')이면 이동
        elif com == 'U':
            dir = '^'
            game[here[0]][here[1]] = dir
            # 다음 위치가 올바른 경우
            if isLoc(here[0]-1, here[1], game):
                ground = game[here[0]-1][here[1]]
                if ground == '.':
                    game[here[0]][here[1]] = '.'
                    # print([here[0]-1, here[1]], '으로 이동')
                    game[here[0]-1][here[1]] = dir
                    here = [here[0]-1, here[1]]
                elif ground == '-':
                    # print('물이 있어서 이동이 불가능합니다')
                    game[here[0]][here[1]] = dir
                else:
                    # print('벽이 있습니다!')
                    game[here[0]][here[1]] = dir

        # 아래측으로 방향 전환 뒤, 평지('.')이면 이동
        elif com == 'D':
            dir = 'v'
            game[here[0]][here[1]] = dir
            # 다음 위치가 올바른 경우
            if isLoc(here[0]+1, here[1], game):
                ground = game[here[0]+1][here[1]]
                if ground == '.':
                    game[here[0]][here[1]] = '.'
                    # print([here[0]+1, here[1]], '으로 이동')
                    game[here[0]+1][here[1]] = dir
                    here = [here[0]+1, here[1]]
                elif ground == '-':
                    # print('물이 있어서 이동이 불가능합니다')
                    game[here[0]][here[1]] = dir
                else:
                    # print('벽이 있습니다!')
                    game[here[0]][here[1]] = dir

    print('#{}'.format(t+1), end=' ')
    for g in game:
        print(''.join(g))
```

## 멋지다..!
```python
Y = [-1, 1, 0, 0]
X = [0, 0, -1, 1]
for t in range(1, int(input())+1):
    h, w = map(int, input().split())
    T = '^v<>'
    e = {'U': 0, 'D': 1, 'L': 2, 'R': 3}
    b = []

    for i in range(h):
        b.append([*input()])
        for j in range(w):
            if b[i][j] in T:
                y, x = i, j
                D = T.index(b[i][j])

    n = int(input())
    o = input()
    for c in o:
        if c == 'S':
            p, q = y + Y[D], x + X[D]
            while -1 < p < h and -1 < q < w:
                if b[p][q] == '#':
                    break
                if b[p][q] == '*':
                    b[p][q] = '.'
                    break
                p, q = p + Y[D], q + X[D]
        else:
            d = e[c]
            p, q = y + Y[d], x + X[d]
            if -1 < p < h and -1 < q < w and b[p][q] == '.':
                b[y][x] = '.'
                b[p][q] = T[d]
                y, x = p, q
                D = d
            else:
                b[y][x] = T[d]
                D = d

    print('#%i'%t,end=" ")
    for i in b:
        print(*i,sep='')
```
