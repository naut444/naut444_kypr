<script type="text/x-mathjax-config">MathJax.Hub.Config({tex2jax:{inlineMath:[['\$','\$'],['\\(','\\)']],processEscapes:true},CommonHTML: {matchFontHeight:false}});</script>
<script type="text/javascript" async src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.1/MathJax.js?config=TeX-MML-AM_CHTML"></script>

# DP(動的計画法)

## [Coin Changing Problem](https://onlinejudge.u-aizu.ac.jp/courses/library/7/DPL/1/DPL_1_A)
* $M$ 種類のコインがあり、それらの額面は $c_0, c_1, \cdots, c_{M-1}$ である。このとき、 $N$ 円支払うのに必要な最小のコインの枚数は何枚か？
* 解法
	* $\text{DP}[i][n]$ を、 $i$ 番目のコインまでを使って、 $n$ 円支払うために必要なコインの最小枚数であるとする。
	* $\text{DP}[i][n]$ を求めるには、添字の小さい以下の状態での $\text{DP}$ の値が分かればよい。
    * $\text{DP}[i-1][n]$ : $i-1$ 番目までのコインを使って $n$ 円支払う。
 		* $\text{DP}[i-1][n-c_i]$ : $i-1$ 番目までのコインを使って、 $n - c_i$ 円支払い、さらに $c_i$ 円支払って、合計 $n$ 円支払う。
	* したがって、添字の小さい方から、順に計算し、最終的に求めたい値である $\text{DP}[M-1][N]$ を求める。
  
``` rust
use proconio::input;
use std::cmp::min;

fn main(){
    input!{
        n: usize,
        m: usize,
        c: [usize; m],
    }
    // dp[i][j] : i 番目のコインまでを使って、j円ちょうど支払うときに必要なコインの最小枚数
    let mut dp = vec![vec![1000000; n+1]; m];
    dp[0][0] = 0;

    for i in 0..m{
        for j in 0..n+1{
            if i >= 1{
                dp[i][j] = dp[i-1][j];
            }

            if j >= c[i]{
                dp[i][j] = min(dp[i][j], dp[i][j-c[i]] + 1);
            }
        }
    }
    println!("{}", dp[m-1][n])
}
```

## [0-1 Knapsack Problem](https://onlinejudge.u-aizu.ac.jp/courses/library/7/DPL/1/DPL_1_B)
* 容量 $W$ のナップザックと $N$ 個の品物があり、それらの価値、重さはそれぞれ $v_i, w_i$ である。
* 容量を超えないようにしながら、価値を最大化するように品物のいくつかを選びたい。

``` rust
use proconio::input;
use std::cmp::max;

fn main(){
    input!{
        N: usize,
        W: usize,
        dat: [[usize; 2]; N],
    }

    // dp[i][j] : i 番目までで、重さがjのときの最大価値
    let mut dp = vec![vec![0; W+1]; N+1];
    dp[0][0] = 0;

    for i in 0..N{
        for j in 0..=W{
            dp[i+1][j] = max(dp[i][j], dp[i+1][j]);
            if j+dat[i][1] <= W{
                dp[i+1][j+dat[i][1]] = max(dp[i][j] + dat[i][0], dp[i+1][j+dat[i][1]]);
                }
            }
        }

    let mut ans = dp[N].iter().max().unwrap();
    println!("{}", ans);
}
```

## [Knapsack Problem](https://onlinejudge.u-aizu.ac.jp/courses/library/7/DPL/1/DPL_1_C)
* 容量 $W$ のナップザックと $N$ 種類の品物があり、それらの価値、重さはそれぞれ $v_i, w_i$ である。
* 同じ種類の品物はいくつでも選べるとき、重さの総和が $W$ を超えないように入れる方法の内、最も価値の総和が高くなるように選ぶとき、その合計の最大値を求める。

``` rust
use proconio::input;
use std::cmp::max;

fn main(){
    input!{
        N: usize,
        W: usize,
        vw: [[usize; 2]; N],
    }
    // dp[i][j] : i 番目の品物までで、重さがjの最大価値
    let mut dp = vec![vec![0; W+1]; N];
    dp[0][0] = 0;

    // 遷移の仕方(i番目の品物の重さをw,価値をvとする)
    // dp[i][j] <- dp[i][j-w] + v, dp[i-1][j-w] + v, dp[i-1][j]

    for i in 0..N{
        let val = vw[i][0];
        let weight = vw[i][1];
        for j in 0..=W{
            if j >= weight{
                dp[i][j] = max(dp[i][j - weight] + val, dp[i][j]);
            }
            if i >= 1{
                dp[i][j] = max(dp[i-1][j], dp[i][j]);
                if j  >= weight{
                    dp[i][j] = max(dp[i][j], dp[i-1][j - weight] + val);
                }
            }
        }
    }
    let mut ans = dp[N-1].iter().max().unwrap();
    println!("{}", ans);
}
```
