---
title: Codeforces 暑期特訓：我想成為演算法大師 - 2017-2018 ACM-ICPC, Asia Daejeon Regional Contest - Week 10
date: 2025-09-18 18:50:35
tags:
  - algorithm
  - codeforces
  - competitive-programming
---

# 2017-2018 ACM-ICPC, Asia Daejeon Regional Contest

- [Contest Problems](https://codeforces.com/gym/101667)

## Happy Number

> **Problem:** [Happy Number](https://codeforces.com/gym/101667/problem/D)
>
> **Solution:** [GitHub Code](https://github.com/wulukewu/cp-code/blob/main/codeforces/gym/101667/D.py)

```python
n = int(input())
nn = n

visit = []
ans = False
while True:
    x = 0
    for i in str(nn):
        j = int(i)
        x += j ** 2
    nn = x

    if x in visit:
        break
    else:
        visit.append(x)

    if x == 1:
        ans = True
        break
    elif x == n:
        break

if ans:
    print('HAPPY')
else:
    print('UNHAPPY')
```
