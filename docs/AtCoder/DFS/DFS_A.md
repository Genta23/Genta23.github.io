---
title: 0 深さ優先探索
layout: default
parent: 深さ優先探索
grand_parent: AtCoder
---

# 深さ優先探索 チュートリアル
深さ優先探索のチュートリアルのような問題です。

<a href="https://atcoder.jp/contests/atc001/tasks/dfs_a" target="_blank">A-深さ優先探索</a>

## 解説

フラグの変更を行ってから探索を行います。(再帰関数を走らせる)

ここを逆にしてしまうと、フラグの変更が行われないまま再帰関数が走り続け、探索が終了しません。

```cpp
bool flag[503][503];

void dfs(vector<string> &c, int h, int w, int y, int x){
    int dy[] = {0, 1, 0, -1}, dx[] = {1, 0, -1, 0};
    rep(k, 4){
        int ny = y + dy[k], nx = x + dx[k];
        if(checker(ny, nx, h, w) && c[ny][nx] != '#' && !flag[ny][nx]){
            flag[ny][nx] = true;
            dfs(c, h, w, ny, nx);
        }
    }
}

int main(){
    /* 入力 */
    int h, w; cin >> h >> w;
    vector<string> c(h); rep(i, h) cin >> c[i];
    
    /* 探索の前処理 */
    pair<int, int> s, g;
    rep(i, h){
        rep(j, w){
            if(c[i][j] == 's') s.first = i, s.second = j;
            if(c[i][j] == 'g') g.first = i, g.second = j;
        }
    }

    /* 深さ優先探索 */
    flag[s.first][s.second] = true;
    dfs(c, h, w, s.first, s.second);

    /* 解答 */
    cout << (flag[g.first][g.second] ? "Yes" : "No") << endl;
    return 0;
}
```

dfs関数の引数が多くて厄介だと思ってしまいました。

stackを使用して関数を作らずに実装してみます。(業務などでは関数を使用するべきなのかもしれませんが)

```cpp
bool flag[503][503];

int main(){
    /* 入力 */
    int h, w; cin >> h >> w;
    vector<string> c(h); rep(i, h) cin >> c[i];

    /* 探索の前処理 */
    int dy[] = {0, 1, 0, -1}, dx[] = {1, 0, -1, 0};
    pair<int, int> s, g;
    rep(i, h){
        rep(j, w){
            if(c[i][j] == 's') s.first = i, s.second = j;
            if(c[i][j] == 'g') g.first = i, g.second = j;
        }
    }

    /* 深さ優先探索 */
    stack<pair<int, int>> st;
    flag[s.first][s.second] = true;
    st.push({s.first, s.second});
    while(!st.empty()){
        auto v = st.top();
        st.pop();
        int y = v.first, x = v.second;
        rep(k, 4){
            int ny = y + dy[k], nx = x + dx[k];
            if(checker(ny, nx, h, w) && c[ny][nx] != '#' && !flag[ny][nx]){
                flag[ny][nx] = true;
                st.push({ny, nx});
            }
        }
    }

    /* 解答 */
    cout << (flag[g.first][g.second] ? "Yes" : "No") << endl;
    return 0;
}
```

BFSでのqueueの代わりにstackを使用するだけで達成することができた。

関数を使用した場合より実行時間が減少した。(関数を走らせたときの引数のコピーがないからか??)

しかし、関数を使用すればあらかじめ用意しておいてペーストして完了なんてことも可能かも。