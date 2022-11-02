<script type="text/x-mathjax-config">MathJax.Hub.Config({tex2jax:{inlineMath:[['\$','\$'],['\\(','\\)']],processEscapes:true},CommonHTML: {matchFontHeight:false}});</script>
<script type="text/javascript" async src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.1/MathJax.js?config=TeX-MML-AM_CHTML"></script>
# 競プロ用のスニペット置き場

* アルゴリズムや、データ構造のコードを保存して、再利用するためにまとめる。
* 基本的に、[AOJ](https://onlinejudge.u-aizu.ac.jp/courses/list)の問題を使う。ただし、入出力等の方式はAtCoderの環境に合わせてある。
* Rustの練習のために書いているので、基本的にはRustを用いるが、たまに普段遣いのPythonを使う(予定)。

## よく使うフレーズ
* [頻出](https://naut444.github.io/freq) $\cdots$ ちょっとしたメモ。ITP1, ALDS1, ITP2 程度に相当。

## ITP2(プログラミング応用)
* [二分探索](https://naut444.github.io/ITP2/binary_search) $\cdots$ 数列の二分探索等。

## DSL(データの集合とクエリ処理)
* [UnionFind](https://naut444.github.io/DSL/union_find_tree) $\cdots$ UnionFindの実装。
* [尺とり法](https://naut444.github.io/DSL/two_pointers) $\cdots$ 尺とり法の基本的な問題。
* [累積和](https://naut444.github.io/DSL/cumulative_sum) $\cdots$ 累積和の基本的な問題。

## DPL(組み合わせ最適化)
* [DP(動的計画法)](https://naut444.github.io/DPL/dp_problems) $\cdots$ 動的計画法の基本的な問題。
* [DPの応用問題](https://naut444.github.io/DPL/adv_dp) $\cdots$ 動的計画法だけでは解けないが、動的計画法を用いる問題。最長部分増加列問題など。
* [パターン](https://naut444.github.io/DPL/pattern) $\cdots$ 条件を満たす領域のうち、最大面積を求める問題。 $H \times W$ のタイルの中で条件を満たすものの中でできる正方形で最も面積の大きいものを求めるなど。

## GRL(グラフ)
* [最短距離問題](https://naut444.github.io/GRL/shortest_path) $\cdots$ 各種の最短経路問題。Dijkstra法、Bellman-Ford法、Floyd-Warshall法について。(グラフ関連は統一した書き方にしたいので、書き直す予定。)
* [全域木](https://naut444.github.io/GRL/spanning_tree) $\cdots$ 最小全域木問題等。
* [サイクル](https://naut444.github.io/GRL/cycle_detection_for_a_directed_graph) $\cdots$ 有向グラフの閉路の存在判定、トポロジカルソート。

## 数学
* Rustを書く上で必要になる数学のトピックのざっくりしたまとめ。ちまちま書き足す。


## 工事中
