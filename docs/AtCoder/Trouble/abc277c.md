---
title: distanceの計算量
layout: default
parent: トラブルシューティング
grand_parent: AtCoder
---

# 座標圧縮を試みてTLEした話 (ABC277 C問題)
追記 : 座標圧縮はもっと単純なコードで実現することができます。これはそれを知らなかったときに書いたものです。

該当のコードは以下になります。

```cpp
for(auto itr = st.begin(); itr != st.end(); ++itr){
    int d = distance(st.begin(), itr);
    comp[*itr] = d;
    decomp.push_back(*itr);
}
```

stは実際に使用する大きい数値をsetに格納したもので、compで圧縮、decompで解凍を試みています。

例えば{1, 5, 3, 7, 3}のような数列があったら、setに突っ込むことで{1, 3, 5, 7}とします。

setのイテレータをbeginからendにかけて取得します。distanceを使用することで、itrとbeginの距離を求めます。

例えば、{1, 3, 5, 7}において5の要素がある場所のイテレータを取得した場合は、begin(要素1がある場所)との距離は2になります。

これを利用することで、{1, 3, 5, 7}を{0, 1, 2, 3}に圧縮しようと考えました。

decompの方は、setよりvectorの方が扱いやすいと考えたため変換しています。

## 実際にはTLE 考察

上記の実装では、制限時間に解くことができませんでした。

- setの要素をfor文によって取り出す部分はO(N)
- mapに対する挿入はO(log(N))かかる
- distanceの計算にかかる時間は知らない。O(1)だと思っている。
- vectorの末尾への挿入はO(1)で可能

自分の知識はこのような状態だったため、原因としてdistanceの計算量の部分が実はO(N)であると考えました。

## 結果 原因判明
自分の予想は正しく、distanceの時間計算量がO(N)でした。

distanceは配列やvector, map, setなど様々なコンテナに対し適用することができるのですが、実はものによって計算量が異なることが判明しました。

- 配列、vectorに使用 O(1)
- map, setに使用O(N) (Nは要素間の距離)

mapやsetは配列やvectorとは異なり、データ構造に二分木を用いているためでしょう。

同じ理由でlower_bound, upper_boundを使用する際にも注意が必要だったはずです。

今後時間があるときにイテレータに関してまとめて書きたいと思っています。

### 参考にしたサイト
<a href="https://atcoder.jp/contests/apg4b/tasks/APG4b_ai" target="_blank">AI - 4.04.イテレータ</a>