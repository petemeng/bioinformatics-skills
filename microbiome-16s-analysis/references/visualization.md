# Visualization Defaults

## Contents

- General Style
- ggplot2 Theme
- Palettes
- Microbiome Plot Types
- Export Defaults
- Heatmap Defaults

## General Style

- Use ggplot2 for most R-based microbiome figures.
- Prefer clean white backgrounds, visible axes, no minor grid clutter, and readable labels.
- Keep group colors and factor order stable across all figures in one project.
- Export vector PDF for manuscripts and SVG/PNG when collaborators need editable or preview formats.
- Save the plotting table alongside final figures.

## ggplot2 Theme

Use a compact publication theme as a starting point:

```r
theme_microbiome <- function(base_size = 10, bg = "white") {
  ggplot2::theme_classic(base_size = base_size) +
    ggplot2::theme(
      rect = ggplot2::element_rect(fill = bg),
      panel.background = ggplot2::element_rect(fill = "transparent", color = "black"),
      panel.border = ggplot2::element_rect(fill = "transparent", color = "transparent"),
      panel.grid = ggplot2::element_blank(),
      axis.text = ggplot2::element_text(color = "black"),
      axis.title = ggplot2::element_text(color = "black"),
      legend.title = ggplot2::element_blank(),
      legend.key = ggplot2::element_rect(fill = "transparent", color = "transparent")
    )
}
```

For faceted plots, use light strip backgrounds and bold strip labels only when it improves scanning.

## Palettes

Good defaults:

```r
pal_okabe_ito <- c(
  "#0072B2", "#D55E00", "#009E73", "#CC79A7",
  "#E69F00", "#56B4E9", "#F0E442", "#000000"
)

pal_taxa_16 <- c(
  "#4E79A7", "#F28E2B", "#E15759", "#76B7B2", "#59A14F",
  "#EDC948", "#B07AA1", "#FF9DA7", "#9C755F", "#BAB0AC",
  "#D37295", "#A0CBE8", "#FFBE7D", "#8CD17D", "#F1CE63",
  "#86BCB6"
)

pal_treatment_two_group <- c(Control = "#8DD3C7", Treatment = "#FB8072")
pal_enrichment_two_group <- c(Baseline = "#C5C5C5", Enriched = "#FA5471")
```

For heatmaps, a blue-white-red z-score palette is acceptable when centered at zero:

```r
colorRampPalette(c("navy", "white", "firebrick3"))(100)
```

## Microbiome Plot Types

Alpha diversity:

- Use boxplots or violins plus points.
- Facet by a major design variable when comparing treatments within groups.
- Add Wilcoxon or model-based annotations only when the test matches the design.

Beta diversity:

- PCoA: color by main group, shape by treatment or batch, draw zero axes, and report axis variance.
- PERMANOVA annotation: include formula term, R2, and p-value.
- Ellipses should be used only when each group has enough samples.

CAP/db-RDA:

- Use constrained axes and annotate term-level R2/p-values from permutation tests.
- Use the same color mapping as PCoA figures in the same project.

Taxonomic composition:

- Use stacked bars for top taxa plus `Others`.
- Facet by treatment or group when it improves comparison.
- Use y-axis label `Relative Abundance`.

LEfSe/LDA bars:

- Use horizontal bars ordered by LDA score.
- Use two clear colors for enriched groups.
- Use dynamic figure height based on the number of significant taxa.

## Export Defaults

Starting sizes:

- Alpha diversity: `12 x 10 cm`.
- PCoA: `12 x 7 cm`.
- CAP single panel: about `6 x 5 in`.
- Family composition: `6 x 6 in`.
- Genus composition: `8 x 6 in`.
- STAMP-style comparison: `16 x 8 cm` to `16 x 12 cm`.
- LEfSe bars: width `12 cm`, height `2 + 0.4 * n_features`, clamped to a practical range.

Adjust dimensions when label count changes; never let labels overlap silently.

## Heatmap Defaults

- Display group-mean relative abundance, not raw counts, unless the caption states otherwise.
- Apply row-wise z-score for pattern heatmaps.
- Fix column order to match the experimental design.
- Use row clustering, then optionally cut clusters into 4-6 groups.
- Add column annotations for treatment/group and row cluster annotations when helpful.
- Export the heatmap and a CSV containing feature IDs, taxonomy labels, cluster IDs, and displayed values.
