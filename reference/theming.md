[← Back to Reference](README.md) · [Home](../README.md)

# Theming & Styling

All Devices domain supplements share a common visual theme. The theme is maintained centrally so that updates to branding, fonts, or layout propagate to every supplement automatically.

---

## How It Works

There are two layers of styling:

### 1. Base theme (shared across all repos)

The base theme lives in the [DEV.tooling](https://github.com/IHE/DEV.tooling) repository under `theme/`:

| File | What It Controls |
|------|-----------------|
| `theme/ihe-base.css` | HTML output — fonts, colors, tables, layout, admonitions |
| `theme/ihe-base-theme.yml` | PDF output — page size, margins, fonts, headers, footers |

Every supplement repo's CI workflow automatically fetches these files from DEV.tooling at build time. You don't need to copy them into each repo.

### 2. Per-repo custom styles (optional, additive)

Each supplement repo can have its own `AsciiDoc_Source/theme/custom.css` file. This is for styling that is specific to one particular document — for example, special table formatting or diagram conventions unique to that supplement.

**This is true cascading:** the per-repo CSS is appended after the base CSS, so it adds to or overrides specific rules without replacing the entire theme. If the file is empty or missing, the base theme is used as-is.

---

## What Happens at Build Time

When a supplement repo's CI workflow runs (on every push to `main` or PR):

1. The workflow checks out the supplement repo
2. It checks out the `theme/` directory from DEV.tooling (just that directory, via sparse checkout)
3. For **HTML**: the base CSS (`ihe-base.css`) and the per-repo CSS (`custom.css`) are concatenated into a single stylesheet. Base styles come first, per-repo additions come second. The combined stylesheet is applied to the rendered HTML.
4. For **PDF**: the base PDF theme (`ihe-base-theme.yml`) is applied directly by asciidoctor-pdf. It controls page layout, fonts, colors, headers, and footers.
5. The rendered HTML and PDF are uploaded as downloadable build artifacts.

```
DEV.tooling                          Supplement repo
┌──────────────────┐                 ┌──────────────────────────────┐
│ theme/           │   fetched at    │ AsciiDoc_Source/             │
│   ihe-base.css   │──build time──→  │   main.adoc                  │
│   ihe-base-      │                 │   metadata.adoc              │
│     theme.yml    │                 │   theme/                     │
└──────────────────┘                 │     custom.css (optional)    │
                                     └──────────────────────────────┘
                                               │
                                               ▼
                                     ┌──────────────────────────────┐
                                     │ Build output (artifacts)     │
                                     │   index.html (base + custom) │
                                     │   combined.css               │
                                     │   supplement.pdf             │
                                     └──────────────────────────────┘
```

---

## How to Update the Base Theme

If you need to change the shared branding (colors, fonts, layout, headers/footers):

1. Edit `theme/ihe-base.css` (for HTML) or `theme/ihe-base-theme.yml` (for PDF) in [DEV.tooling](https://github.com/IHE/DEV.tooling)
2. Commit and push to `main`
3. The next time any supplement repo builds, it will pick up the changes automatically

No changes are needed in individual supplement repos.

---

## How to Add Per-Repo Styling

If a specific supplement needs custom styling that shouldn't apply to other documents:

1. Edit `AsciiDoc_Source/theme/custom.css` in that supplement's repo
2. Add only the CSS rules you need — don't duplicate the base styles
3. Commit and push

**Example:** If a supplement has wide tables that need more space:

```css
/* Give tables in this supplement more breathing room */
table {
  font-size: 0.85em;
}

th, td {
  padding: 0.4em 0.6em;
}
```

These rules layer on top of the base theme. Everything else (fonts, colors, headings, etc.) comes from the base.

---

## PDF Theme Reference

The PDF theme file (`ihe-base-theme.yml`) uses Asciidoctor PDF's theme format. Key sections:

| Section | What It Controls |
|---------|-----------------|
| `page` | Page size, margins, orientation |
| `base` | Default font, size, color |
| `heading` | Heading font sizes and colors |
| `title-page` | Title page layout |
| `header` | Running header text and styling |
| `footer` | Page numbers and footer text |
| `table` | Table borders and header styling |

For full documentation, see the [Asciidoctor PDF Theming Guide](https://docs.asciidoctor.org/pdf-converter/latest/theme/).

---

## FAQ

**Q: I updated the base theme but my supplement still looks the same.**
A: The supplement needs to rebuild. Push any change to `main` (or re-run the workflow from the Actions tab) to trigger a fresh build that fetches the updated theme.

**Q: Can I completely replace the base theme for one repo?**
A: The per-repo `custom.css` cascades on top of the base — it doesn't replace it. If you need something radically different, you'd modify the workflow in that specific repo. But this should be rare.

**Q: Where do I put logos or custom fonts?**
A: Add them to `theme/` in DEV.tooling. The PDF theme YAML can reference font files and images for headers/footers. Update the workflow if you need additional files copied at build time.
