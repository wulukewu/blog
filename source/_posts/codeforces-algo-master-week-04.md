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
> **Solution:** [GitHub Code](https://github.com/wulukewu/cp-code/blob/main/codeforces/group/jtU6D2hVEi/533277/C_Grasshoper_K.cpp)

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
> **Solution:** [GitHub Code](https://github.com/wulukewu/cp-code/blob/main/codeforces/group/jtU6D2hVEi/533277/E_Grasshopper_and_Money.cpp)

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
> **Solution:** [GitHub Code](https://github.com/wulukewu/cp-code/blob/main/codeforces/group/jtU6D2hVEi/533277/J_Milk_in_Bottles_K.cpp)

- 開一個一維 `dp` 陣列儲存不同 `n` 對應到答案的子問題，在轉移狀態時對每個容量的瓶子去裝，找數量最小的出來
- `dp[i]` 表示 `i` 的牛奶最少可以有幾瓶，所以此題所求即為 `dp[n]`
- 另外用一個 `last_bottles` 陣列表示在當前狀態下，選擇哪個瓶子所對應到的子問題，得出的答案數量會最小

```cpp
void solve() {
    ifstream fcin("input.txt");
    ofstream fcout("output.txt");

    int n, k;
    fcin >> n >> k;

    vector < int > bottles(k);
    FOR(i, 0, k) fcin >> bottles[i];

    vector < int > dp(n+1, INT_MAX);
    dp[0] = 0;

    vector < int > last_bottles(n+1, 0);
    FOR(i, 1, n+1){
        for(int j: bottles){
            if(dp[i-j]+1<dp[i] and i-j>=0 and dp[i-j]!=INT_MAX){
                dp[i] = dp[i-j] + 1;
                last_bottles[i] = j;
            }
        }
    }

    if(dp[n]==INT_MAX){
        fcout << -1 << endl;
    }else{
        fcout << dp[n] << endl;

        vector < int > ans;
        int nn = n;
        while(nn>0){
            int l = last_bottles[nn];
            ans.PB(l);
            nn -= l;
        }

        sort(ALL(ans));
        for(int i: ans){
            fcout << i << ' ';
        }
    }
}
```

## L. Array Filling

> **Problem:** [L. Array Filling](https://codeforces.com/group/jtU6D2hVEi/contest/533277/problem/L)
>
> **Solution:** [GitHub Code](https://github.com/wulukewu/cp-code/blob/main/codeforces/group/jtU6D2hVEi/533277/L_Array_Filling.cpp)

每組 `(i, j)` 因倍數關係的時候都更新 `dp`

```cpp
void solve() {
    ifstream fcin("input.txt");
    ofstream fcout("output.txt");

    int n;
    fcin >> n;

    vector < int > dp(n+5, 1);
    for(int i=2; i<=n+1; i++){
        for(int j=2; j*j<=i; j++){
            if(i%j==0){
                dp[i] += dp[j];
                if(j*j!=i){
                    dp[i] += dp[i/j];
                }
            }
        }
    }

    fcout << dp[n+1] << endl;
}
```

# Contest XX. Greedy

- [Contest Problems](https://codeforces.com/group/jtU6D2hVEi/contest/533371)

## C. Gifts

> **Problem:** [C. Gifts](https://codeforces.com/group/jtU6D2hVEi/contest/533371/problem/C)
>
> **Solution:** [GitHub Code](https://github.com/wulukewu/cp-code/blob/main/codeforces/group/jtU6D2hVEi/533371/C_Gifts.cpp)

- `v` 用 pair 來存，分別存著每個物品的總消耗及票卷用下去時能少多少
- `sort(v)` 讓總消耗越小的越先取，能讓取的數量最大化

```cpp
void solve() {
    int n, b;
    cin >> n >> b;

    vector < pair < int, int > > v(n);
    FOR(i, 0, n){
        int p, s;
        cin >> p >> s;
        v[i].F = p+s;
        v[i].S = p/2;
    }

    sort(ALL(v));

    int cnt = 0;
    int total = 0;
    priority_queue < int > pq;

    FOR(i, 0, n){
        total += v[i].F;
        pq.push(v[i].S);

        if(total - pq.top() <= b){
            cnt = i + 1;
        }else{
            break;
        }
    }

    cout << cnt << endl;
}
```

## E. Badgers

> **Problem:** [E. Badgers](https://codeforces.com/group/jtU6D2hVEi/contest/533371/problem/E)
>
> **Solution:** [GitHub Code](https://github.com/wulukewu/cp-code/blob/main/codeforces/group/jtU6D2hVEi/533371/E_Badgers.cpp)

- 若總共取了 `k` 個， 則第 `i` 隻的消耗為 `hi + gi * (k-1)`
- 最大化數量為 `k` 個，所以做 for 迴圈去判斷 `k` 可以到多少還不會超過 `t`

```cpp
void solve() {
    int n;
    cin >> n;

    vector < pair < int, int > > v(n);
    FOR(i, 0, n) cin >> v[i].F;
    FOR(i, 0, n) cin >> v[i].S;

    int t;
    cin >> t;

    int ans = 0;
    FOR(k, 1, n+1){
        vector < int > costs(n);
        FOR(i, 0, n){
            costs[i] = v[i].F + v[i].S * (k-1);
        }
        sort(ALL(costs));

        int total = 0;
        FOR(i, 0, k){
            total += costs[i];
        }

        if(total <= t){
            ans = k;
        }else{
            break;
        }
    }

    cout << ans << endl;
}
```
