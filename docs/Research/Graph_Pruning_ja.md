---
title: Graph Pruning (日本語メモ)
layout: default
parent: none
---
<script type="text/javascript" id="MathJax-script" async src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-chtml.js"></script>
## Multi-agent Pathfinding on Large Maps Using Graph Pruning : This Way or That Way?

### MAPFのアプローチ手法
-  The search-based solvers are easily able to
find solutions on large sparsely populated maps while
having trouble dealing with small densely populated maps.
-  the reduction-based solvers
are able to deal with the small densely populated maps
but are unable to find a solution for large maps even
with a small number of agents.

reduction-basedの解法は、large instancesのときに問題を抱えているため、最近の研究ではマップから不必要な頂点を除くことが提案されている。

それぞれのランダムな最短経路を用いて行われる。最短経路周辺の頂点は探索に用いられるが、それ以外は除かれる。

この論文では、元となる研究の拡張を行う。
元となる研究の4種類の枝刈り方法を説明し、その後に新しい4種類の戦略を説明する。

今回、最適化を図る指標 : メイクスパンを最小化したい。

計算量を削減するテクニックとして2個が提案されている。
1. makespanの下限を利用する。lower bound \\(H\\)は最短経路の中で最も経路が長いものを指している。
2. エージェントが確実に存在していない時間帯があることに留意する。例えば、スタートからd離れている頂点があった時、0,...,(d-1)の時間では、エージェントは確実に存在しない。同様にしてゴールからd離れた頂点でも、H-d+1,...,Hの時間では、エージェントは確実に存在しない。

### sub-graphを利用する
取れる選択肢が非常に多いことから、まず着想としてはたった1個の最短距離上に存在する頂点のみを考え、それ以外は通行不可能な障害物として扱うという方法がある。

ただ、この手法は一般的に完全性が担保されない。

### Solving Strategies
\\(SP_i\\)はエージェント\\(a_i \in A\\)の最短経路上に存在する頂点の集合を表す。最短経路長は\\(|SP_i|\\)として表現する。
これを全てのエージェントに関する集合にし、\\(SP_A = \bigcup_{a_i \in A} {SP}_i\\)と表現する。

与えられたグラフ、エージェント\\((G, A)\\)の最短経路の下限を、
\\(LB_{mks}(G, A) = max_{a_i \in A}|SP_i|\\)のように定義する。

k - restricted graphというものを考える。これは、\\(Gres^{SP_A}_k\\)のように表記する。
k - restricted graphは\\(SP_i\\)に含まれる頂点と、距離\\(k\\)以内に存在する頂点からなるグラフである。

### 今回提案されている新たな手法
最短パスの選択は前処理の過程で行われるため、高速なヒューリスティック関数が必要になる。
この理由から、それぞれのエージェントは個別に扱われ、衝突の考慮はされない。
どの頂点が\\(Gres_0\\)の初期グラフに挿入されるべきかを判断する4種類の新しい手法を提案する。

#### Single Path
original studyを同じ手法を採択する。それぞれのエージェント最短経路をランダムで選択する。
このアプローチ手法を\\(single-path\\)または\\(SP\\)と表記する。

#### All Paths.
各エージェントの全ての最短経路を採択する。
figure1では最短経路は多くあったのにも関わらず、実際に採択された経路は1個だけであった。

全てのエージェントの全ての最短経路上の頂点を発見するためには、startからgoalにかけてBFSを利用することで求めることができる。
これを\\(all-paths\\)または\\(AP\\)と表記する。

#### Random Paths
全ての可能な経路を考える代わりに、全ての経路の中からいくつか選択することが考える。
まず初めに必要な最短経路の数を考える。
これは与えられたマップとそれぞれのエージェントのスタートとゴールの位置に依存することを覚えておいてください。

今回は魔法の定数を採択する代わりに、エージェントiの全ての最短経路上の頂点集合の要素数を最短経路長で割ったものを採択する。
最短経路が1個であれば、ここの数は1が使用されるし、figure1のような状況であれば、N/2が採択される。

それか、ランダムに最短経路を採択するというアプローチもある。スタートから\\(SP_i^{All}\\)の頂点をランダムウォークする。正しい方向に。(正しい方向はBFSによって計算される)

\\(SP_A^{Rand} = \bigcup_{a_i \in A} SP_i^{All}\\)
このアプローチを\\(random-paths\\)または\\(RP\\)と表記する。

#### Distant Paths
\\(RP\\)の欠点は、選択された最短経路のプロパティに対する保障がないこと??

1個以上の最短経路を使用するという考えは、衝突を回避するために、underlying solverにマップの異なる場所の使用を通じてエージェントをナビゲートすることを許すことになる。
ところが、ランダムなパスを使用すると多くの頂点を共有するパスを生成する可能性がある。

そこで、多様で(diverse)遠い道(distant)を見つけたいと思います。

多様な道を探索するための多項式時間アルゴリズムは存在している。この場合、多様性というのは共有する頂点が最小となることを示す。

\\(SP^{All}_i\\)から徐々に\\(SP^{Dist}_i\\)のパスを構築します。それぞれのステップにおいて、現在のpathに新しいvertexを追加できるかを試します。

\\(distant-paths\\)または\\(DP\\)のように記述されます。