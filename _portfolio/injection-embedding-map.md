---
title: "Prompt-injection map: does UMAP separate safe and malicious prompts?"
excerpt: "A single-cell workflow applied to language-model prompts. 203 synthetic prompts are embedded with a local model, reduced with PCA, and mapped with UMAP to see whether injection attempts and harmless lookalikes fall apart."
collection: portfolio
group: "Tools and software"
order: 5
tags: [Python, LLM, UMAP, PCA, embeddings, AI safety]
header:
  teaser: projects/injection-embedding-map.jpg
teaser_alt: "UMAP of 203 synthetic prompts, with injection attempts as blue circles and benign lookalikes as orange crosses"
---

<a class="project-cta" href="https://github.com/Adrian-I-lab/agent-hook-guards#injection-embedding-map-v03-prototype">View the code on GitHub</a>

**Status: prototype.** Part of [agent-hook-guards](/projects/agent-hook-guards/) from version 0.3.0.

![UMAP of the 203-item synthetic corpus. Left panel coloured by label, 105 injections and 98 benign lookalikes. Right panel coloured by technique, circles for injections and crosses for benign lookalikes.](/images/projects/injection-embedding-map-full.png)

## How the idea came about

Most of my work is single-cell data. Each cell is a long vector of gene counts, and the first move is almost always PCA, then UMAP, to see which cells sit together. While building safety guards for AI coding agents, I started looking at language-model prompts the same way. An embedding model turns each prompt into a long vector of numbers, which is the same shape of data. So I wanted to have a play and see whether the standard single-cell recipe would pull safe prompts and malicious prompts apart, the way it pulls cell types apart.

## What it does

1. **A labelled corpus.** 203 short prompts, all written for this project. 105 are prompt-injection attempts and 98 are hard negatives, benign texts written to look like an attack. Each has one of seven technique labels (for example instruction override, role play, encoded payload) and one of four channels (user message, retrieved document, tool output, file content). Every attack has a harmless goal, such as revealing a fake canary string. None is copied from a public jailbreak list.
2. **Embedding.** Each prompt is embedded with `nomic-embed-text` (768 dimensions) through a local Ollama server. Nothing leaves the machine.
3. **Reduction.** Each dimension is standardised, PCA keeps the first 20 components, and UMAP maps those 20 scores to 2D.
4. **The map.** Two panels, coloured by label and by technique.

## What the map shows

- Injections and lookalikes **partly** separate. Most injections sit to the lower left and most lookalikes to the upper right, but the groups overlap.
- The separation is weak. The silhouette score for injection versus benign is **0.074** in the 20-component PCA space, and **0.172** on the 2D UMAP map. Two dimensions discard most of the variance, so the flat picture looks cleaner than the data are.
- Encoded prompts (pink, top of the right panel) cluster together **whatever their label**. The embedding sees base64 and hex as similar text, whatever they decode to.
- [Guessing] Some of the separation may come from surface features the injections share, such as the fake canary string, rather than from the instruction itself. This has not been tested yet.

## How it is built

- Python 3.13.5, numpy 2.1.3, scikit-learn 1.6.1, matplotlib 3.10.0, umap-learn 0.5.12, numba 0.61.0, pynndescent 0.6.0. Ollama 0.40.2 with `nomic-embed-text`.
- PCA: 20 components, which explain 0.4783 of the variance (PC1 0.0595, PC2 0.0477).
- UMAP: `n_neighbors=15`, `min_dist=0.1`, `metric=euclidean`, `random_state=20261010`. A rerun on the same machine gave identical coordinates.
- The script refuses any embedding server that is not on `localhost`. Embeddings and figures are cached locally and kept out of git.

## A simple detector, for comparison

The same repository has a rule-based tripwire, scored once on a held-out 30% of the corpus (28 injections, 28 lookalikes):

- It flags **21 of 28** injections (recall 0.750, Wilson 95% 0.566 to 0.873).
- It wrongly flags **1 of 28** lookalikes (false-positive rate 0.036, Wilson 95% 0.006 to 0.177).

The map is a picture for exploration. No classifier is trained on the embeddings.

## Limits

- 203 prompts is small, and one author wrote both the injections and the lookalikes.
- The corpus is synthetic. Real attacks are more varied.
- The map comes from one embedding model, one set of UMAP parameters, and one seed.
- It is a prototype. The canary-string effect above is the next thing to test.

MIT licence.
