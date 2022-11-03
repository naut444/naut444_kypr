<script type="text/x-mathjax-config">MathJax.Hub.Config({tex2jax:{inlineMath:[['\$','\$'],['\\(','\\)']],processEscapes:true},CommonHTML: {matchFontHeight:false}});</script>
<script type="text/javascript" async src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.1/MathJax.js?config=TeX-MML-AM_CHTML"></script>

# 複合問題
* いくつかのデータ構造やアルゴリズムを組み合わせる問題。

## [The Number of Windows](https://onlinejudge.u-aizu.ac.jp/courses/library/3/DSL/3/DSL_3_C)
* 数列 $A = (a_1, a_2, \cdots, a_N)$ と、 $Q$ 個のクエリが与えられる。クエリでは、整数 $x_i$ が与えられ、各質問について、 $\displaystyle 1 \leq l \leq r \leq N, \sum_{i=l}^{r} a_i \leq x_i$ を満たす整数の組 $(l, r)$ の組の個数を求める。

* 考え方
  * まず、区間ごとに和を出しておいて、クエリに高速に答えることができたら理想だが、 $N \sim 10^{5}$ の制約から、 区間和の全列挙は $O(N^{2})$ 程度なので、あきらめる。クエリの個数は $500$ 程度なので、毎回数えると、 $O(N \times Q)$ 程度になり、それなら十分高速である。
  * $\displaystyle \sum_{i=l_i}^{r_i} a_j$ をたくさん求める必要があり、区間の更新がないときは、累積和が高速。 正数列の累積和は(狭義)単調増加数列になるので、尺とり法との相性がよい。

```rust
use proconio::input;

fn main(){
    input!{
        N: usize,
        Q: usize,
        A: [u64; N],
        X: [u64; Q],
    }

    // 累積和を求める
    let mut S = vec![0];
    for i in 0..N{
        S.push(S[i] + A[i]);
    }

    // println!("{:?}", S);

    for i in 0..Q{
        let ans = calc_ans(&S, X[i], N);
        println!("{}", ans);
    }
}

// 答えを求める関数
fn calc_ans(list: &Vec<u64>, val: u64, length: usize) -> usize{
    let mut cnt = 0;
    let mut r: usize = 0;

    for l in 0..length+1{
        while r < length && list[r+1] - list[l] <= val{
            r += 1;
        }
        cnt += r - l;
        // println!("{} {} {}", l, r, cnt);
    }
    cnt
}
```

