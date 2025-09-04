---
title: Codeforces 暑期特訓：我想成為演算法大師 - 團練 ICPC Asia Tehran Regional Contest 2022 - Week 8
date: 2025-09-04 19:36:47
tags:
  - algorithm
  - codeforces
  - competitive-programming
---

# The 2022 ICPC Asia Tehran Regional Contest

- [Contest Problems](https://codeforces.com/gym/105637)

## A. Final Price

> **Problem:** [A. Final Price](https://codeforces.com/gym/105637/problem/A)
>
> **Solution:** [GitHub Code](https://github.com/wulukewu/cp-code/blob/main/codeforces/gym/105637/A.cpp)

```cpp
void solve() {
    int n;
    cin >> n;
    int ans = 0,in;
    while(n--){
        cin >> in;
        ans += in;
    }
    cout << ans << endl;
}
```

## B. Flower Festival

> **Problem:** [B. Flower Festival](https://codeforces.com/gym/105637/problem/B)
>
> **Solution:** [GitHub Code](https://github.com/wulukewu/cp-code/blob/main/codeforces/gym/105637/B3.cpp)

```cpp
void solve() {
    int n;
    double far;
    cin >> n >> far;
    int car;
    double time = -1;
    for(int i=1;i<=n;i++){
        double loc,speed,dis,t;
        cin >> loc >> speed;
        dis = far - loc;
        t = dis / speed;
        if(time == -1 or t < time){
            time = t;
            car = i;
        }
    }
    cout << car << endl;
}
```

## E. Parking Party

> **Problem:** [E. Parking Party](https://codeforces.com/gym/105637/problem/E)
>
> **Solution:** [GitHub Code](https://github.com/wulukewu/cp-code/blob/main/codeforces/gym/105637/E2.cpp)

```cpp
void solve() {
    int n, m;
    cin >> n >> m;
    vector < string > arr(n);
    for(int i=0; i<n; i++) cin >> arr[i];

    vector < vector < bool > > v(n, vector < bool > (m, false));

    int cnt = 0;
    // left
    for(int i=0; i<n; i++){
        for(int j=0; j<m; j++){
            if(arr[i][j]=='o') break;
            if(!v[i][j]){
                cnt++;
                v[i][j] = true;
            }
        }
    }
    // right
    for(int i=0; i<n; i++){
        for(int j=m-1; j>=0; j--){
            if(arr[i][j]=='o') break;
            if(!v[i][j]){
                cnt++;
                v[i][j] = true;
            }
        }
    }
    // up
    for(int j=0; j<m; j++){
        for(int i=0; i<n; i++){
            if(arr[i][j]=='o') break;
            if(!v[i][j]){
                cnt++;
                v[i][j] = true;
            }
        }
    }
    // down
    for(int j=0; j<m; j++){
        for(int i=n-1; i>=0; i--){
            if(arr[i][j]=='o') break;
            if(!v[i][j]){
                cnt++;
                v[i][j] = true;
            }
        }
    }

    cout << cnt << endl;
}
```

## J. Magic with Cards

> **Problem:** [J. Magic with Cards](https://codeforces.com/gym/105637/problem/J)
>
> **Solution:** [GitHub Code](https://github.com/wulukewu/cp-code/blob/main/codeforces/gym/105637/J.cpp)

```cpp
void solve() {
    int n,start,end;
    cin >> n >> start >> end;
    vector<int> step(2*n+1,-1);
    queue<int> next;
    next.push(start);
    int now = 0;
    while(step[end] == -1){
        if(next.empty()) break;
        int move = next.size();
        while(move--){
            int go = next.front();
            next.pop();
            step[go] = now;
            int one,two;
            if(go <= n){
                one = go * 2 - 1;
            }
            else{
                one = (go - n) * 2;
            }
            if(go%2 == 0){
                two = go - 1;
            }
            else{
                two = go + 1;
            }
            if(step[one] == -1) next.push(one);
            if(step[two] == -1) next.push(two);
        }
        now += 1;
    }
    cout << step[end] << endl;
}
```
