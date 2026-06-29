[← Back to Org Admin Playbook](org-admin.md) · [Home](../README.md)

# GitHub App Setup & Maintenance

The automated repo creation workflow is powered by a GitHub App called **"IHE Devices Automation"**. This page documents how it was set up and how to maintain it.

---

## How It Works

1. Someone opens a "New Repository Request" issue on [DEV.tooling](https://github.com/IHE/DEV.tooling)
2. The issue has a `repo-request` label applied automatically by the form template
3. A GitHub Actions workflow detects the new issue
4. The workflow generates a short-lived token from the GitHub App (no personal access tokens)
5. **It checks that the issue author is an active member of `dev-co-chairs`.** If not, it comments, closes the issue, and stops — no repo is created.
6. Using that token, it creates the repo, assigns teams, fills in template placeholders, seeds team membership, comments on the issue, and closes it

**Teams assigned to each new repo:**

| Team | Repo role | How |
|------|-----------|-----|
| `dev-co-chairs` | Admin | Existing team, granted Admin |
| `{name} maintainer` | Maintain | **Created** as a child of `devices-domain` |
| `{name} writer` | Write | **Created** as a child of `devices-domain` |

`{name}` is the repo name without the `DEV.` prefix (e.g. `DEV.WIA` → team names `WIA writer`, `WIA maintainer`; GitHub slugs `wia-writer`, `wia-maintainer`). Creating teams and managing their repo access requires the app's **Organization → Members: Read and write** permission (already granted). If the `devices-domain` parent team doesn't exist, the per-repo teams are still created but without a parent, and the run logs a warning. See the [Teams reference](../reference/teams.md) for the model.

**Also at creation time, the workflow:**

- **Fills in template placeholders.** `{{double-brace}}` tokens in `README.md` and `AsciiDoc_Source/metadata.adoc` are replaced — the title and description from what we know, and anything unknown (status, revision, date, editor) with a `TODO:` marker so authors can spot them.
- **Seeds team membership.** The requester is added as a **maintainer of both** new teams (maintainer status doesn't inherit from the parent, so they're added to each directly). Optional **Writers** and **Maintainers** lists from the form are added too — usernames via team membership, emails via org invitation. Each person's result (added / invited / failed) is reported in the issue comment, so a mistyped username is visible and can be retried with [Add People to a Team](https://github.com/IHE/DEV.tooling/issues/new/choose).

The key files are in the [DEV.tooling](https://github.com/IHE/DEV.tooling) repo:
- `.github/ISSUE_TEMPLATE/new-repo-request.yml` — the issue form
- `.github/workflows/create-repo.yml` — the automation workflow

---

## Maintaining the App

### Checking App Status

- **App settings:** Organization Settings → Developer settings → GitHub Apps → IHE Devices Automation
- **Installation:** Organization Settings → Integrations → Installed GitHub Apps → IHE Devices Automation → Configure

### If the Workflow Fails

1. Go to the [Actions tab on DEV.tooling](https://github.com/IHE/DEV.tooling/actions)
2. Click the failed run to see logs
3. Common issues:
   - **"Resource not accessible by integration"** — the app is missing a permission. See "Updating Permissions" below.
   - **"Bad credentials"** — the app's private key may have been rotated or the secret is wrong. See "Rotating the Private Key" below.
   - **"Not Found" on template repo** — the template repo may have been renamed or deleted.

### Updating Permissions

If the app needs new permissions:

1. Go to **Organization Settings → Developer settings → GitHub Apps → Edit**
2. Go to **Permissions & events**
3. Change the needed permissions
4. Click **"Save changes"**
5. Go to **Organization Settings → Integrations → Installed GitHub Apps → Configure**
6. A banner will appear asking to **review and accept** the new permissions — approve it

Current required permissions:

**Repository permissions:**
- Administration: Read and write
- Contents: Read and write
- Issues: Read and write
- Metadata: Read-only

**Organization permissions:**
- Members: Read and write

### Rotating the Private Key

If the private key needs to be replaced:

1. Go to **Organization Settings → Developer settings → GitHub Apps → Edit**
2. Under **Private keys**, click **"Generate a private key"**
3. A new `.pem` file downloads
4. Go to **Organization Settings → Secrets and variables → Actions**
5. Update the `DEVICES_APP_PRIVATE_KEY` secret with the new `.pem` file contents
6. Optionally revoke the old key on the app settings page
7. Delete the `.pem` file from your computer

### Org Secrets

The workflow uses two organization secrets:

| Secret | What it is |
|--------|-----------|
| `DEVICES_APP_ID` | The App ID (a number, found on the app settings page) |
| `DEVICES_APP_PRIVATE_KEY` | The app's private key (contents of the `.pem` file) |

These are stored at: **Organization Settings → Secrets and variables → Actions**

---

## Setting Up for Another Domain

To replicate this automation for another IHE domain:

1. Either reuse the existing app (if org-wide) or create a new one
2. In the new domain's tooling repo:
   - Add an issue template with the domain's templates in the dropdown
   - Add a workflow like `create-repo.yml`, changing the team name and repo prefix
3. Create a `repo-request` label in that repo
4. Ensure the org secrets are accessible to the new repo

For full setup-from-scratch instructions, see [docs/github-app-setup.md in DEV.tooling](https://github.com/IHE/DEV.tooling/blob/main/docs/github-app-setup.md).
