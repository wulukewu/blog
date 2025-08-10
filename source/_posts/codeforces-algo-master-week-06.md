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
- `TC3` 是錯的

```cpp
void solve() {
    int n;
    cin >> n;

    vector < int > G(n+1);
    FOR(i, 1, n+1){
        cin >> G[i];
    }

    vector < int > state(n+1, 0);
    vector < bool > mobster(n+1, false);
    int ans = 0;

    auto dfs = [&](auto&& self, int u) -> void {
        state[u] = 1;
        int v = G[u];
        if(state[v]==0){
            self(self, v);
        }
        else if(state[v]==1){
            ans++;
            while(true){
                state[v] = 2;
                mobster[v] = false;
                if(v==u) break;
                v = G[v];
            }
            return;
        }

        if(state[u]==2) return;

        if(mobster[v]){
            mobster[u] = false;
        }else{
            mobster[u] = true;
            ans++;
        }

        state[u] = 2;
    };

    FOR(i, 1, n+1){
        if(state[i]==0) dfs(dfs, i);
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
> **Solution:** [GitHub Code]()

## D. Roadblock

> **Problem:** [D. Roadblock](https://codeforces.com/group/jtU6D2hVEi/contest/533284/problem/D)
>
> **Solution:** [GitHub Code]()
