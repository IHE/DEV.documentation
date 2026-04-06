[← Back to Reference](README.md) · [Home](../README.md)

# Build System

Every supplement repo has a CI/CD workflow that automatically builds HTML and PDF versions of the document. This page explains how it works.

---

## Overview

The build is a GitHub Actions workflow defined in `.github/workflows/publish.yml` in each supplement repo. It runs automatically on:

- **Every push to `main`** — builds the latest version
- **Every pull request against `main`** — builds the PR to check for errors

The workflow is included in the [DEV.supplement-template](https://github.com/IHE/DEV.supplement-template), so every new supplement repo gets it automatically.

---

## What the Build Produces

| Output | Format | How to Access |
|--------|--------|--------------|
| `index.html` | Rendered HTML with CSS styling | Download from the Actions tab |
| `combined.css` | The stylesheet (base + custom) | Included in the artifact |
| `supplement.pdf` | Formatted PDF with headers/footers | Download from the Actions tab |

### Downloading Build Artifacts

1. Go to the supplement repo on GitHub
2. Click the **Actions** tab
3. Click the latest successful workflow run
4. Scroll to **Artifacts** and download `supplement-output`
5. Unzip to get the HTML and PDF files

Build artifacts are retained for 90 days.

---

## What Happens During a Build

1. **Checkout** — the workflow checks out the supplement repo's code
2. **Fetch shared theme** — checks out the `theme/` directory from [DEV.tooling](https://github.com/IHE/DEV.tooling) (see [Theming & Styling](theming.md))
3. **Install tools** — installs Ruby, Asciidoctor, and Asciidoctor PDF
4. **Prepare theme** — concatenates the base CSS with any per-repo custom CSS
5. **Build HTML** — Asciidoctor renders the AsciiDoc source to HTML with the combined stylesheet
6. **Build PDF** — Asciidoctor PDF renders to PDF using the base PDF theme
7. **Upload artifacts** — the HTML and PDF are uploaded as downloadable artifacts

---

## Build Tools

| Tool | Version | Purpose |
|------|---------|---------|
| [Asciidoctor](https://asciidoctor.org/) | Latest | Renders `.adoc` files to HTML |
| [Asciidoctor PDF](https://docs.asciidoctor.org/pdf-converter/latest/) | Latest | Renders `.adoc` files to PDF |
| Ruby | 3.3 | Runtime for Asciidoctor |

These are installed fresh on each build by the GitHub Actions workflow. You don't need to manage versions.

---

## Local Preview

You can build locally without waiting for CI. The supplement template includes a `Makefile`:

```bash
make html          # Build HTML
make pdf           # Build PDF
make all           # Build both
make clean         # Remove build output
```

**Requirements:** Ruby and the `asciidoctor` and `asciidoctor-pdf` gems:
```bash
gem install asciidoctor asciidoctor-pdf
```

Or use Docker (no local Ruby needed):
```bash
make docker-html
make docker-pdf
```

> **Note:** Local builds don't fetch the shared theme from DEV.tooling. For a local preview with the full theme, clone DEV.tooling and copy the theme files manually, or just rely on CI for themed output.

---

## Troubleshooting

**Build fails with "input file is missing"**
→ Check that `AsciiDoc_Source/main.adoc` exists and the path hasn't changed.

**Build fails with AsciiDoc syntax error**
→ Click into the failed workflow run on the Actions tab and read the log. Asciidoctor prints the file and line number of the error.

**Build succeeds but output looks wrong**
→ Check the shared theme in DEV.tooling — a recent change there may have introduced an issue. Also check your per-repo `custom.css` for conflicting rules.

**Build artifacts are missing**
→ Artifacts are only uploaded on pushes to `main`, not on PRs. Check that you're looking at a workflow run triggered by a push, not a PR.
