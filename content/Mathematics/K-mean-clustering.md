# K-Mean Clustering

The algorithm will categorize the items into "kk" groups or clusters of similarity. To calculate that similarity we will use the Euclidean distance as a measurement. The algorithm works as follows:

1. **Initialization**: We begin by randomly selecting k cluster centroids
2. **Assignment Step**: Each data point is assigned to the nearest centroid, forming clusters.
3. **Update Step**: After the assignment, we recalculate the centroid of each cluster by averaging the points within it.
4. **Repeat**: This process repeats until the centroids no longer change or the maximum number of iterations is reached.

## Mathematical Formulation

1. Euclidean Distance

The distance between a data point \(x\) and a centroid \(c\) is calculated as:

$$ d(x,c) = \sqrt{\sum_{i=1}^{n}(x_i-c_i)^2} $$

where:

\(x_i\) is the \(i^{th}\) feature of the data point.
\(c_i\) is the \(i^{th}\) feature of the centroid.
\(n\) is the number of features.

2. Centroid Update

After assigning all data points to clusters, each centroid is updated as the mean of all points belonging to that cluster:

$$ c_j = \frac{1}{|C_j|}\sum_{x_i \in C_j} x_i $$

where:

\(C_j\) is the set of points assigned to the \(j^{th}\) cluster.
\(|C_j|\) is the number of points in that cluster.