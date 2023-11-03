---
title: 1591 有向グラフ 往復経路
layout: default
parent: ダイクストラ法
grand_parent: AtCoder
---

# ABC035 D問題
今回はこちらの問題を解いていきます。

<a href="https://atcoder.jp/contests/abc035/tasks/abc035_d" target="_blank">D - トレジャーハント</a>

## 解説
動ける時間と初期ノードと目的ノードの往復にかかる時間の差が所持金を増やすことに費やせる時間になります。

まず、このようなことを考えました。

- 所持金を2箇所以上の町で稼ぐことはあるのか

この問いに対する答えは、「所持金を稼ぐ町は1箇所」になります。(簡単に証明できます)

ということは初期ノードと目的ノードを往復して余った時間と目的ノードで1分あたりに稼げる時間の積の最大値が今回の答えになります。

### 行きと帰りで経路が異なる可能性がある

今回ノード間にある道は一方通行です。ということは行きと帰りで経路が異なり、移動に必要な時間も異なります。

行きは初期ノードから目的ノードまでダイクストラすれば良いので、問題ありませんが、帰りの経路をどのように計算するべきか悩みました。

それぞれの目的ノードを初期ノードと考え、それぞれダイクストラ法を適用すると、制限時間に終わりそうにありません。

これを解決する手法は非常に簡単で、<strong>有向辺の向きを反転させたグラフをもう一つ用意すれば良い</strong>です。

## 実装

```cpp
struct Edge{ ll to, cost; };
using Graph = vector<vector<Edge>>;

// { distance, from }
using Pair = pair<ll, int>;

void dijkstra(const Graph& g, vector<ll>& distance, vector<ll>& parent, int start){
    min_priority_queue<Pair> que;
    distance[start] = 0;
    que.push({0LL, start});
    while(!que.empty()){
        auto [cand_cost, cand_pos] = que.top(); que.pop();
        if(distance[cand_pos] < cand_cost) continue;

        for(auto [to, cost] : g[cand_pos]){
            if(chmin(distance[to], cand_cost + cost)){ //値の更新が起きている
                que.push({cand_cost + cost, to});
                parent[to] = cand_pos;
            }
        }
    }
}

int main(){
    int n, m; ll t; cin >> n >> m >> t;
    vector<ll> a(n); rep(i, n) cin >> a[i];
    Graph g1(n), g2(n);
    rep(i, m){
        int a, b; ll c; cin >> a >> b >> c;
        a--, b--;
        g1[a].push_back({b, c});
        g2[b].push_back({a, c});
    }

    vector<ll> dist1(n, infl), parent1(n, -1);
    dijkstra(g1, dist1, parent1, 0);

    vector<ll> dist2(n, infl), parent2(n, -1);
    dijkstra(g2, dist2, parent2, 0);

    ll max = 0;
    rep(i, n){
        ll tmp = t - (dist1[i] + dist2[i]);
        if(tmp > 0) chmax(max, a[i]*tmp);
    }

    cout << max << endl;
    return 0;
}
```