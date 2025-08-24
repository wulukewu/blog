---
title: Codeforces 暑期特訓：我想成為演算法大師 - 團練 Independence Day Programming Contest 2023 - Week 7
date: 2025-08-24 16:27:30
tags:
  - algorithm
  - codeforces
  - competitive-programming
---

# Mirror of Independence Day Programming Contest 2023 by MIST Computer Club

- [Contest Problems](https://codeforces.com/gym/104308)

## A. Rain Rain Go Away, Come Again Another Day!

> **Problem:** [A. Rain Rain Go Away, Come Again Another Day!](https://codeforces.com/gym/104308/problem/A)
>
> **Solution:** [GitHub Code](https://github.com/wulukewu/cp-code/blob/main/codeforces/gym/104308/a.cpp)

```cpp
void solve(){
    int n;
    cin >> n;

    vector < int > arr(n);
    FOR(i, 0, n) cin >> arr[i];

    int h = arr[0];
    int ans = 0;
    FOR(i, 1, n){
        if(arr[i]>h){
            h = arr[i];
        }else{
            ans += h-arr[i];
        }
    }

    int h2 = arr[n-1];
    for(int i=n-1; i>=0; i--){
        if(arr[i]==h) break;
        ans -= h-arr[i];
        if(arr[i]>h2){
            h2 = arr[i];
        }else{
            ans += h2-arr[i];
        }
    }

    cout << ans << endl;
}
```

## B. Signature Nightmare

> **Problem:** [B. Signature Nightmare](https://codeforces.com/gym/104308/problem/B)
>
> **Solution:** [GitHub Code](https://github.com/wulukewu/cp-code/blob/main/codeforces/gym/104308/b.cpp)

```cpp
void solve(){
    int n, k;
    cin >> n >> k;
    vector<int> a(n), b(n);
    int sumA = 0, sumB = 0;
    FOR(i, 0, n){
        cin >> a[i];
        sumA += a[i];
    }
    FOR(i, 0, n){
        cin >> b[i];
        sumB += b[i];
    }

    auto can = [&](int x){
        int need = 0;
        FOR(i, 0, n){
            int req = x * a[i];
            if(req > b[i]){
                need += req - b[i];
            }
            if(need > k){
                return false;
            }
        }
        return need <= k;
    };

    int L = 0, R = (sumB + k) / sumA, ans = 0;
    while(L <= R){
        int mid = (L+R) / 2;
        if(can(mid)){
            ans = mid;
            L = mid + 1;
        }else{
            R = mid - 1;
        }
    }

    cout << ans << endl;
}
```

## C. Optimal Pairing

> **Problem:** [C. Optimal Pairing](https://codeforces.com/gym/104308/problem/C)
>
> **Solution:** [GitHub Code](https://github.com/wulukewu/cp-code/blob/main/codeforces/gym/104308/c.cpp)

```cpp
void solve(){
    int n;
    cin >> n;

    vector < int > arr(n);
    for(int i=0; i<n; i++) cin >> arr[i];

    sort(arr.begin(), arr.end());

    int ans = 0;
    for(int i=n-1; i>=0; i-=2){
        ans += arr[i];
    }

    cout << ans << endl;
}
```

## D. Unwanted Divisors

> **Problem:** [D. Unwanted Divisors](https://codeforces.com/gym/104308/problem/D)
>
> **Solution:** [GitHub Code](https://github.com/wulukewu/cp-code/blob/main/codeforces/gym/104308/d.cpp)

```cpp
void solve(){
    int n,q;
    cin >> n >> q;
    vector<int> as(n),bs(q);
    FOR(i,0,n) cin >> as[i];
    sort(ALL(as));
    FOR(i,0,q) cin >> bs[i];

    for(auto num : bs){
        int ans = 0;
        for(int i=1;i*i<=num;i++){
            if(num % i == 0){
                auto it1 = binary_search(ALL(as), i);
                ans += (1-it1);
                if(i*i == num) break;
                auto it2 = binary_search(ALL(as), num/i);
                ans += (1-it2);
            }
        }
        cout << ans << endl;
    }
}
```

## G. Keyboard Warrior Roshid

> **Problem:** [G. Keyboard Warrior Roshid](https://codeforces.com/gym/104308/problem/G)
>
> **Solution:** [GitHub Code](https://github.com/wulukewu/cp-code/blob/main/codeforces/gym/104308/g.py)

```python
t = int(input())

change = ["z","x","c","v","b","n","m"]
while(t):
    t -= 1
    s = str(input())
    for i in change:
        if(i in s):
            s = s.replace(i,"")
    print(s)
```

## H. Wonder Island

> **Problem:** [H. Wonder Island](https://codeforces.com/gym/104308/problem/H)
>
> **Solution:** [GitHub Code](https://github.com/wulukewu/cp-code/blob/main/codeforces/gym/104308/h.cpp)

```cpp
void solve(){
    int n, k;
    cin >> n >> k;
    vector<int> v(k);

    v[0] = n;
    v[1] = n;
    v[2] = n;

    FOR(i, 3, k){
        v[i] = (v[i-1] + v[i-3]) % MOD;
    }

    cout << (v[k-1] * 2) % MOD << endl;
}
```

## I. Colorful Queries

> **Problem:** [I. Colorful Queries](https://codeforces.com/gym/104308/problem/I)
>
> **Solution:** [GitHub Code](https://github.com/wulukewu/cp-code/blob/main/codeforces/gym/104308/i2.cpp)

```cpp
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

    T rangeSum(int l, int r) { // [l, r)
        return sum(r) - sum(l);
    }


    int select(const T &k) { // 前綴區間和 >= k 的位置
        int x = 0;
        T cur{};
        for (int i = 1 << __lg(n); i; i /= 2) {
            if (x + i <= n and cur + a[x + i - 1] <= k) {
                x += i;
                cur = cur + a[x - 1];
            }
        }
        return x;
    }
};

void solve(){
    int n, q;
    cin >> n >> q;

    vector < int > c(n);
    map < int, int > mp;
    int t = q + 5;
    BIT < int > bt(q+n+10);

    FOR(i, 0, n){
        cin >> c[i];
        if(!mp.count(c[i])) mp[c[i]] = i+t+1;
        bt.add(i+t+1, 1);
    }

    int x;
    while(q--){
        cin >> x;
        cout << bt.sum(mp[x])+1 << endl;
        t--;
        bt.add(mp[x], -1);
        mp[x] = t;
        bt.add(t, 1);
    }
}
```

## K. An Incantation Long Remembered

> **Problem:** [K. An Incantation Long Remembered](https://codeforces.com/gym/104308/problem/K)
>
> **Solution:** [GitHub Code](https://github.com/wulukewu/cp-code/blob/main/codeforces/gym/104308/k.py)

```python
t = int(input())
while(t):
    t -= 1
    n = int(input())
    sma = [" " for i in range(n)]
    for i in range(n):
        sma[i] = str(input())
    big = str(input())

    ok = True
    ans = 1
    while(len(big) > 0 and ok):
        ok = False
        for i in sma:
            if(big.count(i) != 0):
                ok = True
                ans += big.count(i)
                big = big.replace(i,"")
    if(not ok):
        print("No")
    else:
        print("Yes")
        print(ans)
```
