---
title: lazy segment tree
layout: default
parent: 考察 解説
grand_parent: Atcoder (Dec. 2, 2023〜)
---

<script type="text/javascript" id="MathJax-script" async src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-chtml.js"></script>

# lazy segment treeを理解したい

segment tree 入門 の続きです。

## lazy segment tree

segment treeでは、1個の要素の更新に対して\\(O(logN)\\)かかるため、区間の更新を行おうとすると\\(O(NlogN)\\)かかってしまいます。

そこでsegment treeにデータを一時保存する機能を持たせ、区間更新や区間加算のクエリに対して\\(O(logN)\\)で応答できるようにしたものが、lazy segment treeになります。

具体的にはsegment treeと全く同じ構造を持った完全二分木を用いて、そこにデータを一時保存します。

### 区間更新

区間更新を行うための場所を特定するために掘っていくイメージです。そのときにlazy機能を実現する完全二分木のデータを掃き出すと考えると良さそう。

<div style="width: 100%; aspect-ratio: 16/9;">
    <iframe src="https://docs.google.com/presentation/d/e/2PACX-1vR6EiSXR8sVsatOo90_3KH9IyqRNZ7NXs6FGrSTvaByVZdGF0KZJfBOALR0-FyxhYgsaU-rGPFWBuXY/embed?start=false&loop=false&delayms=1000" frameborder="0" width="80%" height="100%" allowfullscreen="true" mozallowfullscreen="true" webkitallowfullscreen="true"></iframe>
</div>

あるノードとその祖先のノードに異なるクエリに関する遅延情報が入り込む可能性は考えられます。区間加算問題の場合は累積和、区間最小値問題の場合は祖先が持っている遅延情報を優先するようになります。(祖先の遅延情報の方が最新のため)

青色で表された場所の更新が終わった後、その祖先に対し更新情報を伝播させる必要があります。

<div style="width: 100%; aspect-ratio: 16/9;">
    <iframe src="https://docs.google.com/presentation/d/e/2PACX-1vTlT-O0vhufl66tf7jW78LefVgOod25f_62L6V0i6_LnJ-cOiq_10xLYsVCt-QNodaXPFLWIMs_ttZV/embed?start=false&loop=false&delayms=1000" frameborder="0" width="80%" height="100%" allowfullscreen="true" mozallowfullscreen="true" webkitallowfullscreen="true"></iframe>
</div>

### 区間情報取得 (最小値)

最低限調べる必要があるノードまで掘っていきます。

<div style="width: 100%; aspect-ratio: 16/9;">
    <iframe src="https://docs.google.com/presentation/d/e/2PACX-1vRxTBF31LvYUZ392ngPHyiiY1nZZHt79eG-sPn7aakZ3fTHytGL9OwRBTewje79X2rGkz4PNvOdp7kB/embed?start=false&loop=false&delayms=1000" frameborder="0" width="80%" height="100%" allowfullscreen="true" mozallowfullscreen="true" webkitallowfullscreen="true"></iframe>
</div>

再起的に最小値を求めていきますが、区間外の情報にはマスクをしてINFとして扱っています。

<div style="width: 100%; aspect-ratio: 16/9;">
    <iframe src="https://docs.google.com/presentation/d/e/2PACX-1vRw1M18q9LVkl21mJSdfwnTnRaF4j7rLjmizBuAV3kUBdyFs5csNATbcXYJlYszWIe16rwEYLGBJG0D/embed?start=false&loop=false&delayms=1000" frameborder="0" width="80%" height="100%" allowfullscreen="true" mozallowfullscreen="true" webkitallowfullscreen="true"></iframe>
</div>

本来であれば区間更新によって区間内のすべての情報の更新が必要でしたが、lazyがあることによって、更新箇所を最低限に留めることができます。

必要なときに必要なだけ情報を更新するという方針に基づき、Lazy segment treeは高速に動作します。