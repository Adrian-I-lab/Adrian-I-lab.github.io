# adrian-i-lab.github.io

Source for [adrian-i-lab.github.io](https://adrian-i-lab.github.io/), the personal site of Adrian Ilich, Research Officer (Bioinformatician) in Systems Virology at QIMR Berghofer.

The site holds a short profile, a CV, publications, and interactive figures such as the [JEV brain-organoid 3D UMAP](https://adrian-i-lab.github.io/jev-3d-umap/).

## Build locally

The site is built with Jekyll from the [Academic Pages](https://github.com/academicpages/academicpages.github.io) template (MIT licence, see `LICENSE`).

```bash
bundle install
bundle exec jekyll serve -l -H localhost
```

Then open `http://localhost:4000`. GitHub Pages rebuilds the live site on every push to `main`.

## Layout

- `_config.yml` holds the site identity and sidebar.
- `_pages/` holds the About (`about.md`), CV (`cv.md`), and Publications pages.
- `_publications/` holds one Markdown file per paper.
- `jev-3d-umap/` is a self-contained interactive Plotly page.
