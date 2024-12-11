# Exploration

+ Preliminary exploration of data to better understand its characteristics.
+ Motivations:
  - Help select the right tool for preprocessing or analysis.
  - Make use of human's abilities to recognize patterns.
+ Related to Exploratory Data Analysis (EDA)
  - Focus on visualization
  - Clustering and Anomaly Detection were viewed as exploratory techniques.
    * More than that in Data Mining specifically.
    
## Summary Statistics

+ Numbers that summarize properties of the data.
  - Frequency
  - Location - mean.
  - Spread - standard deviation.
+ Most summary statistics can be calculated in one/two passes.

### Frequency and Mode

+ **Frequency** - Percentage of time the value occurs.
+ **Mode** - The most frequent value.
+ Typically used with categorical data.

### Percentiles

+ More useful for continuous data.
+ p'th percentile is a value $x$ of an attribute such that p% of the seen
  values of the attribute are _less than_ $x$.
  - e.g. The 60'th percentile is the value $x$ such that 60% of all values of
    the attribute are less than $x$.

### Location: Mean and Median

+ **mean** - Average.
  - Sensitive to outliers.
+ **median** - Value at middle position.

### Spread: Range and Variance

+ **Range** - Difference between min and max value.
+ **Variance/Standard Deviation** - Most common measure of the spread of a set
  of points.
+ AAD
+ MAD
+ Interquartile range.

# Visualization

+ The conversion of data into a visual or tabular format so that the
  characteristics of the data and relationships can be analyzed.
+ One of the most powerful and appealing techniques.
  - Good for humans.
  
## Representation

+ The mapping of information to a visual format.
+ Data objects, attributes, and relationships are translated into points,
  lines, shapes, etc.

## Arrangement

+ The placement of visual elements.
+ Can make a large difference in how easy it is to understand the data.

## Selection for Visualization

+ Elimination or de-emphasis of certain objects and attributes.
+ May choose a subset of attributes.
+ Dimensionality reduction may take place.
  - Usually to 2 or 3 dimensions.
+ May choose a subset of objects to display.

## Visualization Techniques

+ **Histogram** - Shows the distribution of values of a single variable in
  buckets. Can also be 2-dimensional.
  - Can provide correlation, distribution, and frequency information too.
+ **Box Plots** - Shows distribution. Skewness determines location of "tail".
+ **Scatter Plots** - Sumarize relationship between pairs (or triples) of
  attributes. Additional attributes can determine colour, size, etc.
+ **Contour Plots** - Partition continuous attributes into regions of similar
  values. Consider elevation maps.
+ **Matrix Plots** - Usefull when objects are sorted according to class.
  Typically, attributes are normalized.
+ **Parallel Coordinates** - For highly dimensional data. Parallel lines
  crossing various axes. Ordering of attributes is important in seeing
  groupings.
+ **Star Plots** - For highly dimensional data. Pokemon stat charts.
+ **Chernoff Faces** - Attributes are associated with a characteristic of a
  face.
