---
title: "R: cluster analysis"
datePublished: Sat Mar 18 2017 13:54:04 GMT+0000 (Coordinated Universal Time)
cuid: ckrunin79009hk4s1cuogcz2m
slug: r-cluster-analysis
tags: r

---

### Hierarchical Clustering

Calculate distance matrix (euclidean distance as default):

```sql
dist(tab)
```

Use first 3 columns:

```sql
dist(tab[,1:3])
```

Hierarchical clustering with euclidean distance and average linkage:

```sql
hc <- hclust(distance_matrix)
```

Print the dendrogram:

```sql
plot(as.dendrogram(hc))
```

Dendrogram function with different graphic options:

```sql
library(ggdendro)
ggdendrogram(hc, theme_dendro = FALSE)
```

Another dendrogram option:

```sql
library(devtools)
myplclust(hc, lab.col = unclass(tab$col))  #lab.col to color based on column value
abline(h=1.5,col="red")   #dendrogram cut
```

### K-Means Clustering

k-means non-hierarchical clustering with two groups:

```sql
kmeans(tab,centers=2)
```

Exclude columns 11 and 12 and divide into 5 groups:

```sql
Clust <- kmeans(tab[,-c(11:12)], centers=5)
```

Built a table having every different version of col for columns and the different groups as rows, to see how the col value are distributed inside the groups:

```sql
table(Clust$cluster, tab$col)
```

Other useful parameters:

* iter.max: max iteration number before stop.
    
* nstart: different number of centroids to stard. nstart = 100 means 100 different test with different centroids, then it can choose the best.