---
title: "quarto-bioinfo-report-template: analysis reports that keep measurement and interpretation apart"
excerpt: "A Quarto book template for bioinformatics analysis reports, with a house style guide, automated style checks, a rendered gallery on synthetic data, and instructions for AI agents."
collection: portfolio
group: "Tools and software"
order: 4
tags: [Quarto, R, Python, reproducibility, scientific writing]
header:
  teaser: projects/quarto-bioinfo-report-template.jpg
teaser_alt: "Terminal output of the template's style check runner, a table of tests and five checks with zero errors, ending in check_style: PASS"
---

<a class="project-cta" href="https://github.com/Adrian-I-lab/quarto-bioinfo-report-template">View the code on GitHub</a>

![Terminal output of the template's style check runner, a table of tests and five checks with zero errors, ending in check_style: PASS](/images/projects/quarto-bioinfo-report-template.jpg)

## What it does

This is a GitHub template repository. Click **Use this template** to start a new analysis report as a [Quarto](https://quarto.org) book.

It ships:

- a starter book, with chapter skeletons, a synthesis chapter, and reference pages;
- a gallery of ten chapters that shows every element on synthetic data;
- a style guide, and five automated style checks with tests;
- a scaffold script that names the book and sets its chapters;
- instructions and five ready prompts for AI agents.

## Why it exists

An analysis report is an argument. The house style keeps measurement and interpretation apart. Every chapter before the synthesis describes and counts. One labelled chapter interprets. The detection floor comes before any result, numbers in prose are computed rather than typed, and a claim of no difference needs an equivalence result or the detection floor beside it.

Rules like these drift unless something checks them. So the template checks them.

## How it is built

- Quarto 1.9.37, with R for the starter chapters and an optional Python chapter in the gallery.
- Shared helpers and analysis parameters live in `_setup.R`, `_setup.py`, and `_params.yml`, and every chapter reads them.
- Five style checks, in Python with the standard library only:
  - a register guard, for interpretive phrasing outside the synthesis;
  - an absence guard, for "no difference" claims without support;
  - a prose linter;
  - a caption check;
  - a synthesis check.
- An opt-out needs a written reason, or it is itself a violation.

## How it is tested

Run with `bash scripts/check_style.sh`. On 2026-10-10 it passed with these results.

- **72 Python tests** and **15 R tests**, with 0 failures and 0 skipped.
- The five checks examined between 2 and 30 files each, with 0 errors and 1 warning.
- Each check has tests that show it failing on a planted defect and passing on the fix.
- The runner fails if any step examined nothing, so an empty run cannot report a pass.

## Limits

- Users who only write Python still need R. The starter chapters and several gallery chapters run R code.
- CI has not completed a run yet. Only Linux and macOS have been tried.
- The citations in the example bibliography have not been verified.
- The checks read prose, not methods. They cannot tell a typed number from a computed one, or a wrong statistic from a right one.
- The register guard is a heuristic over English, and its false-positive rate on real report prose is unknown.
- It is a rough prototype (v0.1) with no outside users yet.

MIT licence.
