---
title: Codeforces 暑期特訓：我想成為演算法大師 - Week 5
date: 2025-07-29 12:06:13
tags:
  - algorithm
  - codeforces
  - competitive-programming
---

> From [LI2 Contests](https://codeforces.com/group/jtU6D2hVEi) Group

# Contest 19. Dynamic Programming

- [Contest Problems](https://codeforces.com/group/jtU6D2hVEi/contest/533280)

## C. Turtle and Money

> **Problem:** [C. Turtle and Money](https://codeforces.com/group/jtU6D2hVEi/contest/533280/problem/C)
>
> **Solution:** [GitHub Code](https://github.com/wulukewu/cp-code/blob/main/codeforces/group/jtU6D2hVEi/533280/C_Turtle_and_Money.cpp)

- `dp[i][j]` 表示從 `(0, 0)` 走到 `(i, j)` 的加總最大值
- `tar = dp[x][y] - arr[x][y]` 就可以知道上一個數值，可能為 `dp[x-1][y]` 或 `dp[x][y-1]`

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

    int offs[2][2] = {{-1, 0}, {0, -1}};

    vector < vector < int > > dp(n, vector < int > (m, 0));
    FOR(i, 0, n){
        FOR(j, 0, m){
            int val = -INT_MAX;
            FOR(k, 0, 2){
                int x = i + offs[k][0];
                int y = j + offs[k][1];
                if(x>=0 and x<n and y>=0 and y<m){
                    val = max(val, dp[x][y]);
                }
            }
            if(val == -INT_MAX){
                val = 0;
            }
            val += arr[i][j];
            dp[i][j] = val;
        }
    }

    cout << dp[n-1][m-1] << endl;

    int x = n-1;
    int y = m-1;
    stack < char > ans;
    while(x>0 or y>0){
        char dir;
        int tar = dp[x][y] - arr[x][y];
        if(x-1>=0 and dp[x-1][y]==tar){
            dir = 'D';
            x--;
        }else if(y-1>=0 and dp[x][y-1]==tar){
            dir = 'R';
            y--;
        }
        ans.push(dir);
    }

    while(!ans.empty()){
        cout << ans.top();
        ans.pop();
    }
}
```

## E. Knight

> **Problem:** [E. Knight](https://codeforces.com/group/jtU6D2hVEi/contest/533280/problem/E)
>
> **Solution:** [GitHub Code](https://github.com/wulukewu/cp-code/blob/main/codeforces/group/jtU6D2hVEi/533280/E_Knight.cpp)

- `dp[i][j]` 為走到 `(i, j)` 的種數

```cpp
void solve() {
    int n, m;
    cin >> n >> m;

    int offs[2][2] = {{-1, -2}, {-2, -1}};
    vector < vector < int > > dp(n, vector < int > (m, 0));
    dp[0][0] = 1;
    FOR(i, 0, n){
        FOR(j, 0, m){
            FOR(k, 0, 2){
                int x = i + offs[k][0];
                int y = j + offs[k][1];
                if(x>=0 and x<n and y>=0 and y<m){
                    dp[i][j] += dp[x][y];
                }
            }
        }
    }

    cout << dp[n-1][m-1] << endl;
}
```

## I. Levenshtein Distance

> **Problem:** [I. Levenshtein Distance](https://codeforces.com/group/jtU6D2hVEi/contest/533280/problem/I)
>
> **Solution:** [GitHub Code](https://github.com/wulukewu/cp-code/blob/main/codeforces/group/jtU6D2hVEi/533280/I_Levenshtein_Distance.cpp)

- 兩個字串由後往前找 `(i, j)` ：

  1. 如果 `s1[i]==s2[j]`: 則可以視 `(i+1, j+1)` 為相同的答案
  2. 如果不一樣，那麼就找以下三種作法的最小值+1：
     1. Replace: `(i+1, j+1)`
     2. Delete: `(i+1, j)`
     3. Insert: `(i, j+1)`

- 初始化最後一排及最後一列，此狀態問題為字串`s1`/`s2`與空字串的差，即為字串`s1`/`s2`的長度，因此遞減為`0`
- 可以參考[這支影片](https://youtu.be/XYi2-LPrwm4)

![Levenshtein Distance](../../../images/posts/codeforces-algo-master-week-05/levenshtein-distance.png)


```cpp
void solve() {
    string s1, s2;
    cin >> s1 >> s2;

    int l1 = s1.size();
    int l2 = s2.size();

    vector < vector < int > > dp(l1+1, vector < int > (l2+1, 0));
    FOR(i, 0, l1+1) dp[i][l2] = l1-i;
    FOR(j, 0, l2+1) dp[l1][j] = l2-j;

    for(int i=l1-1; i>=0; i--){
        for(int j=l2-1; j>=0; j--){
            if(s1[i]==s2[j]){
                dp[i][j] = dp[i+1][j+1];
            }else{
                int val = min(dp[i+1][j], dp[i][j+1]);
                val = min(val, dp[i+1][j+1]);
                dp[i][j] = val + 1;
            }
        }
    }

    cout << dp[0][0] << endl;
}
```

## N. Knapsack Problem

> **Problem:** [N. Knapsack Problem](https://codeforces.com/group/jtU6D2hVEi/contest/533280/problem/N)
>
> **Solution:** [GitHub Code]()

## P. Weights

> **Problem:** [P. Weights](https://codeforces.com/group/jtU6D2hVEi/contest/533280/problem/P)
>
> **Solution:** [GitHub Code]()
