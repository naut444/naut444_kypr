<script type="text/x-mathjax-config">MathJax.Hub.Config({tex2jax:{inlineMath:[['\$','\$'],['\\(','\\)']],processEscapes:true},CommonHTML: {matchFontHeight:false}});</script>
<script type="text/javascript" async src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.1/MathJax.js?config=TeX-MML-AM_CHTML"></script>

# 尺とり法

## [The Smallest Window1](https://onlinejudge.u-aizu.ac.jp/courses/library/3/DSL/3/DSL_3_A)
* 数列 $A = (a_0, a_1, \cdots, a_{N-1})$ と整数 $S$ が与えられる。要素の総和が $S$ 以上となる連続する部分列のうち、最も短いものの長さを求める。

``` rust
use proconio::input;

fn main(){
    input!{
        N: usize,
        S: usize,
        A: [usize; N],
    }
    let mut ans = 2 * N;
    let mut r = 0;
    let mut s = 0;

    for l in 0..N{
        while s < S{
            if r == N{
                break
            }

            s += A[r];
            r += 1;
            
        }

        if s >= S{
            if r - l < ans{
            ans = r - l;
            }
        }
        s -= A[l];
    }
    if ans == 2 * N{
        println!("{}", 0);
    }
    else{
        println!("{}", ans);
    }
}
```

## [The Smallest Window2](https://onlinejudge.u-aizu.ac.jp/courses/library/3/DSL/3/DSL_3_B)
* $A = (a_0, a_1, \cdots, a_{N-1})$ と、整数 $K$ が与えられる。そこで、 $1, 2, \cdots, K$ をすべて含む連続する部分列のうち、最も短いものの長さを求める。存在しないときは、0とする。

* 解法のイメージ
    * 尺とり法で、右側が条件を満たすまで動かしながら、答えを求めたい。 $1, 2, \cdots, K$ が登場した回数を管理し、それが0より大きいものの個数も管理すれば、条件を満たすかどうかが確認しやすい。また、登場した回数を毎回 $K$ まで調べて判定すると、計算量が大きくなる場合が存在しうる。

```rust
use proconio::input;
use std::cmp::min;

fn main(){
    input!{
        N: usize,
        K: usize,
        A: [usize; N],
    }
    
    let mut cnt = vec![0; K+1];

    let mut ok = 0;
    let mut ans = N + 1;

    let mut r = 0;
    if A[0] <= K{
        cnt[A[0]] += 1;
        ok += 1;
    }
    for l in 0..N{
        while ok < K && r < N - 1{
            r += 1;
            if A[r] <= K{
                cnt[A[r]] += 1;
                if cnt[A[r]] == 1{
                    ok += 1;
                }
            }
        }

        if ok == K{
            ans = min(ans, r - l + 1);
        }

        if A[l] <= K{
            cnt[A[l]] -= 1;
            if cnt[A[l]] == 0{
                ok -= 1;
            }
        }
    }

    if ans == N+1{
        println!("0");
    }
    else{
        println!("{}", ans);
    }
}
```
