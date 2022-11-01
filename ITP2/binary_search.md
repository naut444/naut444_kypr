<script type="text/x-mathjax-config">MathJax.Hub.Config({tex2jax:{inlineMath:[['\$','\$'],['\\(','\\)']],processEscapes:true},CommonHTML: {matchFontHeight:false}});</script>
<script type="text/javascript" async src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.1/MathJax.js?config=TeX-MML-AM_CHTML"></script>

# 二分探索

## [Binary Seatch](https://onlinejudge.u-aizu.ac.jp/courses/lesson/8/ITP2/6/ITP2_6_A)
* 単調増加数列 $A = (a_0, a_1, \cdots, a_{N-1})$ について、値 $k$ が存在するかを高速に判定する。

``` rust
use::proconio::input;

fn main(){
    input!{
        n: usize,
        a: [usize; n],
        q: usize,
        k: [usize; q],
    }
    for i in 0..q{
        let pos = binary_search(&a, k[i]); 
        if pos == a.len(){
            println!("{}", 0);
        }
        else{
            if a[pos] == k[i]{
                println!("{}", 1)
            }
            else{
                println!("{}", 0)
            }
        }
    }
}

fn binary_search(l: &Vec<usize>, val: usize) -> usize{
    // l の要素に val を挿入するとき、順序を保つようなindex
    let mut ok = l.len();
    let mut ng = 0;
    let mut m = 0;

    while ok - ng != 0{
        m = (ng + ok) / 2;
        if l[m] >= val {
            ok = m;
        }
        else {
            ng = m + 1;
        }
    }
    ok
}
```

## [Includes](https://onlinejudge.u-aizu.ac.jp/courses/lesson/8/ITP2/6/ITP2_6_B)
* 単調増加数列 $A, B$ について、 $B$ のすべての要素が $A$ に含まれているかを判定する。
* いろいろな解き方が考えられる問題。尺とり法、二分探索、setに入れて判定...
``` rust
use::proconio::input;

fn main(){
    input!{
        n: usize,
        a: [isize; n],
        m: usize,
        b: [isize; m],
    }

    let mut now = 0;
    for i in 0..m{
        while b[i] != a[now]{
            now += 1;

            if now == n{
                println!("0");
                return ()
            }
        }
    }
    println!("1");
}
```


