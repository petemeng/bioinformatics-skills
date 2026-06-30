---
name: microbiome-16s-analysis
description: Reusable 16S amplicon and microbiome analysis workflows. Use when Codex helps plan, write, review, or troubleshoot QIIME 2, DADA2, ASV/OTU table cleanup, taxonomy processing, alpha diversity, beta diversity, PERMANOVA, CAP/db-RDA, taxonomic composition plots, LEfSe-style biomarker analysis, differential abundance, DE-ASV heatmaps, Quarto/Qmd downstream reports, or publication-ready microbiome figures.
---

# Microbiome 16S Analysis

Use this skill to build reproducible 16S microbiome analysis workflows without relying on private project code. Prefer the active project's files and metadata over these defaults.

## Reference Map

Read only the relevant reference files:

- For QIIME 2, DADA2, ASV/OTU tables, taxonomy parsing, diversity, ordination, composition, LEfSe, differential abundance, or Quarto/Qmd downstream reports, read `references/16s.md`.
- For plant/root microbiome modules beyond the baseline 16S workflow, including root/rhizosphere/endosphere designs, drought or treatment-by-compartment-by-genotype PCoA/PERMANOVA, betaNTI/RCbray community assembly, differential ASV taxonomy overview plots, network hubs, metagenome/metabolome integration, or beneficial isolate validation, read `references/plant-root-microbiome-analyses.md`.
- For ggplot2, ordination plots, stacked taxonomic bars, LDA bars, heatmaps, palettes, and export dimensions, read `references/visualization.md`.
- For project layout, conda/R environments, reproducibility, privacy-safe paths, and command logging, read `references/environment.md`.

## Working Rules

- Inspect current project files before applying generic assumptions.
- Keep raw data read-only and write derived outputs to explicit result directories.
- Validate sample IDs across feature tables, taxonomy, metadata, representative sequences, and trees before analysis.
- Record filtering thresholds, distance metrics, model formulas, package/tool versions, random seeds, and output paths.
- Prefer a runnable Quarto `.qmd` report as the main local downstream R artifact unless the user asks for a plain script.
- Treat exploratory thresholds as exploratory in reports; distinguish them from final statistical criteria.
- Do not expose private paths, patient/sample identifiers beyond what is already public, tokens, server names, or unpublished project details.

## Public Reuse

This public version intentionally excludes private scripts, notebooks, raw data, and project-specific paths. When adapting it to a real project, replace placeholders with project-local paths and keep sensitive metadata out of published artifacts.
