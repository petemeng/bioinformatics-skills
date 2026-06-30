# Environment And Reproducibility

## Project Layout

Use a portable layout with configurable roots:

```text
project/
  analysis/
  data/
    raw/
    filter_otu/
    renamed_otu/
  results/
    alpha/
    beta/
    composition/
    differential_abundance/
  logs/
  scripts/
```

Do not hard-code personal workstation paths in reusable scripts. Define `project_root`, `data_dir`, and `results_dir` once, then build paths from those variables.

## Remote Upstream, Local Downstream

Many 16S projects are easiest to split across environments:

- Run upstream processing on a server or HPC system: QIIME 2 import, demux summaries, DADA2/Deblur, taxonomy assignment, artifact export, and command logging.
- Transfer exported artifacts/tables to a local workstation for downstream R analysis: table cleanup, ASV renaming, diversity, composition plots, differential abundance, manuscript figures, and a Quarto `.qmd` report.
- Treat server outputs as reproducible upstream artifacts and local R outputs as downstream analysis products.

When writing this workflow, include a transfer checklist:

- Feature table export, usually BIOM plus TSV.
- Taxonomy export.
- Representative sequences FASTA.
- Metadata table.
- Denoising stats and command log when available.
- Optional `.qza`/`.qzv` artifacts for provenance and review.

After transfer, verify file presence, sample ID consistency, and table dimensions before starting downstream analysis.

## Tooling

Common tools and packages:

- QIIME 2 CLI and `biom` for artifact import/export.
- Quarto for local downstream R reports (`.qmd`) when available.
- R packages: `tidyverse`, `data.table`, `vegan`, `ape`, `picante`, `Biostrings`, `ggplot2`, `ggpubr`, `ggsci`, `RColorBrewer`, `patchwork`, `phyloseq`, `microeco`, `pheatmap`, `DESeq2`.
- Optional conflict handling in R: prefer `dplyr::select`, `dplyr::filter`, and `base::setdiff` when using the `conflicted` package.

## Reproducibility Rules

- Record QIIME 2 version, classifier database/version, manifest format, and denoising parameters.
- Record R package versions with `sessionInfo()` or `sessioninfo::session_info()`.
- Keep random seeds explicit for rarefaction, ordination, bootstrapping, or stochastic algorithms.
- Save command logs and analysis summaries for read depth, ASV count, sample count, filtering thresholds, and rarefaction depth.
- Prefer a project-level Quarto `.qmd` for local downstream R analysis and render it to HTML when Quarto is installed.
- Write tables in CSV/TSV with clear row/column identifiers.
- Avoid overwriting final outputs without explicit versioned filenames or user confirmation.

## Privacy Rules

- Do not publish raw reads, metadata with sensitive identifiers, unpublished project names, server paths, tokens, credentials, or private scripts.
- Before publishing, scan the repository for local paths, usernames, server names, emails, sample identifiers, and raw data extensions.
- Keep private source notebooks and scripts outside the public skill repository.
