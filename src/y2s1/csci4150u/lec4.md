# OLAP

# On-Line Analytical Processing (OLAP)

+ Use of multidimensional array.
  - Some data analysis & exploration operations are easier.

## Creating a Multidimensional Array (General Procedure)

+ Identify which attributes are to be the dimensions.
+ Identify which attribute is to be the target (class).
+ **Dimensions must have discrete values**.
  - Discretize.
+ Values of the target variable appear as entries in the array.
+ The target value is typically a count or continuous value.
+ Find the value of each entry by summing the values or count of the target
  attribute of all objects that have the attribute values corresponding to that
  entry.

## Data Cube

+ Key operation of OLAP is formation of a data cube.
  - n-dimensional representation of the data with all possible aggregates.
  - By all possible aggregates, we mean: selecting a proper subset of the
    dimensions and summing over all remaining dimensions.

## Slicing

+ Selecting a subset of cells by specifying a specific value for one/more
  dimensions.

## Dicing

+ Selecting a subset of cells by specifying a range of attribute values for
  dimensions.
  - Slicing grabs a specific value, dicing grabs a range of values.

## Roll-Up

+ Attributes are often hierarchical.
  - Consider that a date has a year, month, week.
+ Merge/Group an attribute. Usually by summing its values within a group.

## Drill-Down

+ Split an attribute into a finer-grained version.
