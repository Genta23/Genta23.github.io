---
title: 二次元配列外参照対策
layout: default
parent: ライブラリ解説(自作)
grand_parent: AtCoder
---

# 二次元配列の配列外参照を防ぐ (自作)
二次元配列などを扱う問題において、次に探索する座標に関する情報を得たいことがあると思います。

しかし、何も考えずに実装を行うとdata[0][-1]のような探索を行ってしまい、これは配列外参照となりエラーの原因になります。

これを防ぐために、よくこのような処理を行う記述することがありました。

```cpp
// h, wをそれぞれ二次元配列の高さ、幅とする
rep(i, h){
    rep(j, w){
        if(i >= 0 && j >= 0 && i < h && j < w){
            //ここに処理を記述する
        }
    }
}
```

この条件を書くのが結構めんどくさいな〜と思ったので関数を作成してテンプレートの組み込むことにしました。

判定を行う関数は以下のようなものになります。

```cpp
template<typename T> bool check(T s, T t, T s__, T t__){
    if(s < 0) return false;
    if(t < 0) return false;
    if(s >= s__) return false;
    if(t >= t__) return false;
    return true;
}
```

簡単に説明すると、やってはいけない条件に抵触するとドボンって感じです。

これを短くしテンプレートに組み込みやすいようにしました。

```cpp
template<typename T> bool checker(T s, T t, T s__, T t__){ return ((s >= 0 && t >= 0 && s < s__ && t < t__) ? true : false); }
```

こちらの関数は先程と少し異なり、全ての条件をみたしたときにtrue、1個でも満たせない条件があればfalseを返すようになっています。