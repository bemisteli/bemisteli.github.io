# bemisteli.github.io

Source for my personal research website, built with [Quarto](https://quarto.org) and deployed
to GitHub Pages.

## Editing

Content lives in four Markdown files. Edit, commit, push — GitHub renders and deploys.

| File | Page |
| --- | --- |
| `index.qmd` | Home / about |
| `research.qmd` | Projects |
| `publications.qmd` | Publication list |
| `cv.qmd` | CV and contact |
| `_quarto.yml` | Navigation, title, theme |
| `styles.scss` | Colours and typography |

## Local preview (optional)

Not required — the site builds on GitHub. If you want to see changes before pushing,
[install Quarto](https://quarto.org/docs/get-started/) and run:

```bash
quarto preview
```

## Deployment

`.github/workflows/publish.yml` installs Quarto on a GitHub runner, renders the site, and
deploys it. Repository **Settings → Pages → Source** must be set to **GitHub Actions**.

The original tutorial this site started from is kept at
[`docs/quarto-tutorial.md`](docs/quarto-tutorial.md) (by Catalina Albury).
