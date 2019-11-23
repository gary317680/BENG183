<style TYPE="text/css">
code.has-jax {font: inherit; font-size: 100%; background: inherit; border: inherit;}
</style>
<script type="text/x-mathjax-config">
MathJax.Hub.Config({
    tex2jax: {
        inlineMath: [['$','$'], ['\\(','\\)']],
        skipTags: ['script', 'noscript', 'style', 'textarea', 'pre'] // removed 'code' entry
    }
});
MathJax.Hub.Queue(function() {
    var all = MathJax.Hub.getAllJax(), i;
    for(i = 0; i < all.length; i += 1) {
        all[i].SourceElement().parentNode.className += ' has-jax';
    }
});
</script>
<script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.4/MathJax.js?config=TeX-AMS_HTML-full"></script>


# Application of clustering:
## 1. Ancestry analysis
In class, we learned that the gene expression profile can be used as a basis for clustering cancer patients into more specific cancer subgroups. Another perhaps natural criterion to classify biological samples is to cluster them by ancestral history. Consider the human populations as an example. Let us, for a moment, assume that the human ancestry is closely associated with the geography of the world. Then one may roughly sort the human populations into several groups: European, Asian, American, African, and Australian. With this scheme of grouping, one may classify people with shared European ancestry into one group, people sharing Asian ancestry into another, etc. 

How might we go about measuring shared ancestry between two individuals? One idea involves sequencing the entire genome of each individual, and compare the number of differences in the genomic sequence. Then one can develop a metric for clustering by counting the number of different bases in their genome between a pair of individuals, build a pair-wise distance matrix, and then cluster individuals with low pair-wise differences into the same group, ensuring that the individuals across different groups have high pair-wise difference. The above matric-based clustering could be straight-forward to implement, but the clustering result may not accurately reflect the actual partition of human populations into those ancestral groups. 

One reason that the above metric may fail is that although two individuals in the same cluster may have low number of different bases between their genomic sequence, their genomic differences may be located at different loci. Therefore, individuals of different ancestry may be put into the same clusters.

### Enter model-based clustering
A non distance-based clustering technique.
#### - general discription:
  We assume a model, predefine the number of clusters, and group the samples together based on how well they fit with the assumed model. In class, our goal of metric-based clustering is to group the samples so that the similarity between individuals within the same group is high (smaller distance) while the similarity between individuals across different groups is low (larger distance). Anologously, the model-based clustering technique aims to group the samples so that individuals forming a group satisfy (as closely as possible) the characteristic properties of the group, as specified by the model. As an example, let us use the Hardy-Weinberg Principle as a model to cluster a theoretical population.
 
  For example, [this paper] uses Hardy-Weinberg Equilibrium as their key model in order to study the ancestry of African populations.

#### Hardy-Weinberg Equlibrium
  Suppose we focus our attention on a gene in a diploid organism (having two copy of the genome, like human) that has two alleles (alternative version of the gene). Then the Hardy-Weinberg principle asserts that the frequencies of the alleles in a population stays constant from one generation to the next if the population satisfies the following assumptions:
  1. All individuals, regardless of which alleles of the gene they have, share equal survival and reproductive rates.
  2. No new alleles are created (no mutation)
  3. No migration (in or out) occurs in the population 
  4. The population is large (to reduce sampling error)
  5. Mating in the population is random.

  Before we see how this assumptions are satisfied, let us explore the use of Herdy-Weinberg principle. Consider a gene that has two alleles, A, and a. Let p be the frequency of allele A, and q be the frequency of allele a. Then p + q = 1 and $$(p + q)^2 = p^2 + 2pq + q^2 = 1$$. In this case, p^2 represents the frequency of genotype AA, 2pq the frequency of genotype Aa, and q^2 the frequency of aa. By the Hardy-Weinberg principle, populations that satisfy the above 5 assumptions should maintain constain values for p and q. That is, population in Hardy-Weinberg equilibrium have predictable allele frequencies. If a population has p = 0.7 and q = 0.3, then by randomly sampling from the population a large number of time, 70% of the alleles seen will be A, and allele a will be seen for 30% of time. Moreover, of the randomly sampled individuals, 49% of them will have genotype AA, 42% of them with genotype Aa, and 9% with genotype aa.

  If the gene has 3 alleles (such as the gene for ABO blood type), we can introduce an additional variable, r, to denote the frequency of the third allele. In this case, we have p + r + q = 1, and $$(p + r + q)^2 = p^2 + r^2 +q^2 + 2pr + 2pq + 2qr = 1$$. Each of p, r, and q denotes the allele frequencies, whereas the pair-wise products of p, r, and q represent the frequency of each genotype.

$$e= mc^{2}$$
$\e=mc^{2}$
