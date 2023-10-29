---
title: 193 無向グラフ 連結成分数
layout: default
parent: 幅優先探索
grand_parent: AtCoder
---

# ABC284 C問題
今回はこちらの問題を解いていきます。

<a href="https://atcoder.jp/contests/abc284/tasks/abc284_c" target="_blank">C - Count Connected Components</a>

## 解説
ノード1から順に調べていくのですが、未訪問の場合はcntをインクリメントして、探索に入ります。

```cpp
using Graph = vector<vector<int>>;
bool flag[103];

int main(){
    /* 入力 */
    int n, m; cin >> n >> m;
    Graph g(n);
    rep(i, m){
        int u, v; cin >> u >> v;
        u--, v--;
        g[u].push_back(v);
        g[v].push_back(u);
    }

    /* 幅優先探索 */
    int cnt = 0;
    rep(i, n){
        if(!flag[i]){
            cnt++;
            queue<int> que;
            flag[i] = true;
            que.push(i);
            while(!que.empty()){
                auto v = que.front();
                que.pop();
                for (int x : g[v]) {
                    if(flag[x]) continue; //探索済みのノードは調べない

                    flag[x] = true;
                    que.push(x);
                }   
            } 
        }
    }
    cout << cnt << endl;
    return 0;
}
```

Union Find 使うと速そうだなーと考えながら書いてました。

