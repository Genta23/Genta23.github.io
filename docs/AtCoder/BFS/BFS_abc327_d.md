---
title: 709 他始点 二部グラフ
layout: default
parent: 幅優先探索
grand_parent: AtCoder
---

# ABC327 D問題
今回はこちらの問題を解いていきます。

<a href="https://atcoder.jp/contests/abc327/tasks/abc327_d" target="_blank">D - Good Tuple Problem</a>

## 解説
今回の問題では必ずしもすべての頂点が連結なわけではありません。(これを考慮し忘れてWAしました)

この問題では連結グラフが複数存在するため、そのグラフがすべて二部グラフかということを判定すれば良いです。

これは多始点BFSで行うことが可能です。

頂点を順に見ていき探索済みでなかったらそこを0として探索を開始します。隣接に遷移したときに始点からの距離をmod 2で記録します。
偶奇に関して矛盾が発生したとき、条件を満たせません。

多始点なので最悪時間計算量がすごいことになるかもと警戒するかもしれませんが、flagを使用し探索済みの頂点を管理することで、通常のBFSと同じ時間計算量になります。

```cpp
using Graph = vector<vector<int>>;

int main(){
    int n, m; cin >> n >> m;
    Graph g(n);
    vector<int> a(m), b(m);
    rep(i, m){
        int tmp; cin >> tmp;
        tmp--;
        a[i] = tmp;
    }
    rep(i, m){
        int tmp; cin >> tmp;
        tmp--;
        b[i] = tmp;
    }
    rep(i, m){
        g[a[i]].push_back(b[i]);
        g[b[i]].push_back(a[i]);
    }

    vector<int> flag(n, -1);
    rep(i, n){
        queue<int> que;
        if(flag[i] != -1) continue;
        flag[i] = 0;
        que.push(i);
        while(!que.empty()){
            int v = que.front(); que.pop();
            for(int x : g[v]){
                if(flag[x] == -1){
                    flag[x] = (flag[v] + 1) % 2;
                    que.push(x);
                }
            }
        }
    }

    rep(i, m){
        if(flag[a[i]] == flag[b[i]]){
            cout << "No" << endl;
            return 0;
        }
    }
    cout << "Yes" << endl;
    return 0;
}
```