# Application of clustering:
## 1. Ancestry analysis
In class, we learned that the gene expression profile can be used as a basis for clustering cancer patients into more specific cancer subgroups. Another perhaps natural criterion to classify biological samples is to cluster them by ancestral history. Consider the human populations as an example. Let us, for a moment, assume that the human ancestry is closely associated with the geography of the world. Then one may roughly sort the human populations into several groups: European, Asian, American, African, and Australian. With this scheme of grouping, one may classify people with shared European ancestry into one group, people sharing Asian ancestry into another, etc. 

How might we go about measuring shared ancestry between two individuals? One idea involves sequencing the entire genome of each individual, and compare the number of differences in the genomic sequence. Then one can develop a metric for clustering by counting the number of different bases in their genome between a pair of individuals, build a pair-wise distance matrix, and then cluster individuals with low pair-wise differences into the same group, ensuring that the individuals across different groups have high pair-wise difference. The above matric-based clustering could be straight-forward to implement, but the clustering result may not accurately reflect the actual partition of human populations into those ancestral groups. 

One reason that the above metric may fail is that although two individuals in the same cluster may have low number of different bases between their genomic sequence, their genomic differences may be located at different loci. Therefore, individuals of different ancestry may be put into the same clusters.

### Enter model-based clustering
A non distance-based clustering technique.
#### - general discription:
  We assume a model, predefine the number of clusters, and group the samples
  together based on how well they fit with the assumed model.
