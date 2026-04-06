[← Back to Playbooks](README.md) · [Home](../README.md)

# Org Admin Playbook — Devices Domain

> **Audience:** Non-technical IHE staff with GitHub "Owner" access on the IHE org.
> Everything here uses the GitHub web interface. No code, no command line.

---

## Your Role

You are responsible for the organizational infrastructure that supports the Devices domain on GitHub. Most day-to-day repo creation is **automated** — but you handle one-time setup, troubleshooting, and fallback procedures.

**What you do:**
1. Maintain the GitHub App and automation that creates repos
2. Create repos manually if the automation is unavailable (see [Manual Repo Creation](org-admin-manual-repo-creation.md))
3. Create new teams when needed (one-time setup)

**What you don't do:**
- Manage who is on the Devices team (that's the Domain Lead's job)
- Configure CI/CD, branch protection, or any technical settings (baked into the template)
- Write or review document content

---

## How New Repos Are Created (Automated)

New repositories are created automatically when someone opens a **"New Repository Request"** issue on the [DEV.tooling](https://github.com/IHE/DEV.tooling) repo.

### What happens:

1. A Domain Lead or Lead Author opens the issue form at [DEV.tooling → New Issue](https://github.com/IHE/DEV.tooling/issues/new/choose)
2. They fill in the repo name, template, and description
3. A GitHub Actions workflow automatically:
   - Creates the repo from the selected template
   - Assigns the `dev-co-chairs` team as Admin
   - Comments on the issue with the new repo URL
   - Closes the issue

### Your responsibility:

- **Monitor for failures.** If a workflow fails, check the [Actions tab on DEV.tooling](https://github.com/IHE/DEV.tooling/actions) to see what went wrong.
- **Fall back to manual creation** if the automation is broken. See [Manual Repo Creation](org-admin-manual-repo-creation.md).
- **Maintain the GitHub App.** See [GitHub App Setup & Maintenance](org-admin-github-app.md).

---

## Procedure: Creating the Devices Team (One-Time Setup)

> You only need to do this once.

1. Go to **[github.com/orgs/IHE/new-team](https://github.com/orgs/IHE/new-team)**
2. **Team name:** `Devices-Domain`
3. **Description:** "IHE Devices domain team"
4. **Visibility:** Visible
5. Click **"Create team"**
6. **Add the Domain Lead:** Team page → Members → "Add a member" → search by username → Add
7. **Promote to Maintainer:** Find them in the Members list → click the role dropdown → change to **"Maintainer"**

> "Maintainer" lets the Domain Lead add/remove team members on their own.

---

## Quick Reference

| Template | When to use |
|----------|-------------|
| `DEV.supplement-template` | New supplement document |

---

## Troubleshooting

**"The automation failed"**
→ Check the [Actions tab on DEV.tooling](https://github.com/IHE/DEV.tooling/actions). Click the failed run to see the error. If it's a permissions issue, see [GitHub App Setup & Maintenance](org-admin-github-app.md). If you can't fix it, [create the repo manually](org-admin-manual-repo-creation.md).

**"Someone asks me to add them to a team"**
→ Redirect them to the Domain Lead. You only manage Org-level things, not team membership.

---

## Related Pages

- [Manual Repo Creation (Fallback)](org-admin-manual-repo-creation.md) — step-by-step for creating repos without automation
- [GitHub App Setup & Maintenance](org-admin-github-app.md) — how the automation works and how to maintain it
