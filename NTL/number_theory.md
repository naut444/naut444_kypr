<script type="text/x-mathjax-config">MathJax.Hub.Config({tex2jax:{inlineMath:[['\$','\$'],['\\(','\\)']],processEscapes:true},CommonHTML: {matchFontHeight:false}});</script>
<script type="text/javascript" async src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.1/MathJax.js?config=TeX-MML-AM_CHTML"></script>

# (初等)整数論

## [素因数分解](https://onlinejudge.u-aizu.ac.jp/courses/library/6/NTL/1/NTL_1_A)
* 自然数 $N$ が与えられるので、それを素因数分解する。ただし、出力形式は、 $N = 126$ であれば、`126: 2 3 3 7`のようにする。

```rust
use proconio::input;

fn main(){
    input!{
        n: usize,
    }
    let mut pf = prime_factorize(n);
    pf.sort();
    print!("{}:", n);
    for p in pf{
        print!(" {}", p);
    }
    println!("");
}

// 素因数分解
fn prime_factorize(n: usize) -> Vec<usize>{
    let mut N = n;
    let mut ret = vec![];

    for i in 2..n{
        // k^2 > N となる k が素因数であるならば、k * l = N となるl(> 1)で、l^2 < Nとなるものが存在する。
        if i * i > N{
            break
        }
        if N % i == 0{
            while N % i == 0{
                ret.push(i);
                N /= i;
            }
        }
    }

    if N != 1{
        ret.push(N);
    }
    ret
}
```
