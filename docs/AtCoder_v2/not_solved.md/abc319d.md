---
title: 631 二分探索ド忘れ
layout: default
parent: 解けなかった問題 解説
grand_parent: Atcoder (Dec. 2, 2023〜)
---

<script type="text/javascript" id="MathJax-script" async src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-chtml.js"></script>

# ABC319 D問題

## 思考ログ

ある程度解の範囲を絞って、横幅を大きくしていったときに、初めて条件を満たしたときの横幅がそのまま答えになりそう。

しかし、与えたれる数字の制約が大きくて困った。数時間考えるも分からず。

## 解答

縦幅 -> 横幅の順に決めるのはやはり困難なので、横幅 -> 縦幅というように逆からアプローチしていく。

横幅を決定した時に最適な縦幅を一意に定めることができ、それが単調現象であるということを考えると、二分探索が思いつくはず。

lowerは題意の条件を満たさない集合の上限値、upperは満たす集合の下限値。

今回は全ての単語の前に空白を挿入し、1文字分ずれて計算を行っていることに注意。

## コード

```cpp
int main(){
    int n, m; cin >> n >> m;
    vector<ll> l(n); rep(i, n) {cin >> l[i]; l[i]++;}

    ll max = 0, sum = 0;
    rep(i, n){
        sum += l[i];
        chmax(max, l[i]); 
    }

    ll upper = sum;
    ll lower = max - 1;

    while(lower + 1 < upper){ // 二分探索の終了条件
        ll mid = (lower + upper) / 2;
        ll tmp = 0, cnt = 1;
        rep(i, n){
            if(tmp + l[i] > mid){
                cnt++, tmp = l[i];
            }
            else{
                tmp += l[i];
            }
        }
        if(cnt > m) lower = mid;
        else upper = mid;
    }
    cout << upper - 1 << endl;
    return 0;
}
```