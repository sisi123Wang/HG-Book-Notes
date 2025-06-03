# Replicability analysis in genome-wide association studies via Cartesian hidden Markov models  
### by Pengfei Wang, Wensheng Zhu  
**Author**: Siyi Wang  
**Date**: May 27, 2025  

---

## Introduction

The question of replicability—whether an association observed in one study can be consistently observed in others—remains essential for ensuring robustness in GWAS findings. Heller and Yekutieli (2014) derived local Bayes false discovery rates for each SNP and proposed optimal rejection regions that minimize false negatives while controlling the FDR, enabling replicability assessment across multiple GWAS datasets.

## Problem in the past

While past studies were primarily designed to detect associations in at least one study, they were not necessarily meant to confirm replication across studies. Thus, a small p-value may not guarantee that the signal is detected in other studies, except in the case of extremely small p-values. The purpose of this paper is to develop a replicability analysis method that maintains FDR control while improving the ability to detect truly replicated associations.

## Methods and Conclusion

In GWAS, adjacent SNPs tend to exhibit local dependency due to the co-segregation of nearby genomic loci during meiosis. As a result, SNPs in linkage disequilibrium are not statistically independent, and the assumption of p-value independence across the genome does not hold. This presents a challenge for statistical inference, particularly for replicability analysis, where ignoring the local correlation structure can reduce power and lead to incorrect error estimation.

To address this, the Cartesian Hidden Markov Model (CHMM) provides a natural and effective extension of classical hidden Markov models (HMMs) for modeling multi-study GWAS data. Unlike traditional HMMs, which use a one-dimensional hidden state space, CHMM defines a multidimensional state space as the Cartesian product of individual HMM state sets. For example, in a two-study scenario, each SNP can be in one of four joint hidden states: (non-significant, non-significant), (non-significant, significant), (significant, non-significant), or (significant, significant). These joint states evolve across the genome via a four-state Markov chain, capturing the LD structure between adjacent SNPs.

CHMM can help with testing for pleiotropy, in which a single SNP affects multiple traits or diseases. In the application involving bipolar disorder and schizophrenia, the focus is on identifying SNPs with replicated effects across both conditions.

This paper introduces the repLIS procedure, which leverages the CHMM framework. When CHMM parameters are known, repLIS statistics can be computed using the forward-backward algorithm. When parameters are unknown, they are estimated via the EM algorithm, and a plug-in version of repLIS is used based on maximum likelihood estimates. Thus, in real data analysis, the efficiency of hypothesis ranking by repLIS can serve as a useful tool for true FDR control, especially when true associations are unknown. Compared to procedures like repfdr, repLIS exhibits superior performance in ranking replicated SNPs, particularly under non-independence conditions. This is because repLIS leverages spatial correlation to identify entire clusters of replicated signals, rather than relying solely on isolated SNPs with extremely small p-values.

Visualization via Manhattan plots illustrates that SNPs with strong pleiotropic effects, identified by repfdr and repLIS, tend to cluster on specific chromosomes, such as chromosomes 3 and 10, lying above the horizontal significance threshold. Simulation studies further confirm the theoretical guarantees of repLIS. The procedure maintains FDR control at the pre-specified level $\alpha$ and demonstrates lower false negative rates (FNR) and higher average true positive (ATP) counts, especially when the signal strength parameter $\mu$ increases. On ROC curves, repLIS achieves uniformly higher sensitivity compared to repfdr, indicating a more effective ranking of true replicated signals for a given number of rejections.

In conclusion, repLIS provides an efficient and robust tool for detecting and ranking replicated associations in GWAS, particularly when accounting for the complex dependency structure of SNPs.
