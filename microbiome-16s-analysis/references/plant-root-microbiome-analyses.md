# Plant Root Microbiome Analyses

Use this reference when a 16S or plant microbiome dataset needs analysis beyond the baseline workflow in `16s.md`. This file is an analysis menu: choose the modules supported by the project data and combine them into a complete workflow. Do not treat it as a reproduction recipe.

Patterns here include applicable ideas extracted from `https://github.com/Zhenghong-Wang/Root-hair-mutants-microbiome`. The cloned repository did not include an obvious license file, so reuse analysis ideas, structure, and parameter logic; do not copy large code blocks into public outputs.

## How To Use On A New Dataset

1. Start with `references/16s.md`: validate feature table, taxonomy, metadata, sample IDs, representative sequences/tree when available, and compartment rules.
2. If metadata contains both root and rhizosphere samples, split by compartment first, then rarefy and run standard downstream analyses within each compartment unless the user explicitly asks for a combined cross-compartment model.
3. Inspect metadata for `compartment`, `group`, `condition`, `treatment`, `genotype`, `inoculation`, `isolate`, and batch/block fields.
4. Select only modules with required inputs. For example, betaNTI/RCbray needs null-model outputs or enough data to generate them; network hubs need node-property tables; multiomics modules need KO/metabolite tables.
5. For every chosen module, output analysis tables, figure files, statistical summaries, and a short methods note with filtering, normalization, formulas, seeds, and package versions.

## Core Design Conventions

- Keep biological compartments explicit: root, rhizosphere, endosphere, and bulk soil should not be silently mixed.
- Use fixed factor levels before ordination, composition plots, faceting, and statistics.
- Build combined factors such as `condition_compartment_genotype` only when the question requires cross-compartment comparison or a single multi-factor display.
- Expand abbreviations in final plots: `Rhi` to `Rhizosphere`, `En` to `Endosphere`, `DT` to `Drought`, and `WT` to `Water` when those labels are used.

## Baseline Plus Plant-Root Modules

Run these after the standard 16S outputs are available:

- Alpha diversity: richness/Shannon/Simpson-style indices by compartment, genotype, condition, and treatment; use Wilcoxon/Kruskal/ANOVA models according to design.
- Beta diversity: Bray-Curtis PCoA, CAP/db-RDA when constrained effects are needed, PERMANOVA with `adonis2`, and pairwise Adonis for targeted contrasts.
- Composition: phylum/family/genus stacked bars, top-taxa panels, compartment-separated panels, and relative abundance tables.
- Biomarkers and differential taxa: LEfSe-style family biomarkers, Wilcoxon/DESeq2/ANCOM-style ASV or taxa contrasts, and heatmaps for significant ASVs.

## Cross-Factor PCoA And PERMANOVA

Use this module for drought/water, genotype, condition, and compartment interaction views:

- Filter zero-sum features before distance calculation.
- Rarefy within the plotted dataset when using rarefaction-dependent Bray-Curtis workflows.
- Compute Bray-Curtis distance and PCoA with `wcmdscale()` or an equivalent ordination function.
- Test `adonis2(distance ~ betaGroup, permutations = 999)` for a combined display, or use formulas such as `distance ~ condition * genotype` within a compartment.
- Join ordination coordinates to metadata and map color/shape to stable biological fields.
- Put PERMANOVA R2 and p-value in the figure caption or companion statistics table.

## Community Assembly

Use this only when betaNTI and RCbray inputs can be generated or are already available:

- Plot betaNTI as boxplot plus jitter by genotype or treatment.
- Add dashed reference lines at `+2` and `-2`.
- Facet by condition and compartment.
- Summarize RCbray process calls into stacked percentage bars by genotype, condition, and compartment.
- Use process labels such as homogeneous selection, heterogeneous selection, dispersal limitation, homogenizing dispersal, and drift when present.
- Report the null-model method and thresholds; do not interpret community assembly plots without those details.

## Differential ASV Overview

Use this when an interpretable ASV-level summary across taxa is needed:

- Start from differential ASV results, abundance table, taxonomy, and metadata.
- Filter to the target condition, compartment, genotype/treatment contrast, and taxonomic rank.
- Calculate relative abundance for the displayed ASVs within the plotted subset.
- Plot ASVs ordered by family or another stable rank on the x-axis and `-log10(p)` or `-log10(FDR)` on the y-axis.
- Encode relative abundance by point size and enrichment/depletion/non-significant status by shape or color.
- Use repelled labels for significant ASVs and alternating family background blocks for dense plots.
- Highlight biologically relevant families such as Rhizobiaceae, Pseudomonadaceae, Comamonadaceae, Oxalobacteraceae, Flavobacteriaceae, and Chitinophagaceae when relevant.

## Network Hubs

Use this when a co-occurrence or association network has already been built:

- Join node centrality metrics with taxonomy.
- Keep network construction parameters separate from node-property visualization.
- Compare degree, closeness, and betweenness across genotype/treatment groups within a condition and compartment.
- Nominate hub candidates using high degree plus high closeness or high betweenness.
- A practical exploratory cutoff is a high quantile, such as 0.9, from a fitted distribution for each centrality metric.
- Plot degree versus closeness and degree versus betweenness with threshold lines and ASV labels.
- Do not infer direct ecological interaction direction from correlation networks without caveats.

## Multiomics And Isolate Follow-Up

Use these modules only when the required non-16S inputs exist:

- Metagenome: use KO count/abundance tables plus metadata; filter KOs detected in fewer than two samples before ordination; reuse PCoA/PERMANOVA grouping logic.
- Metabolome: use metabolite abundance, class/taxonomy, and metadata; filter low-confidence annotations before composition or PCA; show class shifts with stacked or alluvial plots.
- Beneficial isolates: combine isolate taxonomy, phylogeny, and plant phenotype assays; plot primary root length, shoot fresh weight, survival/ROS rates, and growth curves with raw points plus summary/error.
- Isolate statistics: use t-tests for simple pairwise contrasts and ANOVA plus post hoc tests for multi-isolate screens.

## Plotting Defaults

- Use `theme_classic()` or `theme_bw()` with readable axis text and minimal grids.
- Use boxplots or bar summaries plus jittered points for phenotype, alpha diversity, and betaNTI plots.
- Use `facet_grid(condition ~ compartment)` or separate compartment panels for condition-by-compartment designs.
- Fix factor levels and taxonomic ordering before plotting; never rely on alphabetic order for final figures.
