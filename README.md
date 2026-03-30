# DEV.documentation

Documentation, playbooks, and governance for the IHE Devices domain on GitHub.

## Viewing the Site

This site is published via GitHub Pages from the `docs/` directory. Once Pages is enabled, it will be available at the GitHub Pages URL for this repo.

## Editing

All content is in `docs/` as plain Markdown files. To make changes:

1. Edit any `.md` file in `docs/` (you can do this directly on GitHub's web editor)
2. Commit to `main`
3. The site rebuilds automatically — no build tools needed

## Structure

```
docs/
├── index.md              ← Home page
├── _config.yml           ← Jekyll site configuration
├── playbooks/
│   ├── index.md          ← Playbooks landing page
│   ├── org-admin.md      ← Org Admin playbook
│   ├── domain-lead.md    ← Domain Lead playbook
│   ├── contributor.md    ← Contributor & Lead Author guide
│   └── reviewer.md       ← Reviewer guide
└── governance/
    └── index.md          ← Governance policies and roles
```
