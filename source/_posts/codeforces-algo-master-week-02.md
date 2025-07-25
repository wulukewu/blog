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

- [Contest Problems](https://codeforces.com/group/jtU6D2hVEi/contest/533121)

## C. Street with monuments

> **Problem:** [C. Street with monuments](https://codeforces.com/group/jtU6D2hVEi/contest/533121/problem/C)
>
> **Solution:** [GitHub Code](https://github.com/wulukewu/cp-code/blob/main/codeforces/group/jtU6D2hVEi/533121/C_Street_with_monuments.cpp)

用 `upper_bound()` 找第一個大於目標 `target=d[i]+r` 的元素

```cpp
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

> **Problem:** [E. Diplomas](https://codeforces.com/group/jtU6D2hVEi/contest/533121/problem/E)
>
> **Solution:** [GitHub Code](https://github.com/wulukewu/cp-code/blob/main/codeforces/group/jtU6D2hVEi/533121/E_Diplomas.cpp)

如果答案邊長是 `ans` ，那麼邊長為 `ans+1` 也會成立，所以目標是用 binary_search 找到最小的邊長

```cpp
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

> **Problem:** [J. Forest Clearing](https://codeforces.com/group/jtU6D2hVEi/contest/533121/problem/J)
>
> **Solution:** [GitHub Code](https://github.com/wulukewu/cp-code/blob/main/codeforces/group/jtU6D2hVEi/533121/J_Forest_Clearing.cpp)

用 binary_search 找最少能砍完的天數

記得開 `long long` ，其中 `y += a * (mid - mid / k);` 及 `y += b * (mid - mid / m);` 可能會卡在 WA，因為 $10^9 \times 10^{18} = 10^{27}$ 會溢位。

```cpp
void solve() {
    int a, k, b, m, x;
    cin >> a >> k >> b >> m >> x;

    int l = 1;
    int r = 2 * x;

    int ans = r;
    while(l<=r){
        int mid = l + (r-l) / 2;

        int y = 0;
        if((mid - mid / k) > x / a){
            y = x + 1;
        }else{
            y += a * (mid - mid / k);
        }
        if((mid - mid / m) > x / b){
            y = x + 1;
        }else{
            y += b * (mid - mid / m);
        }

        if(y>=x){
            r = mid - 1;
            ans = mid;
        }else{
            l = mid + 1;
        }
    }

    cout << ans << endl;
}
```

# Contest 13. Recursion

- [Contest Problems](https://codeforces.com/group/jtU6D2hVEi/contest/533123)

## C. Transformations

> **Problem:** [C. Transformations](https://codeforces.com/group/jtU6D2hVEi/contest/533123/problem/C)
>
> **Solution:** [GitHub Code](https://github.com/wulukewu/cp-code/blob/main/codeforces/group/jtU6D2hVEi/533123/C_Transformations.cpp)

BFS 從 `b` 每次做 `-1` 或 `/2` 的動作推到 `a`
其中 queue `q` 是存 BFS 路徑的節點，再用 map `prev` 、 `op` 存經過每個數字的下個點及做的動作，最後 `now` 從 `a` 跑到 `b`，再將答案的等式做出來。

```cpp
void solve() {
    int a, b;
    cin >> a >> b;

    queue < int > q;
    map < int, int > prev;
    map < int, char > op;

    q.push(b);
    prev[b] = 0;
    op[b] = ' ';

    while(!q.empty()){
        int c = q.front();
        q.pop();

        if(c==a) break;

        int next = c - 1;
        if(next>=a and prev.find(next)==prev.end()){
            q.push(next);
            prev[next] = c;
            op[next] = '+';
        }

        if(c%2==0){
            next = c / 2;
            if(next>=a and prev.find(next)==prev.end()){
                q.push(next);
                prev[next] = c;
                op[next] = '*';
            }
        }
    }

    string ans = to_string(a);
    int now = a;
    while(now!=b){
        if(op[now]=='+'){
            ans = "(" + ans + " + 1)";
        }else{
            ans = ans + " * 2";
        }
        now = prev[now];
    }

    cout << b << " = " << ans << endl;
}
```

## E. Weighing

> **Problem:** [E. Weighing](https://codeforces.com/group/jtU6D2hVEi/contest/533123/problem/E)
>
> **Solution:** [GitHub Code](https://github.com/wulukewu/cp-code/blob/main/codeforces/group/jtU6D2hVEi/533123/E_Weighing.cpp)

每個砝碼 (`w[i]`) 有三種選擇：

1. 放左邊 `-w[i]`
2. 放右邊 `+w[i]`
3. 不放 `+0`

- `m` 為砝碼總重，如果將所有砝碼都放在同一邊，陣列左右都設成`m`，重量不會超過
- 初始化二維 `dp` ，第一行 `idx=0` 什麼都不放，所以在平衝狀態 `dp[0][m]` 為 `true`
- 對於每行 `dp[i][j]` 為 `true` 的狀態，分別考慮當前砝碼做三個動作後的平衡狀態（寫入 `dp[i+1]` 列）
- 迴圈跑完後，最後看若將重量為 `k` 的砝碼放在右邊，是不是還存在這樣的狀況

```cpp
void solve() {
    int k, n;
    cin >> k >> n;

    int m = 0;
    vector < int > w(n);
    FOR(i, 0, n){
        cin >> w[i];
        m += w[i];
    }

    vector < vector < bool > > dp(n+1, vector < bool > (2*m+1, false));
    dp[0][m] = true;

    FOR(i, 0, n){
        int current = w[i];
        FOR(j, 0, 2*m+1){
            if(dp[i][j]){
                dp[i+1][j] = true;

                int idx_left = j - current;
                if(idx_left >= 0){
                    dp[i+1][idx_left] = true;
                }

                int idx_right = j + current;
                if(idx_right < 2*m+1){
                    dp[i+1][idx_right] = true;
                }
            }
        }
    }

    if(k+m>=0 and k+m<2*m+1 and dp[n][k+m]){
        cout << "YES" << endl;
    }else{
        cout << "NO" << endl;
    }
}
```

## J. Coins

> **Problem:** [J. Coins](https://codeforces.com/group/jtU6D2hVEi/contest/533123/problem/J)
>
> **Solution:** [GitHub Code](https://github.com/wulukewu/cp-code/blob/main/codeforces/group/jtU6D2hVEi/533123/J_Coins.cpp)

用 dfs 遍歷每個可能

```cpp
void solve() {
    int n, m;
    cin >> n >> m;

    int a;
    set < int > aset;
    FOR(i, 0, m){
        cin >> a;
        if(aset.find(a)==aset.end()){
            aset.insert(a);
        }
    }

    vector < int > arr;
    for(int i: aset){
        arr.PB(i);
    }

    int total = 0;
    FOR(i, 0, m) total += arr[i];
    total *= 2;

    if(n>total){
        cout << -1 << endl;
        return;
    }

    vector < int > v(m, 0);
    int ans_cnt = 2*m+1;
    vector < int > ans(m, 0);
    auto dfs = [&](int rem, int idx, int coin_cnt, auto&& self) -> void {
        if(idx>=m){
            if(rem==0 and coin_cnt < ans_cnt){
                ans = v;
            }
            return;
        }else if(rem<0){
            return;
        }

        if(v[idx]<2){
            v[idx] += 1;
            self(rem-arr[idx], idx, coin_cnt+1, self);
            v[idx] -= 1;
        }
        self(rem, idx+1, coin_cnt, self);
    };

    dfs(n, 0, 0, dfs);

    int k = 0;
    FOR(i, 0, m){
        k += ans[i];
    }
    cout << k << endl;

    if(k>0){
        vector < int > ans_sort;
        FOR(i, 0, m){
            FOR(j, 0, ans[i]){
                ans_sort.PB(arr[i]);
            }
        }
    
        sort(ALL(ans_sort));
    
        for(int i: ans_sort){
            cout << i << ' ';
        }
        cout << endl;
    }
}
```

## L. Peaceful Queens

> **Problem:** [L. Peaceful Queens](https://codeforces.com/group/jtU6D2hVEi/contest/533123/problem/L)
>
> **Solution:** [GitHub Code](https://github.com/wulukewu/cp-code/blob/main/codeforces/group/jtU6D2hVEi/533123/L_Peaceful_Queens.cpp)

- 用 dfs 做下去，每層往上確認能不能放進去，走到底之後再存到 `ans` 的陣列裡
- 用三個一維陣列 `visit` 、 `diag_pos` 、 `diag_neg` 來存直線跟對角能不能放的狀態

```cpp
void solve() {
    int n;
    cin >> n;

    vector < vector < int > > ans;

    vector < bool > visit(n, false);
    vector < bool > diag_pos(n, false);
    vector < bool > diag_neg(n, false);
    auto dfs = [&](vector < int > v, auto&& self) -> void {
        int m = v.size();
        if(m==n){
            ans.push_back(v);
            return;
        }

        FOR(i, 0, n){
            if(!visit[i] and !diag_pos[m-i+n-1] and !diag_neg[m+i]){
                v.push_back(i);
                visit[i] = true;
                diag_pos[m-i+n-1] = true;
                diag_neg[m+i] = true;
                self(v, self);
                v.pop_back();
                visit[i] = false;
                diag_pos[m-i+n-1] = false;
                diag_neg[m+i] = false;
            }
        }
    };

    dfs(vector < int >(), dfs);

    for(auto i: ans){
        cout << "(";
        FOR(j, 0, n){
            cout << i[j]+1;
            if(j<n-1){
                cout << ",";
            }
        }
        cout << ")" << endl;
    }
}
```
