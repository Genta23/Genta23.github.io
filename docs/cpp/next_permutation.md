---
title: next permutation
layout: default
parent: cpp
---

# next_permutation
順列を扱いたいときはnext_permutationを使うと良い。

```cpp
#include <algorithm>
```

## next_permutationの使用方法
```cpp
vector<int> num{1, 2, 3};
do{
    for(auto x : num){
        cout << x << " ";
    }
    cout << endl;
}while( next_permutation(num.begin(), num.end()) );

//結果
1, 2, 3
1, 3, 2
2, 1, 3
2, 3, 1
3, 1, 2
3, 2, 1
```

next_permutationは次の順列を生成し、numに格納する。

最後から最初の順列を生成したときのみfalseを返し、それ以外はtrueを返す。したがって上記のコードの場合、numの最終状態は昇順にソートされた状態である。

上記のようなコードで全列挙をしたい場合、初期状態は昇順にソートされている必要があるが、あらかじめ要素数がわかっている場合はdo-while文ではなくfor文を使用すればいいため、必ずしも初期配列である必要はない。

### 練習問題
<a href="https://atcoder.jp/contests/abc293/tasks/abc293_c" target="_blank">AtCoder ABC 293 C - Make Takahashi Happy</a>

### 参考にしたサイト
<a href="http://vivi.dyndns.org/tech/cpp/permutation" target="_blank">順列生成 next_permutation, prev_permutation 入門</a>