---
title: 転倒数
layout: default
parent: segment tree
grand_parent: Atcoder (Dec. 2, 2023〜)
---

<script type="text/javascript" id="MathJax-script" async src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-chtml.js"></script>

# 転倒数

転倒数を検索するとBITが出現するのですが、segment treeでゴリ押しました。

転倒数はバブルソートの交換回数と一致します。詳しくは<a href="https://ikatakos.com/pot/programming_algorithm/dynamic_programming/inversion" target="_blank">転倒数</a>

ABC332のD問題では転倒数を求めるために、要素が本来の位置にいなければswapという実装をしました。これは\\(O(N^2)\\)であり、制約によってはTLEします。

今回はsegment treeを使用し、\\(O(logN)\\)で転倒数を求めました。

## コード

問題 : <a href="https://atcoder.jp/contests/chokudai_S001/tasks/chokudai_S001_j" target="_blank">J - 転倒数</a>

```cpp
template <typename T>
class RMQ {
public:
    const T INF = numeric_limits<T>::max();
    int n;
    vector<T> dat, lazy;
    RMQ(int n_) : n(), dat(n_ * 4, 0), lazy(n_ * 4, 0) {
        int x = 1;
        while (n_ > x) x *= 2;
        n = x;
    }

    void update(int a, int b, T x, int k, int l, int r) {
        eval(k);
        if (a <= l && r <= b) {  // 完全に内側の時
            lazy[k] = x * (b - a);
            eval(k);
        } else if (a < r && l < b) {                     // 一部区間が被る時
            update(a, b, x, k * 2 + 1, l, (l + r) / 2);  // 左の子
            update(a, b, x, k * 2 + 2, (l + r) / 2, r);  // 右の子
            dat[k] = dat[k * 2 + 1] + dat[k * 2 + 2];
        }
    }
    void update(int a, int b, T x) { update(a, b, x, 0, 0, n); }

    T query_sub(int a, int b, int k, int l, int r) {
        eval(k);
        if (r <= a || b <= l) {  // 完全に外側の時
            return 0;
        } else if (a <= l && r <= b) {  // 完全に内側の時
            return dat[k];
        } else {  // 一部区間が被る時
            T vl = query_sub(a, b, k * 2 + 1, l, (l + r) / 2);
            T vr = query_sub(a, b, k * 2 + 2, (l + r) / 2, r);
            return vl + vr;
        }
    }
    T query(int a, int b) { return query_sub(a, b, 0, 0, n); }

private:
    /* lazy eval */
    void eval(int k) {
        if (lazy[k] == 0) return;  // 更新するものが無ければ終了
        if (k < n - 1) {             // 葉でなければ子に伝搬
            lazy[k * 2 + 1] += lazy[k]/2;
            lazy[k * 2 + 2] += lazy[k]/2;
        }
        // 自身を更新
        dat[k] += lazy[k];
        lazy[k] = 0;
    }
};

int main(){
    int n; cin >> n;
    vector<int> a(n); rep(i, n) cin >> a[i];
    rep(i, n) a[i]--;

    RMQ<int> rmq(n);
    ll ans = 0; // 制約に気を付ける
    rep(i, n){
        ans += (i - rmq.query(0, a[i]+1)); // rmq.query(a[i], a[i]+1)としていてそれに気が付かなかった笑
        rmq.update(a[i], a[i]+1, 1);
    }
    cout << ans << endl;
    return 0;
}
```

### 注意点

解は最悪制約の2乗となるため、ansにはintではなくlong longを使用しましょう。(intにしたら案の定オーバーフローした。)

### ミス

```cpp
ans += (i - rmq.query(a[i], a[i]+1)); // ミス
ans += (i - rmq.query(0, a[i]+1)); // 正しい
```

queryによって自分より左側の条件を満たす要素の個数を知りたいため、[0, a[i]+1)とする必要がある。(i - queryとすることによって条件を満たさないものの個数。1個の要素に着目した際の転倒数)