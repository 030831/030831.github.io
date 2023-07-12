---
layout: post
title: "Gaounuri_Algorithm_Study_Week 1-1"
categories: Gaounuri_Algorithm_Study
---

[문제 링크](https://www.acmicpc.net/problem/1049)

> 📌 사용알고리즘 : DP


원래는 그리디로 풀수있을꺼같았지만 `dp` 연습할겸 `dp` 로 풀었습니다.

그리디로 풀자면 1개 가격과 6개 가격 상품을 오름차순으로 정렬해서 각각 최소값을 구한후에
1개로 `N` 개를 전부 구매하는법과 6개와 1개 상품을 섞어서 `N` 개를 구매했을때 둘중 최소값이 정답이 됩니다.

```python
N+=6
dp=[ [0]*107 for _ in range(107) ]
```

2차원 `dp` 를 선언해줍니다. 이때 최대크기를 106 으로 잡았습니다.
문제에서는 적어도 `N` 개를 구매해야하는데 $N$ 이 100 일때 가격보다 106 개를 샀을때의 가격이 더 쌀수도있기 때문입니다. 따라서 범위도  $N$ 에다가 6 을 더해주었습니다.

```python
for i in range(1,M+1):
    six,one = MI()
    dp[i][1] = one
    dp[i][6] = six
```

초기값을 할당해줍니다. 

* `dp[i][j]` : `i` 번째 상품을 고려했을때 `j` 개의 제품을 구매하기 위한 최소값

`dp` 테이블 정의는 위와 같습니다.

```python
for i in range(1,N+1):
    if 1<i<6:
        dp[1][i] += dp[1][i-1] + dp[1][1]
        continue
    dp[1][i] = min(dp[1][i-1]+dp[1][1] , dp[1][i-6]+dp[1][6])
```
    
첫번째 상품을 고려할때는 6 개 제품을 사기전까지는  1개 가격을 계속 더해주고
6개 제품을 살수있을때 의사코드는 다음과 같습니다.

```python
dp[1][i] = min(dp[1][i-1]+dp[1][1] , dp[1][i-6]+dp[1][6])
```
이전에 `i-1` 개를 샀던 가격의 총 합에서 한개를 더 살지 , `i-6` 개를 샀던 가격의 총합에서 6개를 더 살지에 대한 것입니다.

이 점화식을 모든 상품에 대해 고려를 해보면

```python
for i in range(2,M+1):
    for j in range(1,N+1):
        if j<6:
            dp[i][j] = min(dp[i-1][j-1]+ dp[i][1]  , dp[i][j-1]+ dp[i][1] , dp[i-1][j])
            continue
        dp[i][j] = min(dp[i-1][j-1]+dp[i][1] , dp[i][j-1]+dp[i][1] , dp[i-1][j-6]+dp[i][6] , dp[i][j-6]+dp[i][6] , dp[i-1][j])
```
위와 같은 식이 됩니다.

```python
if j<6:
	dp[i][j] = min(dp[i-1][j-1]+ dp[i][1]  , dp[i][j-1]+ dp[i][1] , dp[i-1][j])
```

`i-1` 번째 상품에서 `j-1`	 개의 제품 가격에서 `i` 번째 상품 1개를 더 살지 , `i` 번째 상품에서 `j-1` 개의 제품 가격에서 `i` 번째 상품을 1개 더 살지 , `i-1` 번째 상품에서 `j` 개의 제품 가격을 고려할지에 대한 부분입니다.

```python
dp[i][j] = min(dp[i-1][j-1]+dp[i][1] , dp[i][j-1]+dp[i][1] , dp[i-1][j-6]+dp[i][6] , dp[i][j-6]+dp[i][6] , dp[i-1][j])

# j 가 6이상일때 추가된 부분- dp[i-1][j-6]+dp[i][6] , dp[i][j-6]+dp[i][6]
```
똑같이 `j` 가 6이상일때는 `i-1` 번째 상품에서 `j-6` 개의 제품가격에서 6개를 살지
`i` 번째 상품에서 `j-6` 개의 제품가격에서 6개를 살지에 대한 부분입니다.

> 📌 전체 코드

```python
import sys
input=sys.stdin.readline
from collections import deque
LMI=lambda:list(map(int,input().split()))
LMS=lambda:list(map(str,input().split()))
MI=lambda:map(int,input().split())
I=lambda:int(input())
GI=lambda x:[ LMI() for _ in range(x) ]
GS=lambda x:[ LMS() for _ in range(x) ]
V=lambda x,y:[ [False]*y for _ in range(x) ]

N,M = MI()
N+=6

dp=[ [0]*107 for _ in range(107) ]

for i in range(1,M+1):
    six,one = MI()
    dp[i][1] = one
    dp[i][6] = six

for i in range(1,N+1):
    if 1<i<6:
        dp[1][i] += dp[1][i-1] + dp[1][1]
        continue
    dp[1][i] = min(dp[1][i-1]+dp[1][1] , dp[1][i-6]+dp[1][6])
for i in range(2,M+1):
    for j in range(1,N+1):
        if j<6:
            dp[i][j] = min(dp[i-1][j-1]+ dp[i][1]  , dp[i][j-1]+ dp[i][1] , dp[i-1][j])
            continue
        dp[i][j] = min(dp[i-1][j-1]+dp[i][1] , dp[i][j-1]+dp[i][1] , dp[i-1][j-6]+dp[i][6] , dp[i][j-6]+dp[i][6] , dp[i-1][j])

print(min(dp[M][N-6:N]))
```
