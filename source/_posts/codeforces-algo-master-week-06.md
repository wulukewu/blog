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
> **Solution:** [GitHub Code]()

## J. Mafija

> **Problem:** [J. Mafija](https://codeforces.com/group/jtU6D2hVEi/contest/533255/problem/J)
>
> **Solution:** [GitHub Code]()

# Contest 20. DSU

- [Contest Problems](https://codeforces.com/group/jtU6D2hVEi/contest/533282)

## A. Islands

> **Problem:** [A. Islands](https://codeforces.com/group/jtU6D2hVEi/contest/533282/problem/A)
>
> **Solution:** [GitHub Code]()

## F. Vessels

> **Problem:** [F. Vessels](https://codeforces.com/group/jtU6D2hVEi/contest/533282/problem/F)
>
> **Solution:** [GitHub Code]()

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
