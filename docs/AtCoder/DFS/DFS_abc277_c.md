---
title: 540 圧縮座標 グラフDFS
layout: default
parent: 深さ優先探索
grand_parent: AtCoder
---

# ABC277 C問題
今回はこちらの問題を解いていきます。

<a href="https://atcoder.jp/contests/abc277/tasks/abc277_c" target="_blank">C - Ladder Takahashi</a>

## 解説
複数のはしごを使用して1階から行ける最高の階数を求めよという問題ですね。

BFSかDFSを使用すると簡単に解けると思います。

1. 階数が最大で10の9乗なので、探索フラグをそのまま管理するとメモリ不足になるため圧縮座標を使用する必要があります。
2. 圧縮座標の使用さえ問題なければ、シンプルなDFSの問題になります。

以下のコードはstackを使用してものになります。

```cpp
bool flag[400007];

int main(){
    int n; cin >> n;
    vector<int> a(n), b(n); rep(i, n) cin >> a[i] >> b[i];

    /* 圧縮座標を作成する temp:準備するためのset comp:圧縮 decomp:解凍 */
    set<int> temp; rep(i, n) temp.insert(a[i]), temp.insert(b[i]);
    vector<int> decomp;
    map<int, int> comp;
    int cnt = 0;
    for(int x : temp){
        comp[x] = cnt;
        decomp.push_back(x);
        cnt++;
    }

    /* 圧縮座標でのグラフ作成 */
    Graph g(2*n);
    rep(i, n){
        int ca = comp[a[i]], cb = comp[b[i]];
        g[ca].push_back(cb), g[cb].push_back(ca);
    }

    //初期地点から移動できない場合を除く
    if(decomp[0] != 1){ cout << 1 << endl; return 0; }

    /* 深さ優先探索 */
    stack<int> st;
    st.push(0);
    flag[0] = true;
    int max = -1;
    while(!st.empty()){
        int v = st.top();
        st.pop();
        for(int x : g[v]){
            if(!flag[x]){
                st.push(x);
                flag[x] = true;
                chmax(max, x);
            }
        }
    }

    cout << decomp[max] << endl;
    return 0;
}
```

以下は再帰関数を使用したものです。

```cpp
bool flag[400007];
int ans_max = -1;

void dfs(Graph &g, int v){ 
    for(int x : g[v]) if(!flag[x]){
        flag[x] = true;
        chmax(ans_max, x);
        dfs(g, x); 
    }
}

int main(){
    /*
      ここまで全く同じ
    */

    dfs(g, 0);
    
    cout << decomp[ans_max] << endl;
    return 0;
}
```

