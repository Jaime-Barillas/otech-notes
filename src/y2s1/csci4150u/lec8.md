# Classification: Nearest Neighbor Classifier

## Nearest Neighbor Classifiers

+ check distance against records, class is same as nearest one.
+ $k$-nearest neighbor.
  - check the $k$ nearest records against the target record.
  - use class label of $k$ neighbors to choos the target's class.
    * e.g. by majority vote. (prefer odd $k$ in this case.)

for weighted vote: w ~= 'similarity'.
for choice of proximity measure matters:
- cosine similarity: _note_: the bit-vectors symbolize _presence_ of a word in the document: 1 = it's there, 0 = it's not.

_note_: quiz slide "normalization": normalization means to _standardize_ the _scale_. $\frac{x - \bar{x}}{\delta}$: x minus avg.x all over std.deviation.

_note_: KNN - basically have _no_ model => large inference time.

This is where the quiz stops.

# Naive Bayes

Why is denominator not important: We want to check if P(Yes|X) > P(No|X). ie. Only the numerator mattters. The denominator is the same and doesn't contain Y.

Estimate Probabilities from Data Slide:
+ Normal distribution: equation fixed (generalized form):
See slides:
~$P(X_i|Y_j) = \frac{1}{\sqrt{2\text{std.dev}^2_ij}}e^\frac{(x_i - m_{ij})^2}{2\text{std.dev}^2_{ij}}~

# BBN - Conditional Independence Slide
P(a, b, c, d) = P(a|c)\*P(b|c)\*P(c|d)\*P(d)


# K-Means

## Hierarchical Clustering

+ Advantage: No need to know number of clusters in advance.
+ Can 'cut' the dendrogram at the proper level.
  - Draw line horizontally
  - take groups below the line.
+ Course focuses on Agglomerative.
  - At start, assume each point is its own cluster.
+ **Proximity** - Ambiguously refer to similarity or distance.
  - similarity and distance are opposites.
+ Proximity matrix
  - proximity between _points_ on first iteration (since points are their own cluster).
  - proximity between _clusters_ on every other iteration.
    * As clusters are merged.
  - min/max measures are sensitive to outliers and noise.
+ **Always _merge_ the _closest_ cluster**.
  - Distance measures are between _clusters_.
    * ex: check proximity of every point in a cluster with the chosen measure (e.g. max, min, etc.)
    * _Then_ merge the _closest_ cluster.
+
