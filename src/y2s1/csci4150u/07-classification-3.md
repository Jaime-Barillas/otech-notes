# Support Vector Machines

**linear hyperplane** - A decision boundary (linear, a line).
**margin** - Distance to the closest points on both sides of the hyperplane.
**support vector** - Data point on the margin.

+ Find a linear hyperplane that will separate the data.
+ Essentially a function that returns 1 or -1:
$$
f(\vec{x}) =
\begin{cases}
1 &\text{if } \vec{w} \cdot \vec{x} + b \ge 1 \\
-1 &\text{if } \vec{w} \cdot \vec{x} + b \le -1
\end{cases}
$$
+ The margins are at $f(\vec{x}) = 1$ and $f(\vec{x}) = -1$.
+ $\vec{w}$ is normal to the hyperplane.
+ Margin width (across both sides of the line) is $\frac{2}{||\vec{w}||}$.

Fitting the model:
+ Should maximize the margin width.
  - Equivalent to minimizing $L(\vec{w}) = \frac{||\vec{w}||^2}{2}$.
  - Such that if $x$ belongs to class $y$, the model returns 1 otherwise -1.
    * Or vice versa.
+ Finding the hyperplane is a **constrained optimization** problem.

What if the problem is not linearly separable?
+ Introduce slack variables.
+ Involves hyper parameter $k$, usually set to 1 or 2.

Summary:
+ The learning problem is formulated as a convex optimization problem.
+ Robust to noise.
  - Overfitting is handled by maximizing the margin of the decision boundadry.
+ Can handle irrelevant and redundant better than many other techniques.
+ Possible to handle instances which are not linearly separable.

# Ensemble Techniques

+ Construct a _set_ of classifiers.
+ Predict the class label of test records by combining the predictions of the
  multiple classifiers.
