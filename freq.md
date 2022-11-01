<script type="text/x-mathjax-config">MathJax.Hub.Config({tex2jax:{inlineMath:[['\$','\$'],['\\(','\\)']],processEscapes:true},CommonHTML: {matchFontHeight:false}});</script>
<script type="text/javascript" async src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.1/MathJax.js?config=TeX-MML-AM_CHTML"></script>
# よく使うフレーズ

* 随時更新

## 数列への基本的な操作
* 最大値、最小値 $\cdots$ 数列 $A = (a_0, a_1, a_2, \cdots, a_{N-1})$ に対して、 $M = \max{(A)},m = \min{(A)}$ を求める。
``` rust
let M = A.iter().max().unwrap();
let m = A.iter().min().unwrap();
```

* 総和 $\cdots$ 数列 $A$ に対して、 $S = \displaystyle \sum_{a \in A} a$ を求める。
``` rust
let S = A.iter().sum();
```
