# 全域木

## 最小全域木
* $G = G(V, E)$ で、最小全域木の辺の重みの総和を計算する。

``` rust
use proconio::input;

struct UnionFind {
    n: usize,
    parent : Vec<usize>,
    rank : Vec<usize>,
    size : Vec<usize>,
}

fn make_uf(n: usize) -> UnionFind{
    UnionFind { n: n, parent: vec![n; n], rank: vec![0; n], size: vec![1; n]}
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

fn main(){
    input!{
        N: usize,
        M: usize,
        edge: [[usize; 3]; M],
    }
    let mut Uf = make_uf(N);

    // edge -> 重みでソートする(貪欲に辺を追加したいため)
    let mut E = vec![vec![]; M];
    for i in 0..M{
        for j in 0..3{
            E[i].push(edge[i][j]);
        }
    }

    E.sort_by(|a, b| a[2].cmp(&b[2]));

    // 辺の重みが小さいものから、連結していない -> 答えを増やして、それらを連結させる
    let mut ans = 0;
    for i in 0..M{
        let s = E[i][0];
        let t = E[i][1];
        let w = E[i][2];

        if !Uf.issame(s, t){
            ans += w;
            Uf.unite(s, t);
        }
    }
    println!("{}", ans);
}
```
