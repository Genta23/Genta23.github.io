---
title: RMQ, RAQ 区間更新
layout: default
parent: segment tree
grand_parent: Atcoder (Dec. 2, 2023〜)
---

<script type="text/javascript" id="MathJax-script" async src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-chtml.js"></script>

# RMQ and RAQ 区間更新

区間加算系の問題です。lazy segment tree を使用します。

例題 : <a href="https://judge.u-aizu.ac.jp/onlinejudge/description.jsp?id=DSL_2_H&lang=ja" target="_blank">RMQ and RAQ</a>

## 使用するテンプレート

```cpp
/* RMQ：[0,n-1] について、区間ごとの最小値を管理する構造体
    update(a,b,x): 区間[a,b) の要素に x を加算。O(log(n))
    query(a,b): [a,b) での最小の要素を取得。O(log(n))
*/
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
            lazy[k] = x;
            eval(k);
        } else if (a < r && l < b) {                     // 一部区間が被る時
            update(a, b, x, k * 2 + 1, l, (l + r) / 2);  // 左の子
            update(a, b, x, k * 2 + 2, (l + r) / 2, r);  // 右の子
            dat[k] = min(dat[k * 2 + 1], dat[k * 2 + 2]);
        }
    }
    void update(int a, int b, T x) { update(a, b, x, 0, 0, n); }

    T query_sub(int a, int b, int k, int l, int r) {
        eval(k);
        if (r <= a || b <= l) {  // 完全に外側の時
            return INF;
        } else if (a <= l && r <= b) {  // 完全に内側の時
            return dat[k];
        } else {  // 一部区間が被る時
            T vl = query_sub(a, b, k * 2 + 1, l, (l + r) / 2);
            T vr = query_sub(a, b, k * 2 + 2, (l + r) / 2, r);
            return min(vl, vr);
        }
    }
    T query(int a, int b) { return query_sub(a, b, 0, 0, n); }

private:
    /* lazy eval */
    void eval(int k) {
        if (lazy[k] == 0) return;  // 更新するものが無ければ終了
        if (k < n - 1) {             // 葉でなければ子に伝搬
            lazy[k * 2 + 1] += lazy[k];
            lazy[k * 2 + 2] += lazy[k];
        }
        // 自身を更新
        dat[k] += lazy[k];
        lazy[k] = 0;
    }
};
```

区間更新の区間最小値取得問題 RMQのテンプレートとは、いくつか異なる点があります。
- dat, lazyの初期値がINFではなく0
- lazyの更新が上書きてはなく累積和 (= から += に変更)
- lazyを吐き出した後、INFではなく0に戻す

参考サイト : <a href="https://algo-logic.info/segment-tree/" target="_blank">セグメント木を徹底解説！0から遅延評価やモノイドまで</a>

## コード

```cpp

// 上記のRMQ, RAQテンプレート

int main(){
    int n, q; cin >> n >> q;
    RMQ<int> rmq(n);

    rep(i, q){
        int tmp; cin >> tmp;
        if(tmp == 0){
            int s, t, x; cin >> s >> t >> x;
            rmq.update(s, t+1, x);
        }
        else{
            int s, t; cin >> s >> t;
            cout << rmq.query(s, t+1) << endl;
        }
    }
    return 0;
}
```