---
title: Codeforces 暑期特訓：我想成為演算法大師 - 2024 TOPC - Week 11
date: 2025-09-20 08:01:36
tags:
  - algorithm
  - codeforces
  - competitive-programming
---

# 2024 ICPC Asia Taiwan Online Programming Contest

- [Contest Problems](https://codeforces.com/gym/105383)

## A. Animal Farm

> **Problem:** [A. Animal Farm](https://codeforces.com/gym/105383/problem/A)
>
> **Solution:** [GitHub Code](https://github.com/wulukewu/cp-code/blob/main/codeforces/gym/105383/A.cpp)

```cpp
signed main(){
    int n;
    cin >> n;

    vector < string > a(n);
    vector < int > b(n);
    for(int i=0; i<n; i++){
        cin >> a[i] >> b[i];
    }

    int m = 0;
    for(int i=0; i<n; i++){
        if(a[i]=="pig"){
            m = max(m, b[i]);
        }
    }

    int ans = m;
    for(int i=0; i<n; i++){
        if(a[i]!="pig" and b[i]<m){
            ans += b[i];
        }
    }
    cout << ans << endl;
}
```

## J. Just Round Down

> **Problem:** [J. Just Round Down](https://codeforces.com/gym/105383/problem/J)
>
> **Solution:** [GitHub Code](https://github.com/wulukewu/cp-code/blob/main/codeforces/gym/105383/J.py)

```python
x = float(input())
print(int(x))
```

## K. Kingdom's Development Plan

> **Problem:** [K. Kingdom's Development Plan](https://codeforces.com/gym/105383/problem/K)
>
> **Solution:** [GitHub Code](https://github.com/wulukewu/cp-code/blob/main/codeforces/gym/105383/K.cpp)

```cpp
void solve() {
    int n, m;
    cin >> n >> m;

    vector < vector < int > > G(n);
    vector < int > unlock(n, 0);
    int u, v;
    FOR(i, 0, m){
        cin >> u >> v;
        u--; v--;
        G[u].PB(v);
        unlock[v]++;
    }

    vector < bool > visit(n, false);
    vector < int > ans;
    prior < int > q;
    FOR(i, 0, n){
        if(unlock[i]==0){
            q.push(i);
            // visit[i] = true;
            // ans.PB(i);
        }
    }

    while(!q.empty()){
        int x = q.top();
        q.pop();
        // cout << x << endl;

        visit[x] = true;
        ans.PB(x);

        for(int y: G[x]){
            unlock[y]--;
            if(unlock[y]==0){
                q.push(y);
                // visit[y] = true;
                // ans.PB(y);
            }
        }
    }

    if(ans.size()==n){
        for(int i: ans){
            cout << i+1 << ' ';
        }
        cout << endl;
    }else{
        cout << "IMPOSSIBLE" << endl;
    }
}
```
