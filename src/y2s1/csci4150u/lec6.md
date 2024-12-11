# Overview

+ pipeline from scratch
+ different ways to build models

## Classification

class attribute: thing we want to predict given other attributes.
+ classification -> class attribute we want to predict is discrete.
  - in contrast, in regression it would be continuous.

Model training set should be seperate from the training set.
The model should generalize to unseen cases to predict attributes.

## Decision Tree Methods

There can be multiple decision trees that fit the same data.

phases of model
training(learning?)/induction phase -> training.
+ learning/making the model
inference/deduction phase -> predicting
+ testing the model

**Tree Induction Algorithm** - Attempts to generate the decision tree.
+ model comes out as a decision tree.

**Hunt's Algorithm**
+ Recursive.
+ base start (for binary class attribute): stat with always returning No
+ parenthisized number under rect in a is # of No and # of Yes. (~distribution)
+ choose next attribute (still binary)
+ check yes/no branches and look at # of Yes/No _of the class variable)
  - in b, yes branch results in _no_ wrong classifications because, of the entities
    that have home owner as yes, there are _0_ defaulted borrowers.
    * Leave this as a leaf and check another attribute for the other branch.
+ _Ideally_, the goal is to build tree nodes that have one of the # of yes/no as 0.
  - See 'd' on slide 22.
+ Stop branch when hitting perfectly pure numbers (or certain threshold maybe?)

## Dealing With Different Attribute Types

+ **Multi-Way Split** - preferred in this course (except for continuous variables).
  - Nominal
  - Ordinal
  - Continuous
+ **Binary Split** - preferred in this course for continuous variables.
  - Nominal
  - Ordinal: Preserve order
  - Continuous

## Selecting The Best Attribute To Consider Next

+ For each attribute:
  - Check class count/distribution for each attribute value.
  - Choose the attribute that has counts that most skewed(biggest difference,
    closest to having one count 0 -> closest to certainty (high level of "purity").)
+ i.e. we want to build a tree by choosing branching nodes with maximal purity.

# Lec 7

induction = training, create model from training set w/ use of learning algorithm.

Decision tree
+ Transparent - you can see how the model came up w/ the answer.
+ information theory based
+ 'purity' in leaf nodes.
+ Hunt's Algorithm
+ **Greedy approach**: nodes with purer class distribution are preferred.

## Measures of Node Impurity

+ Gini Index
+ Entropy
+ Misclassification Error

Finding the Best Split Slide
+ weighted impurity -> weighted average.

Computinhg Gini Index  for a collection of nodes slide:
$n_i$ = # of records in child node (table visualization: sum columns)
$n$ = # of records in all columns of table visualization.

## Continuous Attributes: Gini Index

For efficient computation, for each attribute:
+ Sort the attributes
+ Linear scan, updating the count matrix.
+ Choose the split position w/ the smallest Gini Index.

TODO: Split info

## Decision Tree

Advantages:
+ Inexpensive to construct.
+ Extremely fast at classifying unknown records.
+ Easy to interpret for small-sized trees.
+ Robust to noise (especially w/ overfitting avoidance methods.)

Disadvantages:
+ Space of tree is exponentially large.
+ Greedy approach don't find best tree often.
+ Does not take into account interactions between attributes.
+ Each decision boundary involves only a single attribute.
