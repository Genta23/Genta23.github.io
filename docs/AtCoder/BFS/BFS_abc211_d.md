---
title: 755 無向グラフ 最短経路数
layout: default
parent: 幅優先探索
grand_parent: AtCoder
---

# ABC209 D問題
今回はこちらの問題を解いていきます。

<a href="https://atcoder.jp/contests/abc211/tasks/abc211_d" target="_blank">D - Number of Shortest paths</a>

## 解説
ノード1からの距離を利用する点が今回のポイントになります。

1 -> 2 -> 3 -> 1 -> N のような経路の遷移は起こり得ません、理由はループができている時点で最短経路とは言えないからです。

今回の問題を解くときに、なんとなくNN(ニューラルネットワーク)のグラフをイメージすると分かりやすいかもしれないです。

距離dのノードは距離d-1のノード集合から遷移が起き、距離d+1のノード集合に対して遷移します。

```cpp
using Graph = vector<vector<int>>;
const int MOD = 1000000007;

int main(){
    /* 入力 */
    int n, m; cin >> n >> m;
    Graph g(n);
    rep(i, m){
        int a, b; cin >> a >> b;
        a--, b--;
        g[a].push_back(b);
        g[b].push_back(a);
    }

    /* 距離計算の準備 */
    vector<int> dis(n, inf), cnt(n);
    dis[0] = 0, cnt[0] = 1;
    
    /* 幅優先探索 */
    queue<int> que;
    que.push(0);
    while(!que.empty()){
        auto v = que.front();
        que.pop();
        for(int x : g[v]){
            if(dis[x] <= dis[v]) continue;
            if(dis[x] == inf){
                que.push(x); //ノードがキューに入るのは1回限定
                dis[x] = dis[v] + 1;
            }
            cnt[x] = (cnt[x] + cnt[v]) % MOD;
        }
    }
    cout << cnt[n-1] << endl;
    return 0;
}
```

以下距離というと、ノード1からの最短遷移回数を指します。

ノードp -> ノードq のような遷移を考えるとき、qの距離はpより大きい必要があります。そうでない場合は continue;

もし探索していないノードがある場合はqueにpushして、距離の登録を行います。(これは1回しか行われない)

cntに関する計算がif(dis[x] == inf)の外側にある理由は、条件を満たす複数のノードから遷移を受け取るからです。
