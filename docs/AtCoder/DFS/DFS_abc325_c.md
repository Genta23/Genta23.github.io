---
title: 400 8方向探索 連結成分数
layout: default
parent: 深さ優先探索
grand_parent: AtCoder
---

# ABC325 C問題
今回はこちらの問題を解いていきます。

<a href="https://atcoder.jp/contests/abc325/tasks/abc325_c" target="_blank">C - Sensors</a>

## 解説
シンプルな深さ優先探索です。8方向に探索するので、グローバルに方向ベクトルを定義しました。

```cpp
bool flag[1007][1007];
int dy[] = {0, 1, 1, 1, 0, -1, -1, -1}, dx[] = {1, 1, 0, -1, -1, -1, 0, 1};

void dfs(vector<string> &s, int y, int x, int h, int w){
    rep(i, 8){
        int ny = y + dy[i], nx = x + dx[i];
        if(checker(ny, nx, h, w) && !flag[ny][nx] && s[ny][nx] == '#'){
            flag[ny][nx] = true;
            dfs(s, ny, nx, h, w);
        }
    }
}

int main(){
    int h, w; cin >> h >> w;
    vector<string> s(h); rep(i, h) cin >> s[i];

    /* 深さ優先探索 cntで文字数を数え上げる */
    int cnt = 0;
    rep(i, h) rep(j, w) if(!flag[i][j] && s[i][j] == '#'){ 
        cnt++; 
        flag[i][j] = true; 
        dfs(s, i, j, h, w); 
    }
    
    cout << cnt << endl;
    return 0;
}
```
