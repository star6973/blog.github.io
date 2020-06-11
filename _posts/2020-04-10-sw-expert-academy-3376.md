---
layout: post
title: SW Expert Academy [3376]
date: 2020-04-10 15:00:00-18:00:00
categories: ProblemSolving
tag: ProblemSolving
---

# [3376] 파도반 수열
> [SW Expert Academy](https://swexpertacademy.com/main/main.do){: target="_blank"}

## my solution
```python
# 재귀함수의 대표적인 문제: 피보나치 수열
# 1. 기본 재귀적 풀이
'''
    def fibo(n):
        return fibo(n-1) + fibo(n-2) if n >= 2 else n
    
    for n in range(1, 11):
        print(n, fibo(n))
    
'''
# 재귀함수를 동적계획법으로 풀어보자.
# 동적 계획법 풀이에서는 부분문제들을 해결할 때마다 값을 저장하는 캐시를 만든다.
# 이런 동적 계획법 문제를 풀 때 해결할 수 있는 방법은

# 2-1) 반복적 동적 계획법 풀이(iterative)
#      : 부분문제의 답을 계산할 캐시의 형태나 타입은 문제의 특성에 따라 다양하게 설정할 수 있는데,
#        이렇게 쉬운 문제에서는 n 번째 피보나치 수를 n 인덱스에 저장하는 1차원 배열이면 충분해 보인다.
'''

    # n을 100이라고 하자
    def fibo(n):
        if n < 2:
            return n

        cache = [0 for _ in range(n+1)]
        cache[1] = 1
        
        for i in range(2, 100+1):
            cache[i] = cache[i-1] + cache[i-2]
            
        return cache[n]
    
    print(fibo(100))

    # 부분문제의 값을 저장하고 그 부분문제를 해결해 나가며 최종적으로 답을 구한다. 이 풀이는 아까 봤던 반복적 풀이와 원리적으로 같다.
    # 둘을 굳이 분류한 것은 아까처럼 두 변수를 반복할 수 있는 것은 피보나치 알고리즘의 특수한 특성일 뿐이며 지금처럼 푸는 것이 동적 계획법의 정석이기 때문이다.

'''

# 2-2) 재귀적 동적 계획법 풀이(recursive)
# 반복적 동적 계획법 풀이도 훌륭하지만, 재귀 함수를 사용하는 동적 계획법 풀이도 존재한다.
# 앞서 기본 재귀적 풀이의 비효율성을 언급하면서 ‘동적 계획법을 도입해 첫 부분문제를 해결하고,
# 다시 필요할 때마다 가져다 쓰자’고 했는데 그 느낌을 살리기에는 이 방법이 더 괜찮다.
# 앞의 이 방법은 재귀적 풀이와 비슷하다. 차이가 있다면 캐시를 만들고, 그 캐시에서 "부분문제의 답을 찾고 없으면 직접 계산"한다는 정도이다.

'''

    def fibo(n):
        cache = [-1 for _ in range(n+1)]
        
        def iterate(n):
            # 기저사례 1
            if i < 2:
                return i
            
            # 기저사례 2
            if cache[n] != -1:
                return cache[0]
            
            # 기저사례 충족못할 시 값을 실제로 구함
            cache[n] = iterate(n-1) + iterate(n-2)

            return cache[n]

        return iterate(n)

    for n in range(1, 11):
        print(n, fibo(n))
        
    # 함수를 재귀적으로 풀 때 언제나 염두에 두어야 할 것은 함수가 더 이상 재귀 호출하지 않고 끝날 조건을 설정해줘야 하는 것이다. 
    # 그렇지 않으면 함수 스택이 넘치게 된다. 재귀 함수가 끝나도록 하는 조건을 기저사례(base case)라 한다.
    #
    # 피보나치 알고리즘에서 기저사례는 n이 2 미만일 때다.(n은 0 이상의 정수로 이미 한정했다) 
    # 그리고 우리의 알고리즘에서는 기저사례가 하나 더 추가되는데, 그것은 원하는 부분문제를 이미 해결했을 때이다. 
    # 5번째 피보나치 수를 구하는 부분문제를 해결했다면, 다시 그 수가 필요할 때 그대로 가져다 쓰면 된다.
    # 캐시의 모든 인덱스를 '-1'로 초기화했다. 꼭 '-1'일 필요는 없는데, 
    # 모든 피보나치 수는 0 이상의 정수이기 때문에 캐시 값이 '-1'이라는 것은 n 번째 피보나치 수를 아직 계산 안 했다는 것이 된다. 
    # 캐시의 n 번째 인덱스의 값이 '-1'이 아니면 n 번째 피보나치 수를 구하는 부분문제를 이미 해결다는 뜻이기에 그대로 값을 반환한다.
    # 두 기저사례를 하나라도 만족하지 못하면 재귀함수를 호출해 값을 구한다. 이때, 새로 구한 n 번째 값을 캐시에 저장함으로써 향후 이 값이 필요할 때 다시 사용한다.

'''

def PN(n):
    cache = [0 for _ in range(n+1)]

    if n < 4:
        return 1
    else:
        cache[1] = 1
        cache[2] = 1
        cache[3] = 1

        for i in range(4, n+1):
            cache[i] = cache[i-2] + cache[i-3]

        return cache[n]

for t in range(int(input())):
    print('#{0} {1}'.format(t+1, PN(int(input()))))
```
