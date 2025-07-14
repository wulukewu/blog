---
title: Codeforces 暑期特訓：我想成為演算法大師 - Week 1
date: 2025-07-05 16:27:23
tags:
  - algorithm
  - codeforces
  - competitive-programming
---

> From [LI2 Contests](https://codeforces.com/group/jtU6D2hVEi) Group

# Contest 09. Prefix sums and countings

- [Contest Problems](https://codeforces.com/group/jtU6D2hVEi/contest/104997)

## C. Rectangle Sum

> **Problem:** [C. Rectangle Sum](https://codeforces.com/group/jtU6D2hVEi/contest/104997/problem/C)
> 
> **Solution:** [GitHub Code](https://github.com/wulukewu/cp-code/blob/main/codeforces/group/jtU6D2hVEi/104997/C_Rectangle_Sum.cpp)

```cpp
#include <iostream>
#include <vector>

using namespace std;

int main(){
    int n, m, k;
    cin >> n >> m >> k;

    vector < vector < int > > dp(n+1, vector < int > (m+1, 0));
    for(int i=1; i<=n; i++){
        for(int j=1; j<=m; j++){
            cin >> dp[i][j];
        }
    }

    for(int i=1; i<=n; i++){
        for(int j=1; j<=m; j++){
            dp[i][j] += dp[i-1][j] + dp[i][j-1] - dp[i-1][j-1];
        }
    }

    // for(int i=0; i<=n; i++){
    //     for(int j=0; j<=m; j++){
    //         cout << dp[i][j] << ' ';
    //     }
    //     cout << endl;
    // }

    int x1, y1, x2 ,y2;
    for(int i=0; i<k; i++){
        cin >> x1 >> y1 >> x2 >> y2;
        cout << dp[x2][y2] - dp[x2][y1-1] - dp[x1-1][y2] + dp[x1-1][y1-1] << endl;
    }
}
```

## F. Counting Sort

> **Problem:** [F. Counting Sort](https://codeforces.com/group/jtU6D2hVEi/contest/104997/problem/F)
> 
> **Solution:** [GitHub Code](https://github.com/wulukewu/cp-code/blob/main/codeforces/group/jtU6D2hVEi/104997/F_Counting_Sort.cpp)

```cpp
#include <iostream>
#include <vector>

using namespace std;

int main(){
    vector < int > arr;

    int n;
    while(cin >> n){
        arr.push_back(n);
    }

    n = arr.size();

    vector < int > brr(101, 0);
    for(int i=0; i<n; i++){
        brr[arr[i]]++;
    }

    for(int i=0; i<=100; i++){
        for(int j=0; j<brr[i]; j++){
            cout << i << ' ';
        }
    }
}
```

## G. Haybale Stacking

> **Problem:** [G. Haybale Stacking](https://codeforces.com/group/jtU6D2hVEi/contest/104997/problem/G)
> 
> **Solution:** [GitHub Code](https://github.com/wulukewu/cp-code/blob/main/codeforces/group/jtU6D2hVEi/104997/G_Haybale_Stacking.cpp)

```cpp
#include <iostream>
#include <vector>

using namespace std;

int main(){
    int n, k;
    cin >> n >> k;

    vector < int > arr(n, 0);
    int a, b;
    for(int i=0; i<k; i++){
        cin >> a >> b;
        arr[a-1]++;
        arr[b]--;
    }

    for(int i=1; i<n; i++){
        arr[i] += arr[i-1];
    }

    for(int i=0; i<n; i++){
        cout << arr[i] << ' ';
    }
}
```

# Contest 10. Sorting

- [Contest Problems](https://codeforces.com/group/jtU6D2hVEi/contest/105000)

## C. Sorting halves

> **Problem:** [C. Sorting halves](https://codeforces.com/group/jtU6D2hVEi/contest/105000/problem/C)
> 
> **Solution:** [GitHub Code](https://github.com/wulukewu/cp-code/blob/main/codeforces/group/jtU6D2hVEi/105000/C_Sorting_halves.cpp)

```cpp
#include <bits/stdc++.h>
// #include <iostream>
// #include <vector>
// #include <algorithm>

using namespace std;

int main(){
    ifstream fcin("input.txt");
    ofstream fcout("output.txt");

    int n;
    // cin >> n;
    fcin >> n;

    vector < int > arr(n);
    for(int i=0; i<n; i++){
        // cin >> arr[i];
        fcin >> arr[i];
    }

    sort(arr.begin(), arr.begin() + n/2);
    // sort(arr.begin() + n/2, arr.end(), greater<int>());
    sort(arr.begin() + n/2, arr.end(), [](int a, int b){
        return a > b;
    });

    for(int i=0; i<n; i++){
        // cout << arr[i] << ' ';
        fcout << arr[i] << ' ';
    }
}
```

## D. Merge Lists

> **Problem:** [D. Merge Lists](https://codeforces.com/group/jtU6D2hVEi/contest/105000/problem/D)
> 
> **Solution:** [GitHub Code](https://github.com/wulukewu/cp-code/blob/main/codeforces/group/jtU6D2hVEi/105000/D_Merge_Lists.cpp)

```cpp
#include <bits/stdc++.h>
// #include <iostream>
// #include <vector>
// #include <sstream>

using namespace std;

int main(){
    ifstream fcin("input.txt");
    ofstream fcout("output.txt");

    int n;
    string line;
    vector < int > arr;
    vector < int > brr;

    getline(fcin, line);
    stringstream ss(line);
    while(ss >> n){
        arr.push_back(n);
    }
    getline(fcin, line);
    ss.clear();
    ss.str(line);
    while(ss >> n){
        brr.push_back(n);
    }

    // for(int i: arr){
    //     cout << i << ' ';
    // }
    // cout << endl;
    // for(int i: brr){
    //     cout << i << ' ';
    // }
    // cout << endl;

    vector < int > crr;
    int n1 = arr.size();
    int n2 = brr.size();
    int j = 0;
    for(int i=0; i<n1; i++){
        while(arr[i]>brr[j] and j<n2){
            crr.push_back(brr[j]);
            j++;
        }
        crr.push_back(arr[i]);
    }

    while(j<n2){
        crr.push_back(brr[j]);
        j++;
    }

    for(int i: crr){
        fcout << i << ' ';
    }
}
```

## F. Sorting. Stone method.

> **Problem:** [F. Sorting. Stone method.](https://codeforces.com/group/jtU6D2hVEi/contest/105000/problem/F)
> 
> **Solution:** [GitHub Code](https://github.com/wulukewu/cp-code/blob/main/codeforces/group/jtU6D2hVEi/105000/F_Sorting_Stone_method.cpp)

```cpp
#include <bits/stdc++.h>
// #include <iostream>
// #include <vector>

using namespace std;

int main(){
    ifstream fcin("input.txt");
    ofstream fcout("output.txt");

    int n;
    fcin >> n;

    vector < int > arr(n);
    for(int i=0; i<n; i++){
        fcin >> arr[i];
    }

    bool det = true;
    for(int i=n-1; i>0; i--){
        for(int j=0; j<i; j++){
            if(arr[j]>arr[j+1]){
                swap(arr[j], arr[j+1]);
                det = false;
                for(int k: arr){
                    fcout << k << ' ';
                }
                fcout << endl;
            }
        }
    }

    if(det){
        fcout << 0;
    }
}
```

## K. Olympiad Results

> **Problem:** [K. Olympiad Results](https://codeforces.com/group/jtU6D2hVEi/contest/105000/problem/K)
> 
> **Solution:** [GitHub Code](https://github.com/wulukewu/cp-code/blob/main/codeforces/group/jtU6D2hVEi/105000/K_Olympiad_Results.cpp)

```cpp
#include <bits/stdc++.h>
// #include <iostream>
// #include <vector>

using namespace std;

int main(){
    ifstream fcin("input.txt");
    ofstream fcout("output.txt");

    int n;
    fcin >> n;

    int a, b;
    vector < pair < int, int > > arr(n);
    for(int i=0; i<n; i++){
        fcin >> a >> b;
        arr[i] = make_pair(a, b);
    }

    sort(arr.begin(), arr.end(), [](pair < int, int > p1, pair < int, int > p2){
        if(p1.second == p2.second){
            return p1.first < p2.first;
        }else{
            return p1.second > p2.second;
        }
    });

    for(auto i: arr){
        fcout << i.first << ' ' << i.second << endl;
    }
}
```