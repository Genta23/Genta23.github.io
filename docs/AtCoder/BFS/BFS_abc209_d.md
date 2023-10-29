---
title: 686 木 ノード間の距離
layout: default
parent: 幅優先探索
grand_parent: AtCoder
---

# ABC209 D問題
今回はこちらの問題を解いていきます。

<a href="https://atcoder.jp/contests/abc209/tasks/abc209_d" target="_blank">D - Collision</a>

## 解説
N個のノードに対して、N-1本の経路になってくるため、今回作成する無向グラフは木です。

適当なノードを選択し、幅優先探索によって選択したノードからの距離を計算します。(このノードをvとしておく)

木を想定していることから、ある2点間(p, q)の距離は、(p, v)の距離と(q, v)の距離の差と同義です。

(p, q)の距離が偶数なら街で出会います。奇数なら道路上で出会います。

```cpp
using Graph = vector<vector<int>>;

int main(){
    /* 入力 */
    int n, q; cin >> n >> q;
    Graph g(n);
    rep(i, n-1){
        int a, b; cin >> a >> b;
        a--, b--;
        g[a].push_back(b);
        g[b].push_back(a);
    }
    vector<pair<int, int>> query(q);
    rep(i, q){
        int c, d; cin >> c >> d;
        c--, d--;
        query[i].first = c;
        query[i].second = d;
    }

    /* 幅優先探索  */
    vector<int> d(n, inf);
    queue<int> que;
    que.push(0);
    d[0] = 0;
    while(!que.empty()){
        auto v = que.front();
        que.pop();
        for(int x : g[v]){
            if(d[x] != inf) continue;

            que.push(x);
            d[x] = d[v] + 1;
        }
    }
    rep(i, q) cout << (abs(d[query[i].first] - d[query[i].second])%2 == 0 ? "Town" : "Road") << endl;
    return 0;
}
```

今回はflagを使用せずに、選択したノードからの距離が記録されたかされてないかで判定しています。(dをinfで初期化している。)

ノードが複数回探索されることもあり得るのではと一瞬考えたが、大丈夫。だって木ですから。