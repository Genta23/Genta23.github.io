---
title: 885 link-slider 到達可能数
layout: default
parent: 幅優先探索
grand_parent: AtCoder
---

# ABC311 D問題
今回はこちらの問題を解いていきます。

<a href="https://atcoder.jp/contests/abc311/tasks/abc311_d" target="_blank">D - Grid Ice Floor</a>

## 解説
停止マスに関するフラグと、通過や停止が起きるマスなのかどうかを管理するフラグの2本を用意して解きました。

```cpp
bool pass_flag[203][203];
bool st_flag[203][203];

int main(){
    int n, m; cin >> n >> m;
    vector<string> s(n); rep(i, n) cin >> s[i];
    int dy[] = {0, 1, 0, -1}, dx[] = {1, 0, -1, 0};

    queue<pair<int, int>> que;
    que.push({1, 1});
    pass_flag[1][1] = true, st_flag[1][1] = true;
    int ans = 1;
    while(!que.empty()){
        auto v = que.front(); que.pop();
        rep(i, 4){
            /* .である限り探索を続ける */
            int ny = v.first, nx = v.second;
            while(1){
                ny += dy[i], nx += dx[i];
                if(s[ny][nx] == '#') break;
                if(!pass_flag[ny][nx]){
                    pass_flag[ny][nx] = true;
                    ans++;
                }
            }
            ny -= dy[i], nx -= dx[i]; //更新を戻す
            /* 停止箇所が未探索の場合queに挿入 */
            if(!st_flag[ny][nx]){
                st_flag[ny][nx] = true;
                que.push({ny, nx});
            }
        }
    }
    cout << ans << endl;
    return 0;
}
```

