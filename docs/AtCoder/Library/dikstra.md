---
title: ダイクストラ法
layout: default
parent: ライブラリ解説
grand_parent: AtCoder
---

# ダイクストラ法で最短経路問題を解く

ダイクストラ法は経路の移動コストが非負であるとき、最短経路問題を解くことができます。

## 実装

```cpp
struct Edge{ int to; ll cost; };
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

void shortestPath(vector<ll>& parent, vector<int>& ans, int goal){
    int v = goal;
    while(v != -1){
        ans.push_back(v);
        v = parent[v];
    }
    reverse(ans.begin(), ans.end());
}

int main(){
    int n, m; cin >> n >> m;
    Graph g(n);
    rep(i, m){
        int a, b, c; cin >> a >> b >> c;
        a--, b--;
        g[a].push_back({b, c});
        g[b].push_back({a, c});
    }

    vector<ll> dist(n, infl), parent(n, -1);
    dijkstra(g, dist, parent, 0); //グラフ startからの距離 親ノード番号(ディクリメント後) start

    //startからgoalまでの経路を表示する。ディクリメント後のノード番号を利用している。
    int goal = n-1;
    vector<int> ans;
    shortestPath(parent, ans, goal);

    rep(i, n) cout << dist[i] << " ";
    cout << endl;

    rep(i, ans.size()) cout << ans[i]+1 << " ";
    cout << endl;
    return 0;
}
```

### dijkstra

dijkstra関数を用いることで、startノードからのそれぞれの頂点の最短経路と、それぞれのノードの親ノードを求めることができます。

### shortestPath

shortestPath関数を用いることで、startノードからgoalノードへの最短経路を求めることができます。