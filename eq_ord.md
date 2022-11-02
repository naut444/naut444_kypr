<script type="text/x-mathjax-config">MathJax.Hub.Config({tex2jax:{inlineMath:[['\$','\$'],['\\(','\\)']],processEscapes:true},CommonHTML: {matchFontHeight:false}});</script>
<script type="text/javascript" async src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.1/MathJax.js?config=TeX-MML-AM_CHTML"></script>

# 同値関係、順序

## 同値関係
### 定義
集合 $A$ における関係 $\circ$ が同値関係であるとは、
  1. すべての $a \in A$ に対して、 $a \circ a$ である。
  2. $a, b \in A$ に対して、 $a \circ b$ ならば、 $b \circ a$ である。
  3. $a, b, c \in A$ に対して、 $a \circ b, b \circ c$ ならば、 $a \circ c$ である。

を満たすことをいう。それぞれ、反射律、対称律、推移律という。また、これらをあわせて、同値律という。

## 順序
### 定義
集合 $A$ における関係 $\circ$ が順序であるとは、
  1. すべての $a \in A$ に対して、 $a \circ a$ である。
  1. $a, b \in A$ に対して、 $a \circ b, b \circ a$ ならば、 $a=b$ 。
  1. $a, b, c \in A$ に対して、 $a \circ b, b \circ c$ ならば、 $a \circ c$ 。

を満たすことをいう。それぞれ、反射律、反対称律、推移律と呼ぶ。また、これらをあわせて、順序の公理と呼ぶ。

集合 $A$ に対して、順序 $\circ$ を定めることができる、つまり、任意の2つの元 $a, b \in A$ に対して、 $a \circ b$ か $b \circ a$ を定める関係を構成できるとき、順序 $\circ$ は $A$ における全順序であるという。

全順序でない順序のことを、半順序と呼ぶ。例えば、整数の集合 $Z$ において、 $a$ が $b$ で割り切れるとき、 $a \circ b$ と書くことにすると、 $4, 2$ では順序を定めることができるが、 $5, 3$ では順序を定めることができない。

また、集合 $A$ に、順序 $\circ$ をひとまとめにした組を順序集合という。 このとき、 $A$ はその台であるという。

### Rustとの関わり
例えば、 $(x_0, y_0), (x_1, y_1), \cdots, (x_{N-1}, y_{N-1})$のようなデータの対があったとき、数学の $\leq$ 等では順序を定めることができない。しかし、 $x,y$ に何らかの意味付けがされて、その組同士で比較したいという状況は生じうる。
そこで、順序を自ら構成することで、それを利用して、ソートや、最大、最小の要素の決定等を行うことができる。

* 構造体に順序を導入する
  * `#[derive(PartialEq, Eq, PartialOrd, Ord)]`のようにして、先に宣言する。(それぞれ、半同値関係、同値関係、半順序、全順序を表す。)

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

## ざっくりしたまとめ

* 半同値関係(訳としてこれが適当であるかは不明)
  * 部分的に定義される同値関係である。
* 同値関係
  * 反射律、対称律、推移律を満たす。
* 半順序
  * 部分的に定義される順序関係である。
* 順序
  * 反射律、反対称律、推移律を満たす。

