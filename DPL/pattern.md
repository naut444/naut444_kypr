<script type="text/x-mathjax-config">MathJax.Hub.Config({tex2jax:{inlineMath:[['\$','\$'],['\\(','\\)']],processEscapes:true},CommonHTML: {matchFontHeight:false}});</script>
<script type="text/javascript" async src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.1/MathJax.js?config=TeX-MML-AM_CHTML"></script>

# パターン

## [Largest Square](https://onlinejudge.u-aizu.ac.jp/courses/library/7/DPL/3/DPL_3_A)
* 横 $W$ 縦 $H$ のタイルがあり、各 $1 cm^{2}$ タイルに分かれ、それぞれがきれいか汚れているかで分類する。(0: きれい, 1: 汚れている)
* きれいなタイルだけで構成できる正方形のうち、最も面積の多いものの面積を求める。

```rust
use proconio::input;

use std::cmp::max;

fn main(){
    input!{
        H: usize,
        W: usize,
        C: [[usize; W]; H],
    }

    let mut dp = vec![vec![0; W]; H];

    for i in 0..H{
        for j in 0..W{
            if C[i][j] == 1{continue}
            if i > 0 && j > 0{
                let mut d = dp[i-1][j-1];
                if d > dp[i-1][j]{
                    d = dp[i-1][j];
                }
                if d > dp[i][j-1]{
                    d = dp[i][j-1];
                }
                dp[i][j] = d + 1;
            }
            else{
                dp[i][j] = 1;
            }
        }
    }
    
    let mut ans: i32 = 0;
    for i in 0..H{
        let mut M = *dp[i].iter().max().unwrap();
        ans = max(ans, M);
    }

    println!("{}", ans.pow(2));
}
```
