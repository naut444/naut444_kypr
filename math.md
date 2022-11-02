<script type="text/x-mathjax-config">MathJax.Hub.Config({tex2jax:{inlineMath:[['\$','\$'],['\\(','\\)']],processEscapes:true},CommonHTML: {matchFontHeight:false}});</script>
<script type="text/javascript" async src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.1/MathJax.js?config=TeX-MML-AM_CHTML"></script>

# プログラミングまわりの数学(書きかけ)

## 順序
### 定義
集合 $A$ における関係 $\circ$ が順序であるとは、
  1. すべての $a \in A$ に対して、 $a \circ a$ である。
  1. $a, b \in A$ に対して、 $a \circ b, b \circ a$ ならば、 $a=b$ 。
  1. $a, b, c \in A$ に対して、 $a \circ b, b \circ c$ ならば、 $a \circ c$ 。

を満たすことをいう。それぞれ、反射律、反対称律、推移律と呼ぶ。また、これらを合わせて、順序の公理と呼ぶ。

集合 $A$ に対して、順序 $\circ$ を定めることができる、つまり、任意の2つの元 $a, b \in A$ に対して、 $a \circ b$ か $b \circ a$ を定める関係を構成できるとき、順序 $\circ$ は $A$ における全順序であるという。
全順序でない順序のことを、半順序と呼ぶ。例えば、 $Z$ において、 $a$ が $b$ で割り切れるとき、 $a \circ b$ と書くことにすると、 $5, 3$ は順序を定めることができない。

また、集合 $A$ に、順序 $\circ$ をひとまとめにした組を順序集合という。 このとき、 $A$ はその台であるという。

### Rustとの関わり
例えば、 $(x_0, y_0), (x_1, y_1), \cdots, (x_{N-1}, y_{N-1})$のようなデータの対があったとき、数学の $\leq$ 等では順序を定めることができない。しかし、 $x,y$ に何らかの意味付けがされて、その組同士で比較したいという状況は生じうる。
そこで、順序を自ら構成することで、それを利用して、ソートや、最大、最小の要素の決定等を行うことができる。

* 構造体に順序を導入する
  * `#[derive(PartialEq, Eq, PartialOrd, Ord)]`のようにして、先に宣言する。(それぞれ、部分同値関係、同値関係、半順序、全順序を表す。)

```rust
#[derive(Eq, Ord)]
struct Pos{
  x: usize,
  y: usize,
}

impl Ord for Pos{
  fn cmp(&self, other: &self) -> Ordering{
    self.x.cmp(&other.x)
  }
}

...
```
