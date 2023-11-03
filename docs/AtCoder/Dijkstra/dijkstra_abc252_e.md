---
title: 1294 グラフ削減 最短経路木
layout: default
parent: ダイクストラ法
grand_parent: AtCoder
---

# ABC252 E問題
今回はこちらの問題を解いていきます。

<a href="https://atcoder.jp/contests/abc252/tasks/abc252_e" target="_blank">E - Road Reduction</a>

## 解説
ダイクストラ法を適用し、構成することができる木を出力してくださいという問題です。
最終的にN-1本の道路が残ります。

これはノード2〜Nと親を結ぶ辺に該当するため、parentの情報を利用することで保守する道路の2頂点の情報を得ることが容易にできます。

あとは2頂点によって構成される道がどのような名前か(道路の番号)を出力すれば良いです。

あらかじめmapを使用して存在する辺の2頂点のペア(無向辺なので{a, b}と{b, a})と道路の番号を結びつけておき、このmapを使用することによって情報を変換、出力することでACすることができます。

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
    int n, m; cin >> n >> m;
    Graph g(n);
    map<pair<int, int>, int> mp;
    rep(i, m){
        int a, b; ll c; cin >> a >> b >> c;
        a--, b--;
        g[a].push_back({b, c});
        g[b].push_back({a, c});
        mp[{a, b}] = i+1;
        mp[{b, a}] = i+1;
    }

    vector<ll> dist(n, infl), parent(n, -1);
    dijkstra(g, dist, parent, 0); //グラフ startからの距離 親ノード番号(ディクリメント後) start

    for(int i=1; i<n; i++){
        auto itr = mp.find({i, parent[i]});
        cout << itr->second << " ";
    }
    cout << endl;
    return 0;
}
```