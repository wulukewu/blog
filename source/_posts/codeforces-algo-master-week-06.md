---
title: Codeforces 暑期特訓：我想成為演算法大師 - Week 6
date: 2025-08-05 12:23:32
tags:
  - algorithm
  - codeforces
  - competitive-programming
---

> From [LI2 Contests](https://codeforces.com/group/jtU6D2hVEi) Group

# Contest 18. Graphs, DFS, BFS

- [Contest Problems](https://codeforces.com/group/jtU6D2hVEi/contest/533255)

## C. Looking for cycle

> **Problem:** [C. Looking for cycle](https://codeforces.com/group/jtU6D2hVEi/contest/533255/problem/C)
>
> **Solution:** [GitHub Code](https://github.com/wulukewu/cp-code/blob/main/codeforces/group/jtU6D2hVEi/533255/C_Looking_for_cycle.cpp)

- `visit` 陣列中， `0` 表示沒進去過，`1` 表示是當前的迴圈，`2` 表示之前已經走過發現沒有 cycle
- 若 `visit` 僅為 `bool` ，會讓沒有 cycle 的路徑重複走很多遍
- `1 → 2 → 3 → 4 → 2` 這樣的 cycle 是 `2 3 4`
- 用 `parent` 陣列存 dfs 走的路徑

```cpp
void solve() {
    int n, m;
    cin >> n >> m;

    vector < vector < int > > G(n);
    int v, u;
    FOR(i, 0, m){
        cin >> v >> u;
        G[v-1].PB(u);
    }

    bool ans = false;
    int start = -1;
    int end = -1;
    vector < int > visit(n, 0);
    vector < int > parent(n, -1);

    auto dfs = [&](auto&& self, int x) -> bool {
        visit[x-1] = 1;

        for(int y: G[x-1]){
            if(visit[y-1]==1){
                start = y;
                end = x;
                return true;
            }
            if(visit[y-1]==0){
                parent[y-1] = x;
                if(self(self, y)){
                    return true;
                }
            }
        }

        visit[x-1] = 2;
        return false;
    };

    FOR(i, 1, n+1){
        if(visit[i-1]==0){
            if(dfs(dfs, i)){
                ans = true;
                break;
            }
        }
    }

    if(ans){
        cout << "YES" << endl;

        int x = end;
        vector < int > cycle;
        while(x!=start){
            cycle.PB(x);
            x = parent[x-1];
        }
        cycle.PB(x);
        reverse(ALL(cycle));
        print(cycle);
    }else{
        cout << "NO" << endl;
    }
}
```

## E. Bipartite graph

> **Problem:** [E. Bipartite graph](https://codeforces.com/group/jtU6D2hVEi/contest/533255/problem/E)
>
> **Solution:** [GitHub Code](https://github.com/wulukewu/cp-code/blob/main/codeforces/group/jtU6D2hVEi/533255/E_Bipartite_graph.cpp)

- 用 bfs 對每個點去跑，如果還沒走過就先設 1，接下來每層再反轉 1 或 2

```cpp
void solve() {
    int n, m;
    cin >> n >> m;

    int u, v;
    vector < vector < int > > G(n+1);
    FOR(i, 0, m){
        cin >> u >> v;
        G[u].PB(v);
        G[v].PB(u);
    }

    bool ans = true;
    vector < int > color(n+1, 0);

    FOR(i, 1, n+1){
        if(color[i]!=0) continue;
        queue < int > q;
        color[i] = 1;
        q.push(i);

        while(!q.empty()){
            int t = q.front();
            q.pop();

            for(int y: G[t]){
                if(color[y]==0){
                    color[y] = 3 - color[t];
                    q.push(y);
                }else if(color[y]==color[t]){
                    ans = false;
                    break;
                }
            }

            if(!ans) break;
        }

        if(!ans) break;
    }

    if(ans){
        cout << "YES" << endl;
        FOR(i, 1, n+1){
            cout << color[i] << ' ';
        }
    }else{
        cout << "NO" << endl;
    }
}
```

## J. Mafija

> **Problem:** [J. Mafija](https://codeforces.com/group/jtU6D2hVEi/contest/533255/problem/J)
>
> **Solution:** [GitHub Code](https://github.com/wulukewu/cp-code/blob/main/codeforces/group/jtU6D2hVEi/533255/J_Mafija.cpp)

- 如果 `A` → `B` 且 `A` 是暴徒，則 `B` 一定是平民
- 如果 `A` → `B` 且 `B` 是暴徒，則 `A` 一定是平民
- 相鄰的兩人不能同時是暴徒，所以最多只能有一半是暴徒（在一個環裡面也是）
- 對於每一個環，先去針對環外面的每條樹，用 DP 最大化暴徒數量，所以要先找到沒有被指到的那些點，也就是 `indeg` 為 `0`
  - **先處理樹**：對於每棵樹的根，在「根是平民」或「根是暴徒」兩種情況下，這棵樹能貢獻的最大暴徒數
  - **再處理環**：利用樹 DP 的結果，算出每個環上節點的貢獻價值

```cpp
void solve() {
    int n;
    cin >> n;

    vector < int > G(n+1);
    FOR(i, 1, n+1){
        cin >> G[i];
    }

    vector < vector < int > > rev(n+1);
    vector < int > indeg(n+1, 0);
    FOR(i, 1, n+1){
        indeg[G[i]]++;
        rev[G[i]].PB(i);
    }

    queue < int > q;
    FOR(i, 1, n+1){
        if(indeg[i]==0){
            q.push(i);
        }
    }
    vector < bool > cycle(n+1, true);
    vector < int > topo;
    while(!q.empty()){
        int u = q.front();
        q.pop();

        cycle[u] = false;
        topo.PB(u);
        int v = G[u];
        indeg[v]--;
        if(indeg[v]==0){
            q.push(v);
        }
    }

    vector < int > dp0(n+1, 0);
    vector < int > dp1(n+1, 0);
    for(int u: topo){
        int c0 = 0;
        int c1 = 1;
        for(int c: rev[u]){
            if(cycle[c]) continue;
            c0 += max(dp0[c], dp1[c]);
            c1 += dp0[c];
        }
        dp0[u] = c0;
        dp1[u] = c1;
    }

    int ans = 0;
    vector < bool > visit(n+1, false);
    FOR(i, 1, n+1){
        if(!cycle[i] or visit[i]) continue;
        vector < int > cyc;
        int u = i;
        while(!visit[u]){
            visit[u] = 1;
            cyc.PB(u);
            u = G[u];
        }
        int k = cyc.size();

        vector < int > val0(k, 0);
        vector < int > val1(k, 0);
        FOR(j, 0, k){
            int idx = cyc[j];
            int sum0 = 0;
            int sum1 = 1;
            for(int c: rev[idx]){
                if(cycle[c]) continue;
                sum0 += max(dp0[c], dp1[c]);
                sum1 += dp0[c];
            }
            val0[j] = sum0;
            val1[j] = sum1;
        }

        int base = 0;
        vector < int > gain(k);
        FOR(j, 0, k){
            base += val0[j];
            gain[j] = val1[j] - val0[j];
        }

        auto sl = [&](int l, int r) -> int {
            if(l>r) return 0;
            int pn = 0;
            int ps = -4e18;
            FOR(j, l, r+1){
                int cs = pn + gain[j];
                int cn = max(pn, ps);
                pn = cn;
                ps = cs;
            }
            return max(pn, ps);
        };

        int ans1 = sl(1, k-1);
        int ans2 = gain[0] + sl(2, k-2);
        int best = max(ans1, ans2);
        ans += base + best;
    }

    cout << ans << endl;
}
```

# Contest 20. DSU

- [Contest Problems](https://codeforces.com/group/jtU6D2hVEi/contest/533282)

## A. Islands

> **Problem:** [A. Islands](https://codeforces.com/group/jtU6D2hVEi/contest/533282/problem/A)
>
> **Solution:** [GitHub Code](https://github.com/wulukewu/cp-code/blob/main/codeforces/group/jtU6D2hVEi/533282/A_Islands.cpp)

- 題目要求的是前幾座橋蓋完之後所有點都能相通
- 用 `pieces` 來存共有幾個不相連的區塊，當 `pieces==1` 的時候就不用再繼續蓋了

```cpp
void solve() {
    int n, m;
    cin >> n >> m;

    vector < int > boss(n);
    FOR(i, 0, n) boss[i] = i;

    int pieces = n;

    auto find_root = [&](auto&& self, int x) -> int {
        if(boss[x]==x) return x;

        int root = self(self, boss[x]);
        boss[x] = root;
        return root;
    };

    auto connect = [&](int x, int y) -> void {
        int root_x = find_root(find_root, x);
        int root_y = find_root(find_root, y);
        if(root_x != root_y){
            boss[root_x] = boss[root_y];
            pieces--;
        }
    };

    int ans = m;
    int u, v;
    FOR(i, 0, m){
        cin >> u >> v;
        u--;
        v--;
        connect(u, v);

        if(pieces==1){
            ans = i+1;
            break;
        }
    }

    cout << ans << endl;
}
```

## F. Vessels

> **Problem:** [F. Vessels](https://codeforces.com/group/jtU6D2hVEi/contest/533282/problem/F)
>
> **Solution:** [GitHub Code](https://github.com/wulukewu/cp-code/blob/main/codeforces/group/jtU6D2hVEi/533282/F_Vessels.cpp)

- `arr` 存 `n` 個容器的容量，`brr` 存 `n` 個容器了多少的水
- 若第 `i` 個容器裝滿，那麼水會流進第 `i+1` 個容器裡，還是滿的話就再繼續往下找，所以這裡用 DSU 優化， `boss` 存還沒滿的那個容器 `idx`
- 如果流到了 `idx` 為 `n` 時，代表流到了地上，就跳出迴圈不用管

```cpp
void solve() {
    int n;
    cin >> n;

    vector < int > arr(n);
    FOR(i, 0, n) cin >> arr[i];
    vector < int > brr(n, 0);

    vector < int > boss(n+1);
    FOR(i, 0, n+1) boss[i] = i;

    auto find_root = [&](auto&& self, int x) -> int {
        if(boss[x]==x) return x;

        int root = self(self, boss[x]);
        boss[x] = root;
        return root;
    };

    int m;
    cin >> m;

    int q, p, x, k;
    FOR(i, 0, m){
        cin >> q;
        if(q==1){
            cin >> p >> x;
            p--;

            while(x>0){
                p = find_root(find_root, p);
                if(p>=n) break;

                int avil = arr[p]-brr[p];
                if(avil>=x){
                    brr[p] += x;
                    x = 0;
                }else{
                    x -= avil;
                    brr[p] = arr[p];
                    boss[p] = find_root(find_root, p+1);
                }
            }
        }else if(q==2){
            cin >> k;
            cout << brr[k-1] << endl;
        }
    }
}
```

# Contest 22. Shortest Paths

- [Contest Problems](https://codeforces.com/group/jtU6D2hVEi/contest/533284)

## C. Rain 2

> **Problem:** [C. Rain 2](https://codeforces.com/group/jtU6D2hVEi/contest/533284/problem/C)
>
> **Solution:** [GitHub Code](https://github.com/wulukewu/cp-code/blob/main/codeforces/group/jtU6D2hVEi/533284/C_Rain_2.cpp)

- BFS 從 `(0, 0)` 走到 `(n-1 ,m-1)` ，用 `priority_queue` 維護，距離越短的優先
- 剪枝用 `dist` 存最短加權距離
- 記得開 `long long` ，然後 `dist` 的值預設要開夠大（不然會 `WA`）

```cpp
void solve() {
    int n, m;
    cin >> n >> m;

    vector < vector < int > > arr(n, vector < int > (m));
    FOR(i, 0, n){
        FOR(j, 0, m){
            cin >> arr[i][j];
        }
    }

    int offs[4][2] = {{1, 0}, {0, 1}, {-1, 0}, {0, -1}};
    vector < vector < int > > dist(n, vector < int > (m, 1e18));
    priority_queue < pair < int, pair < int, int > >,
                 vector < pair < int, pair < int, int > > >,
                 greater < pair < int, pair < int, int > > > > pq;

    dist[0][0] = 0;
    pq.push({0, {0, 0}});

    int ans = -1;
    while(!pq.empty()){
        auto t = pq.top();
        pq.pop();

        int cost = t.F;
        int r = t.S.F;
        int c = t.S.S;

        if(r==n-1 and c==m-1){
            ans = cost;
            break;
        }

        if(cost > dist[r][c]) continue;

        FOR(i, 0, 4){
            int x = r + offs[i][0];
            int y = c + offs[i][1];
            if(!(x>=0 and x<n and y>=0 and y<m)) continue;
            int cost_new = cost+arr[x][y];
            if(cost_new < dist[x][y]){
                dist[x][y] = cost_new;
                pq.push({cost_new, {x, y}});
            }
        }
    }

    cout << ans << endl;

}
```

## D. Roadblock

> **Problem:** [D. Roadblock](https://codeforces.com/group/jtU6D2hVEi/contest/533284/problem/D)
>
> **Solution:** [GitHub Code](https://github.com/wulukewu/cp-code/blob/main/codeforces/group/jtU6D2hVEi/533284/D_Roadblock.cpp)

- 先用 Dijkstra 找到 `1` 到 `n` 的最短路徑
- 最短路徑用 `parent` 存，後面再從 `n` 迴溯到 `1` 存到 `path` 陣列裡
- 針對這條路徑的每段，都試著將其長度變成 `2` 倍，然後都再做一次 Dijkstra，找到最大的增加長度，最後記得把 `2` 倍的長度設定回來

```cpp
void solve() {
    int n, m;
    cin >> n >> m;

    const int INF = 1e18;

    vector < vector < int > > G(n+1);
    vector < vector < int > > dist(n+1, vector < int > (n+1, INF));
    FOR(i, 0, n+1) dist[i][i] = 0;
    int u, v, l;
    FOR(i, 0, m){
        cin >> u >> v >> l;
        G[u].PB(v);
        G[v].PB(u);
        dist[u][v] = min(dist[u][v], l);
        dist[v][u] = min(dist[v][u], l);
    }

    vector < int > d1(n+1, INF);
    vector < int > parent(n+1, -1);
    priority_queue < pair<int,int>, vector <pair<int,int>>, greater <pair<int,int>> > pq;
    pq.push({0, 1});
    d1[1] = 0;

    while(!pq.empty()){
        auto t = pq.top();
        pq.pop();

        int l = t.F;
        int u = t.S;

        if(l!=d1[u]) continue;

        for(int v: G[u]){
            if(d1[v]>l+dist[u][v]){
                d1[v] = l+dist[u][v];
                parent[v] = u;
                pq.push({d1[v], v});
            }
        }
    }

    int ori = d1[n];
    vector < pair < int, int > > path;
    int cur = n;
    while(cur!=1  and parent[cur]!=-1){
        path.EB(parent[cur], cur);
        cur = parent[cur];
    }

    int ans = 0;
    for(auto e: path){
        int a = e.F;
        int b = e.S;
        int d = dist[a][b];

        dist[a][b] = d * 2;
        dist[b][a] = d * 2;

        vector < int > d2(n+1, INF);
        priority_queue < pair<int,int>, vector <pair<int,int>>, greater <pair<int,int>> > pq2;
        pq2.push({0, 1});
        d2[1] = 0;
        while(!pq2.empty()){
            auto t = pq2.top();
            pq2.pop();

            int l = t.F;
            int u = t.S;

            if(l!=d2[u]) continue;

            for(int v: G[u]){
                if(d2[v]>l+dist[u][v]){
                    d2[v] = l+dist[u][v];
                    pq2.push({d2[v], v});
                }
            }
        }

        ans = max(ans, d2[n]-ori);

        dist[a][b] = d;
        dist[b][a] = d;
    }

    cout << ans << endl;
}
```
