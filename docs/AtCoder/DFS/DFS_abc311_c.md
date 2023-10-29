---
title: 448 ループを発見せよ!!
layout: default
parent: 深さ優先探索
grand_parent: AtCoder
---

# ABC311 C問題
今回はこちらの問題を解いていきます。

<a href="https://atcoder.jp/contests/abc311/tasks/abc311_c" target="_blank">C - Find it!</a>

## 解説
これはDFSとはちょっと違うのかな。

グラフを掘っていってループを発見するといった問題です。

1. グラフを掘っていきループが発見できたら、そのループの始点となったノードを記録しておく。setを使用し、setのサイズとループの回数が不一致となったとき、そこをノードの始点(終点)とする。
2. 記録したノードの始点から探索(解の作成)を開始し、再びノードの始点に訪問したら終了。

```cpp
int main(){
    int n; cin >> n;
    vector<int> a(n); rep(i, n) cin >> a[i], a[i]--;

    set<int> st;
    int v = 0, node, cnt = 0;
    while(1){
        st.insert(v), cnt++;
        if(st.size() != cnt) break;
        v = a[v];
    }
    
    vector<int> ans;
    node = v;
    while(1){
        ans.push_back(v);
        v = a[v];
        if(v == node) break;
    }

    cout << ans.size() << endl;
    rep(i, ans.size()) cout << ans[i]+1 << " ";
    cout << endl;

    return 0;
}
```
