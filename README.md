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
- `_portfolio/` holds one Markdown file per project. They appear as cards on `/projects/`.
- `images/projects/` holds the card pictures (640 x 480 JPEG).
- `jev-3d-umap/` is a self-contained interactive Plotly page.

## Adding a project page

A project page goes live only after the project's repository is public. Until then, keep `published: false` in its front matter.

1. Create `_portfolio/<slug>.md` with this front matter.

   ```yaml
   ---
   title: "Short name: what it does"
   excerpt: "One or two sentences for the card."
   collection: portfolio
   group: "Tools and software"   # or "Interactive figures", "Teaching"
   order: 1                      # position within its group
   tags: [Python, ML]
   header:
     teaser: projects/<slug>.jpg
   teaser_alt: "What the picture shows"
   published: false              # remove once the repository is public
   ---
   ```

2. Body sections, in this order. What it does, why it exists, how it works (with software versions), how it is tested, limits, links.
3. Add a 640 x 480 picture to `images/projects/`.
4. Build locally and check the card and the page.
5. Remove `published: false` and add a one-line News item on the About page.
