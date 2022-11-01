# 尺とり法

## [The Smallest Window1](https://onlinejudge.u-aizu.ac.jp/courses/library/3/DSL/3/DSL_3_A)
* 数列$A = (a_0, a_1, \cdots, a_{N-1})$ と整数 $S$ が与えられる。要素の総和が $S$ 以上となる連続する部分列のうち、最も短いものの長さを求める。

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
