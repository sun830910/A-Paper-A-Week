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

