---
title: bit全探索
layout: default
parent: AtCoder
---

# bit
部分集合の列挙を行いたいときに、便利なのがビット全探索である。

使用する際はbitsetのインクルードを忘れずに。
```cpp
#include <bitset>
```

## 利用例
```cpp
vector<int> data = {1, 2, 3, 4, 5};
```
という集合が存在するときにこの中からいくつかの要素を選んで、部分集合を作成したい。

部分集合の要素数が3個のこともあるし、7個のこともある。つまり、部分集合を構成する要素の数が定まらないため、多重ループなどを回すわけにもいかない。

このようなときに便利なのが、bit全探索である。例えば01011このような5ビットの二進数があったとき、1が立っている箇所の要素を取得することで、{2, 4, 5}といった部分集合を表現することが可能となる。

以下は部分集合の和を計算するコードである。
```cpp
vector<int> data = {1, 2, 3, 4, 5};

const int digits = 5; //ビットの桁数 基本的に集合の要素数
for(int i=0; i<(1<<digits); i++){ // 1<<10は1024
    // 2進数に変換 <桁数>の指定はconstである必要がある
    bitset<digits> s(i); 
    
    // 部分集合の和を計算
    int sum = 0;
    for(int j=0; j<digits; j++) sum += s[j]*data[j];
    cout << sum << endl;
}
```

## bitsetに関する注意
bitset<5>などで作成した2進数が11010のような数字の時、s[0]が表すのは0である。

stringなどと異なり、右側のイテレーターの方が小さいことに注意。

### 練習問題

<a href="https://atcoder.jp/contests/abc200/tasks/abc200_d" target="_blank">AtCoder ABC 200 D - Happy Birthday! 2</a>

<a href="https://atcoder.jp/contests/abc321/tasks/abc321_c" target="_blank">AtCoder ABC 321 C - 321-like Searcher</a>

### 参考にしたサイト

<a href="https://qiita.com/hareku/items/3d08511eab56a481c7db" target="_blank">bit全探索について簡単にまとめる</a>