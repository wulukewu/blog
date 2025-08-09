---
title: Codeforces 暑期特訓：我想成為演算法大師 - 團練 FJNU Programming Contest 2024 - Week 6
date: 2025-08-09 18:34:59
tags:
  - algorithm
  - codeforces
  - competitive-programming
---

# 2024 Fujian Normal University Programming Contest

- [Contest Problems](https://codeforces.com/gym/105168)
- [Notes](https://github.com/wulukewu/cp-code/blob/main/codeforces/gym/105168/notes.txt)

## A. Crazy Yesterday

> **Problem:** [A. Crazy Yesterday](https://codeforces.com/gym/105168/problem/A)
>
> **Solution:** [GitHub Code](https://github.com/wulukewu/cp-code/blob/main/codeforces/gym/105168/A.cpp)

```cpp
void solve(){
    int n;
    cin >> n;
    int in;
    while(n--){
        cin >> in;
        if(in == 1) in += 7;
        cout << in-1 << endl;
    }
}
```

## C. Chain Reaction

> **Problem:** [C. Chain Reaction](https://codeforces.com/gym/105168/problem/C)
>
> **Solution:** [GitHub Code](https://github.com/wulukewu/cp-code/blob/main/codeforces/gym/105168/C.cpp)

```cpp
void solve(){
    int n,m;
    cin >> n >> m;
    int a,b;
    FOR(i,0,m) cin >> a >> b;
    cout << n << " ";
    FOR(i,0,n) cout << i+1 << " ";
    cout << endl;
}
```

## D. XOR Pairing

> **Problem:** [D. XOR Pairing](https://codeforces.com/gym/105168/problem/D)
>
> **Solution:** [GitHub Code](https://github.com/wulukewu/cp-code/blob/main/codeforces/gym/105168/D2.cpp)

```cpp
void solve(){
    int n, k;
    cin >> n >> k;

    map < int, int > mp;
    FOR(i, 0, n){
        int a;
        cin >> a;
        mp[a]++;
    }

    int ans = 0;
    set<int> visited;
    for(auto &[x, cx] : mp){
        int tar = k ^ x;
        if(mp.find(tar) == mp.end()){
            continue;
        }

        if(tar == x){
            ans += cx * (cx -1 ) / 2;
        }else if(!visited.count(tar)){
            ans += cx * mp[tar];
        }
        visited.insert(x);
    }

    cout << ans << endl;

}
```

## F. Double Holding

> **Problem:** [F. Double Holding](https://codeforces.com/gym/105168/problem/F)
>
> **Solution:** [GitHub Code](https://github.com/wulukewu/cp-code/blob/main/codeforces/gym/105168/F.cpp)

```cpp
bool cpm(pair<int,int> a,pair<int,int> b){
    return a.F < b.F;
}

void solve(){
    int n,m,E;
    cin >> n >> m >> E;
    vector < pair<int,int> > up;
    vector < pair<int,int> > down;
    int start,end;
    FOR(i,0,n){
        cin >> start >> end;
        up.PB({start,end});
    }
    sort(ALL(up),cpm);
    FOR(i,0,m){
        cin >> start >> end;
        down.PB({start,end});
    }
    sort(ALL(down),cpm);

    // for(auto i:up) cout << i.F << " " << i.S << endl;
    // cout << endl;
    // for(auto i:down) cout << i.F << " " << i.S << endl;

    int i=0,j=0;
    // cout << i << " " << j << " " << E << endl;
    while(i<n and j<m and E >= 0){
        // cout << "123" << endl;
        if(up[i].S <= down[j].F){
            // cout << 1 << endl;
            i += 1;
        }
        else if(up[i].F >= down[j].S){
            // cout << 2 << endl;
            j += 1;
        }
        else if(up[i].S > down[j].F and up[i].S <= down[j].S and up[i].F <= down[j].F){
            // cout << E << " " << (down[j].F-up[i].S) << endl;
            E += (down[j].F-up[i].S);
            i += 1;
        }
        else if(up[i].S > down[j].F and up[i].S <= down[j].S and up[i].F >  down[j].F){
            // cout << E << " " << (up[i].F-up[i].S) << endl;
            E += (up[i].F-up[i].S);
            i += 1;
        }
        else if(up[i].S > down[j].F and up[i].S >  down[j].S and up[i].F <= down[j].F){
            // cout << E << " " << (down[j].F-down[j].S) << endl;
            E += (down[j].F-down[j].S);
            j += 1;
        }
        else if(up[i].S > down[j].F and up[i].S >  down[j].S and up[i].F <= down[j].S){
            // cout << E << " " << (up[i].F-down[j].S) << endl;
            E += (up[i].F-down[j].S);
            j += 1;
        }
        // else cout << up[i].F << " " << up[i].S << " " << down[j].F << " " << down[j].S << endl;
    }
    if(E >= 0) cout << E << endl;
    else cout << -1 << endl;
}
```

## G. Color Contagion

> **Problem:** [G. Color Contagion](https://codeforces.com/gym/105168/problem/G)
>
> **Solution:** [GitHub Code](https://github.com/wulukewu/cp-code/blob/main/codeforces/gym/105168/G2.cpp)

```cpp
const int MAXN = 2e5 + 5;

int fact[MAXN], invfact[MAXN];

void calculateFactorial(int n){
    fact[0] = 1;
    for(int i = 1; i <= n; i++){
        fact[i] = (fact[i-1] * i) % MOD;
    }
    invfact[n] = POW(fact[n], MOD-2);
    for(int i = n-1; i >= 0; i--){
        invfact[i] = (invfact[i+1] * (i+1)) % MOD;
    }
}

vector<int> G[MAXN];
int sz[MAXN];
int dp[MAXN];

void dfs(int u, int p){
    sz[u] = 1;
    dp[u] = 1;
    for(auto nxt: G[u]){
        if(nxt == p){
            continue;
        }

        dfs(nxt, u);
        sz[u] += sz[nxt];
        dp[u] = (dp[u] * dp[nxt]) % MOD;
        dp[u] = (dp[u] * invfact[sz[nxt]]) % MOD;
    }

    dp[u] = (dp[u] * fact[sz[u] - 1]) % MOD;
}


void solve(){
    int n;
    cin >> n;
    for(int i = 1; i <= n; i++){
        G[i].clear();
        sz[i] = 0;
        dp[i] = 0;
    }

    for(int i = 1; i <= n-1; i++){
        int u, v;
        cin >> u >> v;
        G[u].PB(v);
        G[v].PB(u);
    }

    dfs(1, -1);

    cout << dp[1] << endl;

}

signed main(){
    ios::sync_with_stdio(false),cin.tie(0);

    calculateFactorial(MAXN - 1);

    int t;
    cin >> t;
    while(t--) solve();
    return 0;
}
```

## J. Shifting Tournament

> **Problem:** [J. Shifting Tournament](https://codeforces.com/gym/105168/problem/J)
>
> **Solution:** [GitHub Code](https://github.com/wulukewu/cp-code/blob/main/codeforces/gym/105168/J.cpp)

```cpp
void solve(){
    int k;
    cin >> k;

    string s;
    cin >> s;

    vector < int > dp(k+1);
    dp[0] = 1;
    FOR(i, 1, k+1){
        dp[i] = dp[i-1] * 2;
        dp[i] %= 998244353;
    }
    // print(dp);

    int t = 0;
    FOR(i, 0, k){
        if(s[i]=='?') t++;
    }
    // cout << t << endl;

    int q;
    cin >> q;

    int p;
    char c;
    while(q--){
        cin >> p >> c;
        p--;
        if(s[p]=='?' and c!='?') t--;
        else if(s[p]!='?' and c=='?') t++;
        s[p] = c;

        cout << dp[t] << endl;
    }
}
```

## L. Terabyte Connection

> **Problem:** [L. Terabyte Connection](https://codeforces.com/gym/105168/problem/L)
>
> **Solution:** [GitHub Code](https://github.com/wulukewu/cp-code/blob/main/codeforces/gym/105168/L.cpp)

```cpp
void solve(){
    int n;
    cin >> n;

    int ans_p = 0;
    int ans_t = 0;

    int p, t;
    FOR(i, 0, n){
        cin >> p >> t;
        ans_p = max(ans_p, p);
        ans_t = max(ans_t, p+t);
    }

    cout << ans_p << ' ' << ans_t << endl;
}
```
