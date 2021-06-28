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

## 2. Graph Neural Networks with Heterophily

Definition1: Homophily Ratio h
$h=\frac{\mathbf{e}^{\top} \mathbf{D e}}{\mathbf{e}^{\top} \mathbf{C} \mathbf{e}}$

Definition 2: Compatibility Matrix H
$\mathbf{H}=\left(\mathbf{Y}^{\top} \mathbf{A} \mathbf{Y}\right) \oslash\left(\mathbf{Y}^{\top} \mathbf{A} \mathbf{E}\right)$

### 2.2 Framework Design
**(S1) prior belief estimation**
The goal for the first step is to estimate per ndoe $v \in \mathcal{V}$ a prior belief $\mathbf{b}\_{v} \in \mathbb{R}^{|\mathcal{Y}|}$ of its class label $y\_{v} \in \mathcal{Y}$ from the node features $\mathbf{X}$. We can use any off-the-shelf neural network classifier as the estimator:
MLP: $\mathbf{R}^{(k)}=\sigma\left(\mathbf{R}^{(k-1)} \mathbf{W}^{(k)}\right)$
GCN-Cheby: $\mathbf{R}^{(k)}=\sigma\left(\sum_{i=0}^{2} T\_{i}(\tilde{\mathbf{L}}) \mathbf{R}^{(k-1)} \mathbf{W}\_{i}^{(k)}\right)$, $T\_{i}(\tilde{\mathbf{L}})=2 \tilde{\mathbf{L}} T\_{i-1}(\tilde{\mathbf{L}})-T\_{i-2}(\tilde{\mathbf{L}})$, $T\_{0}(\tilde{\mathbf{L}})=\mathbf{I}$ and $T\_{1}(\tilde{\mathbf{L}})=\tilde{\mathbf{L}}=-\mathbf{D}^{-\frac{1}{2}} \mathbf{A} \mathbf{D}^{-\frac{1}{2}}$

the proir belief $\mathbf{B}\_{p}$ of nodes can be given as $\mathbf{B}\_{p}=\operatorname{softmax}\left(\mathbf{R}^{(K)}\right)$ 

**(S2) compatibility-guided propagation**
