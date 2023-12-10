---
title: 1353 発車タイミングの周期
layout: default
parent: 解けた問題 解説
grand_parent: Atcoder (Dec. 2, 2023〜)
---

<script type="text/javascript" id="MathJax-script" async src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-chtml.js"></script>

# ABC319 E問題

## 早速考察

\\(P_i\\)の制約に違和感。この数値に設定されているということは、理由がありそう。

1から8までの最小公倍数は840なので、これに\\(N\\)の制約考慮すると、\\(840 \times 10^5\\)で間に合いそう。

1から9までだと\\(2520 \times 10^5\\)、少しきつそう。この考察で解けそう。

出発する時間のmod 840に着目し、スタートからゴールまでどのくらいの時間がかかるかをあらかじめ計算しておけば、全てのクエリに\\(O(1)\\)で答えることができそう。

## コード

```cpp
int main(){
    int n; ll x, y; cin >> n >> x >> y;
    vector<ll> p(n-1), t(n-1); rep(i, n-1) cin >> p[i] >> t[i];
    int q; cin >> q;
    vector<int> query(q); rep(i, q) cin >> query[i];

    int mod = 840;
    vector<ll> data(mod);

    rep(i, mod){
        ll time = 0;
        time += x;
        rep(j, n-1){
            int r = (i+time) % p[j];
            if(r != 0) time += (p[j] - r);
            time += t[j];
        }
        time += y;
        data[i] = time;
    }

    rep(i, q) cout << query[i] + data[query[i]%mod] << endl;
    return 0;
}
```

## 改善

```cpp
// 改善前
while((i+time) % p[j] != 0) time++; // 割り切れるまで足す O(p)

// 改善後
int r = (i+time) % p[j];
if(r != 0) time += (p[j] - r); // 割り切れるまで足す O(1)
```

このように改善することで実行時間が、2182msから1009msに短縮された(問題の制限時間は3s)。



