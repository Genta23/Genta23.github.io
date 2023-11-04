---
title: 1158 多始点ダイクストラ
layout: default
parent: ダイクストラ法
grand_parent: AtCoder
---
<script type="text/javascript" id="MathJax-script" async src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-chtml.js"></script>

# ABC305 E問題
今回はこちらの問題を解いていきます。

<a href="https://atcoder.jp/contests/abc305/tasks/abc305_e" target="_blank">E - Art Gallery on Graph</a>

## 解説
ダイクストラと酷似した手法を使用します。

\\(K\\)個の頂点についてBFSを使用するなども考えたのですが、TLEしそうです。そこで以下のように考えます。

1. 警備員が存在するノード番号と体力のペアを優先度付きキューに挿入する
2. 体力\\(h_i\\)が最大の警備員が存在するノードを選択し、隣接ノードに\\(h_i - 1\\)の警備員が存在すると仮定して、優先度付きキューに挿入する。
3. これをキューがemptyになるまで繰り返す。
4. キューがemptyになったときに体力が0以上の警備員が存在すると考えられる頂点が「警備されている頂点」です。

## 実装
```cpp
using Graph = vector<vector<int>>;
using Pair = pair<ll, int>; // point, node

int main(){
    int n, m, k; cin >> n >> m >> k;
    Graph g(n);
    rep(i, m){
        int a, b; cin >> a >> b;
        a--, b--;
        g[a].push_back(b);
        g[b].push_back(a);
    }

    max_priority_queue<Pair> que;
    rep(i, k){
        int p; ll h; cin >> p >> h;
        p--;
        que.push({h, p});
    }

    vector<ll> hitpoint(n, -1);
    vector<int> ans;
    while(!que.empty()){
        auto [h, node] = que.top(); que.pop();
        if(hitpoint[node] >= h) continue;
        hitpoint[node] = h; // このタイミングで初めてnodeが保持する値が確定する
        ans.push_back(node);
        
        for(int x : g[node]){;
            if(hitpoint[x] < h - 1){
                que.push({h - 1, x}); // 解の候補として優先度付きキューにpush
            }
        }
    }

    sort(ans.begin(), ans.end());
    cout << ans.size() << endl;
    for(int x : ans) cout << x+1 << " ";
    cout << endl;
    return 0;
}
```

## 思ったこと
普段ダイクストラ法を用いる際は、現在の配列に格納している最短距離より最短な経路が発見された際、更新してからキューにpushしている。
そして、そのノードを探索した際に、不等号(\\( > \\))を利用して不適切なデータを弾いている。

言い換えると、同じ距離の最短経路が存在した際に、先に発見された方のみがキューに挿入される仕様になっている。
そして、ノード探索時の条件を通ったものの数 = 探索されるノードの数となる。

しかし、今回は多始点ダイクストラなので、最初からいくつかの情報がキューに存在する。したがって同じ情報がキューに存在するといった事態が起こり得る。そのためノードの探索条件を通ったものの数 \\( \geqq \\) 探索されるノードの数となる。

探索された回数を解に使用したいと考えたため、キューへの挿入時に最短距離の更新は行わずに、ノードが探索されたときに初めて最短距離が更新されるように設計した。(そしてこの更新回数 = 解の個数)

探索時に弾く条件として\\( > \\)ではなく\\( \geqq \\)を使用している。(ノード挿入条件緩和、ノード探索時条件強化)
