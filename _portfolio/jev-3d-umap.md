---
title: "JEV in brain organoids: interactive 3D cell atlas"
excerpt: "15,000 single cells from Japanese encephalitis virus-infected and mock human brain organoids. Rotate it, filter cell types, and colour by viral load or interferon response."
collection: portfolio
group: "Interactive figures"
order: 1
tags: [scRNA-seq, virology, R, Seurat, plotly]
header:
  teaser: projects/jev-3d-umap.jpg
teaser_alt: "3D UMAP of brain-organoid cells coloured by cell type"
---

<a class="project-cta" href="/jev-3d-umap/">Open the interactive atlas</a>

![3D UMAP of brain-organoid cells coloured by cell type](/images/projects/jev-3d-umap.jpg)

## What it shows

- **15,000 cells**, a stratified sample of **35,496** single cells from Japanese encephalitis virus (JEV)-infected and mock human brain organoids, profiled by single-cell RNA sequencing.
- **Nine annotated cell types**, from cycling progenitors and radial glia to excitatory and inhibitory neurons, astrocytes, and stressed glia.
- **Four colourings.** Cell type, JEV expression (log1p of JEV counts per million), *INSIG1* expression, and an interferon-stimulated gene (ISG) module score over 53 genes.
- **Filters.** Switch each cell type and each sample group (JEV, Mock) on or off. Tap a cell to see its details.

## Why it exists

It was built for a conference poster. A QR code on the poster opens it on a phone, so a reader can explore the 3D structure that a printed 2D UMAP flattens.

## How it was made

1. Analysed in R (4.5.1) with Seurat (5.5.1).
2. The 3D UMAP is computed from the same integrated embedding as the 2D figures (RPCA integration of SCTransform-normalised data, dimensions 1 to 14, seed 42). Cluster labels are unchanged.
3. The ISG score uses Seurat's `AddModuleScore`.
4. Cells are downsampled with stratification by cell type and sample group, so small populations stay visible and the page stays smooth on a phone.
5. Everything is exported to one self-contained HTML file with plotly.js (2.25.2) inlined. There is no server and no tracking.

## Limits

- The data come from ongoing, unpublished work. Treat the figure as an exploration aid, not a result.
- Cell-type labels are working annotations and may change before publication.
- The page is about 4 MB, so it loads slowly on a weak mobile connection.
