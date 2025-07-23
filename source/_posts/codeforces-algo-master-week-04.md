---
title: Codeforces 暑期特訓：我想成為演算法大師 - Week 4
date: 2025-07-22 12:41:11
tags:
  - algorithm
  - codeforces
  - competitive-programming
---

> From [LI2 Contests](https://codeforces.com/group/jtU6D2hVEi) Group

# Contest 19. Dynamic Programming

- [Contest Problems](https://codeforces.com/group/jtU6D2hVEi/contest/533277)

## C. Grasshoper-K

> **Problem:** [C. Grasshoper-K](https://codeforces.com/group/jtU6D2hVEi/contest/533277/problem/C)
>
> **Solution:** [GitHub Code]()

每格的種數由前面 1~k 格加總

```cpp
void solve() {
    int n, k;
    cin >> n >> k;

    vector < int > dp(n, 0);
    dp[0] = 1;
    FOR(i, 1, n){
        FOR(j, 1, k+1){
            if(i-j>=0){
                dp[i] += dp[i-j];
            }
        }
    }

    cout << dp[n-1] << endl;
}
```

## E. Grasshopper and Money

> **Problem:** [E. Grasshopper and Money](https://codeforces.com/group/jtU6D2hVEi/contest/533277/problem/E)
>
> **Solution:** [GitHub Code]()

- 解題過程是先用 dp 把最大值求出來，再讓 `dp` 同時存著最大路徑時走過的點
- 但是這個解法可能會 MLE，總共花了 86 MB（題目限制是 256 MB）

```cpp
void solve() {
    int n, k;
    cin >> n >> k;

    vector < int > coins(n-2);
    FOR(i, 0, n-2) cin >> coins[i];

    vector < pair < int, vector < int > > > dp(n);
    dp[0] = make_pair(0, vector < int >());
    FOR(i, 1, n){
        int max_coins = -INT_MAX;
        int max_coins_idx = -1;
        FOR(j, 1, k+1){
            if(i-j<0) break;
            if(dp[i-j].F>max_coins){
                max_coins = dp[i-j].F;
                max_coins_idx = i-j;
            }
        }

        if(i<n-1){
            dp[i].F = max_coins + coins[i-1];
        }else{
            dp[i].F = max_coins;
        }

        vector < int > v = dp[max_coins_idx].S;
        v.PB(max_coins_idx+1);
        dp[i].S = v;
    }

    cout << dp[n-1].F << endl;
    cout << dp[n-1].S.size() << endl;
    for(int i: dp[n-1].S){
        cout << i << ' ';
    }
    cout << n << endl;
}
```

## J. Milk in Bottles - K

> **Problem:** [J. Milk in Bottles - K](https://codeforces.com/group/jtU6D2hVEi/contest/533277/problem/J)
>
> **Solution:** [GitHub Code]()

## L. Array Filling

> **Problem:** [L. Array Filling](https://codeforces.com/group/jtU6D2hVEi/contest/533277/problem/L)
>
> **Solution:** [GitHub Code]()

# Contest XX. Greedy

- [Contest Problems](https://codeforces.com/group/jtU6D2hVEi/contest/533371)

## C. Gifts

> **Problem:** [C. Gifts](https://codeforces.com/group/jtU6D2hVEi/contest/533371/problem/C)
>
> **Solution:** [GitHub Code]()

## E. Badgers

> **Problem:** [E. Badgers](https://codeforces.com/group/jtU6D2hVEi/contest/533371/problem/E)
>
> **Solution:** [GitHub Code]()
