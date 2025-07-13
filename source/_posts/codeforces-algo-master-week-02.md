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

用 binary_search 找最少能砍完的天數

記得開`long long`，其中 `y += a * (mid - mid / k);` 及 `y += b * (mid - mid / m);` 可能會卡在 WA，因為 $10^9 \times 10^{18} = 10^{27}$ 會溢位。

```
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

- [Contenst Problems](https://codeforces.com/group/jtU6D2hVEi/contest/533123)

## C. Transformations

- [Problem](https://codeforces.com/group/jtU6D2hVEi/contest/533123/problem/C)
- [GitHub Solution]()

BFS 從 `b` 每次做 `-1` 或 `/2` 的動作推到 `a`
其中 queue `q` 是存 BFS 路徑的節點，再用 map `prev` 、 `op` 存經過每個數字的下個點及做得動作，最後 `now` 從 `a` 跑到 `b`，再將答案的等式做出來。

```
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

- [Problem](https://codeforces.com/group/jtU6D2hVEi/contest/533123/problem/E)
- [GitHub Solution]()

## L. Peaceful Queens

- [Problem](https://codeforces.com/group/jtU6D2hVEi/contest/533123/problem/L)
- [GitHub Solution]()
