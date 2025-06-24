# Replicability analysis for genome-wide association studies.
### by Heller & Yekutieli. Annals of Applied Statistics. 2014.
**Author**: Siyi Wang  
**Date**: May 27, 2025  

---

## Introduction

The central question of replicability—can an association observed in one study be reproduced in others?—is essential to ensuring robustness in the GWAS findings. Heller \& Yekutieli (2014) derive local Bayes false-discovery rates (lFDR) for each SNP and propose optimal rejection regions that minimise false negatives while controlling the FDR.

## Problem in the past

While past studies are primarily designed to detect associations in at least one study, not necessarily to confirm replication across studies. Thus, a small p-value may not guarantee that the signal is detected in other studies except when the p-value is vanishingly small. There is a linkage disequilibrium of adjacent SNPs, and traditional methods assume independence, having a problem in statistical efficacy. Replicability analysis here focuses on whether a SNP shows associations in the same direction across multiple independent studies, rather than merely being significant in at least one study. Therefore, the purpose of this paper is to develop a replicability analysis method (empirical Bayes method) that maintains control over the FDR while increasing the ability to detect truly replicated associations.

## Method Overview

We aim to identify SNPs whose associations with a phenotype are replicated across multiple GWAS.
Our framework extends Efron’s two-group empirical-Bayes model to the multi-study setting of Heller \& Yekutieli (2014), estimating local Bayes false-discovery rates (lfdrs) for each SNP across all studies. The goal is to discover SNPs whose association with the phenotype is replicated, observed in at least two studies in the same direction. 
 workflow: z-score transformation $\rightarrow$ hierarchical mixture model
$\rightarrow$ lfdrs $\rightarrow$
replicated SNP list at FDR $q$.  

## Methods

1. **Data Preparation and z-score Transformation**: For each SNP in each study, compute the test statistic $T$ and convert it to z-score $Z_{ij} = \Phi^{-1}[F_0(T_{ij})]$, where $F_0$ is the cumulative distribution function under no association, that z-scores approximately follow a standard normal distribution under the null hypothesis of no association. $Z_{ij}\sim N(0,1)$.

## Model Setup

Let the association status vector for each SNP across n studies. Given $\vec{H_j}$, the z-scores from different studies are mutually independent with conditional density $f(z \mid H_{ij})$:

$\vec{H_j} = (H_{1j}, H_{2j}, \dots, H_{nj}),\quad H_{ij}\in\{-1,0,1\}
$,

$\vec{z_j} = \bigl(z_{1j},\ z_{2j},\ \dots,\ z_{nj}\bigr)
$


Where the values mean:

$$
\begin{aligned}
H_{ij} &= 1 &&\text{indicates positive association in study } i,\\
H_{ij} &= -1 &&\text{indicates negative association in study } i,\\
H_{ij} &= 0 &&\text{indicates no association in study } i.
\end{aligned}
$$

The set of all $3^n$ association configurations is

$$
\mathcal{H} = \{-1,0,1\}^n.
$$

The study-specific local FDR is





$\vec{H_j} = (H_{1j}, H_{2j}, \dots, H_{nj}),\quad H_{ij}\in\{-1,0,1\}
$,

$\vec{z_j} = \bigl(z_{1j},\ z_{2j},\ \dots,\ z_{nj}\bigr)
$

${fdr_i}(z_{ij}=z_0) = \Pr(H_{ij}=0 \mid Z_{ij}=z_0) = \frac{\pi_0(i)\,f_0(z_0)}{f_i(z_0)},
$

where $\pi_0(i)=\Pr(H_{ij}=0)$ is the null proportion in study $i$. The optimal rejection region is

$$
\{z : \mathrm{fdr}_{i}(z)\le t\},
$$

which maximizes discoveries under an FDR threshold $t$.


#### 2b. Conditional densities

Given $H_{ij}$ the z-score density is

$$
f_{i,h}(z)=
\begin{cases}
  f_{0}(z) & h=0,\\
  f_{i,+1}(z) & h=+1,\\
  f_{i,-1}(z) & h=-1,
\end{cases}
$$

with $f_{0}(z)=\phi(z)$ the standard normal density. Conditional independence across studies is assumed:

$$
f(\mathbf z_j \mid \mathbf h)=\prod_{i=1}^n f_{i,h_i}(z_{ij}).
$$

3. **Local Bayes FDR**: In the single-study “two-group model”, the z-score of each SNP can be viewed as arising from a mixture of two types of distributions: with/without association. For a given z-value, the local Bayes FDR is the posterior probability that the SNP has no association. For multiple testing, a rejection region can be constructed based on a threshold $t$ such that the overall FDR is controlled. For the “no replicability” null hypothesis $H_0^{NR}$ (i.e., association in at most one study), let $\pi(\mathbf h)=\Pr(\mathbf H_j=\mathbf h)$. For observed $\mathbf z_j$,

$$
\mathrm{fdr}_{H_0^{NR}}(\vec{z}_j)
= \Pr(\vec{H}_j \in H_0^{NR} \mid \vec{z}_j)
= \frac{\sum_{\vec{h} \in H_0^{NR}} \pi(\vec{h}) \, f(\vec{z}_j \mid \vec{h})}
       {\sum_{\vec{h} \in \mathcal{H}}      \pi(\vec{h}) \, f(\vec{z}_j \mid \vec{h})}\,.
$$


where $\pi(\vec{h})$ is the prior probability of configuration $\vec{h}$, and $f(\vec{z}_j|\vec{h}) = \prod_{i=1}^n f_i(z_{ij} \mid h_i)$. If this posterior probability is below a threshold, the SNP is determined to have cross-study replicability. Overall FDR can be controlled by ranking all SNPs’ local Bayes FDRs and selecting an appropriate threshold.

$$
H_{0}^{NR}: \left\{\vec{h}:\sum_{i=1}^n I(h_i=1) \le 1 \;\wedge\; \sum_{i=1}^n I(h_i=-1)\le 1 \right\},
$$

At most 1 positive association and at most 1 negative association; equivalently, at most one study shows association (regardless of direction) across all studies. We aim to reject $H_{0}^{NR}$, i.e., find SNPs that are “associated in at least two studies in the same direction”.

4. **Parameter Estimation**: For $n$ studies, each SNP in each study may exhibit “positive association (+1)”, “no association (0)”, or “negative association (−1)”. Therefore, the association status vector for each SNP has $3^n$ possible configurations. Replicability analysis here focuses on configurations where the same direction of association appears in at least two studies (such as at least two +1s or at least two −1s), rather than cases where association occurs in only one study. Here we need to estimate $\pi(\vec{h})$ for all configurations $\vec{h}$ and conditional densities $f_i(z|\pm1)$.

- First, for each study, use the R package `locfdr` to estimate marginal density $f_i(z)$ and null proportion $\pi_0(i)$. For each study we estimate the marginal z-score density $f_i$ and the null proportion $\pi_0(i)$ via the \texttt{locfdr} package; the alternative density

  $$
  \hat f_{i,A}(z)
    = 
    \frac{\hat f_i(z)-\hat\pi_0(i)\phi(z)}
         {1-\hat\pi_0(i)}
    \quad\text{for }|z|>\Phi^{-1}(0.75)
  $$

- To stabilise the tail estimates in each study we partition the observed $z$-scores into equal-frequency bins and replace every $z_{ij}$ by its bin centre $\tilde z_{ij}$. The resulting empirical alternative density $\hat f_{i,A}(z)$ is then split into two components,

  $$
    \hat f_{i,+1}(z)=
      \begin{cases}
        \hat f_{i,A}(z), & z>0,\\[4pt]
        0,               & z\le 0,
      \end{cases}
    \qquad
    \hat f_{i,-1}(z)=
      \begin{cases}
        \hat f_{i,A}(-z), & z<0,\\[4pt]
        0,                & z\ge 0,
      \end{cases}
  $$

  so that the positive and negative alternative densities are exact mirror images and have disjoint support.

- Lastly use the Expectation-Maximization algorithm based on composite likelihood (assuming local approximate independence between SNPs) to jointly estimate the $\pi(\vec{h})$ distribution.

5. **Optimal Rejection Region**: Among all rejection regions satisfying expected Bayes FDR $\leq q$, the optimal region is the set formed by local Bayes FDR $\leq t(q)$, maximizing discoveries while minimizing Bayes false negative rate.

# Application to Type 2 Diabetes GWAS

In the application to Type 2 diabetes GWAS, the study utilized millions of SNP z-scores from 6 independent cohorts based on Voight et al. (2010). Initially, through locfdr estimation, two studies were found to have no association signals and were excluded, retaining only 4 studies for empirical Bayes replicability analysis. The BHY09 method was also applied for comparison across both 4 and 6 studies.

Results demonstrated that at significance level $q=0.05$, the empirical Bayes method detected 803 SNPs associated with T2D across the 4 studies, of which 219 exhibited cross-study replicability. For example, rs7903146 in the TCF7L2 gene showed consistent direction across all 4 studies with posterior probability nearly equal to 1, which highlights the advantage of the empirical Bayes approach in accurately assessing replicability through configuration posterior probability distributions.

# Conclusion

The empirical Bayes replicability analysis no longer relies solely on whether a SNP is significant in a single study, but instead incorporates the z-values of that SNP from all studies into the model. By estimating the posterior probabilities of each possible association pattern (in which direction signals appear), it determines which signals truly replicate across studies with consistent direction. This framework models the association status vector $\vec{H}_j \in \{-1,0,1\}^n$ for each SNP across multiple studies and computes the local Bayes FDR for the "non-replicable" null hypothesis $H_0^{NR}$. Through locfdr-style mixture and direction-specific truncation, it estimates study-specific null proportions $\pi_0(i)$ and conditional z-score densities $f_i(z \mid h_i)$, and fits joint prior probabilities $\pi(\vec{h})$ via EM algorithm on the composite likelihood function.

The empirical Bayes method fully utilizes the statistical distribution information from each study, more accurately distinguishing between occasional single strong signals and consistent moderate signals across multiple studies. This enables improved discovery of truly replicable associations in large-scale SNP testing while maintaining FDR at a controlled level. Although computational complexity grows with the number of studies and requires careful handling of binning, log-sum-exp stability, configuration pruning, and weak signal cohorts, this framework provides a principled probabilistic foundation for replicability inference in large-scale association studies.
