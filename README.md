# APOE RNA-seq Data Challenge

Bulk RNA-seq analysis of how APOE line and neddylation inhibition relate to transcription in human brain organoids. This repository covers a DESeq2 analysis, KEGG pathways analysis and descriptive RNA analysis. The portfolio revision adds explicit MultiQC interpretation, a replicate-level look at selected non-coding loci, and a discussion of what the design and data can establish.

The primary comparison uses untreated organoids and organoids treated for 20 days, all harvested at day 80. The 10-day arm was harvested at day 70, which confounds its treatment comparison with harvest stage. The two lines come from a single donor.

## Analysis workflow

The diagram shows the original submission workflow.

![Workflow from count and sample QC through DESeq2 contrasts, KEGG enrichment, and exploratory RNA annotation](assets/workflow.png)

[Download the workflow PDF](assets/workflow.pdf)

### What the revision added

- A MultiQC summary and rationale for retaining all 18 libraries, including the different roles of STAR genomic alignment and Salmon transcriptome quantification.
- A [replicate-level ncRNA figure](figures/ncrna-replicate-responses.png) generated in the revised DESeq2 report. It shows the day-80 responses for PKN2-AS1, MIR124-2HG and DUBR using expression values from all three replicates per group.
- A discussion of the day-70 harvest confounding, the single-donor design, and the limits of interpreting bulk expression and RNA annotations.

See the [figure index](figures/README.md) for the PCA, sample-distance heatmap, volcano plots, and KEGG enrichment panels.

## Next steps

A separate exploratory edgeR analysis tested sample-inclusion sensitivity, ran complementary GO enrichment, and compared circRNA host annotations against an expression-adjusted background. Those checks sit outside the DESeq2 workflow in this repository.

- **Check robustness within DESeq2.** Refit using only the day-80 libraries and check how much individual libraries influence the interaction. Obtain processing and sequencing batch details to investigate the shared STAR mapping pattern in APOE4 at day 20.
- **Extend pathway and circRNA host analyses within DESeq2.** Compare directional KEGG enrichment with a complementary gene-set test. Compare circRNA host discoveries against the tested background while accounting for expression and, where possible, gene length and transcript complexity.
- **Resolve individual RNA isoforms.** Obtain transcript-level quantifications for isoform expression or usage, and original sequencing reads for back-splice junction analysis of circular RNA.
- **Check generalizability.** Repeat the matched-harvest experiment in additional donors and independently edited lines.

## Run locally

1. Place the original inputs and circAtlas annotation in [data/](data/README.md).
2. Use R 4.3 with Bioconductor packages DESeq2, apeglm, AnnotationDbi, org.Hs.eg.db, clusterProfiler, edgeR and genefilter. The report also uses rmarkdown, knitr, ggplot2, dplyr, pheatmap, RColorBrewer, ashr, ggrepel, patchwork and xml2.
3. From the repository root, run:

   ```sh
   Rscript -e 'rmarkdown::render("analysis.Rmd", output_format = "html_document", output_dir = "output")'
   ```

Rendering requires Pandoc. A full render queries the KEGG API. For an offline version with the original pathway narrative and the newly computed non-KEGG sections, use:

```sh
Rscript -e 'rmarkdown::render("analysis.Rmd", output_format = "html_document", output_dir = "output", params = list(run_kegg = FALSE))'
```

To extract the executable code for inspection, run `knitr::purl("analysis.Rmd")` in R. The R Markdown is the executable source of the report. For PDF, use `output_format = "pdf_document"` with a LaTeX installation. The source report is [analysis.Rmd](analysis.Rmd). The original submission is available locally in `reference/` when supplied. Raw inputs, rendered analyses and the original submission are kept local pending data-sharing permission.
