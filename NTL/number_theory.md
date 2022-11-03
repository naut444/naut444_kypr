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

## [べき乗](https://onlinejudge.u-aizu.ac.jp/courses/library/6/NTL/1/NTL_1_B)
* $n, m$ が与えられるので、 $n^{m} \pmod{10^9 + 7}$ を出力する。
* あまりスマートでないので、struct等を用いて適当に書き直すべきかもしれない。

```
fn main(){
    input!{
        m: usize,
        n: usize,
    }
    const MOD: usize = 1000000007;
    let ans = pow(m, n, MOD);
    println!("{}", ans);
}

fn pow(m: usize, n: usize, M: usize) -> usize{
    let mut dp = vec![0; 31];
    dp[0] = m;
    for i in 1..31{
        dp[i] = (dp[i-1] * dp[i-1]) % M;
    }

    let mut ret = 1;
    for i in 0..31{
        if (n >> i) & 1 == 1{
            ret *= dp[i];
            ret %= M;
        }
    }
    ret
}
```

## [最小公倍数](https://onlinejudge.u-aizu.ac.jp/courses/library/6/NTL/1/NTL_1_C)
* $N$ 個の数 $a_1, a_2, \cdots, a_N$ の最小公倍数を求める。

* 基本
    * $a, b$ の最大公約数を $\gcd(a, b)$ 、最小公倍数を $\operatorname{lcm}(a, b)$ と表記する。
    * ここで、 $\displaystyle \operatorname{lcm}(a, b) = \frac{a \times b}{\gcd(a, b)}$ が成り立つ。
* 本問
    * 同様にして、 $N$ 個の整数に対して、 $\gcd(a_1, a_2, \cdots, a_N) = \gcd(a_1, \gcd(a_2, \gcd(a_3, \cdots \gcd(a_{N-1}, a_{N}))))$
    * $\operatorname{lcm}(a_1, a_2, \cdots, a_N) = \operatorname{lcm}(a_1, \operatorname{lcm}(a_2, \operatorname{lcm}(a_3, \cdots, \operatorname{lcm}(a_{N-1}, a_{N}))))$
    * が成り立つ。

```rust
use proconio::input;

fn main(){
    input!{
        n: usize,
        A: [usize; n],
    }

    let mut ans = A[0];
    for i in 1..n{
        ans = lcm(ans, A[i]);
    }
    println!("{}", ans);
}

fn gcd(a:usize, b:usize) -> usize{
    if b == 0{
        a
    }
    else{
        gcd(b, a%b)
    }
}

fn lcm(a:usize, b:usize) -> usize{
    a * b / gcd(a, b)
}
```
