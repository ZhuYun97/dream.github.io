# Main metrics for evaluating contrastive learning

## two key properties

1. alignment(closeness) of features from positive pairs
2. uniformity of the induced distribution of the (normalized) features on the hypersphere

在标准视觉和语言数据集上的大量实验证实了这两个指标和下游任务性能之间的强烈一致性。
directly optimizing for these two metrics leads to representations with comparable or better performance at downstream tasks than contrastive learning.

## contributions

- We propose quantifiable metrics for alignment and uniformity as two measures of representation quality, with theoretical motivations.
- We prove that the contrastive loss optimizes for align- ment and uniformity asymptotically.
- Empirically, we find strong agreement between both metrics and downstream task performance.
- Despite being simple in form, our proposed metrics, when directly optimized with no other loss, empirically lead to comparable or better performance at down- stream tasks than contrastive learning.

## 疑问
为什么要提出这两个指标，以前的有什么问题
