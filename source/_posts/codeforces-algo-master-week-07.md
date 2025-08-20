---
title: Codeforces 暑期特訓：我想成為演算法大師 - Week 7
date: 2025-08-19 12:36:20
tags:
  - algorithm
  - codeforces
  - competitive-programming
---

# NCUMA Online Judge

## 河馬做區間操作!!!

> **Problem:** [河馬做區間操作!!!](https://ncuma-oj.math.ncu.edu.tw/problem/PG)
>
> **Solution:** [GitHub Code](https://github.com/wulukewu/cp-code/blob/main/ncuma-oj/PG_%E6%B2%B3%E9%A6%AC%E5%81%9A%E5%8D%80%E9%96%93%E6%93%8D%E4%BD%9C.cpp)

- `x` 為區間和
- `y` 為區間最大值，如果 `<=1` 的話就不用往下做

```cpp
#include <bits/stdc++.h>
using namespace std;

#define endl '\n'
#define int long long
#define FOR(i, a, b) for(int i = a; i < b; i++)

template<class Info>
struct SegmentTree {
    int n;
    std::vector<Info> info;
    SegmentTree() : n(0) {}
    SegmentTree(int n_, Info v_ = Info()) {
        init(n_, v_);
    }
    template<class T>
    SegmentTree(std::vector<T> init_) {
        init(init_);
    }
    void init(int n_, Info v_ = Info()) {
        init(std::vector<Info>(n_, v_));
    }
    template<class T>
    void init(std::vector<T> init_) {
        n = init_.size();
        info.assign(4 * n + 5, Info());
        std::function<void(int, int, int)> build = [&](int p, int l, int r) {
            if (r - l == 1) {
                info[p] = {init_[l], init_[l]};
                return;
            }
            int m = (l + r) / 2;
            build(2 * p, l, m);
            build(2 * p + 1, m, r);
            pull(p);
        };
        build(1, 0, n);
    }
    void pull(int p) {
        info[p] = info[2 * p] + info[2 * p + 1];
    }

    Info rangeQuery(int p, int l, int r, int x, int y) {
        if (l >= y || r <= x) {
            return Info();
        }
        if (l >= x && r <= y) {
            return info[p];
        }
        int m = (l + r) / 2;
        return rangeQuery(2 * p, l, m, x, y) + rangeQuery(2 * p + 1, m, r, x, y);
    }
    Info rangeQuery(int l, int r) {
        return rangeQuery(1, 0, n, l, r);
    }

    void rangeSqrt(int p, int l, int r, int x, int y){
        if(l>=y or r<=x) return;

        if(info[p].y<=1) return;

        if(r-l==1){
            info[p].x = sqrt(info[p].x);
            info[p].y = info[p].x;
            return;
        }

        int m = (l+r)/2;
        rangeSqrt(2*p, l, m, x, y);
        rangeSqrt(2*p+1, m, r, x, y);
        pull(p);
    }
    void rangeSqrt(int l, int r){
        rangeSqrt(1, 0, n, l, r);
    }
};

struct Info {
    int x = 0; // sum
    int y = 0; // max value
};

Info operator+(const Info &a, const Info &b) {
    return {a.x + b.x, max(a.y, b.y)};
}

void solve() {
    int n, q;
    cin >> n >> q;

    vector < int > arr(n);
    FOR(i, 0, n) cin >> arr[i];

    SegmentTree < Info > st(arr);

    int x, l, r;
    FOR(i, 0, q){
        cin >> x >> l >> r;
        if(x==0){
            cout << st.rangeQuery(l-1, r).x << endl;
        }else{
            st.rangeSqrt(l-1, r);
        }
    }

}

signed main() {
    ios::sync_with_stdio(false),cin.tie(0);
    int t = 1;
    //cin >> t;
    while (t--) solve();
    return 0;
}
```

## 逆序數量

> **Problem:** [逆序數量](https://ncuma-oj.math.ncu.edu.tw/problem/J003)
>
> **Solution:** [GitHub Code](https://github.com/wulukewu/cp-code/blob/main/ncuma-oj/J003_%E9%80%86%E5%BA%8F%E6%95%B8%E9%87%8F.cpp)

- 對每個數字，需要知道在它前面已經有多少個數字比它大
- 用 BIT (Binary Indexed Tree) 將 `O(n^2)` 降為 `O(n log n)`

```cpp
#include <bits/stdc++.h>
using namespace std;

#define endl '\n'
#define int long long
#define FOR(i, a, b) for(int i = a; i < b; i++)

template <class T>
struct BIT {
    int n;
    vector<T> a;
    BIT(int n_ = 0) {
        init(n_);
    }

    void init(int n_) {
        n = n_;
        a.assign(n, T{});
    }

    void add(int x, const T &v) {
        for (int i = x + 1; i <= n; i += i & -i) {
            a[i - 1] += v;
        }
    }

    T sum(int x) {
        T ans{};
        for (int i = x; i > 0; i -= i & -i) {
            ans += a[i - 1];
        }
        return ans;
    }
};

void solve() {
    int n;
    cin >> n;

    int INF = 1e6+5;
    BIT < int > bit(INF);

    int ans = 0;
    int x;
    FOR(i, 0, n){
        cin >> x;
        ans += bit.sum(INF) - bit.sum(x);
        bit.add(x, 1);
    }

    cout << ans << endl;
}

signed main() {
    ios::sync_with_stdio(false),cin.tie(0);
    int t = 1;
    //cin >> t;
    while (t--) solve();
    return 0;
}
```
