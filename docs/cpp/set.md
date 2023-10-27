---
title: set
layout: default
parent: AtCoder
---
# set

setは重複を許さない順序付き集合
```cpp
#include <set>
```
## setの使用方法
```cpp
// 宣言方法
set<int> num; //宣言のみ
set<int> num{3, 1, 3, 4}; // {1, 3, 4}のように格納される

//追加
num.insert(2);

//削除
num.erase(2); //戻り値は削除した要素の個数(0 or 1)

//要素を全て出力1 itrの型を指定するのは大変なので、autoを使用する。
for(auto itr = num.begin(); itr != num.end(); ++itr){
    cout << *itr << endl;
}

//要素を全て出力2
for (auto x : num) {
  cout << x << endl;
}

//探索(要素の有無) 二分探索なので探索時間はlog(N)
if(num.find(2) != num.end()){
    cout << *itr << endl;
}
```

### 参考にしたサイト
<a href="https://qiita.com/bestfitat/items/84b8750ba87cd2ab2633" target="_blank">c++ setとmap</a>

<a href="https://af-e.net/cpp-stl-set/" target="_blank">C++のstd::setの使い方について詳しく解説</a>