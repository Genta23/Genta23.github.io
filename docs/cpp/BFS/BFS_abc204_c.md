---
title: 629 有向グラフ (s, g)の数
layout: default
parent: 幅優先探索
grand_parent: AtCoder
---

# ABC204 C問題
今回はこちらの問題を解いていきます。

<a href="https://atcoder.jp/contests/abc204/tasks/abc204_c" target="_blank">C - Tour</a>

## 解説
この問題を言い換えると、有向グラフがあったとき、スタートとゴールになりうるペアの数を数え上げよという問題です。

まずノード1を選択し、幅優先探索を使用し探索時に訪問したノード数が、ノード1から到達可能なノード数ということになります。

その後、ノード2からノードnまで同じことを繰り返すことによって、ACすることができます。

```cpp
using Graph = vector<vector<int>>;

int main(){
    /* 入力 */
    int n, m; cin >> n >> m;
    Graph g(n);
    rep(i, m){
        int a, b; cin >> a >> b;
        a--, b--;
        g[a].push_back(b);
    }

    /* 幅優先探索 */
    int cnt = 0;
    rep(i, n){
        queue<int> que;
        vector<bool> flag(n);
        que.push(i);
        flag[i] = true;
        while(!que.empty()){
            cnt++;
            auto v = que.front();
            que.pop();
            for(int x : g[v]){
                if(flag[x]) continue;

                que.push(x);
                flag[x] = true;
            }
        }
    }
    cout << cnt << endl;
    return 0;
}
```




