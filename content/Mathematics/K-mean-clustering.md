# K-Means Clustering

K-Means is an **iterative unsupervised learning algorithm** that divides a dataset into \(k\) clusters based on similarity.

## 1. Choose the Number of Clusters

Choose the desired number of clusters \(k\).

For example:

$$
k = 3
$$

This means the algorithm will create **3 clusters**.

---

## 2. Initialize the Centroids

Select \(k\) initial centroids.

$$
c_1,c_2,\ldots,c_k
$$

These centroids can be selected randomly or using an initialization method such as **K-Means++**.

> At this stage, the centroids are fixed while we perform the first assignment.

---

## 3. Calculate Distances

For every data point \(x_i\), calculate its distance from **each centroid**.

For Euclidean distance:

$$
d(x_i,c_j)
=
\sqrt{\sum_{m=1}^{n}(x_{im}-c_{jm})^2}
$$

where:

* \(x_i\) = a data point
* \(c_j\) = the \(j^{th}\) centroid
* \(n\) = number of features

---

## 4. Assign Every Point to the Nearest Centroid

For each point, find the centroid with the smallest distance.

$$
\text{cluster}(x_i)
=
\arg\min_j d(x_i,c_j)
$$

For example:

| Point   | Distance to \(C_1\) | Distance to \(C_2\) | Assignment |
| ------- | ------------------: | ------------------: | ---------- |
| \(x_1\) |                   2 |                   7 | \(C_1\)    |
| \(x_2\) |                   4 |                   3 | \(C_2\)    |
| \(x_3\) |                   1 |                   8 | \(C_1\)    |

**Important:** All points are assigned using the **same, current centroids**. We don't change a centroid after each individual assignment.

---

## 5. Update the Centroids

After **all points have been assigned**, calculate a new centroid for each cluster.

The new centroid is the mean of all points belonging to that cluster:

$$
c_j
=
\frac{1}{|C_j|}
\sum_{x_i\in C_j}x_i
$$

where:

* \(C_j\) = set of points assigned to cluster \(j\)
* \(|C_j|\) = number of points in cluster \(j\)

Thus, the centroid **moves toward the center of its newly assigned points**.

---

## 6. Reassign All Points

Now the centroids have changed.

Therefore, we **recalculate the distance for every point**, including points that were already assigned in the previous iteration.

$$
\text{cluster}(x_i)
=
\arg\min_j d(x_i,c_j^{new})
$$

A point that belonged to \(C_1\) previously may now be closer to \(C_2\).

---

## 7. Repeat the Process

Repeat:

$$
\boxed{
\text{Calculate distances}
\rightarrow
\text{Assign points}
\rightarrow
\text{Update centroids}
\rightarrow
\text{Reassign points}
}
$$

until the clusters become stable.

---

## 8. Check the Stopping Condition

K-Means typically stops when one of the following occurs:

* **Centroids no longer change significantly**
* **Cluster assignments no longer change**
* The change in the objective function becomes very small
* A maximum number of iterations is reached

Conceptually:

```text
Initialize centroids
       ↓
Calculate distances
       ↓
Assign ALL points
       ↓
Update ALL centroids
       ↓
Did assignments/centroids change?
       ↓
    Yes ─────────→ Calculate distances again
       │
       No
       ↓
     STOP
```

---

## 9. Final Clusters

When the algorithm stops, the final assignments represent the resulting \(k\) clusters.

The overall process can be summarized as:

$$
\boxed{
\text{Initialize}
\rightarrow
\text{Assign}
\rightarrow
\text{Update}
\rightarrow
\text{Assign}
\rightarrow
\text{Update}
\rightarrow
\cdots
\rightarrow
\text{Converge}
}
$$

### Key idea

The **assignment step** asks:

> *"Given the current centroids, which cluster does each point belong to?"*

The **update step** asks:

> *"Given the current cluster assignments, where should each centroid be?"*

These two steps repeatedly correct each other until the clusters stabilize.
