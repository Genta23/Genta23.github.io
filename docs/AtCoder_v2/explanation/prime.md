---
title: エラトステネスの篩
layout: default
parent: 考察 解説
grand_parent: Atcoder (Dec. 2, 2023〜)
---

<script type="text/javascript" id="MathJax-script" async src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-chtml.js"></script>

# エラトステネスの篩

<a href="https://algo-logic.info/is-prime/" target="_blank">参考サイト : 素数かどうかを判定するアルゴリズム</a>

\\(O(NloglogN)\\)の計算量で計算が完了する。素数かどうかを判定した配列が返ってくる。 

## 素数判定

```cpp
/*  make_is_prime(N)
    入力：整数 N
    出力：N までの数字が素数か判定したベクトル（i番目がtrueならiは素数）
    計算量：O(nloglogn)
*/
vector<bool> make_is_prime(int N) {
    vector<bool> prime(N + 1, true);
    if (n >= 0) prime[0] = false;
    if (n >= 1) prime[1] = false;
    for (int i = 2; i * i <= N; i++) {
        if (!prime[i]) continue;
        for (int j = i * i; j <= N; j += i) {
            prime[j] = false;
        }
    }
    return prime;
}
```

\\(N\\)までの素数を判定する。

```cpp
vector<bool> f = make_is_prime(N);
```

### 注意点

0-basedであることに注意すること。

