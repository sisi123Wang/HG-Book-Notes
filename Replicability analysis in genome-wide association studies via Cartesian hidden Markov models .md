# Replicability analysis for genome-wide association studies.
### by Heller & Yekutieli. Annals of Applied Statistics. 2014.
**Author**: Siyi Wang  
**Date**: May 27, 2025  

---

## Introduction

The central question of replicability—can an association observed in one study be reproduced in others?—is essential to ensuring robustness in the GWAS findings. Heller \& Yekutieli (2014) derive local Bayes false-discovery rates (lFDR) for each SNP and propose optimal rejection regions that minimise false negatives while controlling the FDR.

## Problem in the past

While past studies are primarily designed to detect associations in at least one study, not necessarily to confirm replication across studies. Thus, a small p-value may not guarantee that the signal is detected in other studies except when the p-value is vanishingly small. There is a linkage disequilibrium of adjacent SNPs, and traditional methods assume independence, having a problem in statistical efficacy. Replicability analysis here focuses on whether a SNP shows associations in the same direction across multiple independent studies, rather than merely being significant in at least one study. Therefore, the purpose of this paper is to develop a replicability analysis method (empirical Bayes method) that maintains control over the FDR while increasing the ability to detect truly replicated associations.

## Methods and Conclusion

We aim to identify SNPs whose associations with a phenotype are replicated across multiple GWAS.
Our framework extends Efron’s two-group empirical-Bayes model to the multi-study setting of Heller \& Yekutieli (2014), estimating local Bayes false-discovery rates (lfdrs) for each SNP across all
studies. The goal is to discover SNPs whose association with the phenotype is replicated, observed in at least two studies in the same direction. 
 workflow:
\emph{z-score transformation $\rightarrow$ hierarchical mixture model
$\rightarrow$ lfdrs $\rightarrow$
replicated SNP list at FDR $q$}.  
