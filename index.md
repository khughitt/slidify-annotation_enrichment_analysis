---
title       : Annotation Enrichment Analysis - An Alternative Method for 
              Evaluating the Functional Properties of Gene Sets
subtitle    : Kimberly Glass, Michelle Girvan
author      : Keith Hughitt
framework   : io2012
highlighter : highlight.js
hitheme     : tomorrow
widgets     : [mathjax]
mode        : selfcontained
github      : 
  user: khughitt
  repo: slidify-annotation_enrichment_analysis
---

<!-- Custom Styles -->
<style type='text/css'>
    .title-slide {
        background: #e3f2fc;
        text-shadow: 2px 2px 5px #666;
        color: #2d4152;
    }
    slides > slide {
        height: 800px;
        margin-top: -400px;
    }
    img {
        max-height: 560px;
        max-width: 964px;
    }
    slide a {border-bottom: none;}
    .small li { font-size: 20px; }
    .smaller li p { font-size: 16px; }
    .caption { font-size: 14px; display: block; }
    .references li { font-size: 14px; }
    .references li p { font-size: 14px; }
</style>

<!-- Custom JavaScript -->
<script src="http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.min.js"></script>
<script type='text/javascript'>
$(function() {
    $("p:has(img)").addClass('centered');
});
</script>

## Overview

>- Recent high-throughput methods (microarray, RNA-Seq, etc) made it easy to
   produce large datasets comparing samples in different conditions.
>- The end result of many of these analyses, however, is often a large list of
   genes that are associated with one condition or the other.
>- Numerous tools have been developed to look for "enrichment" in these
   resulting gene sets for genes associated with a particular known pathway or
   functional annotation.
>- These methods (GSEA, etc) often use statistics which make some assumptions
   about the distribution of annotations which may not be valid.
>- What are the effects of these assumptions the resulting interpretation?
>- Can we do better?

---

## Example: T. cruzi co-expression network

One example of where this kind of enrichment analysis could be useful is for
determining possible roles for clusters of co-expressed genes.

![tcruzi coex network](assets/img/ModuleMembership.png)

---.segue .dark

## Background

---

## Gene Ontology (GO)

### What is GO?

<q>The Gene Ontology is a controlled vocabulary, a set of standard terms—words
and phrases—used for indexing and retrieving information. In addition to
defining terms, GO also defines the relationships between the terms, making
it a structured vocabulary. (geneontology.org)</q>

- Provides a common language to describe features of genes from all different
  species.
- Includes three separate ontologies relating to:
  - Location (cellular component)
  - Process/pathway involved in (biological process)
  - Specific function (molecular function)
- Each ontology is represented by a directed acyclic graph (DAG).
- Deeper levels in the ontology correspond to more specific descriptions.
- Machine-readable.

---

## Gene Ontology (GO)

### Gene Ontology structure

![GO example](assets/img/diag-ontology-graph.gif)
<span class='caption'>
(source: http://www.geneontology.org/GO.ontology.structure.shtml)
</span>

---

## Many functional enrichment tools exist

![Huang table 1](assets/img/Huang_table1.png)
<span class='caption'>Huang et al. (2009) Table 1</span>

---

## Common statistics used for enrichment analysis

- Fisher's Exact Test (FET)
- Binomial test
- Hypergeometric test
- Chi-squared test

All of these methods assume that, under the null hypothesis, genes are equally
likely to be selected.

---

## Gene Set Enrichment Analysis (GSEA)

- Most popular tool for enrichment analysis
- Uses variant of Kolmogorov–Smirnov test
  - Compares distributions of two samples
  - Null hypothesis: the samples were drawn from the same distribution
- Looks for enrichment in genes with a known property (e.g. GO annotation) at
  the top of a list of genes ranked by differential expression, etc.

---

## Gene Set Enrichment Analysis (GSEA)

![GSEA fig1](assets/img/Subramanian_F1.large.jpg)
<span class='caption'>Subramanian et al. (2005) Figure 1</span>

---

## Fisher's Exact Test (FET)

- The most common test statistic used for functional enrichment
- Considers the overlap between experiment gene set and set of genes with some
  known functional annotation.


---.segue .dark

## Results

---

## Gene ontology characteristics

### Gene-term graph

- Downloaded all human gene-term associations from the
  [Gene Ontology website](http://www.geneontology.org/).
- Constructed a gene/annotation bipartite graph, represented by an 
  $n_G \times n_T$ adjacency matrix 

![adj matrix](assets/img/adj_matrix_small.png)

- $n_G$ - number of genes
- $n_T$ - number of GO terms
- $A_{ij} = 1$ - Gene $i$ is annotated with term $j$
- $A_{ij} = 0$ - Gene $i$ is not annotated with term $j$

---

## Gene ontology characteristics

### Gene and term degree distributions

![fig 1](assets/img/srep04191-f1.jpg)

- <span class='red'>Biological Process</span> terms dominate the human
  annotations.
- Degree of term ($k_t$) distribution is "heavy-tailed"; most terms are
  associated with only a few genes, but some terms are used for a huge 
  number of genes.

---

## Gene ontology characteristics

### Biological Process

The remainder of the results are based on the biological process ontology:

- 656,783 annotations
- 15,213 genes (avg: 43.2 annotations)
- 10192 terms (avg: 64.4 annotations)

---

## Question: What is the effect of annotation database properties on functional enrichment analysis?

### Experiment design:

- Created 200 random gene sets:
  - $N_g$=200 genes in each set (a "typical" gene set size)
  - Varied number of annotations ($M_g$)

---

## Question: What is the effect of annotation database properties on functional enrichment analysis?

  - Determined FET enrichment score for each of the 10192 BP GO terms
  - # Unique annotations $\propto$ GO enrichment significance!

![fig2a-b](assets/img/srep04191-f2a-b.jpg)

---

## Perhaps the problem isn't quite so bad after correcting for multiple testing...

- Multiple testing correction alone is not enough to deal with this bias,
  although it does seem to severely reduce the problem.

![figs2](assets/img/srep04191-fs2.jpg)

---

## Annotation Enrichment Analysis (AEA)

![fig3](assets/img/srep04191-f3.jpg)

---

## Limitations

- Performance of AEA only compared with Fisher's Exact Test (FET); how does the
  performance comparew to other GO methods?
- Only looked at Biological Process ontology -- is the picture the same for the
  other GO sub-ontologies?
- Currently only implemented in C++ (R bindings would be nice.)

---.references

## References





- Kimberly Glass, Michelle Girvan,   (2014) Annotation Enrichment Analysis: an Alternative Method For Evaluating The Functional Properties of Gene Sets.  <em>Scientific Reports</em>  <strong>4</strong>  <a href="http://dx.doi.org/10.1038/srep04191">10.1038/srep04191</a>
- D. W. Huang, B. T. Sherman, R. A. Lempicki,   (2008) Bioinformatics Enrichment Tools: Paths Toward The Comprehensive Functional Analysis of Large Gene Lists.  <em>Nucleic Acids Research</em>  <strong>37</strong>  1-13  <a href="http://dx.doi.org/10.1093/nar/gkn923">10.1093/nar/gkn923</a>
- A. Subramanian, P. Tamayo, V. K. Mootha, S. Mukherjee, B. L. Ebert, M. A. Gillette, A. Paulovich, S. L. Pomeroy, T. R. Golub, E. S. Lander, J. P. Mesirov,   (2005) Gene Set Enrichment Analysis: A Knowledge-Based Approach For Interpreting Genome-Wide Expression Profiles.  <em>Proceedings of The National Academy of Sciences</em>  <strong>102</strong>  15545-15550  <a href="http://dx.doi.org/10.1073/pnas.0506580102">10.1073/pnas.0506580102</a>

