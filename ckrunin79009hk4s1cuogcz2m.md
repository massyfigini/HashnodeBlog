## R: cluster analysis

### Hierarchical Clustering


Calculate distance matrix (euclidean distance as default):
```
dist(tab)   
```

Use first 3 columns:
```
dist(tab[,1:3])   
```

Hierarchical clustering with euclidean distance and average linkage:
```
hc <- hclust(distance_matrix)   
```

Print the dendrogram:
```
plot(as.dendrogram(hc))   
```

Dendrogram function with different graphic options:
```
library(ggdendro)
ggdendrogram(hc, theme_dendro = FALSE)   
```

Another dendrogram option:
```
library(devtools)
myplclust(hc, lab.col = unclass(tab$col))  #lab.col to color based on column value
abline(h=1.5,col="red")   #dendrogram cut
``` 

### K-Means Clustering

k-means non-hierarchical clustering with two groups:
``` 
kmeans(tab,centers=2)
``` 

Exclude columns 11 and 12 and divide into 5 groups:
``` 
Clust <- kmeans(tab[,-c(11:12)], centers=5)   
``` 

Built a table having every different version of col for columns and the different groups as rows, to see how the col value are distributed inside the groups:
``` 
table(Clust$cluster, tab$col)   
``` 

Other useful parameters:  
- iter.max: max iteration number before stop.  
- nstart: different number of centroids to stard. nstart = 100 means 100 different test with different centroids, then it can choose the best.