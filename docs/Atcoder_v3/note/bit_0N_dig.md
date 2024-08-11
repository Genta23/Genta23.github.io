---
title: bit 0-N dig cnt
layout: default
parent: note
grand_parent: Atcoder (Aug. 11, 2024〜)
---

<script type="text/javascript" id="MathJax-script" async src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-chtml.js"></script>

## 0からNまでの数字を2進数表記した時に、i桁目において1が立つのは何回だろうか

- <a href="https://atcoder.jp/contests/abc356/editorial/10138" target="_blank">D - Masked Popcount 解説</a>

この解説の図を見ると、意外とすぐにわかる。

\\(f(i, N)\\)を0からNまでの数字を2進数表記した時に、i桁目において1が立つのは何回かという問いに対する解とする。

\\(r = 2^i\\), \\(y = \lfloor \frac{N+1}{2r} \rfloor\\), \\(x = (N+1) \bmod 2r\\) として、

\\(f(i, N) = yr + max(0, x-r)\\)で求めることができる。(理由は考えてください)

### ABC356 D問題

- <a href="https://atcoder.jp/contests/abc356/tasks/abc356_d" target="_blank">D - Masked Popcount</a>

```cpp
int f(int i, ll n){
    ll r = 1ll << i, y = n / (2*r), x = n % (2*r);
    return (y*r + max(0ll, x-r)) % mod;
}
int main(){
    ll n, m; cin >> n >> m;
    ll ans = 0; int i = 0;
    while(m > 0){
        if(m%2 == 1) ans += f(i, n+1), ans %= mod;
        m /= 2, i++;
    }
    cout << ans << endl;
    return 0;
}
```