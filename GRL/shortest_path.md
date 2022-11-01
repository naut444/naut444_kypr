<script type="text/x-mathjax-config">MathJax.Hub.Config({tex2jax:{inlineMath:[['\$','\$'],['\\(','\\)']],processEscapes:true},CommonHTML: {matchFontHeight:false}});</script>
<script type="text/javascript" async src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.1/MathJax.js?config=TeX-MML-AM_CHTML"></script>

# 最短経路問題

* コードをもう少し再利用可能な形に書き換えて洗練させたいので、一時的なページにする。

## [Single Source Shortest Path](https://onlinejudge.u-aizu.ac.jp/courses/library/5/GRL/1/GRL_1_A)
* 有向グラフ $G = G(V, E)$ で、ある始点 $r$ から各頂点への最短経路を求める。ただし、各辺の重みは正である。

``` rust
use proconio::input;
use std::collections::BinaryHeap;
use std::cmp::Ordering;


// 構造体
#[derive(Clone)]
struct Edge{
    start: usize,
    end: usize,
    weight: usize,
}

fn build_edge(s: usize, t: usize, w: usize) -> Edge{
    Edge {
        start: s,
        end: t,
        weight: w,
    }
}

// heapqに入れる(始点からの距離, 現在の位置)の状態の組
#[derive(Copy, Clone, Eq, PartialEq)]
struct State{
    distance: usize,
    position: usize,
}

// heapqに状態を入れるためには、状態の集合に順序を定める必要がある
impl Ord for State {
    fn cmp(&self, other: &Self) -> Ordering{
        other.distance.cmp(&self.distance).then_with(|| self.position.cmp(&other.position))
    }
}

impl PartialOrd for State {
    fn partial_cmp(&self, other: &Self) -> Option<Ordering>{
        Some(self.cmp(other))
    }
}


// Dijkstra法
fn dijkstra_algorithm(n:usize, graph: Vec<Vec<Edge>>, start: usize) -> Vec<usize>{
    let mut inf = 1 << 30;
    let mut dist = vec![inf; n];
    dist[start] = 0;
    
    let mut pq = BinaryHeap::new();
    pq.push(State{distance: 0, position: start});

    while let Some(State{distance, position}) = pq.pop(){
        for e in graph[position].iter(){
            if dist[e.end] > dist[position] + e.weight{
                dist[e.end] = dist[position] + e.weight;
                pq.push(State{distance: dist[e.end], position: e.end});
            }
        }
    }
    dist
}

fn main(){
    input!{
        N: usize,
        M: usize,
        start: usize,
        edges: [[usize; 3]; M],
    }

    let mut graph = vec![vec![]; N];
    for i in 0..M{
        let s = edges[i][0];
        let t = edges[i][1];
        let w = edges[i][2];
        graph[s].push(build_edge(s, t, w));
    }
    let ans = dijkstra_algorithm(N, graph, start);
    let inf = 1 << 30;
    let inf = inf as usize;
    for i in 0..N{
        let a = ans[i];
        if a == inf{
            println!("INF");
        }
        else{
            println!("{}", a);
        }    
    }
}
```

## [Single Source Shortest Path(Negative Edges)](https://onlinejudge.u-aizu.ac.jp/courses/library/5/GRL/1/GRL_1_B)
* 有向グラフ $G = G(V, E)$ で、ある始点 $r$ から各頂点への最短経路を求める。

``` rust
use proconio::input;

fn main(){
    input!{
        N: usize,
        M: usize,
        start: usize,
        edge: [[isize; 3]; M],
    }

    let inf = 1 << 30;
    let mut dist = vec![inf; N];
    dist[start] = 0;
    let mut has_negative_cycle = false;

    // Bellman Ford algorithm
    for i in 0..N{
        for j in 0..M{
            let s = edge[j][0];
            let s = s as usize;
            let t = edge[j][1];
            let t = t as usize;
            let w = edge[j][2];
            
            if dist[s] != inf{
                if dist[t] > dist[s] + w{
                    dist[t] = dist[s] + w;
                    if i == N-1{
                        has_negative_cycle = true;
                    }
                }
            }
        }

    }

    if has_negative_cycle == true{
        println!("NEGATIVE CYCLE");
    }
    else{
        for i in 0..N{
            if dist[i] == inf{
                println!("INF");
            }
            else{
                println!("{}", dist[i]);
            }
        }
    }
}
```

## [All Pairs Shortest Path](https://onlinejudge.u-aizu.ac.jp/courses/library/5/GRL/1/GRL_1_C)
* 有向グラフ $G = G(V, E)$ で、全点対間の最短経路を求める。

``` rust
use proconio::input;

#[derive(Clone)]
struct Edge{
    start: usize,
    end: usize,
    distance: isize,
}

fn main(){
    input!{
        N: usize,
        M: usize,
        edges: [[isize; 3]; M],
    }
    const INF: isize = 1 << 60;

    let mut g = vec![vec![]; N];
    let mut dist: Vec<Vec<isize>> = vec![vec![INF; N]; N];

    for i in 0..N{
        dist[i][i] = 0;
    }

    for i in 0..M{
        let s = edges[i][0] as usize;
        let t = edges[i][1] as usize;
        let d = edges[i][2];
        g[s].push(Edge{start: s,end: t,distance: d});
        dist[s][t] = d;
    }

    for k in 0..N{
        for i in 0..N{
            for j in 0..N{
                if dist[i][j] > dist[i][k] + dist[k][j]{
                    if (dist[i][k] == INF) | (dist[k][j] == INF){
                        dist[i][j] = INF;
                    }
                    else{
                        dist[i][j] = dist[i][k] + dist[k][j];
                    }
                }
            }
        }
    }

    let dist1 = dist.to_vec();

    for k in 0..N{
        for i in 0..N{
            for j in 0..N{
                if dist[i][j] > dist[i][k] + dist[k][j]{
                    if (dist[i][k] == INF) | (dist[k][j] == INF){
                        dist[i][j] = INF;
                    }
                    else{
                        dist[i][j] = dist[i][k] + dist[k][j];
                    }
                }
            }
        }
    }

    // 負閉路の検知
    for i in 0..N{
        for j in 0..N{
            if dist[i][j] != dist1[i][j]{
                println!("NEGATIVE CYCLE");
                return ()
            }
        }
    }

    // 答えの出力
    for i in 0..N{
        for j in 0..N{
            if dist1[i][j] >= INF{
                print!("INF");
            }
            else{
                print!("{}", dist1[i][j]);
            }
            if j != N-1{
                print!(" ");
            }
            else{
                println!("");
            }
            }
        }
}
```
