---
title: 2025 TOPC
date: 2025-09-20 12:11:48
tags:
  - algorithm
  - competitive-programming
  - topc
---

# 2024 ICPC Asia Taiwan Online Programming Contest

- [Contest Problems](https://codeforces.com/gym/105383)

## A. Take It or Double It

> **Problem:** [A. Take It or Double It]()
>
> **Solution:** [GitHub Code](https://github.com/wulukewu/cp-code/blob/main/icpc/topc/2025-09-20/A.cpp)

```cpp
void solve() {
    int x, d;
    cin >> x >> d;

    if(x*2>d){
        cout << "take it" << endl;
    }else{
        cout << "double it" << endl;
    }
}
```

## B. Twin Guardians

> **Problem:** [B. Twin Guardians]()
>
> **Solution:** [GitHub Code](https://github.com/wulukewu/cp-code/blob/main/icpc/topc/2025-09-20/B.cpp)

```cpp
void solve() {
    int a,b;
    cin >> a >> b;
    bool check = true;
    if(b-a != 2) check = false;
    else if(a == 1) check = false;
    else{
        for(int i=2;i*i<=a;i++){
            if(a%i == 0){
                check = false;
                break;
            }
        }
        for(int i=2;i*i<=b;i++){
            if(b%i == 0){
                check = false;
                break;
            }
        }
    }
    if(check) cout << "Y" << endl;
    else cout << "N" << endl;

}
```
