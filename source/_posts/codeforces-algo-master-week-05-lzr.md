---
title: Codeforces 暑期特訓：我想成為演算法大師 - 團練 GCPC 2017 - Week 5
date: 2025-08-02 19:15:06
tags:
  - algorithm
  - codeforces
  - competitive-programming
---

# 2017-2018 ACM-ICPC German Collegiate Programming Contest (GCPC 2017)

- [Contest Problems](https://codeforces.com/gym/101873)
- [Notes](https://github.com/wulukewu/cp-code/blob/main/codeforces/gym/101873/notes.txt)

## C. Joyride

> **Problem:** [C. Joyride](https://codeforces.com/gym/101873/problem/C)
>
> **Solution:** [GitHub Code](https://github.com/wulukewu/cp-code/blob/main/codeforces/gym/101873/C.cpp)

```cpp
void solve(){
    int x,n,m,t;
    cin >> x;
    cin >> n >> m >> t;
    vector <vector<int>> path(n+1,vector<int>());
    int a,b;
    FOR(i,0,m){
        cin >> a >> b;
        path[a-1].push_back(b-1);
        path[b-1].push_back(a-1);
    }
    vector< pair<int,int> > ride(n);
    FOR(i,0,n){
        cin >> ride[i].F >> ride[i].S;
    }

    vector < vector < int > > dp(x+1, vector < int > (n, INT_MAX));
    if(ride[0].F <= x){
        dp[ride[0].F][0] = ride[0].S;
    }
    FOR(i, 0, x+1){
        FOR(j, 0, n){
            if(dp[i][j] == INT_MAX) continue;
            for(int k: path[j]){
                int nt = i + t + ride[k].F;
                if(nt <= x){
                    int val = dp[i][j] + ride[k].S;
                    dp[nt][k] = min(dp[nt][k], val);
                }
            }
            if(i+ride[j].F<=x){
                dp[i+ride[j].F][j] = min(dp[i+ride[j].F][j], dp[i][j]+ride[j].S);
            }
        }
    }

    if(dp[x][0]!=INT_MAX){
        cout << dp[x][0] << endl;
    }else{
        cout << "It is a trap." << endl;
    }
}
```

## D. Pants On Fire

> **Problem:** [D. Pants On Fire](https://codeforces.com/gym/101873/problem/D)
>
> **Solution:** [GitHub Code](https://github.com/wulukewu/cp-code/blob/main/codeforces/gym/101873/D3.cpp)

```cpp
void solve(){
    int n, m;
    cin >> n >> m;

    int siz = 0;
    map < string, int > mp;
    vector < vector < string > > v;
    string a, b, s;

    FOR(i, 0, n){
        cin >> a;
        FOR(j, 0, 3) cin >> s;
        cin >> b;

        if(mp.find(a)==mp.end()){
            mp[a] = siz;
            v.PB(vector < string >());
            siz++;
        }
        if(mp.find(b)==mp.end()){
            mp[b] = siz;
            v.PB(vector < string >());
            siz++;
        }

        v[mp[a]].PB(b);
    }

    FOR(i, 0, m){
        cin >> a;
        FOR(j, 0, 3) cin >> s;
        cin >> b;

        if(mp.find(a)==mp.end() or mp.find(b)==mp.end()){
            cout << "Pants on Fire" << endl;
            continue;
        }

        bool is_fact = false;
        queue < string > q;
        q.push(a);
        while(!q.empty()){
            string t = q.front();
            q.pop();

            if(t == b){
                is_fact = true;
                break;
            }

            for(string t2: v[mp[t]]){
                q.push(t2);
            }
        }

        if(is_fact){
            cout << "Fact" << endl;
        }else{
            bool alt_fact = false;
            queue < string > q2;
            q2.push(b);
            while(!q2.empty()){
                string t = q2.front();
                q2.pop();

                if(t == a){
                    alt_fact = true;
                    break;
                }

                // cout << "v[mp[t]]: ";
                // print(v[mp[t]]);
                for(string t2: v[mp[t]]){
                    q2.push(t2);
                }
            }
            if(alt_fact){
                cout << "Alternative Fact" << endl;
            }else{
                cout << "Pants on Fire" << endl;
            }
        }
    }
}
```

## G. Water Testing

> **Problem:** [G. Water Testing](https://codeforces.com/gym/101873/problem/G)
>
> **Solution:** [GitHub Code](https://github.com/wulukewu/cp-code/blob/main/codeforces/gym/101873/G2.cpp)

```cpp
void solve(){
    int n;
    cin >> n;
    vector<int> x(n);
    vector<int> y(n);
    FOR(i, 0, n){
        cin >> x[i] >> y[i];
    }

    int area2 = 0;
    int B = 0;

    FOR(i, 0, n){
        int j = (i+1) % n;
        area2 += x[i]*y[j] - x[j]*y[i];
        B += std::gcd(abs(x[j]- x[i]), abs(y[j]-y[i]));
    }

    area2 = abs(area2);

    int ans = (area2 - B + 2) / 2;

    cout << ans << endl;
}
```

## I. Uberwatch

> **Problem:** [I. Uberwatch](https://codeforces.com/gym/101873/problem/I)
>
> **Solution:** [GitHub Code](https://github.com/wulukewu/cp-code/blob/main/codeforces/gym/101873/I.cpp)

```cpp
void solve(){
    int n, m;
    cin >> n >> m;
    vector<int> x(n);
    vector<int> dp(n, 0);
    FOR(i, 0, n){
        cin >> x[i];
    }

    FOR(i, m, n){
        dp[i] = max(dp[i-1], dp[i-m] + x[i]);
    }

    cout << dp[dp.size()-1];

}
```

## K. You Are Fired

> **Problem:** [K. You Are Fired](https://codeforces.com/gym/101873/problem/K)
>
> **Solution:** [GitHub Code](https://github.com/wulukewu/cp-code/blob/main/codeforces/gym/101873/K.cpp)

```cpp
void solve(){
    int n, d, k;
    cin >> n >> d >> k;

    vector < pair < int, string > > arr(n);
    FOR(i, 0, n){
        cin >> arr[i].S >> arr[i].F;
    }

    sort(ALL(arr), greater());

    int m = 0;
    int idx = 0;
    queue < string > ans;
    while(m<d and idx<k){
        m += arr[idx].F;
        ans.push(arr[idx].S);
        idx++;
    }

    if(m<d){
        cout << "impossible" << endl;
    }else{
        cout << idx << endl;
        while(!ans.empty()){
            cout << ans.front() << ", YOU ARE FIRED!" << endl;
            ans.pop();
        }
    }
}
```
