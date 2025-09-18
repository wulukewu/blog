---
title: Codeforces 暑期特訓：我想成為演算法大師 - NYCU 2025 Team Selection Programming Contest - Week 9
date: 2025-09-17 11:21:31
tags:
  - algorithm
  - codeforces
  - competitive-programming
---

# National Yang Ming Chiao Tung University 2025 Team Selection Programming Contest

- [Contest Problems](https://codeforces.com/gym/106059)

## K. Karl's Dormitory Allocation

> **Problem:** [K. Karl's Dormitory Allocation](https://codeforces.com/gym/106059/problem/K)
>
> **Solution:** [GitHub Code](https://github.com/wulukewu/cp-code/blob/main/codeforces/gym/106059/K.py)

```python
import math

n, m = map(int, input().split())
arr = list(map(int, input().split()))
arr.sort(reverse=True)

a = arr[m-1]
b = arr[m]
# print(a, b)
v = (a + b) / 2
total = m * v

e = total / n
pay = v - e

# print(pay, e)
p = math.floor(pay)
q = math.ceil(e)
# print(p, q)


r = abs(m * p - (n-m)*q)
print(p, q, r)
```

## L. Lantern Festival

> **Problem:** [L. Lantern Festival](https://codeforces.com/gym/106059/problem/L)
>
> **Solution:** [GitHub Code](https://github.com/wulukewu/cp-code/blob/main/codeforces/gym/106059/L.cpp)

```cpp
void solve(){
    int n;
    cin >> n;

    int x;
    int ans = 0;
    for(int i=0; i<n; i++){
        cin >> x;
        if(x==1) ans++;
    }

    cout << ans << endl;
}
```
