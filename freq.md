<script type="text/x-mathjax-config">MathJax.Hub.Config({tex2jax:{inlineMath:[['\$','\$'],['\\(','\\)']],processEscapes:true},CommonHTML: {matchFontHeight:false}});</script>
<script type="text/javascript" async src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.1/MathJax.js?config=TeX-MML-AM_CHTML"></script>

# よく使うフレーズ

* 随時更新

## 便利なfor文
* 数列 $A$ に対して、なにかの操作を行う。Pythonっぽくも書ける。

``` rust
let A = vec![0, 1, 2, 3, 4];
for a in A{
  println!("{}", a);
}
```

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

### 少し特殊なデータ構造
* deque(両端キュー)
  * `let mut q = VecDeque::new();` で両端キューを生成する。
  * `q.push_back(x)`でxを末尾に挿入でき、`q.push_front(y)`でyを先頭に挿入できる。
  * `q.pop_back()`で末尾の要素を取り出せ、`q.pop_front()`で先頭の要素を取り出せる。

``` rust
use std::collections::VecDeque;

fn main(){
    let mut dq = VecDeque::new();

    dq.push_back(1);
    dq.push_back(2);
    println!("{:?}", dq); // [1, 2]

    dq.push_front(3);
    println!("{:?}", dq); // [3, 1, 2]

    let f = dq.pop_front().unwrap();
    println!("{}", f); // 3

    let b = dq.pop_back().unwrap();
    println!("{}", b); // 2
}
```
