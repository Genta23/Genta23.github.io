---
title: next gene
layout: default
has_children: true
---
<script type="text/javascript" id="MathJax-script" async
src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-chtml.js"></script>
# ARC 167
初めてARCに参加してみた。

## A問題
この問題を読んで少し考えたとき、アンバランス度を最小にするためにはなるべく均等な高さになるようにパンを置く必要があるだろうなと考えた。

そこで頭のなかでイメージした図が以下のようなものである。

<div class="image-box"><img src="/src/image/arc167_a1.png" alt="考えたイメージ" width="60%"></div>

大きい値を持ったものから皿に置いていき、皿が全て埋まったら折り返すイメージである。
    
細かい証明は公式の解答を参照しよう。

<a href="https://atcoder.jp/contests/arc167/editorial/7391" target="_blank"></a>

## B問題

### B問題 解法編
解法を思いついていたが、\\(B = 10^{18}\\)という制約の前に完敗。

解説を読んで自分の言葉に落とし込んだものをまとめてみる。

\\(A\\) の約数の個数や \\(A^B\\) の約数の個数までは自分も求めることができていた。

問題なのは、この説明の部分である。

\\(X\\) の正の約数の相乗平均は \\(\sqrt{X}\\) です。(\\(X\\) 正の約数の集合を \\(S\\) として、写像 \\(f:S→S; \:f(a)= X/a\\) が全単射であるため)

この説明をもう少し噛み砕いて説明します。

まずは前半の部分、「\\(X\\) の正の約数の相乗平均は \\(\sqrt{X}\\) です。」について。

相乗平均はいつも使用している平均(相加平均)の積バージョンです。

具体例を考えてみましょう。

- 12の約数は1, 2, 3, 4, 6, 12で総積は\\(12^3\\)になります。つまり、相乗平均は\\(\sqrt{12}\\)です。
- 18の約数は1, 2, 3, 6, 9, 18で総積は\\(18^3\\)になります。つまり、相乗平均は\\(\sqrt{18}\\)です。
- 9の約数は1, 3, 9で総積は\\(9^{\frac{3}{2}}\\)になります。つまり、相乗平均は3です。

後半の部分は、理由を簡潔に説明しています。

- 12の約数で説明すると、(1, 12), (2, 6), (3, 4)のように \\((a, X/a)\\) が1対1で対応するため、約数の総積を取った時は \\(a_1 \cdot a_2 \cdot a_3 \cdot \: \cdot\cdot\cdot \: \cdot \frac{X}{a_3} \cdot \frac{X}{a_2} \cdot \frac{X}{a_1} = \sqrt{X} \: \times \\) (約数の個数)と考えることができるとものである。
  9の約数の場合は、少し例外的に考える必要があるが、それでも \\(\sqrt{X} \: \times \\) (約数の個数) は満たす。

したがって、ある数 \\(A\\) があったときに、相乗平均は \\(\sqrt{A}\\) で、その個数が \\(\prod^e_1(e_i + 1)\\) なので、総積は \\(A^{\frac{\prod^e_1(e_i + 1)}{2}}\\)と考えることができる。

今回の問題に落とし込むと、相乗平均は \\(\sqrt{A^B}\\) で、その個数が \\(\prod^e_1(e_iB + 1)\\) なので、総積は \\(A^{\frac{B\prod^e_1(e_iB + 1)}{2}}\\)と考えることができる。

問題は \\(A^{\frac{B\prod^e_1(e_iB + 1)}{2}}\\) を何回 \\(A\\) で割れるかというものなので、 \\(\frac{B\prod^e_1(e_iB + 1)}{2}\\) の整数部分を求めれば良い。

整数部分という表現なのは、値が整数にならない場合があるためである。

実際に \\(B\\) が奇数かつ \\(A\\) が平方数 (\\(= e_i\\) が全て偶数)のときには整数にならない。

では、実装を考える。

### B問題 実装編

実装では、なかなかACを出すことができなかった。理由として2で割るという処理が挙げられる。

modの世界で2で割るという行為は、少し注意が必要らしい。modの割り算に関するものはこちらがわかりやすかった。

<a href="https://www.creativ.xyz/modulo-basic/" target="_blank">競技プログラミングにおける剰余の基礎と mod 逆元</a>

よく考えてみれば、modを使用している際は偶奇の判定でさえ注意する必要があるのに、いつも通り演算してうまくいくわけがない。

ACを出している方のコードを読んでみるとかなり複雑な実装になっているものもあったが、最も簡単なものはmintクラスを使用する方法だろう。

modの計算を特に意識せずに自動的に行ってくれるクラスになっている。詳しい内容は時間があるときに理解していきたい。

mintに関しては以下の記事が大変参考になった。

<a href="https://qiita.com/uesho/items/1ee5c3e665c72c035880" target="_blank">【競プロライブラリ】mint(modint)で学ぶC++</a>

最終的にACしたコードは以下のものである。

```cpp
#include <atcoder/modint>

int main(){
    ll a, b, p = 2; cin >> a >> b;
    map<int, int> data;
    
    /* 素因数分解を行う */
    while(1){
        if(a == 1) break;
        if(p*p > a) { data[a]++; break;}
        
        if(a%p == 0){
            a = a/p;
            data[p]++;
        }
        else p++; 
    }
    
    //flag : true 分子が2で割り切れる
    bool flag = false;
    if(b%2 == 0) flag = true;
    b %= MOD;
    mint ans = b;
    for (const auto& [k, v] : data) {
        ans *= b*v + 1;
        if(v%2 == 1) flag = true;
    }
    //分子が奇数の場合は1引く必要がある modの割り算なので、切り捨てとかない
    if(!flag) ans--;
    ans = ans/2;
    cout << ans.val() << endl;
    return 0;
}
```

$$
\begin{align*}
\frac{\partial \theta}{\partial t}= \frac{\partial}{\partial z}
\left[ K(\theta) \left (\frac{\partial \psi}{\partial z} + 1 \right) \right]\
\end{align*}
$$

$$ A $$


[Just the Docs]: https://just-the-docs.github.io/just-the-docs/
[GitHub Pages]: https://docs.github.com/en/pages
[README]: https://github.com/just-the-docs/just-the-docs-template/blob/main/README.md
[Jekyll]: https://jekyllrb.com
[GitHub Pages / Actions workflow]: https://github.blog/changelog/2022-07-27-github-pages-custom-github-actions-workflows-beta/
[use this template]: https://github.com/just-the-docs/just-the-docs-template/generate
