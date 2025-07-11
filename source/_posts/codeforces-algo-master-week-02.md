---
title: Codeforces 暑期特訓：我想成為演算法大師 - Week 2
date: 2025-07-08 14:51:32
tags:
  - algorithm
  - codeforces
  - competitive-programming
---

> From [LI2 Contests](https://codeforces.com/group/jtU6D2hVEi) Group

# Contest 11. Binary Search

- [Contenst Problems](https://codeforces.com/group/jtU6D2hVEi/contest/533121)

## C. Street with monuments

- [Problem](https://codeforces.com/group/jtU6D2hVEi/contest/533121/problem/C)
- [GitHub Solution](https://github.com/wulukewu/cp-code/blob/main/codeforces/group/jtU6D2hVEi/533121/C_Street_with_monuments.cpp)

用 `upper_bound()` 找第一個大於目標 `target=d[i]+r` 的元素

```
void solve() {
    int n, r;
    cin >> n >> r;

    vector<int>d(n);
    FOR(i, 0, n) cin >> d[i];

    int ans = 0;
    FOR(i, 0, n){
        auto it_upper = upper_bound(ALL(d), d[i]+r);
        ans += d.end()-it_upper;
    }

    cout << ans << endl;
}
```

\* 記得開 `long long`

## E. Diplomas

- [Problem](https://codeforces.com/group/jtU6D2hVEi/contest/533121/problem/E)
- [GitHub Solution](https://github.com/wulukewu/cp-code/blob/main/codeforces/group/jtU6D2hVEi/533121/E_Diplomas.cpp)

如果答案邊長是 `ans` ，那麼邊長為 `ans+1` 也會成立，所以目標是用 binary_search 找到最小的邊長

```
void solve() {
    int w, h, n;
    cin >> w >> h >> n;

    int l = 1;
    int r = max(w, h) * n;
    int ans = r;

    while(l<=r){
        int mid = l + (r-l)/2;
        int cnt = ((int)mid/w) * ((int)mid/h);
        if(cnt >= n){
            ans = mid;
            r = mid - 1;
        }else{
            l = mid + 1;
        }
    }

    cout << ans << endl;
}
```

## J. Forest Clearing

- [Problem](https://codeforces.com/group/jtU6D2hVEi/contest/533121/problem/F)
- [GitHub Solution]()

# Contest 13. Recursion

- [Contenst Problems](https://codeforces.com/group/jtU6D2hVEi/contest/533123)

## C. Transformations

- [Problem](https://codeforces.com/group/jtU6D2hVEi/contest/533123/problem/C)
- [GitHub Solution]()

## E. Weighing

- [Problem](https://codeforces.com/group/jtU6D2hVEi/contest/533123/problem/E)
- [GitHub Solution]()

## L. Peaceful Queens

- [Problem](https://codeforces.com/group/jtU6D2hVEi/contest/533123/problem/L)
- [GitHub Solution]()
