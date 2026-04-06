[← Back to Playbooks](README.md) · [Home](../README.md)

# Domain Lead Playbook — Devices Domain

> **Audience:** The technical lead(s) for the Devices domain.
> You have **Team Maintainer** status and **Admin** access to Devices repos.

---

## Your Role

Once a new repo is created and the team is assigned, you take over.

**What you do:**
1. Manage team membership — add/remove contributors and lead authors
2. Request new repos (via issue form or Org Admin)
3. Configure new repositories — branch protection, settings
4. Oversee the contribution workflow — PRs, reviews, merges

**What you don't do:**
- Create repos yourself (request via issue form or Org Admin)
- Manage Org-level settings (billing, base permissions)

---

## Procedure 1: Managing the Devices Team

### Adding a Contributor

1. Go to **[github.com/orgs/IHE/teams/devices-domain/members](https://github.com/orgs/IHE/teams/devices-domain/members)**
2. Click **"Add a member"**
3. Search for their GitHub username
4. Click **"Add"** — they'll receive an invitation

> They're added as a "Member" by default. Only promote to "Maintainer" if they should also manage the team roster.

### Removing a Contributor

1. Same team page → Members
2. Find the person → click the dropdown → **Remove from team**

---

## Procedure 2: Requesting a New Repository

1. Go to **[DEV.tooling → New Issue](https://github.com/IHE/DEV.tooling/issues/new/choose)**
2. Select **"New Repository Request"**
3. Fill in the repo name (`DEV.{name}`), template, and description
4. Submit the issue

The automation will create the repo, assign the `dev-co-chairs` team as Admin, and comment on the issue with the new repo URL. The issue closes automatically when done.

> **If the automation is down:** Email an Org Admin with the same details. See the [Org Admin fallback procedure](org-admin-manual-repo-creation.md).

---

## Procedure 3: Configuring a New Repository

After a new repo is created, do these steps:

### 3a. Set Up Branch Protection

> Note: Branch protection rules are not copied from the template — this must be done manually for each new repo.

1. Go to repo **Settings → Branches**
2. Click **"Add branch protection rule"**
3. Branch name pattern: `main`
4. Recommended settings:
   - ☑ Require a pull request before merging
     - Require 1 approval
   - ☑ Require status checks to pass before merging
     - Select the CI build check (e.g., `build`)
   - ☑ Do not allow bypassing the above settings
5. Click **"Create"**

### 3b. Update the README

The template README has placeholders. Replace them with the actual supplement name, description, and any other metadata.

---

## Procedure 4: CI/CD — What the Template Gives You

The supplement template comes with a pre-configured GitHub Actions workflow that builds your document automatically.

- **File:** `.github/workflows/publish.yml`
- **Triggers:** Push to `main` and pull requests against `main`
- **What it does:**
  1. Checks out the supplement repo
  2. Fetches the shared IHE theme from [DEV.tooling](https://github.com/IHE/DEV.tooling)
  3. Renders AsciiDoc → HTML (with shared + per-repo CSS)
  4. Renders AsciiDoc → PDF (with shared PDF theme)
  5. Uploads the HTML and PDF as downloadable build artifacts

### Downloading Build Output

1. Go to the **Actions** tab of the repo
2. Click the latest successful workflow run
3. Scroll to **Artifacts** and download `supplement-output`

### Checking Build Status

1. Go to the **Actions** tab of the repo
2. Green check = build succeeded, artifacts available for download
3. Red X = build failed — click into it to read the error log

### Common CI Issues

| Problem | Fix |
|---------|-----|
| Build fails | Click the red X, read the log. Usually an AsciiDoc syntax error or missing file. |

For more detail on how the build system and theming work, see the [Build System](../reference/build-system.md) and [Theming & Styling](../reference/theming.md) reference pages.

---

## Procedure 5: The Contribution Workflow

### Branch Strategy

```
main (protected — no direct pushes)
 └── feature/{description}
 └── fix/{description}
 └── editorial/{description}
```

### Workflow

1. Contributor creates a branch from `main`
2. Contributor makes edits, commits, pushes
3. Contributor opens a Pull Request
4. CI builds automatically — checks the PR
5. You or a reviewer reviews the PR
6. After approval + CI green → merge to `main`
7. CI builds updated HTML + PDF (downloadable from Actions tab)

---

## Quick Reference

| Task | URL |
|------|-----|
| Manage team | `github.com/orgs/IHE/teams/devices-domain/members` |
| Request a new repo | [DEV.tooling → New Issue](https://github.com/IHE/DEV.tooling/issues/new/choose) |
| Repo settings | `github.com/IHE/{repo}/settings` |
| Branch protection | `github.com/IHE/{repo}/settings/branches` |
| CI/CD runs | `github.com/IHE/{repo}/actions` |
