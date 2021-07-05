# Project on Unsupervised Anomaly Detection


## Objective:
This project is about implementing an unsupervised anomaly detection algorithm capable of
working on streaming datasets

## Contributors:
Gurami Keretchashvili (me)
Guillaume Barthe
Temur Malishava

## Getting Started
1. Clone this repo (see [tutorial)](https://docs.github.com/en/github/creating-cloning-and-archiving-repositories/cloning-a-repository-from-github/cloning-a-repository.)
2. pip install -r /path/to/requirements.txt


## 1 Anomaly Detection Without Streaming

### 1.1 Categories on anomaly detection

Anomaly detection refers to finding patterns in data that do not conform to expected behavior.

There exists three main categories of anomaly detection which are the following:

- Normal data instances belong to a cluster in the data, while anomalies do not belong to any
    cluster.
- Normal data instances lie close to their closest cluster centroid while anomalies are far away.
- Normal data instances belong to large and dense clusters, while anomalies either belong to
    small or sparse clusters.

Please note that in this work we focused on the second category.

### 1.2 Dataset we used

We used a vowels dataset that can be found here. It was made of 1456 instances and had 12
features. The number of anomalies inside the dataset is 50.

As you can see on Fig. 1 the dimension of this dataset is high and we decided to use **Principal
Component Analysis** in order to reduce dimensionality with minimum information loss.

![alt text](https://github.com/gurokeretcha/Data-Stream-Mining/blob/main/Anomaly%20Detection%20using%20k-means%20in%20streaming/result%20images/Picture4.png "")
Figure 1: Header of the dataframe used in our experiments **before** PCA. This comes from the
dataset vowels.mat

![alt text](https://github.com/gurokeretcha/Data-Stream-Mining/blob/main/Anomaly%20Detection%20using%20k-means%20in%20streaming/result%20images/Picture5.png "")
Figure 2: Header of the dataframe used in our experiments **after** PCA. This comes from the dataset
vowels.mat

## 2 Streaming Adaptation

### 2.1 streaming set up


In order to adapt the code to a streaming method we worked a follows :

- Reduce the amount of memory by using a sliding window of 300 points. When a new data-
    point arrives, you remove the oldest one before adding it
- Recompute the optimal number of clusters every 200 iteration so that the algorithm is able
    to adapt to the data without consuming too much time
- Apply PCA and detection of outliers at each iteration


### 2.2 Time consumption and memory

As you can imagine, the visualization is expensive and we wanted to measure the time consump-
tion of our algorithm with and without visualization. On the following figures, you can observe
that the average amount of time taken at each iteration when we update the plots is 10 times
higher than the one without visualization. Furthermore, you can easily spot the calculation of the
optimal number of clusters every 200 iteration without visualization and this shows why it was
necessary to avoid doing it at every iteration.

![alt text](https://github.com/gurokeretcha/Data-Stream-Mining/blob/main/Anomaly%20Detection%20using%20k-means%20in%20streaming/result%20images/Picture2.png "")
Figure 3: Time consumption of our algorithm without visualization. Mean time = 0.04 s ; Total
time = 53.1 s


![alt text](https://github.com/gurokeretcha/Data-Stream-Mining/blob/main/Anomaly%20Detection%20using%20k-means%20in%20streaming/result%20images/Picture1.png "")
Figure 4: Time consumption of our algorithm with visualization. Mean time = 0.40 s ; Total time =
580.6 s

About memory usage, the sliding window of 300 points makes it efficient in terms of memory but
it is worth mentioning that updating plots at each iteration consumes a decent amount of RAM.

### 2.3 Results analysis

In the end, the detection of anomalies seems convenient but it needs to be confirmed by some
metrics like precision and recall. In order to do so, we need a more suited dataset where anomalies
are equally spread between clusters. We did not manage to find such dataset, therefore we decided
that the right way to do this would be to create a generator of anomalies.

By using this generator at some random iteration, we would be able to determine if the algorithm
is able to efficiently detect anomalies.


### 2.4 Visualization

The objective of Principal Component Analysis was to reduce the dimension to 2 so that we would
be able to visualize the clustering in real time. Therefore we decided to add a plot and update it
at each iteration of the streaming algorithm.  you can observe our visualization. This is a
screenshot but in our implementation you can find the animated version of this visualization that
updates itself over time.

Visualization of the clustering in real time. Centroids are marked as triangles and outliers
are circled.
[![Demo CountPages alpha](https://media1.giphy.com/media/zZfz8MLmHy62ZIhLNP/giphy.gif?cid=790b7611330efcee86b9ec264e32c4452d88513b61bb367d&rid=giphy.gif&ct=g)]


As we expected, the anomalies we predict are the points in each cluster that are the furthest away
from the centroids and they are easily identifiable for the observer.

## 3 Conclusion and future work

In the future, it would be interesting to implement K-nearest neighbours approach or more genrally methods that are not about clustering.



