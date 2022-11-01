<script type="text/x-mathjax-config">MathJax.Hub.Config({tex2jax:{inlineMath:[['\$','\$'],['\\(','\\)']],processEscapes:true},CommonHTML: {matchFontHeight:false}});</script>
<script type="text/javascript" async src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.1/MathJax.js?config=TeX-MML-AM_CHTML"></script>

# 応用的なDP
* 遷移のわかりにくいDPや、一工夫が必要な問題。

## [Longest Increasing Subsequence](https://onlinejudge.u-aizu.ac.jp/courses/library/7/DPL/1/DPL_1_D)
* 数列 $A = (a_0, a_1, \cdots, a_{N-1})$ で最長部分増加列の長さを求める。

* 解法のアイディア
  1. $DP[i]$ を $i$ 番目の要素が末尾の最長部分増加列の長さとすると、遷移は、 $\displaystyle DP[i] = \max_{j < i, a_j < a_i}{DP[j]} + 1$ のように書ける。
  2. つまり、自分の添字より小さいもので、かつ自分よりも小さい要素のうち、最もそこまでの最長部分増加列の長さが長いものを検索したい。ということになる。
  3. そこで、長さが $k$ となる最長部分増加列の末尾の要素を $\text{L}[k]$ とすると、 $\text{L}$ は単調増加数列になる。(更新は常に単調性を保つことから確認できる。)

```rust
use proconio::input;

fn binary_search(list: &Vec<usize>, val: usize) -> usize{
    let mut ok = list.len();
    let mut ng = 0;

    while ok - ng > 1{
        let m = (ok + ng) / 2;
        if list[m] < val{
            ng = m;
        }
        else{
            ok = m;
        }
    }
    ok
}

fn main(){
    input!{
        n: usize,
        A: [usize; n],
    }

    // dp[i] : i 番目の要素を使ってできるLISの長さ
    let mut dp = vec![0; n];
    // l[i] : 長さ i のLISの末尾の要素の値
    let inf = 1 << 31;
    let mut l = vec![inf; n+1];
    l[0] = 0;

    for i in 0..n{
        let pos = binary_search(&l, A[i]);
        l[pos] = A[i];
        dp[i] = pos;
    }

    println!("{}", dp.iter().max().unwrap());
}
```
