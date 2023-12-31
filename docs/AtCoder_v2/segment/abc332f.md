---
title: 一次関数と遅延セグメント木
layout: default
parent: segment tree
grand_parent: Atcoder (Dec. 2, 2023〜)
---

<script type="text/javascript" id="MathJax-script" async src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-chtml.js"></script>

# ABC332 F問題

最終的な期待値を求める問題です。区間に対する更新を行う必要があるため、遅延セグメント木を使用しました。

主な方針としては、公式解答にある通りです。

## 評価方法

今までは、最小値だったり合計だったり評価を行いやすいものを遅延セグメント木に載せていたのですが、今回は評価の方法が少し難しく苦戦しました。

まず、評価式を整理しておきましょう。

区間\\([L, R]\\)を\\(X\\)に変更するとき、期待値は以下のように表現することができる。

\\[E_i = \frac{p-1}{p}E_i + \frac{1}{p}X\,\,\, (p = R - L + 1)\\]

未知数と\\(t\\)として、定数(既知の数)を\\(a, b\\)で表現してみます。

\\[t= at + b\\]

この変更の要求が複数回くるのが今回の問題です。

\\[t_1 = a_1t_0 + b_1\\]
\\[t_2 = a_2t_1 + b_2\\]

これを\\(t_0, t_2\\)の式にすると、

\\[t_2 = a_2(a_1t_0 + b_1) + b_2 = a_2a_1t_0 + (a_2b_1 + b_2)\\]

3本の式を合成してみると...

\\[t_3 = a_3a_2a_1t_0 + (a_3a_2b_1 + a_3b_2 + b_3)\\]

これを連続的に行なっていくことで、初期値\\(t_0\\)に対して\\(a't_0 + b'\\)という演算により解を求めれそう。

したがって、\\(a', b'\\)が分かればよく、これを知るための漸化式を考える。

更新前の状態を\\(a_{k+1}, b_{k+1}\\)、更新後の状態を\\(a_{k}, b_{k}\\)、更新の時のクエリ内容を\\(a_q, b_q\\)と表現すると以下のように表現できる。

\\[a_{k+1} = a_qa_k\\]
\\[b_{k+1} = a_qb_k + b_q\\]
\\[(a_q = \frac{p-1}{p}, b_q = \frac{1}{p}X)\\]

初期値は\\(a_0 = t_0, b_0 = 0\\)とする。

この式を実装している部分は、以下の箇所になります。

```cpp
dat[k].first = lazy[k].first * dat[k].first;
dat[k].second = lazy[k].first * dat[k].second + lazy[k].second;
```

今回の演算の単位元は\\(a側が1, b側が0\\)としました。

### コード

modint998244353を使用しています。

```cpp
using mint = modint998244353;

template <typename T>
class RMQ {
public:
    const T INF = numeric_limits<T>::max();
    int n;
    vector<T> dat, lazy;
    RMQ(int n_) : n(), dat(n_ * 4, {1, 0}), lazy(n_ * 4, {1, 0}) {
        int x = 1;
        while (n_ > x) x *= 2;
        n = x;
    }

    void update(int a, int b, T x, int k, int l, int r) {
        eval(k);
        if (a <= l && r <= b) {  // 完全に内側の時
            lazy[k].first = x.first;
            lazy[k].second = x.second;
            eval(k);
        } else if (a < r && l < b) {                     // 一部区間が被る時
            update(a, b, x, k * 2 + 1, l, (l + r) / 2);  // 左の子
            update(a, b, x, k * 2 + 2, (l + r) / 2, r);  // 右の子
            //dat[k] = min(dat[k * 2 + 1], dat[k * 2 + 2]); 区間取得しないため使用しない
        }
    }
    void update(int a, int b, T x) { update(a, b, x, 0, 0, n); }

    T query_sub(int a, int b, int k, int l, int r) {
        eval(k);
        if (r <= a || b <= l) {  // 完全に外側の時
            return {-1, -1};
        } else if (a <= l && r <= b) {  // 完全に内側の時
            return {dat[k].first, dat[k].second};
        } else {  // 一部区間が被る時
            T vl = query_sub(a, b, k * 2 + 1, l, (l + r) / 2);
            T vr = query_sub(a, b, k * 2 + 2, (l + r) / 2, r);
            return (vl.first == -1 ? vr : vl);
        }
    }
    T query(int a, int b) { return query_sub(a, b, 0, 0, n); }

private:
    /* lazy eval */
    void eval(int k) {
        if (lazy[k].first == 1 && lazy[k].second == 0) return;  // 更新するものが無ければ終了
        if (k < n - 1) {             // 葉でなければ子に伝搬
            lazy[k * 2 + 1].first = lazy[k].first * lazy[k * 2 + 1].first;
            lazy[k * 2 + 1].second = lazy[k].first * lazy[k * 2 + 1].second + lazy[k].second;
            lazy[k * 2 + 2].first = lazy[k].first * lazy[k * 2 + 2].first;
            lazy[k * 2 + 2].second = lazy[k].first * lazy[k * 2 + 2].second + lazy[k].second;
        }
        // 自身を更新
        dat[k].first = lazy[k].first * dat[k].first;
        dat[k].second = lazy[k].first * dat[k].second + lazy[k].second;
        lazy[k].first = 1;
        lazy[k].second = 0;
    }
};

int main(){
    int n, m; cin >> n >> m;
    vector<int> a(n); rep(i, n) cin >> a[i];
    vector<int> l(m), r(m), x(m); rep(i, m) cin >> l[i] >> r[i] >> x[i];
    rep(i, m) l[i]--, r[i]--;

    RMQ<pair<mint, mint>> rmq(n);

    rep(i, n) rmq.update(i, i+1, {(mint)a[i], (mint)0});

    rep(i, m){
        mint p = r[i] - l[i] + 1;
        rmq.update(l[i], r[i]+1, {(p-1)/p, x[i]/p});
    }

    rep(i, n){
        auto v = rmq.query(i, i+1);
        mint ans = v.first + v.second;
        cout << ans.val() << " ";
    }
    cout << endl;
    return 0;
}
```

### 考えていたことメモ

- 疑問 : 今回クエリが特殊(区間更新、1点取得)だから、わざわざlazyを使用する必要はあるのか。

区間ごとの期待値などを求められているわけではないので、今回は区間に関する演算は全く設定していない。それでもlazyからdatに向けての処理を書いたのは、葉のデータだけは正確に構築する必要があるため。

では、クエリ区間に完全に覆われるような部分のみにデータを格納し、解を出力する際に木の葉からルートに辿ることで、演算内容を回収するという実装はどうだろうか。

これ手法は、今回の演算が可換でないことにより不可能となる。lazy segって演算順序も保持してるのか。強いなぁ。

じゃあ、演算命令と順序をセットにして、通ったルートの演算をソートしてから実行するのは??

ソートが影響で各クエリに対して、最悪\\(O(NlogN)\\)かかるし、それを各要素について計算すると\\(O(N^2logN)\\)で間に合わないか。
