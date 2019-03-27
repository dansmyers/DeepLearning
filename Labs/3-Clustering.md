# Clustering

## Description

This lab will let you practice implementing K-means clustering in `scikit-learn`. Our goals are to visualize the output of the model and experiment with a few different parameter choices.

## Generate Data Samples

`scikit-learn` includes a set of generators to create example datasets. A useful one is `make_blobs`, which does what it says on the tin.

Put the following into a file:

```
# Set matplotlib to use PDF output on Codio
import matplotlib
matplotlib.use('Agg')
from matplotlib import pyplot as plt

from sklearn.cluster import KMeans
from sklearn.datasets.samples_generator import make_blobs

# Generate sample data
centers = [[10, 10], [-10, -10], [10, -10]]
X, labels_true = make_blobs(n_samples=750, centers=centers, cluster_std=2.0,
                            random_state=0)

# Plot sample data
plt.figure()
for point in X:
  plt.plot(point[0], point[1], 'ok')

plt.savefig('initial_data.pdf')
```

After you run the program, the sample dataset will show up in a file named `initial_data.pdf`. Try modifying some of the parameters, changing the cluster centers, the `cluster_std` parameter, and the number of clusters.

## Fit a Model

Add the following code to your file:

```
# Cluster model
model = KMeans(n_clusters = 6)
model.fit(X)

labels = model.labels_
unique_labels = set(labels)

plt.figure()

for label in range(len(unique_labels)):
 
  points_in_cluster = [X[k] for k in range(len(X)) if labels[k] == label]
  x_values = [p[0] for p in points_in_cluster]
  y_values = [p[1] for p in points_in_cluster]
  plt.plot(x_values, y_values, 'o')

plt.savefig('clustered_pdf')
```

Using the K-means clusterer is easy: it takes one input, which is the nuber of clusters. Call the `fit` method to find the clusters for a set of data. The `labels_` property is an internal parameter that's set after the clustering is done and gives the assignment of each point to one of the clusters.

## Experiments

What happens if you generate more clusters than actually exist in the data?

What if you generate too few?

What if the clusters overlap?

What if the clusters are sparse?


## Another Generator

Try generating your data using the following statement. What does it produce?

```
X, labels_true = make_circles(n_samples=750, noise = .01,
                            random_state=0)
```

What is the effect of running k-means on this dataset? Could it ever work correctly?


## Density-Based Clustering

Use the following line change to a DBSCAN-based model. You need to add a line at the top of your program to import it.

```
model = DBSCAN(eps=0.1, min_samples=10)
```

Change back to the `make_blobs` generator and experiment with running DBSCAN against the blob dataset. Try to find as many interesting failure cases as you can. Are there ways that DBSCAN fails to cluster the data?

## More Challenges

Look up the and the `AgglomerativeClustering` model and experiment with using it. Try it with both the circles and blobs data and see what kind of results it produces.

If you want one more dataset to play with, look up the `make_moons` generator. It creates interleaved half circles.
