# Application of clustering:
## Ancestry analysis
### Introduction
&nbsp;&nbsp;&nbsp;&nbsp; In class, we learned that the gene expression profile can be used as a basis for clustering cancer patients into more specific cancer subgroups. Another perhaps natural criterion to classify biological samples is to cluster them by ancestral history. Consider the human population as an example. Let us, for a moment, assume that human ancestry is closely associated with the geography of the world. Then one may roughly sort the human populations into several groups: European, Asian, American, African, and Australian. With this scheme of grouping, one may classify people with shared European ancestry into one group, people sharing Asian ancestry into another, etc. 

&nbsp;&nbsp;&nbsp;&nbsp; How might we go about measuring the shared ancestry between two individuals? One idea involves sequencing the entire genome of each individual, and comparing the number of differences in the genomic sequence. Then one can develop a metric for clustering by counting the number of different bases in their genome between a pair of individuals, build a pairwise distance matrix, and then cluster individuals with low pairwise differences into the same group, while ensuring that the individuals across different groups have high pairwise differences. The above metric-based clustering could be straight-forward to implement, but the clustering result may not accurately reflect the actual partition of human populations into those ancestral groups. 

&nbsp;&nbsp;&nbsp;&nbsp; One reason that the above metric may fail is that although two individuals in the same cluster may have a low number of different bases between their genomic sequence, their genomic differences may be located at different loci. As a consequence, individuals of different ancestries may be put into the same clusters by the above metric.

&nbsp;&nbsp;&nbsp;&nbsp; Before we examine an alternative clustering technique, let us briefly review the K-means clustering, a distance-based clustering technique.

### K-means clustering
&nbsp;&nbsp;&nbsp;&nbsp; In K-means clustering, K refers to the number of clusters predefined by the user. During every step of this iterative algorithm, there is one "centroid" for every cluster that get updated by taking the average of all members in the cluster (hence "K-means").

#### general description:
&nbsp;&nbsp;&nbsp;&nbsp; Let S denote the set of data points that we wish to cluster. We first predefine the number of clusters (K), and we initialize K "centroids," one for each cluster. These initial centroids could be selected among the given data points or artificially synthesized, and they serve as the "representatives" for their respective clusters. Then, for every data points in S, we compute the distance (Euclidean distance) from the point to each of the K centroids, and we assign this point to the cluster "represented" by the centroid closest to this point. Let us call this step the "assignment step". After each data points have been assigned a cluster, we update each of the K "centroids" by to the average of the data points in its represented cluster. Call this step the "update step". We now iteratively repeat the "assignment step" and the "update step" until the cluster assignments converge (the assignment for each datapoint does not change significantly) or until a preset number of iteration has been run (to avoid infinite loop).

&nbsp;&nbsp;&nbsp;&nbsp; We will now introduce an alternative clustering method.

### Model-based clustering
&nbsp;&nbsp;&nbsp;&nbsp; A probabilistic, non-distance based clustering technique.

#### general description:
&nbsp;&nbsp;&nbsp;&nbsp; Similar to K-means clustering, we also have a set, S, of data points that we wish to cluster. We also predefine the number of clusters, say, K. In model-based clustering, we need to assume a model which we expect our datapoints to follow. In this algorithm, the datapoints are grouped together based on how well they fit with the assumed model. Recall that in class, our goal of metric-based clustering is to group the samples so that the similarity between individuals within the same group is high (smaller distance) while the similarity between individuals across different groups is low (larger distance). Analogously, the model-based clustering technique aims to group the samples so that individuals forming a group satisfy (as closely as possible) the characteristic properties of the group, as specified by the model. As an example, let us use the Hardy-Weinberg Principle as a model to cluster a theoretical population.

&nbsp;&nbsp;&nbsp;&nbsp; For example, Tishkoff et al. (7) uses Hardy-Weinberg Equilibrium as their key model in order to study the ancestry of African populations.

#### Hardy-Weinberg Equilibrium (3)
&nbsp;&nbsp;&nbsp;&nbsp; Suppose we focus our attention on a gene in a diploid organism (having two copies of the genome, like human) that has two alleles (alternative version of the gene). Then the Hardy-Weinberg principle asserts that the frequencies of the alleles in a population stays constant from one generation to the next if the population satisfies the following assumptions:
  1. All individuals, regardless of which alleles of the gene they have, share equal survival and reproductive rates.
  2. No new alleles are created (no mutation)
  3. No migration (in or out) occurs in the population 
  4. The population is large (to reduce sampling error)
  5. Mating in the population is random.

&nbsp;&nbsp;&nbsp;&nbsp; Before we see how these assumptions are satisfied, let us explore the use of Hardy-Weinberg principle. Consider a gene that has two alleles, A, and a. Let p be the frequency of allele A, and q be the frequency of allele a. Then p + q = 1 and ![(p + q)^2 = p^2 + 2pq + q^2 = 1](https://raw.githubusercontent.com/gary317680/BENG183/master/expanded_binomial.png). In this case, ![p^2](https://raw.githubusercontent.com/gary317680/BENG183/master/p_square.png) represents the frequency of genotype AA, ![2pq](https://raw.githubusercontent.com/gary317680/BENG183/master/2pq.png) the frequency of genotype Aa, and ![q^2](https://raw.githubusercontent.com/gary317680/BENG183/master/q_square.png) the frequency of aa. By the Hardy-Weinberg principle, populations that satisfy the above 5 assumptions should maintain constant values of p and q. That is, populations in Hardy-Weinberg equilibrium have predictable allele frequencies. If a population has p = 0.7 and q = 0.3, then by randomly sampling from the population a large number of times, 70% of the alleles seen will be A, and allele a will be seen for 30% of the time. Moreover, among the randomly sampled individuals, 49% of them will have genotype AA, 42% of them with genotype Aa, and 9% with genotype aa.

&nbsp;&nbsp;&nbsp;&nbsp; If the gene has 3 alleles (such as the gene for ABO blood type), we can introduce an additional variable, r, to denote the frequency of the third allele. In this case, we have p + r + q = 1, and ![(p + r + q)^2 = p^2 + r^2 +q^2 + 2pr + 2pq + 2qr = 1](https://raw.githubusercontent.com/gary317680/BENG183/master/expanded_trinomial.png). Each of p, r, and q denotes the allele frequencies, whereas the pairwise products of p, r, and q represent the frequency of each genotype.

#### How do we use the Hardy-Weinberg equilibrium?
&nbsp;&nbsp;&nbsp;&nbsp; From the above discussion, we know that if a population is stable enough (in Hardy-Weinberg equilibrium), the probability of sampling a particular allele or genotype from this population is the same frequency of the allele or genotype characteristic of the population. In practice, however, we do not know the allele frequency of the population before hand, and we can only figure out the frequencies by taking samples from the population. Yet, each sample that we take from the population may not necessarily represent the population (that is, the allele frequencies we observe from the sample may not be the actual frequencies of the entire population). However, Pritchard et al. demonstrates that it is possible to obtain an accurate estimate of the allele frequencies by repeatedly sampling and estimating, via the Markov Chain Monte Carlo algorithm (5). This is the basis for [STRUCTURE](https://web.stanford.edu/group/pritchardlab/structure.html), a software commonly used in population genetics (5).

STRUCTURE solves the following bioinformatics problem:
> Given a list of observation (a group of people, presumably coming from one or more ancestral groups), figure out a way to assign each of these observations into groups, so that each assignment is the most likely assignment (among all the groups that a person could be assigned to, the assigned group is the group that the person most likely came from).

&nbsp;&nbsp;&nbsp;&nbsp; In our ancestry analysis, the list of observation is the group of people that we have, each with his or her own genotypes measured. Our goal is then to put these people into ancestral groups that they most likely have come from.

#### How are the Hardy-Weinberg assumptions satisfied?
&nbsp;&nbsp;&nbsp;&nbsp; Because our ancestry analysis only takes place in a short period of time, mutation rates and migration can be neglected. Because the study spans across a time in less than one generation, the survival and reproductive rates, as well as random mating are not significant factors in our analysis. That is, because human evolution occurs at a slow pace (as opposed to organisms like bacteria or viruses), the human population is stable enough such that we can assert that the allele frequencies in each subpopulation are approximately constant in the timeframe we are interested in. The only assumption that remains is that the population has to be large. While the qualification of a "large" population can be subjective, if we restrict our attention to ancestral groups that have a long history, the population in these groups should be "large" enough. Nonetheless, care needs to be taken to sample as many people as possible in order to reduce sampling error.

##### Caveat
&nbsp;&nbsp;&nbsp;&nbsp; Though migration may not be a huge factor during the span of the study, it could potentially lead to sampling errors (for example, the person sampled from a particular region could be recent immigrant from else where). In addition, the analysis may be made more valid by avoiding the genes at or near "mutation hotspots" in the human genome.

### A comparison of the model-based clustering algorithm with K-means clustering algorithm
&nbsp;&nbsp;&nbsp;&nbsp; These two clustering algorithms are similar in that they both require a pre-defined number of clusters, K. In each algorithm, there is a function that measures how well the current cluster assignment is. In the K-means algorithm, the function could be a simple euclidean distance between the data point and the centroids representative of each cluster. In the model-based algorithm, the function could be the likelihood of the current assignment for each data point. Both algorithms repeatedly try to assign each data point a cluster, and update their respective measuring function. 

<p align="center">
  <img src="https://raw.githubusercontent.com/gary317680/BENG183/master/K-means_algo_from_lecture.png">
</p>
<p align="center"> Figure 10 An illustration of K-means algorithm. Dots: data points. Cross: center. Red and blue colors: clustering outcome. (a) data. (b) initial cluster centers. (c) cluster assignment. (d) center update. (e) cluster update. (f) center update. From BENG183 Lecture "Intro to Machine Learning" Week2 (10). </p>

The algorithms can stop when the cluster assignments converge (stabilize) or after a predefined number of iterations have been run. Ideally, the final cluster assignment outputted by each algorithm should be optimal in the sense that, for K-means algorithm, the intra-cluster distances is small, and the inter-cluster distances are high, and for the model-based algorithm, the intra-cluster allele frequency distribution resembles that of the predefined clusters. In other words, the amount of uncertainty in all cluster assignment should be minimized; in other words, the final cluster assignment is the one with the most confidence (see Figure 11).

<p align="center">
  |classification|uncertainty|
  |:---:|:---:|
  |<img src="https://raw.githubusercontent.com/gary317680/BENG183/master/Model-based_clustering_ex.png"> | <img src="https://raw.githubusercontent.com/gary317680/BENG183/master/Model-based_clustering_uncertainty_ex.png"> |
</p>

&nbsp;&nbsp;&nbsp;&nbsp; As discussed in class, for K-means clustering, it is difficult to figure out the optimal number for K without trying multiple values of K. The same principle holds for the model-based clustering. However, in our ancestry analysis, it is possible to use the demographic information collected during the sampling step to facilitate an estimation for K. Nonetheless, care should be taken in interpreting the meaning of K. The value of K that yields the optimal cluster assignment may not actually correspond to K real populations! 


### Choosing the significant genetic marker for analysis
&nbsp;&nbsp;&nbsp;&nbsp; Note that in our ancestry analysis, our model makes use of allele frequencies as a defining feature for each ancestral group. If there is a gene with the same allele frequencies in all populations, such a gene is not useful in distinguishing between different populations. Therefore, we must select genes that have varying allele frequencies distribution in different populations to aid our analysis. One common technique is to use Principal Component Analysis (PCA), which was introduced in an earlier section. The key idea is to use PCA to identify principle components that can best explain the variations between data points. In practice, we can genotype the individuals from all populations at various loci, perform PCA on the genotype vectors, and identify useful gene loci that can best help distinguish the population (gene markers). For example, Tishkoff et al uses PCA to identify the major factors that distinguish between African populations (see Figure 8), and use the appropriately selected genetic markers to perform the model-based clustering algorithm (via STRUCTURE, see Figure 9). 

<p align="center">
  <img src="https://raw.githubusercontent.com/gary317680/BENG183/master/PCA_analysis.png">
</p>
<p align="center"> Figure 8 PCA analysis, based on genotypes of the individuals, from Tishkoff, et al. (7) </p>

<p align="center">
  <img src="https://raw.githubusercontent.com/gary317680/BENG183/master/Model-based_clustering.png">
</p>
<p align="center"> Figure 9 Clustering results of the African population analysis from Tishkoff, et al. (7) </p>
