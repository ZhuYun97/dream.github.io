GNN under heterophily 

heterophily: linked nodes are likely from different classes or have dissimilar features.
Definition 1: The edge homophily ratio $h=\frac{\left|\left\{(u, v):(u, v) \in \mathcal{E} \wedge y\_{u}=y_{v}\right\}\right|}{|\mathcal{E}|}$ (intra-class edges)
Definition 2: Graphs with strong homophily have high edge homophily ratio $h \rightarrow 1$, graphs with high heterophily have small edge homophily ratio $h \rightarrow 0$

key designs:
1. ego- and neighbor-embedding separation
2. higher-order neighborhoods
3. combination of intermediate representations

H2GCN: A Framework for Networks with Homophily or Heterophily
- (S1) feature embedding
$\mathbf{r}\_{v}^{(0)}=\sigma\left(\mathbf{x}\_{v} \mathbf{W}\_{e}\right)$
- (S2) neighborhood aggregation
Following designs D1 and D2, the neighbor- hood N(v) of our framework involves two sub-neighborhoods without the egos
$\mathbf{r}\_{v}^{(k)}=$ COMBINE $\left(\right.$ AGGR $\left\{\mathbf{r}\_{u}^{(k-1)}: u \in \bar{N}\_{1}(v)\right\}$, AGGR $\left.\left\{\mathbf{r}\_{u}^{(k-1)}: u \in \bar{N}\_{2}(v)\right\}\right)$
By design D3, each node’s final representation combines all its intermediate representations:
$\mathbf{r}\_{v}^{(\text {final })}=\operatorname{COMBINE}\left(\mathbf{r}\_{v}^{(0)}, \mathbf{r}\_{v}^{(1)}, \ldots, \mathbf{r}\_{v}^{(K)}\right)$
- (S3) classification
$y\_{v}=\arg \max \left\{\operatorname{softmax}\left(\mathbf{r}\_{v}^{(\text {final })} \mathbf{W}\_{c}\right)\right\}$

