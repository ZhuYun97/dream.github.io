# Strategies for graph sampling

## 2020 ICLR GRAPHSAINT

### Samplers

integrate several light-weight and efficient samplers

- Random node sampler
  - according to a node distribution $P(u) \propto\left\|\widetilde{\boldsymbol{A}}\_{:, u}\right\|^{2}$, we sample $\left|\mathcal{V}\_{s}\right|$ nodes from $\mathcal{V}$ randomly.
- Random edge sampler
  - we sample edges according to $p_{e} \propto \widetilde{\boldsymbol{A}}\_{v, u}+\widetilde{\boldsymbol{A}}\_{u, v}=\frac{1}{\operatorname{deg}(u)}+\frac{1}{\operatorname{deg}(v)}$
- Random walk based samplers
  - r root nodes selected uniformly at random and each walker goes h hops

For all the above samplers, we return the subgraph induced from the sampled nodes. The induction step adds more connections into the subgraph, and empirically helps improve convergence.

## 2020 arXiv GraphCrop: Subgraph Cropping for Graph Classification

### GraphCrop

GraphCrop utilizes a node-centric strategy to crop a contiguous subgraph from the original graph while maintaining its connectivity.

- Pipeline

    1. We select a node v (the initial one) uniformly at random from the set of all nodes in V;
    2. We evaluate the connectivity from the nodes in Vi to v;
    3. We select the ⌈ρ#V⌉ nodes in V with the highest con- nectivity to v and all the edges between them to form the subgraph, where ρ ∈ [0, 1] is a hyper-parameter to adjust the size of the cropped subgraph.
    4. ![pipeline](../assets/img/subgraphs/pipeline.png)
  - 补充：使用diffusion matrix去测量connectivity $\mathbf{S}=\sum\_{k=0}^{\infty} \Theta\_{k} \mathbf{T}^{k}$

- 为什么能够work？
Qualitatively, GraphCrop generates novel augmented graphs, which simulate the real-world noise of sub-structure omission, and preserves the original class labels in most cases

> 节点分类一般使用DropEdge，对于图分类而言，DropEdge不是一种好办法，因为它会打破图的结构属性。对于chemical graphs，之前研究发现相同类别的graph往往具有相同的subgraph，这暗示了图分类中存在着不必要的子结构

## Fast learning with graph convolutional networks via importance sampling

### Cluster-gcn: An ef- ficient algorithm for training deep and large graph convolutional networks

### Semi-Supervised Node Classification with Data Augmentation

## Accurate, efficient and scalable graph embedding.
