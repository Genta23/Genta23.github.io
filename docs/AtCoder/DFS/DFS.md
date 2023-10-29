---
title: 深さ優先探索
layout: default
parent: AtCoder
has_children: true 
---

# DFS
DFSは探索方法の1種である。メモ化再帰などで効率化を図れる場合はBFSより有効らしい。

ポイントは探索予定と探索済のノードを管理することである。

BFSがキューを使用するのに対して、DFSではスタックを使用する。

しかし実際にはスタックは使用せず、再帰関数で実装する。

フラグの変更タイミングをミスってTLEとかたまにやらかすので注意。

## 実装上の注意
```cpp
void dfs(int node, vector<vector<int>> graph, vector<bool> seen){
    seen[node] = true;

    for(auto x : graph[node]) if(!seen[x]) check(x, graph, seen);
}

int main(){
    vector<vector<int>> graph(n); // 有向グラフの作成
    vector<bool> seen(n); // 探索済を管理する

    dfs(start, graph, seen);

    return 0;
}
```
上記はDFSのコードだが、この状態だと実行時間が増大する可能性がある。

以下のように修正することによって問題が解決する。

```cpp
void dfs(int node, vector<vector<int>> graph, vector<bool> seen) // 修正前
void dfs(int node, vector<vector<int>> &graph, vector<bool> &seen) // 修正後
```
問題点は値渡しと参照渡しの違いである。

修正前のコードは値渡し、つまり関数を呼び出す度に数値をコピーしている。そのためオブジェクト構築のために時間が必要となり処理時間が増大する。

したがって値渡しではなく、参照渡しを使用し処理時間の増大が起きないように修正する。

### 参考にしたサイト
<a href="https://qiita.com/drken/items/4a7869c5e304883f539b" target="_blank">DFS (深さ優先探索) 超入門！ 〜 グラフ・アルゴリズムの世界への入口 〜【前編】</a>

<a href="https://qiita.com/agate-pris/items/05948b7d33f3e88b8967" target="_blank">C++ 値渡し、ポインタ渡し、参照渡しを使い分けよう</a>

### 練習問題 (BFSの練習問題として表記したものを除外しています。)

