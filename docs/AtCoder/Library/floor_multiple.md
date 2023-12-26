---
title: 任意の整数の倍数切り捨て
layout: default
parent: ライブラリ解説(自作)
grand_parent: AtCoder
---

# mの倍数のうちk以下の整数の最大値を求める

cppは正負で剰余の出し方が異なるのでめんどくさいです。そんなときに使用しましょう！

```cpp
template<typename T> T floor_multiple(T k, T m){ return k - ((k%m+m)%m); }
```

## 使用上の注意点

k, mで型を合わせる必要があります。

```cpp
ll k = 1234;
// floor_multiple(k, 3); これはアカン
floor_multiple(k, 3LL);
```

まぁ別の解決方法もあるのですが...型厳密にした方がミスを防げるかなと

```cpp
template<typename T, typename U> T floor_multiple(T k, U m){ return k - ((k%m+m)%m); }
```

kにlong longを使用した場合はmにもlong longを使用しましょう。

## mの倍数のうちk以上の整数の最小値を求める

このテンプレートを応用することによって、mの倍数のうちk以上の整数の最小値を求めることができるようになります。

なぜなら<b>mの倍数のうちk以上の整数の最小値 = mの倍数のうちk-1以下の整数の最大値 + m</b>だからです。

```cpp
int k = 1234, m = 3;
floor_multiple(k-1, m) + m;
```