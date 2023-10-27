---
title: 400 8方向探索 連結成分数
layout: default
parent: 幅優先探索
grand_parent: AtCoder
---

# ABC325 C問題
今回はこちらの問題を解いていきます。

<a href="https://atcoder.jp/contests/abc325/tasks/abc325_c" target="_blank">C - Sensors</a>

## 解説
#が見つかれば、そこを起点にし幅優先探索を行います。

幅優先探索を行った回数が解になります。

```cpp
template<typename T> bool checker(T s, T t, T s__, T t__){ return ((s >= 0 && t >= 0 && s < s__ && t < t__) ? true : false); }
bool flag[1007][1007];

int main(){
    /* 入力 */
    int h, w; cin >> h >> w;
    vector<string> s(h); rep(i, h) cin >> s[i];
    
    /* 探索の準備 探索ベクトルの作成 */
    int dy[] = {0, 1, 1, 1, 0, -1, -1, -1}, dx[] = {1, 1, 0, -1, -1, -1, 0, 1};

    /* 幅優先探索 */
    int cnt = 0;
    rep(i, h){
        rep(j, w){
            if(s[i][j] == '#' && !flag[i][j]){
                cnt++;
                queue<pair<int, int>> que;
                flag[i][j] = true;
                que.push({i, j});
                while(!que.empty()){
                    auto v = que.front();
                    que.pop();
                    int cy = v.first, cx = v.second;
                    rep(k, 8){
                        int ny = cy + dy[k], nx = cx + dx[k];
                        if(checker(ny, nx, h, w) && s[ny][nx] == '#' && !flag[ny][nx]){
                            flag[ny][nx] = true;
                            que.push({ny, nx});
                        }
                    }
                }
            }
        }
    }
    cout << cnt << endl;
    return 0;
}
```

dy, dx は探索の方向ベクトルです。

cy, cx は current_y, current_x の略で現在の座標を示しています。

ny, nx は next_y, next_x の略で現在の座標を示しています。

checker は自作ライブラリで配列外参照を行わないために、確認を行っています。