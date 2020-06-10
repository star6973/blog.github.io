---
layout: post
title: SW Academy [1873]
date: 2020-01-01 09:00:00-12:00:00
categories: ProblemSolving
tag: ProblemSolving
---

# [1873] 상호의 배틀필드

    상호는 전차로 시가전을 하는 것을 테마로 한 새로운 게임 “배틀 필드”를 개발하기로 했다.
    그래서 먼저 간단하게 프로토 타입 게임을 만들었다.
    이 프로토 타입에서 등장하는 전차는 사용자의 전차 하나뿐이며, 적이나 아군으로 만들어진 전차는 등장하지 않는다.
    사용자의 전차는 사용자의 입력에 따라 격자판으로 이루어진 게임 맵에서 다양한 동작을 한다.
    
    다음 표는 게임 맵의 구성 요소를 나타낸다.
    
    문자	의미
    
        .	평지(전차가 들어갈 수 있다.)
        *	벽돌로 만들어진 벽
        #	강철로 만들어진 벽
        -	물(전차는 들어갈 수 없다.)
        ^	위쪽을 바라보는 전차(아래는 평지이다.)
        v	아래쪽을 바라보는 전차(아래는 평지이다.)
        <	왼쪽을 바라보는 전차(아래는 평지이다.)
        >	오른쪽을 바라보는 전차(아래는 평지이다.)
        다음 표는 사용자가 넣을 수 있는 입력의 종류를 나타낸다.
    
    문자	동작
    
        U	Up : 전차가 바라보는 방향을 위쪽으로 바꾸고, 한 칸 위의 칸이 평지라면 위 그 칸으로 이동한다.
        D	Down : 전차가 바라보는 방향을 아래쪽으로 바꾸고, 한 칸 아래의 칸이 평지라면 그 칸으로 이동한다.
        L	Left : 전차가 바라보는 방향을 왼쪽으로 바꾸고, 한 칸 왼쪽의 칸이 평지라면 그 칸으로 이동한다.
        R	Right : 전차가 바라보는 방향을 오른쪽으로 바꾸고, 한 칸 오른쪽의 칸이 평지라면 그 칸으로 이동한다.
        S	Shoot : 전차가 현재 바라보고 있는 방향으로 포탄을 발사한다.
    
    전차가 이동을 하려고 할 때, 만약 게임 맵 밖이라면 전차는 당연히 이동하지 않는다.
    전차가 포탄을 발사하면, 포탄은 벽돌로 만들어진 벽 또는 강철로 만들어진 벽에 충돌하거나 게임 맵 밖으로 나갈 때까지 직진한다.
    만약 포탄이 벽에 부딪히면 포탄은 소멸하고, 부딪힌 벽이 벽돌로 만들어진 벽이라면 이 벽은 파괴되어 칸은 평지가 된다.
    강철로 만들어진 벽에 포탄이 부딪히면 아무 일도 일어나지 않는다.
    게임 맵 밖으로 포탄이 나가면 아무런 일도 일어나지 않는다.
    
    초기 게임 맵의 상태와 사용자가 넣을 입력이 순서대로 주어질 때, 모든 입력을 처리하고 나면 게임 맵의 상태가 어떻게 되는지 구하는 프로그램을 작성하라.

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
