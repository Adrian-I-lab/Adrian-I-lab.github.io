---
title: "Diet and mucosal CD4 T cells: interactive 3D UMAP"
excerpt: "22,810 mucosal CD4 T cells from a dietary model, in eight annotated states from Th0 and activated cells to regulatory and follicular helper-like cells."
collection: portfolio
group: "Interactive figures"
order: 2
tags: [scRNA-seq, immunology, R, plotly]
header:
  teaser: projects/diet-3d-umap.jpg
teaser_alt: "3D UMAP of mucosal CD4 T cells coloured by cell state"
---

<a class="project-cta" href="https://adrian-i-lab.github.io/dietmodel-3d-umap.io/">Open the interactive UMAP</a>

![3D UMAP of mucosal CD4 T cells coloured by cell state](/images/projects/diet-3d-umap.jpg)

## What it shows

- **22,810 mucosal CD4 T cells** from my PhD work on how diet shapes T cell development.
- **Eight annotated states.** Th0, *Il31ra*-high Th0, *Stat1*-high Th0, early activated T cells, natural Tregs, active Tregs, cycling T cells, and Tfh-like cells.
- Rotate and zoom in 3D. Click a state in the legend to hide or show it.

## How it was made

- Built in R with the plotly package and published as a standalone HTML page on GitHub Pages ([source repository](https://github.com/Adrian-I-lab/dietmodel-3d-umap.io)).
- Made on 2025-02-22.

## Limits

- The layout is fixed at phone size, so it looks small on a desktop screen.
- It colours by cell state only. There is no gene-expression view.
- The data are from unpublished PhD work. Labels are working annotations.
- The page is about 6 MB.
