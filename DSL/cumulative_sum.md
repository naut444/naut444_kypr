<script type="text/x-mathjax-config">MathJax.Hub.Config({tex2jax:{inlineMath:[['\$','\$'],['\\(','\\)']],processEscapes:true},CommonHTML: {matchFontHeight:false}});</script>
<script type="text/javascript" async src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.1/MathJax.js?config=TeX-MML-AM_CHTML"></script>

# 累積和

## [The Maximum Number of Customers](https://onlinejudge.u-aizu.ac.jp/courses/library/3/DSL/5/DSL_5_A)
* あるレストランに、 $N$ 人の客が来店し、 $i$ 人目の客の入店時刻は $l_i$ 、店を出た時刻は $r_i$ である。最も客が多いとき、レストランには何人の客がいたか？

``` rust
use proconio::input;

fn main(){
    input!{
        N: usize,
        T: usize,
        interval: [[usize; 2]; N],
    }

    let mut dc = vec![0; T+2];
    for i in 0..N{
        dc[interval[i][0]] += 1;
        dc[interval[i][1]] -= 1;
    }

    let mut c = vec![0; T+1];
    for i in 0..=T{
        if i >= 1{
            c[i] = c[i-1] + dc[i];
        }
        else{
            c[i] = dc[i];
        }
    }

    let mut ans = 0;
    for i in 0..=T{
        if ans < c[i]{
            ans = c[i];
        }
    }
    println!("{}", ans);
}
```
