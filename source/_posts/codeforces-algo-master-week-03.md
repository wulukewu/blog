---
title: Codeforces 暑期特訓：我想成為演算法大師 - Week 3
date: 2025-07-16 12:18:27
tags:
  - algorithm
  - codeforces
  - competitive-programming
---

> From [LI2 Contests](https://codeforces.com/group/jtU6D2hVEi) Group

# Contest 11. Binary Search

- [Contest Problems](https://codeforces.com/group/jtU6D2hVEi/contest/533121)

## M. Sorting fractions

> **Problem:** [M. Sorting fractions](https://codeforces.com/group/jtU6D2hVEi/contest/533121/problem/M)
>
> **Solution:** [GitHub Code]()

# Contest 15. Two Pointers

- [Contest Problems](https://codeforces.com/group/jtU6D2hVEi/contest/533249)

## C. Stylish clothes

> **Problem:** [C. Stylish clothes](https://codeforces.com/group/jtU6D2hVEi/contest/533249/problem/C)
>
> **Solution:** [GitHub Code](https://github.com/wulukewu/cp-code/blob/main/codeforces/group/jtU6D2hVEi/533249/C_Stylish_clothes.cpp)

對於每個顏色，用 `lower_bound` 找大於等於的最小顏色，判斷是不是更小的可能。

```cpp
void solve() {
    int n1, n2, n3, n4;

    cin >> n1;
    vector < int > a1(n1);
    FOR(i, 0, n1) cin >> a1[i];
    cin >> n2;
    vector < int > a2(n2);
    FOR(i, 0, n2) cin >> a2[i];
    cin >> n3;
    vector < int > a3(n3);
    FOR(i, 0, n3) cin >> a3[i];
    cin >> n4;
    vector < int > a4(n4);
    FOR(i, 0, n4) cin >> a4[i];

    sort(ALL(a1));
    sort(ALL(a2));
    sort(ALL(a3));
    sort(ALL(a4));

    set < int > s;
    FOR(i, 0, n1) s.insert(a1[i]);
    FOR(i, 0, n2) s.insert(a2[i]);
    FOR(i, 0, n3) s.insert(a3[i]);
    FOR(i, 0, n4) s.insert(a4[i]);

    int colors_cnt = s.size();
    vector < int > brr;
    for(int i: s) brr.PB(i);

    int ans_diff = MOD;
    vector < int > ans(4, -1);
    for(int i: brr){
        auto it1 = lower_bound(ALL(a1), i);
        auto it2 = lower_bound(ALL(a2), i);
        auto it3 = lower_bound(ALL(a3), i);
        auto it4 = lower_bound(ALL(a4), i);
        if(it1==a1.end() or it2==a2.end() or it3==a3.end() or it4==a4.end()) break;

        int c1 = *it1;
        int c2 = *it2;
        int c3 = *it3;
        int c4 = *it4;

        vector < int > crr(4);
        crr[0] = c1;
        crr[1] = c2;
        crr[2] = c3;
        crr[3] = c4;

        int diff = 0;
        FOR(i, 0, 4){
            FOR(j, i+1, 4){
                diff = max(diff, abs(crr[i]-crr[j]));
            }
        }

        if(diff<ans_diff){
            ans = crr;
            ans_diff = diff;
        }
        if(ans_diff==0) break;
    }

    FOR(i, 0, 4){
        cout << ans[i] << ' ';
    }
    cout << endl;
}
```

## G. Elves and Reindeer

> **Problem:** [G. Elves and Reindeer](https://codeforces.com/group/jtU6D2hVEi/contest/533249/problem/G)
>
> **Solution:** [GitHub Code]()

WA on test 2

```cpp
void solve() {
    int m, n;
    cin >> m >> n;

    vector < pair < int, int > > arr(m);
    FOR(i, 0, m){
        cin >> arr[i].F;
        arr[i].S = i+1;
    }
    int x;
    set < pair < int, int > > brr;
    FOR(i, 0, n){
        cin >> x;
        brr.insert(make_pair(x, i+1));
    }

    // for(auto b: brr){
    //     cout << b.F << ' ' << b.S << endl;
    // }

    sort(ALL(arr));

    vector < pair < int, pair < int, int > > > ans;

    for(auto a: arr){
        auto it1 = brr.lower_bound(make_pair(a.F, -1));
        if(it1 == brr.begin()) continue;
        it1--;

        auto it2 = brr.upper_bound(make_pair(a.F, n+1));
        if(it2 == brr.end()) continue;

        pair < int, int > p1 = *it1;
        pair < int, int > p2 = *it2;
        // cout << p1.F << ' ' << p1.S << ' ' << p2.F << ' ' << p2.S << endl;

        ans.PB(make_pair(a.S, make_pair(p1.S, p2.S)));

        brr.erase(it1);
        brr.erase(it2);
    }

    cout << ans.size() << endl;
    for(auto a: ans){
        cout << a.F << ' ' << a.S.F << ' ' << a.S.S << endl;
    }
}
```

# Contest 16. Linear Data Structures

- [Contest Problems](https://codeforces.com/group/jtU6D2hVEi/contest/533250)

## C. Brackets

> **Problem:** [C. Brackets](https://codeforces.com/group/jtU6D2hVEi/contest/533250/problem/C)
>
> **Solution:** [GitHub Code]()

用 stack 儲存左括號，並判斷是否能消去右括號

```cpp
void solve() {
    string s;
    cin >> s;

    int ans = true;
    stack < char > st;
    for(char i: s){
        if(i=='(' or i=='[' or i=='{'){
            st.push(i);
        }else if(i==')'){
            if(st.size()>0 and st.top()=='('){
                st.pop();
            }else{
                ans = false;
                break;
            }
        }else if(i==']'){
            if(st.size()>0 and st.top()=='['){
                st.pop();
            }else{
                ans = false;
                break;
            }
        }else if(i=='}'){
            if(st.size()>0 and st.top()=='{'){
                st.pop();
            }else{
                ans = false;
                break;
            }
        }
    }

    if(ans and st.size()==0){
        cout << "YES" << endl;
    }else{
        cout << "NO" << endl;
    }
}
```

## H. Postfix Notation

> **Problem:** [H. Postfix Notation](https://codeforces.com/group/jtU6D2hVEi/contest/533250/problem/H)
>
> **Solution:** [GitHub Code]()

用 stack 儲存數字，每次的符號提出最上面兩個數做運算再存回去

```cpp
void solve() {
    stack < int > st;

    int a, b;
    char c;
    while(cin >> c){
        if('0'<=c and c<='9'){
            st.push(c-'0');
            continue;
        }

        b = st.top();
        st.pop();
        a = st.top();
        st.pop();

        if(c=='+'){
            st.push(a+b);
        }else if(c=='-'){
            st.push(a-b);
        }else if(c=='*'){
            st.push(a*b);
        }
    }

    cout << st.top() << endl;
}
```

## M. Adjacency Lists

> **Problem:** [M. Adjacency Lists](https://codeforces.com/group/jtU6D2hVEi/contest/533250/problem/M)
>
> **Solution:** [GitHub Code]()

建立大小為 `n` 的二維 `vector` ，裡面存入每個點會連結到的其他點

```cpp
void solve() {
    int n, m;
    cin >> n >> m;

    vector < vector < int > > v(n);
    int a, b;
    FOR(i, 0, m){
        cin >> a >> b;
        v[a-1].PB(b);
        v[b-1].PB(a);
    }

    FOR(i, 0, n){
        cout << v[i].size() << ' ';
        for(int j: v[i]){
            cout << j << ' ';
        }
        cout << endl;
    }
}
```

# Contest 17. Set and Map

- [Contest Problems](https://codeforces.com/group/jtU6D2hVEi/contest/533251)

## B. Same values

> **Problem:** [B. Same values](https://codeforces.com/group/jtU6D2hVEi/contest/533251/problem/B)
>
> **Solution:** [GitHub Code]()

先用 map 存所有出現的數字對應到的位置，再去找到位置後 iterator 往前一格看

```cpp
oid solve() {
    int n;
    cin >> n;

    vector < int > arr(n);
    FOR(i, 0, n) cin >> arr[i];

    map < int, vector < int > > mp;
    FOR(i, 0, n){
        if(mp.find(arr[i])==mp.end()){
            mp.insert(make_pair(arr[i], vector < int > (1, i)));
        }else{
            mp[arr[i]].PB(i);
        }
    }

    FOR(i, 0, n){
        auto it = lower_bound(ALL(mp[arr[i]]), i);
        if(it==mp[arr[i]].begin()){
            cout << -1 << ' ';
        }else{
            it--;
            cout << i-*it << ' ';
        }
    }
}
```

## G. Shooting

> **Problem:** [G. Shooting](https://codeforces.com/group/jtU6D2hVEi/contest/533251/problem/G)
>
> **Solution:** [GitHub Code]()

- 用 map 存每個人的總分，再印出來最高分的所有人
- 這裡用了 `name_set` 及 `name_vector` 來存名字的順序

```cpp
void solve() {
    int n;
    cin >> n;

    map < string, int > mp;
    set < string > name_set;
    vector < string > name_vector;

    string name;
    int score;
    FOR(i, 0, n){
        cin >> name >> score;
        if(mp.find(name)==mp.end()){
            mp.insert(make_pair(name, score));
        }else{
            mp[name] += score;
        }

        if(name_set.find(name)==name_set.end()){
            name_set.insert(name);
            name_vector.PB(name);
        }
    }

    int max_score = 0;
    for(auto i: mp){
        max_score = max(max_score, i.S);
    }
    cout << max_score << endl;

    int cnt = 0;
    for(string i: name_vector){
        if(mp[i]==max_score){
            if(cnt>0) cout << ',';
            cout << i;
            cnt++;
        }
    }
}
```

## L. Set 3

> **Problem:** [L. Set 3](https://codeforces.com/group/jtU6D2hVEi/contest/533251/problem/L)
>
> **Solution:** [GitHub Code]()

做一個 set 按照題意照做，在 `nearest` 時用 `lower_bound` 找到最接近的其中一個位置

```cpp
void solve() {
    int n;
    cin >> n;

    set < int > s;
    string op;
    int x;
    FOR(i, 0, n){
        cin >> op >> x;
        if(op=="add"){
            if(s.find(x)==s.end()){
                s.insert(x);
                cout << "YES" << endl;
            }else{
                cout << "NO" << endl;
            }
        }else if(op=="remove"){
            if(s.find(x)!=s.end()){
                s.erase(x);
                cout << "YES" << endl;
            }else{
                cout << "NO" << endl;
            }
        }else if(op=="nearest"){
            if(s.empty()){
                cout << "ERROR" << endl;
                continue;
            }

            auto it = s.lower_bound(x);

            int y = 0;
            int z = LLONG_MAX;
            if(it!=s.end()){
                if(abs(*it-x)<z){
                    y = *it;
                    z = abs(*it-x);
                }else if(abs(*it-x)==z){
                    y = max(y, *it);
                }
            }
            if(it!=s.begin()){
                it--;
                if(abs(*it-x)<z){
                    y = *it;
                    z = abs(*it-x);
                }else if(abs(*it-x)==z){
                    y = max(y, *it);
                }
            }
            cout << y << endl;
        }
    }

    stack < int > ans;
    for(int i: s){
        ans.push(i);
    }
    while(!ans.empty()){
        cout << ans.top() << ' ';
        ans.pop();
    }
}
```
