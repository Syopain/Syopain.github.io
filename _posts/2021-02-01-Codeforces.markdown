---
layout:     post
title:      "Codeforces Round #693 (Div. 3)"
subtitle:   "A-E题解"
date:       2021-02-01 13:00:00
author:     "Pain"
header-img: "img/post-bg-2015.jpg"
tags:
    - Codeforces
---

> 防止日后看不懂以前自己写过的代码，所以记录一下当时的思路o_0

#### [A. Cards for Friends](https://codeforces.com/contest/1472/problem/A)

如果纸的边长为偶数则可以切开一分为二，切开后每张纸都有同样的尺寸，因此每次切割后纸的张数为原来的二倍。所以对纸的长宽进行切割，直到长和宽都为奇数为止，此时纸的张数大于或等于朋友的数量则输出YES。

```
#include <bits/stdc++.h>
 
using namespace std;
 
using ll = long long;
 
void solve()
{
    ll w, h, n;
    cin >> w >> h >> n;
    ll ans = 1;
    while (w > 0 && w % 2 == 0) {
        ans *= 2;
        w /= 2;
    }
    while (h > 0 && h % 2 == 0) {
        ans *= 2;
        h /= 2;
    }
    cout << (ans >= n ? "YES" : "NO") << endl;
}
 
int main()
{
    ios_base::sync_with_stdio(false);
    cin.tie(0);
 
    ll t;
    cin >> t;
    while(t--) {
        solve();
    }
}
```

#### [B. Fair Division](https://codeforces.com/contest/1472/problem/B)

记录一克糖果和两克糖果的数量，如果一克糖果的数量为奇数，那么糖果的总重量一定是奇数，那必然没法平分，直接输出NO；如果两克糖果数量为奇数，那么至少需要有两棵一克糖果才能平分。

```
#include <bits/stdc++.h>
 
using namespace std;
 
using ll = long long;
 
void solve()
{
    ll n;
    cin >> n;
    ll one_cnt = 0, two_cnt = 0;
    int ai;
    for (int i = 0; i < n; ++i) {
        cin >> ai;
        if (ai == 1) {
            one_cnt++;
        }
        if (ai == 2) {
            two_cnt++;
        }
    }
 
    if (one_cnt & 1) {
        cout << "NO" << endl;
        return;
    }
    if (two_cnt & 1 && one_cnt < 2) {
        cout << "NO" << endl;
        return;
    }
 
    cout << "YES" << endl;
}
 
int main()
{
    ios_base::sync_with_stdio(false);
    cin.tie(0);
 
    ll t;
    cin >> t;
    while(t--) {
        solve();
    }
}
```

#### [C - Long Jumps](https://codeforces.com/contest/1472/problem/C)

使用简单的模拟算法求出每个起点的得分，最后选择最大值，很显然这个算法会超时，因为有太多的重复计算。

这是个简单的动态规划问题，设状态dp[i]为以a[i]为起点的得分，那么状态转移方程dp[i] = a[i] + dp[i+a[i]]，可以看出dp[i]的结果依赖与dp[j](j>i)，所以我们可以从后往前计算；如果该位置的下一个步越界，则dp[i] = a[i]。

```
#include <bits/stdc++.h>
 
using namespace std;
 
using ll = long long;
 
void solve()
{
    ll n;
    cin >> n;
    vector<ll> a(n);
    vector<ll> dp(n);
 
    for (int i = 0; i < n; ++i) {
        cin >> a[i];
    }
 
    for (int i = n - 1; i >= 0; --i) {
        if (i + a[i] < n) {
            dp[i] = a[i] + dp[i + a[i]];
        }
        else {
            dp[i] = a[i];
        }
    }
 
    ll ans = *max_element(dp.begin(), dp.end());
    cout << ans << endl;
 
}
 
int main()
{
    ios_base::sync_with_stdio(false);
    cin.tie(0);
 
    ll t;
    cin >> t;
    while(t--) {
        solve();
    }
}
```

#### [D. Even-Odd Game](https://codeforces.com/contest/1472/problem/D)

对于Alice和Bob来说，最优的方式都是选择可选的最大值，无论奇数还是偶数，让对方失分和自己得分实际意义上是一样的。

```
#include <bits/stdc++.h>
 
using namespace std;
 
using ll = long long;
 
void solve()
{
    ll n;
    cin >> n;
    vector<ll> a(n);
 
    for (int i = 0; i < n; ++i) {
        cin >> a[i];
    }
 
    sort(a.begin(), a.end(), [](ll a, ll b){return a > b;});
 
    ll alice = 0;
    ll bob = 0;
    for (int i = 0; i < n; i += 2) {
        if (a[i] % 2 == 0) {
            alice += a[i];
        }
    }
    for (int i = 1; i < n; i += 2) {
        if (a[i] % 2 == 1) {
            bob += a[i];
        }
    }
    if (alice > bob)
        cout << "Alice" << endl;
    else if (alice < bob)
        cout << "Bob" << endl;
    else
        cout << "Tie" << endl;
}
 
int main()
{
    ios_base::sync_with_stdio(false);
    cin.tie(0);
 
    ll t;
    cin >> t;
    while(t--) {
        solve();
    }
}
```

#### [E. Correct Placement](https://codeforces.com/contest/1472/problem/E)

首先对高度和宽度升序排序，然后对于每个元素，遍历有序表，查找第一个高和宽都小于他的元素。不出意外，这个算法将超时。尝试对以上算法加速处理，给有序表添加索引，记录每个相同身高中最瘦的元素。依旧超时，但通过了更多组数据。

于是我们只能另想办法，首先对高度升序排序，宽度降序排序。对于每一个元素，我们再把身高低于他的元素的宽度进行升序排序，于是我们便又得到了一个有序的宽度表，只需判断宽度表的第一个元素便能得到答案。

有序身高表中每个相同身高的最后一个元素则是该身高最瘦的一个元素，所以我们每处理完一个身高，只需要把该身高最瘦元素插入到身高表中。身高表可使用平衡树或者优先队列，于是我们把原来处理每个元素的时间复杂度从N降低到了LogN。

```
#include <bits/stdc++.h>
 
using namespace std;
 
using ll = long long;
 
const int MAXSIZE = 1000000005;
const int INF = 0x3f3f3f3f;
 
void solve()
{
    ll n;
    cin >> n;
    vector<tuple<ll, ll, ll>> arr(n);
    vector<ll> ans(n);
 
    for (ll i = 0; i < n; ++i) {
        ll hi, wi;
        cin >> hi >> wi;
        arr[i] = make_tuple(i, min(hi, wi), max(hi, wi));
    }
 
    sort(arr.begin(), arr.end(), [](tuple<ll, ll, ll> const& a, tuple<ll, ll, ll> const& b){
        if (get<2>(a) != get<2>(b))
            return get<2>(a) < get<2>(b);
        return get<1>(a) > get<1>(b);
    });
 
    map<ll, ll> m;
    for (int i = 0; i < n; ++i) {
        ans[get<0>(arr[i])] = -1;
        if (i > 0 && get<2>(arr[i-1]) < get<2>(arr[i])) {
            m[get<1>(arr[i-1])] = get<0>(arr[i-1]);
        }
        if (!m.empty() && m.begin()->first < get<1>(arr[i]))
            ans[get<0>(arr[i])] = m.begin()->second + 1;
    }
 
    for (auto i : ans) {
        cout << i << " ";
    }
    cout << endl;
}
 
int main()
{
    ios_base::sync_with_stdio(false);
    cin.tie(0);
 
    ll t;
    cin >> t;
    while(t--) {
        solve();
    }
}
```

