[← Back to Org Admin Playbook](org-admin.md) · [Home](../README.md)

# Manual Repo Creation (Fallback)

> Use this procedure if the automated repo creation workflow is unavailable or broken. Under normal circumstances, repos are created automatically via an issue on [DEV.tooling](https://github.com/IHE/DEV.tooling/issues/new/choose).

---

## What You'll Receive (via Email)

A Domain Lead or Lead Author will send you a request containing:

| Field | Example | Required? |
|-------|---------|-----------|
| **Repository name** | `DEV.wia` | Yes |
| **Which template to use** | `DEV.supplement-template` | Yes |
| **Short description** | "WIA Supplement for Devices" | Yes |

## Step 1: Create the Repository

1. Go to **[github.com/organizations/IHE/repositories/new](https://github.com/organizations/IHE/repositories/new)**

2. Under **"Repository template"**, select the template from the request
   - For supplements: `IHE/DEV.supplement-template`

3. Fill in:
   - **Owner:** `IHE`
   - **Repository name:** the name from the request (e.g., `DEV.wia`)
   - **Description:** the short description from the request
   - **Visibility:** *(follow current policy — TBD)*
   - **Check** ☑ "Include all branches"

4. Click **"Create repository"**

## Step 2: Assign the Team

1. In the new repo, go to **Settings → Collaborators and teams**
   - Direct URL: `github.com/IHE/{repo-name}/settings/access`

2. Click **"Add teams"**

3. Search for **`dev-co-chairs`**

4. Set the role to **"Admin"**

5. Click **"Add"**

## Step 3: Confirm

Reply to the requester with:
- The repo URL: `https://github.com/IHE/{repo-name}`
- Confirmation that the `dev-co-chairs` team has Admin access
- Remind them they can now manage settings and contributors themselves

---

## Email Request Template

Share this with Domain Leads so they know what to send you if the automation is down:

```
Subject: New Devices Domain Repo Request

To: [Org Admin email]

Please create a new repository with the following details:

  Repository name: DEV.__________
  Template: DEV.supplement-template
  Description: ___________________________

Thank you!
```

---

## Troubleshooting

**"I can't find the template in the dropdown"**
→ The template repo might not have "Template repository" checked. Ask the Domain Lead to verify in that repo's Settings → General.

**"The requester says they can't change repo settings"**
→ Check that you assigned the team with the **Admin** role, not Write or Maintain.
