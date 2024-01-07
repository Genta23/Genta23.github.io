---
title: floyd–warshall
layout: default
parent: 考察 解説
grand_parent: Atcoder (Dec. 2, 2023〜)
---

<script type="text/javascript" id="MathJax-script" async src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-chtml.js"></script>

# floyd–warshall

ワーシャルフロイド法のわかりやすい説明は、こちらを参考にしてください。

- <a href="https://algo-logic.info/warshall-floyd/" target="_blank">ワーシャル・フロイド法での全点対最短経路を求めるアルゴリズム</a>

名前からしてすごく難しそうと思ってたんですけど、始点\\(i\\)から終点\\(j\\)までいく時の最短経路を知りたいから全探索しようぜ！！みたいな感じでそんなに難しいアルゴリズムではありませんでした。というか授業で普通にやってました。(名前を完全に忘れていた)

\\(k = 0, 1,...,N-1\\)として順に\\(k\\)を大きくして、始点終点以外の\\(k\\)以下の頂点の使用を許可したときの最短経路を順に確定させていき、最終的に全ての頂点を使用を許した時の最短経路、真の最短経路を求めるというイメージです。

Atcoderに限って言えば...
- \\(O(N^3)\\)でも間にあう(\\(N = 400\\)程度)
- 負の辺が含まれている
- 始点終点以外の\\(k\\)以下の頂点を使用できる時の最短経路を要求される

こんな感じであれば、ワーシャルフロイドを検討してみましょう。有効に使える場面が限定的なので、すぐ気づけそうな気もします。

## 例題

この問題を解きました。

- <a href="https://atcoder.jp/contests/abc208/tasks/abc208_d" target="_blank">D - Shortest Path Queries 2</a>

```cpp
ll dis[403][403];

int main(){
    int n, m; cin >> n >> m;
    rep(i, n){ // 初期化
        rep(j, n){
            dis[i][j] = infl;
            if(i == j) dis[i][j] = 0;
        }
    }
    rep(i, m){ // 行列の作成
        int a, b; ll c; cin >> a >> b >> c;
        a--, b--;
        dis[a][b] = c;
    }

    ll ans = 0;
    rep(k, n){
        rep(i, n){
            rep(j, n){
                chmin(dis[i][j], dis[i][k] + dis[k][j]);
                ans += (dis[i][j] != infl ? dis[i][j] : 0);
            }
        }
    }
    cout << ans << endl;
    return 0;
}
```

\\(k\\)はループの最外で順に上げていきます。全ての始点終点を試してから、\\(k\\)の条件を緩和するイメージです。

ずっと三次元配列を使うものだと誤解していたのですが、実際に使用するのは二次元配列です。