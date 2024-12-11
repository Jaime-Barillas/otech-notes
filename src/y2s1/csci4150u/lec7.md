# How to evaluate a model? - Slide 63
+ Decision tree is transparent. You can build a diagram of its decisions! aaaaa
+ metrics for performance evaluation
+ methods to get those metrics

## Metrics

### Confusion Matrix
+ Focus on predictive capability of a model
+ **Confusion Matrix** - Tabulate the predictions against the actual value.
  - **True Positive** - (A.K.A Ground truth) correctly predict a positive.
    Main diagonal of matrix.
  - **False Positive** - Incorrectly predict a positive.
  - **True Negative** - Correctly predict a negative.
  - **False Negative** - Incorrectly predict a negative. Main diagonal of
    matrix.
+ Accuracy: Sumation of main diagonal over sum of all entries:
  $\frac{TP + TN}{TP + FP + TN + FN}$

+ Precision - how many times you are right: $TP / TP + FP$
+ Recall - tradeoff w/ precision: $TP / TP + FN$
+ F-measure (F1) combination of precision & recall: see slide.
+ If the test data set is imbalanced (consider 99% accuracy, misleading slides
  has an example), accuracy is **not** a good measure.

### Cost Matrix

Cost of misclassifying.
+ Costs depend on actual application.
+ Cost of model: Element-wise product of Cost Matrix with Confusion Matrix, summed up.

## Methodology - Training

Final preformance for bellow is average of all runs.

### Holdout

+ Reserve k% for training and (100 - k)% for testing.
+ Random subsampling: repeated holdout. Randomize data set first, then split.
+ Usually, k = 90%.
+ _Do not_ mix the training subset and the testing subset.

### Cross Validation

+ Partition data into k disjoint subsets of same size.
+ k-fold: train on k-1 partitions, test on the remaining one.
+ Leav-one-out: k = n (number of instances) i.e. number of folds = number of objs, one entity per fold.
+ Larger time complexity in _evaluation_ of the model.

## Model Overfitting

decision tree: complex branching logic is signal that model may be overfitting.
generally, more complex models are prone to overfit than simpler models. for
decision trees, complexity is the number of nodes (relative to the number of features.)

**Training Error** - Error on _training set_. Not usually done since we care more about the test set.
trees with smaller numbers of nodes tend to not overfit.
Slide 86 is pretty good.

Increasing the size of training set combats overfitting. Often, we don't have
a larger dataset to use.

Reducing the number of features can help overfitting. e.g. Removing irrelevant features if any.

Detecting Overfitting: Difference between training error and test error.

## Model Selection

+ Part of training stage.
  - Training -> test -> evaluation stages. We're still in Training.
+ Performed during model building/training.
+ Purpose: Ensure the model is not overly complex.
+ How do?
  - Validation Set method.
  - Incorporate model complexity in learning/evaluation process.

### Validation Set

See slide 93

+ Divide training set into:
  - Training set: for building the model.
  - Validation set: To estimate generalization error. ~5 - 10%. Should not be same as test set.
+ Drawback: Less data available for training.

### Incorporating Model Complexity

+ AKA: regularization
+ Rationale: Occam's Razor
+ More complex models have a higher chance of overfitting.
+ Therefore, include model complexity when evaluating a model.
  - include complexity measure in error/loss calculation ("loss" term used in ML).
  - Gen.Error(Model) $=$ Train.Error(Model, Train.Data) $+ \alpha *$ Complexity(Model)

+ Pessimistic Error Estimate of a decision tree T with k leaf nodes:
$$
\text{err-gen}(T) = \text{err}(T) + \Omega \cdot \frac{k}{N_\text{train}}
$$
+ $\text{err}(T)$: error rate on all training records.
+ $\Omega$: trade-off hyper-parameter (similar to $\alpha$)
  - Relative cost of adding a _leaf_ node.
+ $k$: number of leaf nodes.
+ $N_{train}$: total number of training records

### Pre-Pruning (Early Stopping Rule)
+ Stop the algorithm before it becomes a fully-grown tree.
  - Stop if all instances belong to the same class (full purity).
  - Stop if number of instances is less than some user-specified threshold.
  - Stop if expanding the current node does not improve impurity measures.
  - Stop if estimated generalization error falss below certain threshold.

### Post-Pruning
+ After the fact simplification of a decision tree.
+ Grow tree to its entirety.
+ Subtree replacement.
  - Trim nodes bottom-up.
  - If generalization error improves after trimming, replace sub-tree by a leaf
    node.
  - Class label of leaf node is determined from majority ...
+ Subtree raising.
  - Use most frequent branch.
