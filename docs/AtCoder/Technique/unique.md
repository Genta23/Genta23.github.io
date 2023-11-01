---
title: 重複要素の削除
layout: default
parent: 手法解説
grand_parent: AtCoder
---

# 重複要素の削除
座標圧縮を行う際、重複する要素を削除するために使用するuniqueとeraseに関する説明です。

## uniqueとは何か
uniqueは自動で重複する要素を全て削除できる!!!といった便利なものではないです。

隣り合った要素が同じ値の場合、1個にまとめるといった処理を行なっています。

```cpp
vector<int> data = {1, 2, 3, 3, 2, 2, 2, 7};

unique(data.begin(), data.end()); //出力 {1, 2, 3, 2, 7, 2, 2, 7}
```

想定の出力は{1, 2, 3, 2, 7}なのですが...実際の出力を見ると末尾付近にゴミが溜まっていると思います。

そのゴミを削除するためにeraseを使用するのですが、それについては後ほど説明します。

## uniqueの動作 計算量
まずは同じ動作をするものを自分で作成してみます。

```cpp
auto genta_unique(vector<int> &data){
    auto itr = data.begin();
    rep(i, data.size()){
        if(*itr == data[i]) continue;
        itr++;
        *itr = data[i];
    }
    return ++itr;
}
```
uniqueでは末尾付近のゴミの開始位置のイテレータを返します。

このスライドは結構わかりやすいんじゃないかなと思います。

<div style="width: 100%; aspect-ratio: 16/9;">
    <iframe src="https://docs.google.com/presentation/d/e/2PACX-1vRUrVutWmNFWW0ptMMz9Ifx-bK2TQe6Mwfekr51TGBdnuc1HYHW-Q9WtwWTf9CNOO0uAq4jiJe_Nzax/embed?start=false&loop=false&delayms=1000" frameborder="0" width="100%" height="100%" allowfullscreen="true" mozallowfullscreen="true" webkitallowfullscreen="true"></iframe>
</div>

さらにスライドから、計算量がO(N)であることと、ゴミが発生する理由が分かると思います。

## eraseの使用
ゴミが前述の溜まり方をするため、uniqueで返ってきたイテレータを使用することでvectorの末尾付近(ゴミが存在する領域)を切断することができます。

```cpp
// uniqueの戻り値をitrとする
data.erase(itr, data.end());
```

vectorは1文字の削除でさえも末尾でない限り、O(N)と非常に遅いことに注意。

そこで、M文字削除する場合の時間計算量はO(NM)になるのではと思ったが、[first, last)の連続区間をまとめて削除したい場合、erase(first, last)を使用することでO(N)で削除が行えるとのこと。

### 参考にしたサイト
<a href="http://vivi.dyndns.org/tech/cpp/vector.html" target="_blank">C++ 動的配列クラス std::vector 入門</a>