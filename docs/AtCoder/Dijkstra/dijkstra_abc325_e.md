---
title: 1093 1 &#8594; k &#8594; Nの最短距離
layout: default
parent: ダイクストラ法
grand_parent: AtCoder
---
<script type="text/javascript" id="MathJax-script" async src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-chtml.js"></script>

# ABC325 E問題
今回はこちらの問題を解いていきます。

<a href="https://atcoder.jp/contests/abc325/tasks/abc325_e" target="_blank">E - Our clients, please wait a moment</a>

## 解説
社用車に関する情報に対し、都市1を初期ノードとした最短距離問題を解きます。(ダイクストラを使用)

同様にして、電車に関する情報に対し、都市1を初期ノードとした最短距離問題を解きます。(ダイクストラを使用)

都市kで乗り換える場合の最小値は、都市1から都市kの最短経路 + 都市kから都市Nの最短経路となります。

さらにその中の最小値が今回の解になります。

以下コードになります。

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

int main(){
    int n, a, b, c; cin >> n >> a >> b >> c;
    Graph g1(n), g2(n);
    rep(i, n){
        rep(j, n){
            ll d; cin >> d;
            g1[i].push_back({j, d*a});
            g2[i].push_back({j, d*b + c});
        }
    }

    vector<ll> dist_s(n, infl), parent_s(n, -1);
    dijkstra(g1, dist_s, parent_s, 0);
    
    vector<ll> dist_g(n, infl), parent_g(n, -1);
    dijkstra(g2, dist_g, parent_g, n-1);

    ll min = infl;
    rep(i, n) chmin(min, dist_s[i] + dist_g[i]);

    cout << min << endl;
}
```