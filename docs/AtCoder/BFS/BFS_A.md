---
title: 0 幅優先探索
layout: default
parent: 幅優先探索
grand_parent: AtCoder
---

# 幅優先探索 チュートリアル
幅優先探索のチュートリアルのような問題です。

<a href="https://atcoder.jp/contests/atc002/tasks/abc007_3" target="_blank">A-幅優先探索</a>

## 解説

今回の問題では探索済みのフラグを管理する代わりに、最短経路長を保存する配列を用意することで、探索済みかどうかを判定しました。

```cpp
template<typename T> bool checker(T s, T t, T s__, T t__){ return ((s >= 0 && t >= 0 && s < s__ && t < t__) ? true : false); }
int shortest_path[100][100];

int main(){
    /* 入力 */
    int r, c; cin >> r >> c;
    int sy, sx, gy, gx; cin >> sy >> sx >> gy >> gx;
    sy--, sx--, gy--, gx--;
    vector<string> data(r); rep(i, r) cin >> data[i];

    /* 経路探索の準備 */
    rep(i, r) rep(j, c) shortest_path[i][j] = inf; //最短経路を保存する
    int x[] = {1, 0, -1, 0}, y[] = {0, 1, 0, -1};

    /* 幅優先探索 */
    queue<pair<int, int>> que;
    que.push({sy, sx});
    shortest_path[sy][sx] = 0;
    while(!que.empty()){
        auto v = que.front();
        que.pop();
        int py = v.first, px = v.second;
        rep(k, 4){
            int ny = py + y[k], nx = px + x[k];
            if(checker(ny, nx, r, c) && data[ny][nx] == '.' && shortest_path[ny][nx] == inf){
                shortest_path[ny][nx] = shortest_path[py][px] + 1;
                que.push({ny, nx});
            }
        }
    }
    cout << shortest_path[gy][gx] << endl;
    return 0;
}
```

BFSでは最短経路長を計算できるということを覚えておきましょう。