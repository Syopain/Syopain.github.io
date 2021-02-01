---
layout:     post
title:      "Codeforces Round #697 (Div. 3)"
subtitle:   "A-C题解"
date:       2021-01-26 13:00:00
author:     "Pain"
header-img: "img/post-bg-2015.jpg"
tags:
    - Codeforces
---

#### [A. Odd Divisor](https://codeforces.com/contest/1475/problem/A)

如果n为奇数, 则n必有奇除数(odd divisor)n本身，输出YES；否则将n除以2继续以上判断，直到`n==1`或n为奇数为止。因此可以得出结论，仅当n为2的次幂时n没有奇除数。
```
#include <bits/stdc++.h>
 
using namespace std;
 
using ll = long long;
 
const int INF = 0x3f3f3f3f;
 
void solve()
{
    ll n;
    cin >> n;
    while (n > 1) {
        if (n & 1) {
            cout << "YES" << endl;
            return;
        }
        n /= 2;
    }
    cout << "NO" << endl;
}
 
int main()
{
    ios_base::sync_with_stdio(false);
    cin.tie(0);

    int t;
    cin >> t;
    while(t--) {
        solve();
    }
}
```

#### [B. New Year's Number](https://codeforces.com/contest/1475/problem/B)

如果n可由2020和2021求和得到，则n一定在区间[i * 2020, i * 2021]上，其中i为整数，所以如果存在i满足以上条件则输出YES，否则输出NO。

```
#include <bits/stdc++.h>

using namespace std;

using ll = long long;

const int INF = 0x3f3f3f3f;

void solve()
{
    ll n;
    cin >> n;
    for (int i = 1; i * 2020 <= n ; ++i) {
        if (n >= 2020 * i && n <= 2021 * i) {
            cout << "YES" << endl;
            return;
        }
    }
    cout << "NO" << endl;
}
 
int main()
{
    ios_base::sync_with_stdio(false);
    cin.tie(0);
 
    int t;
    cin >> t;
    while(t--) {
        solve();
    }
}
```

#### [C. Ball in Berland](https://codeforces.com/contest/1475/problem/C)

只要两对男女生的成员不存在交集，则是一种可行的方案。首先我们从最简单的方法思考，枚举每一个组合并判断是否可行，该算法可以得出正确的结果。但从数据规模来看，很显然该算法的时间复杂度太高了。

所以我们要想其他办法，如果没有任何人出现多个对中，最大的组合方法有`k*(k-1)/2`种；如果有一个男生出现在n个对中，则有`n*(n-1)/2`种组合是不符合条件的；女生同理，所以可以统计每一个男女生出现的次数，计算出他们不符合条件的组合数，即可求出结果。

另外需要注意运算结果溢出，所以需要使用64位整数计算。

```
#include <bits/stdc++.h>
 
using namespace std;
 
using ll = long long;
 
const int INF = 0x3f3f3f3f;
 
void solve()
{
    map<int, int> A;
    map<int, int> B;
    int a, b, k;
    cin >> a >> b >> k;
    int input = 0;
    for (int i = 0; i < k; ++i) {
        cin >> input;
        A[input]++;
    }
    for (int i = 0; i < k; ++i) {
        cin >> input;
        B[input]++;
    }
 
    ll ans = (static_cast<ll>(k) * (k - 1)) / 2;
    for (auto iter = A.begin(); iter != A.end(); ++iter) {
        ans -= static_cast<ll>(iter->second) * (iter->second - 1) / 2;
    }
    for (auto iter = B.begin(); iter != B.end(); ++iter) {
        ans -= static_cast<ll>(iter->second) * (iter->second - 1) / 2;
    }
    cout << ans << endl;
}
 
int main()
{
    ios_base::sync_with_stdio(false);
    cin.tie(0);
 
    int t;
    cin >> t;
 
    while(t--) {
        solve();
    }
}
```

