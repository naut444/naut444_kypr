<script type="text/x-mathjax-config">MathJax.Hub.Config({tex2jax:{inlineMath:[['\$','\$'],['\\(','\\)']],processEscapes:true},CommonHTML: {matchFontHeight:false}});</script> <script type="text/javascript" async src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.1/MathJax.js?config=TeX-MML-AM_CHTML"></script>

# UnionFind

## [互いに素な集合](https://onlinejudge.u-aizu.ac.jp/courses/library/3/DSL/1/DSL_1_A)
* $N$ 個の集合 $S_0, S_1, S_2, \cdots, S_{N-1}$ があり、それらについて、以下のような操作を行う。
  * $\text{unite}(x, y)$ : $S_x$ が含まれる集合と、 $S_y$ が含まれる集合を合併する。
  * $\text{issame}(x, y)$ : $S_x$ が含まれる集合と、 $S_y$ が含まれる集合が同じ集合に属しているかを答える。

``` rust
use proconio::input;

// Unionfindの実装
struct UnionFind {
    n: usize,
    parent : Vec<usize>,
    rank : Vec<usize>,
    size : Vec<usize>,
}

impl UnionFind{
    // 要素 x の根を求める
    fn root(&mut self, x: usize) -> usize {
        if self.parent[x] == self.n{
            return x;
        }
        else{
            self.parent[x] = self.root(self.parent[x]);
            self.parent[x]
        }
    }

    // x と y は同じ集合に含まれるか(-> bool)
    fn issame(&mut self, x: usize, y: usize) -> bool {
        self.root(x) == self.root(y)
    }

    // group x と group y の併合
    fn unite(&mut self, mut par: usize, mut child: usize){
        par = self.root(par);
        child = self.root(child);

        if par != child {
            if self.rank[par] < self.rank[child] {
                std::mem::swap(&mut par, &mut child);
            }
            self.parent[child] = par;
            
            if self.rank[par] == self.rank[child]{
                self.rank[par] += 1;
            }
            self.size[par] += self.size[child];
        }
    }
}

fn make_uf(n: usize) -> UnionFind{
    UnionFind { n: n, parent: vec![n; n], rank: vec![0; n], size: vec![1; n]}
}

// ここから本体
fn main(){
    input!{
        n: usize,
        q: usize,
        query: [[usize; 3]; q],
    }
    let mut Uf = make_uf(n);
    
    for i in 0..q{
        let t = query[i][0];
        let x = query[i][1];
        let y = query[i][2];

        if t == 0{
            Uf.unite(x, y);
        }
        else if t == 1 {
            let ans = Uf.issame(x, y);
            if ans == true{
                println!("1")
            }
            else{
                println!("0")
            }
        }
    }
}
```
