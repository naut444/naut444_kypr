<script type="text/x-mathjax-config">MathJax.Hub.Config({tex2jax:{inlineMath:[['\$','\$'],['\\(','\\)']],processEscapes:true},CommonHTML: {matchFontHeight:false}});</script>
<script type="text/javascript" async src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.1/MathJax.js?config=TeX-MML-AM_CHTML"></script>

# サイクル

## [有向グラフの閉路検査](https://onlinejudge.u-aizu.ac.jp/courses/library/5/GRL/4/GRL_4_A)
* 有向グラフ $G = G(V, E)$ に閉路が存在するかを判定する。

``` rust
use proconio::input;
use std::collections::VecDeque;

fn main(){
    input!{
        n: usize,
        m: usize,
        edges: [[usize; 2]; m],
    }

    // グラフ
    let mut graph = vec![vec![]; n];
    // 出次数
    let mut deg = vec![0; n];
    // 見た順
    let mut order = vec![];

    for i in 0..m{
        let s = edges[i][0];
        let t = edges[i][1];
        graph[t].push(s);
        deg[s] += 1;
    }

    let mut deq = VecDeque::new();

    for i in 0..n{
        if deg[i] == 0{
            deq.push_back(i);
        }
    }

    while !deq.is_empty(){
        let v = deq.pop_front().unwrap();
        order.push(v);
        for v2 in graph[v].iter(){
            deg[*v2] -= 1;
            if deg[*v2] == 0{
                deq.push_front(*v2);
            }
        }
    }
    
    let mut seen = vec![false; n];
    for o in order.iter(){
        seen[*o] = true;
    }

    let mut has_cycle = false;

    for i in 0..n{
        if seen[i] == false{
            has_cycle = true;
        }
    }

    if has_cycle == true{
        println!("1");
    }
    else{
        println!("0");
    }

}
```

## [トポロジカルソート](https://onlinejudge.u-aizu.ac.jp/courses/library/5/GRL/4/GRL_4_B)
* $G = G(V, E)$ をトポロジカルソートする。ただし、このグラフは閉路を持たない。(閉路があるかは、上の問題で判定できる。)

``` rust
use proconio::input;
use std::collections::VecDeque;

fn main(){
    input!{
        n: usize,
        m: usize,
        edges: [[usize; 2]; m],
    }

    // グラフ
    let mut graph = vec![vec![]; n];
    // 出次数
    let mut deg = vec![0; n];
    // 見た順
    let mut order = vec![];

    for i in 0..m{
        let s = edges[i][0];
        let t = edges[i][1];
        graph[t].push(s);
        deg[s] += 1;
    }

    let mut deq = VecDeque::new();

    for i in 0..n{
        if deg[i] == 0{
            deq.push_back(i);
        }
    }

    while !deq.is_empty(){
        let v = deq.pop_front().unwrap();
        order.push(v);
        for v2 in graph[v].iter(){
            deg[*v2] -= 1;
            if deg[*v2] == 0{
                deq.push_front(*v2);
            }
        }
    }
    
    for i in 0..n{
        println!("{}", order[n-1-i]);
    }
}
```
