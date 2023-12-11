---
title: 1477 重み付きUnion Find
layout: default
parent: 解けなかった問題 解説
grand_parent: Atcoder (Dec. 2, 2023〜)
---

<script type="text/javascript" id="MathJax-script" async src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-chtml.js"></script>

# ABC328 F問題

矛盾が発生したらダメそう。数字間の差が重要そう。

大きいグループと小さいグループをマージするときに、小さいグループの方を大きいグループの値に合わせるとマージテクのようになって軽く動作するのでは??

でもそうなると、グループのサイズを管理する必要がある。マージの度に全要素のサイズ書き換えていたら流石に間に合わない。ルートに着目する。

こう考えていくとこの機能を持ったものは重み付きにUnion Findという呼び方がされているらしい。

この問題は2通りの解き方がある。

- まず、初めにグラフを用意し、ループを回して2つの要素が違うグループの場合は辺を張る。完成したグラフを足してBFSで距離計算する。最後にループを回して、矛盾がないかを調べる。
- 重み付きUnion Find

前者の解き方は重み付きUnion Findを知らなくても解けますね。この問題では、クエリが来た時点での情報のみを利用することから、すぐに答えを帰る必要があると思い込んでいたのですが。

実は前半のクエリは後半のクエリに影響を及ぼさないため、このような処理が可能になります。気づかなかったー。

## 解き方1 コード

```cpp
int main(){
    int n, q; cin >> n >> q;
    vector<ll> a(q), b(q), d(q);

    rep(i, q){
        ll tmp1, tmp2, tmp3; cin >> tmp1 >> tmp2 >> tmp3;
        tmp1--, tmp2--;
        a[i] = tmp1, b[i] = tmp2, d[i] = tmp3;
    }

    // グラフの作成
    UnionFind uf(n); // 通常のUnion Find
    Graph g(n);
    rep(i, q){
        if(!uf.connected(a[i], b[i])){
            g[a[i]].push_back(Edge(b[i], -d[i]));
            g[b[i]].push_back(Edge(a[i], d[i]));
        }
        uf.merge(a[i], b[i]);
    }

    // 多始点BFSで距離を計算
    vector<ll> dis(n);
    vector<bool> flag(n);
    rep(i, n){
        if(!flag[i]){
            queue<int> que;
            flag[i] = true;
            que.push(i);
            while(!que.empty()){
                int v = que.front(); que.pop();
                for(auto x : g[v]){
                    if(flag[x.to]) continue;
                    flag[x.to] = true;
                    dis[x.to] = dis[v] + x.weight;
                    que.push(x.to);
                }
            }
        }
    }

    // 解
    vector<int> ans;
    rep(i, q) if((dis[a[i]] - dis[b[i]]) == d[i]) ans.push_back(i+1);

    rep(i, ans.size()) cout << ans[i] << " ";
    cout << endl;
    return 0;
}
```

## 解き方2 コード

```cpp
class UnionFind{
public:
    UnionFind() = default;

    explicit UnionFind(size_t n) : m_parents(n), m_sizes(n, 1), m_weights(n){
        iota(m_parents.begin(), m_parents.end(), 0);
    }

    int find(int i){ // 再帰関数でルートを特定
        if(m_parents[i] == i) return i;
		int par = find(m_parents[i]);
		m_weights[i] += m_weights[m_parents[i]]; // 累積和
        return (m_parents[i] = par); // 経路圧縮
    }

    void merge(int a, int b, ll w){
        w += weight(a), w -= weight(b);
        a = find(a), b = find(b);
        if(a != b){
            if(m_sizes[a] < m_sizes[b]) swap(a, b), w = -w;
            m_sizes[a] += m_sizes[b]; // 全てのサイズを変更していると計算時間が増大するので、ルートの情報のみを書き換える
            m_weights[b] = m_weights[a] + w;
            m_parents[b] = a; // ルートの情報を書き換える
        }
    }

    bool connected(int a, int b) { return (find(a) == find(b)); }

    int size(int i) { return m_sizes[find(i)]; }

    ll diff(int x, int y) { return weight(x) - weight(y); }

private:
    vector<int> m_parents, m_sizes;
	vector<ll> m_weights;
    ll weight(int x) { find(x); return m_weights[x]; }
};

int main(){
    int n, q; cin >> n >> q;
    vector<int> a(q), b(q);
    vector<ll> d(q);
    rep(i, q){
        int tmp1, tmp2; ll tmp3;
        cin >> tmp1 >> tmp2 >> tmp3;
        tmp1--, tmp2--;
        a[i] = tmp1, b[i] = tmp2, d[i] = tmp3;
    }

    UnionFind uf(n); // 重み付きUnion Find
    vector<int> ans;
    rep(i, q){
        if(!uf.connected(a[i], b[i])){
            uf.merge(a[i], b[i], -d[i]);
            ans.push_back(i+1);
        }
        else{
            if(uf.diff(a[i], b[i]) == d[i]) ans.push_back(i+1);
        }
    }
    
    rep(i, ans.size()) cout << ans[i] << " ";
    cout << endl;
    return 0;
}
```