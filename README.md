# Scott's Data Science Blog

Source code and generated assets for [swied.com](https://www.swied.com), a
Quarto-based website featuring articles about data science, Python, finance,
and related technical topics.

## Repository layout

```text
.
├── src/        # Quarto configuration, pages, posts, and source assets
├── docs/       # Generated static site published by GitHub Pages
├── notebooks/  # Standalone exploratory Jupyter notebooks
└── setup       # Legacy Conda environment setup notes
```

Make content changes in `src/`. Files under `docs/` are generated build
artifacts and should not be edited by hand.

## Prerequisites

To build and preview the website, install the
[Quarto CLI](https://quarto.org/docs/get-started/).

Python and Jupyter are only required when adding a computational article or
re-running code cells. Existing posts use frozen execution results, so a normal
site render can reuse the cached results without executing their Python code.
The packages needed to re-run a post depend on that post; its imports are the
authoritative source. The root-level `setup` file contains historical Conda
setup notes, but it is not a locked or reproducible environment definition.

## Local development

Clone the repository and start Quarto's development server:

```bash
git clone git@github.com:swied/swied-website.git
cd swied-website
quarto preview src
```

Quarto prints the local preview URL and watches the source files for changes.
Stop the server with `Ctrl+C`.

To produce a complete static build:

```bash
quarto render src
```

The project configuration writes the rendered site to `docs/`.

## Adding or updating content

Articles live in their own directories under `src/posts/`. A typical article
contains a `.qmd` source file, a listing image, and any resources referenced by
the article:

```text
src/posts/article-slug/
├── article-slug.qmd
├── image.png
└── resources/
```

Use the existing posts as examples for front matter, including the title,
description, author, date, categories, and image. Shared post behavior—such as
frozen computation, title banners, and Giscus comments—is configured in
`src/posts/_metadata.yml`.

After changing content:

1. Run `quarto render src`.
2. Review the site with `quarto preview src` and check the affected pages.
3. Run `git diff --check` to catch whitespace errors.
4. Commit both the source changes in `src/` and the generated changes in
   `docs/`.

To intentionally re-run computational cells rather than use frozen results,
make sure the article's Jupyter kernel and Python dependencies are installed,
then render with Quarto's execution enabled:

```bash
quarto render src --execute
```

Some articles call external services, so re-execution may also require network
access and can produce results that differ from the committed output.

## Deployment

The repository is structured for GitHub Pages to publish the `docs/` directory,
with the custom domain recorded in `docs/CNAME`. Once rendered output is merged
into the publishing branch, GitHub Pages serves the updated static files.

Repository maintainers should verify that **Settings → Pages** is configured to
deploy from the intended branch and the `/docs` folder.

## Contributing

For a focused, reviewable change:

1. Create a branch from the latest default branch.
2. Edit source files under `src/` or notebooks under `notebooks/`.
3. Render and inspect the site when the change affects published content.
4. Keep generated `docs/` changes in sync with their source changes.
5. Open a pull request describing the change and how it was validated.

There is currently no automated test suite, so a successful Quarto render and
visual inspection are the primary validation steps.

## License

This repository does not currently declare a software or content license.
