# Root Hair Mutants Microbiome Paper Patterns

Source repository: `https://github.com/Zhenghong-Wang/Root-hair-mutants-microbiome`

Use this reference for plant root hair mutant microbiome analyses, drought/water comparisons, rhizosphere/endosphere contrasts, community assembly, network hubs, beneficial Rhizobiaceae validation, and paper-style figures inspired by this public repository.

## Reuse Rule

The cloned repository did not include an obvious license file. Reuse the analysis ideas and structure, but do not copy large code blocks into public outputs. Link the source repository when a workflow is derived from it.

## Experimental Design

Common design fields:

- `group`: water/control versus drought, often abbreviated as `WT` and `DT`.
- `compartment`: `Rhizosphere`/`Rhi`, `Endosphere`/`En`, plus bulk-soil controls when present.
- `genotype`: root hair mutant or wild-type genotype.
- `betaGroup`: an explicit combined factor made from condition, compartment, and genotype.

Use fixed factor levels before ordination, composition plots, and faceted figures. Keep compartment explicit; use a combined group only when the figure is intended to compare across compartments.

## PCoA And PERMANOVA

Reusable pattern:

- Filter features with zero total abundance.
- Rarefy within the plotted dataset when using rarefaction-dependent Bray-Curtis workflows.
- Compute Bray-Curtis distance.
- Test `adonis2(distance ~ betaGroup, permutations = 999)` or a project-specific formula.
- Optionally run pairwise Adonis for group contrasts.
- Run PCoA with `wcmdscale()` or equivalent.
- Join coordinates to metadata and map color/shape to stable metadata columns.
- Include PERMANOVA R2 and p-value in title, caption, or companion table.

For root/rhizosphere studies, follow the compartment-aware processing rule in `16s.md` unless the analysis explicitly needs a combined cross-compartment view.

## Community Assembly

For betaNTI:

- Plot betaNTI as boxplot plus jitter by genotype.
- Add dashed lines at `+2` and `-2`.
- Facet by condition and compartment.
- Expand abbreviations such as `Rhi`, `En`, `DT`, and `WT` in final labels.

For RCbray process composition:

- Summarize process calls into proportions by genotype, condition, and compartment.
- Display as stacked percent bars.
- Use clear labels for homogeneous selection, heterogeneous selection, dispersal limitation, homogenizing dispersal, and drift when present.
- Report the null-model method and process-calling thresholds that generated the input files.

## Biomarkers And Differential ASVs

Family-level biomarkers:

- Use a microbiome object such as `microeco::microtable` when the inputs are OTU table, taxonomy table, and metadata.
- Run LEfSe-style biomarker discovery at an explicit taxonomic rank, commonly family.
- Treat LEfSe as exploratory unless the project defines confirmatory thresholds.
- Pair LDA bars with abundance plots when possible.

Differential ASV overview:

- Use differential ASV results, OTU/ASV abundance, taxonomy, and metadata.
- Filter to target condition, compartment, genotype contrast, and taxonomic rank.
- Calculate relative abundance for displayed ASVs in the plotted subset.
- Plot ASVs by taxonomic family/rank on the x-axis and `-log10(p)` or `-log10(FDR)` on the y-axis.
- Encode relative abundance by point size and enrichment/depletion/non-significant status by shape.
- Use repelled labels for significant ASVs and alternating taxonomic background blocks for dense plots.
- Highlight biologically relevant families such as Rhizobiaceae, Pseudomonadaceae, Comamonadaceae, Oxalobacteraceae, Flavobacteriaceae, and Chitinophagaceae when appropriate.

## Network Hubs

For network node-property summaries:

- Join node centrality metrics with taxonomy.
- Collapse non-target families to `Others`.
- Compare centrality distributions across genotype groups within a condition and compartment.
- Use degree plus closeness or degree plus betweenness to nominate hub candidates.
- A practical threshold is a high quantile, such as 0.9, from a fitted distribution for each centrality metric.
- Plot degree versus closeness and degree versus betweenness with threshold lines and labels for hub candidates.

Keep network construction, filtering, and node-property plotting documented separately.

## Metagenome And Metabolome

Metagenome:

- Use KO count/abundance tables with metadata.
- Filter KOs detected in fewer than two samples before ordination.
- Reuse the same PCoA/PERMANOVA grouping logic as 16S.

Metabolome:

- Use metabolite abundance, metabolite class/taxonomy, and metadata.
- Filter low-confidence annotation levels before composition or PCA.
- Summarize metabolite classes by genotype/treatment.
- Use stacked/alluvial composition plots to show class shifts across groups.

## Beneficial Isolate Validation

For Rhizobiaceae or beneficial isolates:

- Combine isolate taxonomy, phylogeny, and plant phenotype assays.
- Use circular or fan trees for isolate phylogeny when Newick trees are available.
- Color isolate tree tips or rings by broad taxonomy.
- Compare buffer/control, wild-type isolate, and mutant isolate treatments under PEG/drought-like conditions.
- Plot primary root length, shoot fresh weight, survival/ROS rates, and bacterial growth curves with raw points plus summary/error.
- Use t-tests for simple pairwise contrasts and ANOVA plus post hoc tests for multi-isolate screens.

## Plotting Conventions

- Use stable genotype colors; a useful plant-root palette is orange, forest green, and steel blue for three genotype groups.
- Use `theme_classic()` or `theme_bw()` with minimal grids.
- Use boxplots or bar summaries plus jittered points for phenotype and betaNTI plots.
- Use `facet_grid(condition ~ compartment)` for microbiome condition-by-compartment figures.
- Fix factor levels before plotting; never rely on alphabetic order for publication panels.
