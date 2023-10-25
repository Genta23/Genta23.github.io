---
title: multiset
layout: default
parent: cpp
---

# multiset
mulitsetは重複を許す順序付き集合

```cpp
#include <set>
```
## multisetの使用方法
```cpp
// 宣言
multiset<int> num;
multiset<int> num{3, 1, 4, 4, 3}; // {1, 3, 3, 4, 4}のように格納される

//追加
num.insert(2);

//ある値の要素を全て削除 戻り値は削除した要素数
num.erase(2);

//要素を全て出力1
for(auto itr = num.begin(): itr != num.end(); ++itr){
  cout << *itr << endl;
}

//要素を全て出力2
for (auto x : num) {
  cout << x << endl;
}

//ある値の要素数
num.count(2);
```

### 練習問題
<a href="https://atcoder.jp/contests/abc298/tasks/abc298_c" target="_blank">AtCoder ABC 298 C - Cards Query Problem</a>

### 参考にしたサイト
<a href="https://minus9d.hatenablog.com/entry/2019/07/25/212453" target="_blank">C++のmultisetの使い方</a>