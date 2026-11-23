# APOE RNA-seq Data Challenge

Bulk RNA-seq analysis of how APOE line and neddylation inhibition relate to transcription in human brain organoids. This repository is based on Harisa Muratovic-Colic’s original R Markdown, DESeq2 analysis, and KEGG interpretation. The portfolio revision adds explicit MultiQC interpretation, a replicate-level look at selected non-coding loci, and a discussion of what the design and data can establish.

The primary comparison uses untreated organoids and organoids treated for 20 days, all harvested at day 80. The 10-day arm was harvested at day 70, which confounds its treatment comparison with harvest stage. The two lines come from a single donor.

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
