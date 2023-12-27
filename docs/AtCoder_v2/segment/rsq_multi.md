---
title: 区間加算 区間和
layout: default
parent: segment tree
grand_parent: Atcoder (Dec. 2, 2023〜)
---

<script type="text/javascript" id="MathJax-script" async src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-chtml.js"></script>

# Range Sum Query 区間加算

この問題はBIT(Binary Indexed Tree)でも解けます。どうやらこのBITはsegment treeの機能を限定したものらしいです。

任意区間のクエリに対応可能なものが、segment treeであり、区間の始点を常に0とした時のクエリに対応可能なものがBIT。(任意区間の計算は、終点までの和 - 始点-1までの和で計算可能)

つまりBITはsegment treeの機能を制限したものと考えることができます。

BIT関する記事はこちらのサイトが非常にわかりやすいです。

<a href="https://algo-logic.info/binary-indexed-tree/" target="_blank">Binary Indexed Tree (BIT) 総まとめ！区間加算や二次元BITまで</a>

今回はsegment treeを勉強ばかりなので、segment treeを使用して解きたいと思います。

## 使用するテンプレート

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
            lazy[k] = x * (r - l); // 加算値と管理している区間の幅の積
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
```

区間更新、区間最小値問題ときに使用したテンプレートとは以下の点が異なります。

- 遅延木にデータを格納する際に、<b>加算値とノードが管理している範囲の積</b>を格納するようにしている。
- データの更新を子に伝播する際に、自身のlazyに格納されている値を半分にして伝播するようにしている。(この時lazyに入っている値は必ず偶数であることが保証される。)
- 合計値を取得するクエリの際に、使用するマスクの値は0
- 子から親に向かってデータを伝播する際に使用する関数がminではなく和

## コード

### 一点更新 区間取得

<a href="https://judge.u-aizu.ac.jp/onlinejudge/description.jsp?id=DSL_2_B&lang=jp" target="_blank">Range Sum Query (RSQ)</a>

```cpp

// 上記テンプレート

int main(){
    int n, q; cin >> n >> q;
    RMQ<int> rmq(n);

    rep(i, q){
        int c, x, y; cin >> c >> x >> y;
        if(c == 0) rmq.update(x-1, x, y);
        else cout << rmq.query(x-1, y) << endl;
    }
    return 0;
}
```

### 区間更新 一点取得

<a href="https://judge.u-aizu.ac.jp/onlinejudge/description.jsp?id=DSL_2_E&lang=ja" target="_blank">Range Add Query (RAQ)</a>

```cpp

// 上記テンプレート

int main(){
    int n, q; cin >> n >> q;
    RMQ<ll> rmq(n);

    rep(i, q){
        int c; cin >> c;
        if(c == 0){
            int s, t, x; cin >> s >> t >> x;
            s--, t--;
            rmq.update(s, t+1, x);
        }
        else{
            int s; cin >> s;
            s--;
            cout << rmq.query(s, s+1) << endl;
        }
    }
    return 0;
}
```

### 区間更新 区間取得

<a href="https://judge.u-aizu.ac.jp/onlinejudge/description.jsp?id=DSL_2_G&lang=ja" target="_blank">RSQ and RAQ</a>

```cpp

// 上記のテンプレート

int main(){
    int n, q; cin >> n >> q;
    RMQ<ll> rmq(n);

    rep(i, q){
        int c; cin >> c;
        if(c == 0){
            int s, t, x; cin >> s >> t >> x;
            s--, t--;
            rmq.update(s, t+1, x);
        }
        else{
            int s, t; cin >> s >> t;
            s--, t--;
            cout << rmq.query(s, t+1) << endl;
        }
    }
    return 0;
}
```

## 注意点

### 0-besed vs 1-based

このテンプレは0-basedを想定して作成しています。問題によっては1-basedのものがあるので、気をつけてください。

テンプレでは7個の要素に関して計算する時は、8の要素が存在するとして計算を行っていました。(2の累乗かつ7以上の整数の最小値を使用するため)

そのため使用しないノードが存在しても問題はないため、更新(加算)、取得の両方のクエリが、1-basedの場合は影響が出ないのでは??と思いましたが、ダメでした。

理由は8個の要素を使用する際に0を使用しないノードとして1から8までのノードを作成。そのため、計9個のノードが必要となりますが、n=8としてsegment treeを作成し、8番目の要素を格納する場所がなくなるためです。

どうしても1-basedを使用したい場合は、以下のような解決策があります。

```cpp
// RMQ<int> rmq(n);
RMQ<int> rmq(n+1); // 使用しない0番目の分 1個追加する

/* または */

// while (n_ > x) x *= 2;
while (n_ >= x) x *= 2; // 8個のときは、16個のtreeを生成する
```

### int, long long

一点更新系の問題は、intで十分ですが、区間更新系の問題はintではオーバーフローするため、long longを使用する必要があります。

区間更新、一点取得の問題で制約的に解がオーバーフローするはずがないのにも関わらず、オーバーフローを引き起こしました。

理由はsegment treeでは区間合計値を算出してるので、そこが壊れたと予想。

しかし、解を算出するときに重要な部分は葉であり、この部分は制約的に壊れないことから、それ以外の要因があるのでは??

結論としてはlazyの部分がオーバーフローすると正確な値が伝播されずに、lazyからの汚染によってdataを格納しているsegment treeの葉も壊れる。

制約に注意しよう。