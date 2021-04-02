# Ball*-tree: Efficient spatial indexing for constrained nearest-neighbor search in metric spaces

## Introduction

Various space-partitioning data structures are mainly different in three aspects:

1. The geometric shape of the partition
2. The space-partitioning strategy that builds up the search tree
3. The search algorithm which exploits the data structure to search for the given query.

In the ball-tree algorithm introduced by Andew, each node is split using two steps:

1. choosing farthest point from centroid of points as the centroid of the first sub-partition
2. selecting the farthest point from the fist one as the centroid of the second sub-partition.

This is a simple and effective procedure, it can lead to unbalanced trees.  

This paper suggests a new node splitting method, called ball*-tree, that make more balanced trees and thus accelerates nearest neighbor search.  

This paper's contributions are as follows:

1. We suggest a novel method for selecting the splitting axes in each node using principal component analysis.
2. We propose heuristics for finding the best splitting hyperplane with respect to the seleceted axes
3. In the search stage, we suggest an effcient algorithm for range-constrained NN search.



## Preliminaries and related work

There are two approaches for exact search:

1. Compact partition indexes that directly partition the data points, such as KD-tree, cover-tree and ball-tree
2. Pivot-based indexes that use a set of points, called pivots, to map the points to another space in which the distance is easier to compute.  

  

While pivot-based approaches are faster in medium-sized datasets, the required number of pivots is extremely large for high-dimensional datasets.  

A ball-tree is a binary tree in which every node defines a D-dimensional hypersphere, or ball, containing a subset of the points to be searched. Each node of the tree represents a ball, that is a hyper-spherical partition.  

  

## Space partitioning

### Ball-tree space partitioning

Ball-tree is build using a divide-and-conquer approach. Initially, ball-tree has only one (root) node and all data points are assigned to it. In each step, the partition corresponding to each node is split into two sub-partitions.

1. Select the farthest point from centroid of points in pi as the first (left) child pivot piL.
2. Select the farthest point from piL as the second (right) child pivot piR.
3. Assign each of the data points in pi to the partition whose pivot is closer.
4. Assign the new sub-partitions as children of vi in the ball-tree.



## Ball*-tree

Ball-tree splits have two shortcomings:

1. When splitting a partition, the number of points assigned to each sub-partition is not taken into account. As a result, the partitions do not necessarily have the same number of data points and the resulting tree is unbalnced.
2. The distribution of the points is not considered for defining the separating hyperplane, but the hyperplane is determined by only the two farthest points.  

  

The intuition behind ball*-tree is to detect the best direction for splitting the data points.  

This direction is simply extracted by the first principal component.  

In ball*-tree, the hyperplane is determined in three steps:

1. Apply Principal Component Analysis(PCA) and find the most significant (first) eigenvector w1.
2. Map the data points to the axis corresponding to w1.
3. Find a hyperplane that is perpendicular to w1 and splits the data points in a balanced manner. The splitting criteria is discussed further.



In ball*-tree, our target is to detect the most significant direction of the data. Hence, we apply PCA to transform the data points to a single dimension.  

  

In ball*-tree, the splitting hyperplane is perpendicular to the eugebvectir w1 and us deternubed by w1\* x-b=0.  

其目标函数求最小化的第一个部分是为了要让N1和N2两个群中的数量接近且靠近N/2，第二个部分则是为了让这个切分的点尽量接近于圆心的位置。  



其大意应为使用PCA将所有数据映射至一个一维向量(相当于找到了最有效分成两类的超平面)上，再通过这个一维向量上进行操作找到离圆心最接近同时最有效将两侧数据数量平衡的点进行切分。