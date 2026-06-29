[← Back to Technical Reference](README.md) · [Home](../README.md)

# Teams, Sub-Teams, and Managers

How GitHub teams work in the IHE org, how the Devices domain uses them, and the exact click-by-click steps for the tasks that aren't obvious in the GitHub UI.

> **Audience:** Domain Leads and team Maintainers. Most of this is done through the GitHub web interface — no command line.

---

## 1. The Mental Model

GitHub has two separate ideas that are easy to confuse. Keep them straight and everything else follows.

### Team membership role: Member vs. Maintainer

Every person on a team is either a **Member** or a **Maintainer**. This controls what they can do *to the team itself* — it has nothing to do with repositories.

| Team role | Can do |
|-----------|--------|
| **Member** | Belong to the team. Inherit whatever repo access the team has. |
| **Maintainer** | Everything a Member can, **plus** add/remove people, promote others to Maintainer, edit the team name/description, and manage child teams. |

A "team Maintainer" is what we mean when we say someone **manages** a team. A team can have more than one Maintainer.

> **This is the part that is not obvious in the GitHub UI** — promoting someone to Maintainer isn't a per-row control; you select the member with a checkbox and use a **"change role"** action at the top of the list. See [§4](#4-making-someone-a-manager-team-maintainer).

### Repository permission: what a team can do to a repo

Separately, a team is granted a **permission level** on each repository it's added to. This controls what the team's members can do *to that repo's content*.

| Repo permission | Can do |
|-----------------|--------|
| **Read** | View and clone. |
| **Triage** | Read + manage issues and PRs (no code write). |
| **Write** | Read + push branches, open/merge PRs (subject to branch protection). |
| **Maintain** | Write + manage most repo settings — **but not** delete the repo, change branch protection, or manage access. |
| **Admin** | Full control, including settings, branch protection, and access. |

**The two are independent.** Someone can be a plain Member of a team that has Admin on a repo (so they get Admin on the repo but can't manage the team). Or a Maintainer of a team that only has Read (they manage the roster but can only read the code).

---

## 2. Parent and Child Teams (Sub-Teams)

Teams can be **nested**. A child team sits under a parent team, and this is supported on the GitHub Free plan.

The single rule worth memorizing:

> **Child teams inherit the parent's repository access. Access flows *downward*, never upward.**

So if `devices-domain` (parent) has Read on a repo, every child team automatically has at least Read on that repo too. But giving a child team Write on some repo does **not** give the parent team anything.

Membership does **not** inherit the same way: being a member of the parent team does not automatically add you to a child team's roster. (The parent's members do, however, inherit the child team's *repo access* through the parent → child access chain only for repos the parent itself is granted.) In practice, treat **rosters as independent per team** and rely on nesting only for the downward repo-access inheritance.

### How the Devices domain uses this

```
devices-domain                     (parent — the whole domain)
 ├── DEV.SDPi  →  SDPi maintainer   (child, repo permission: Maintain)
 │                SDPi writer       (child, repo permission: Write)
 ├── DEV.PCIM  →  PCIM maintainer   (child, Maintain)
 │                PCIM writer       (child, Write)
 └── ... one "maintainer" + "writer" child pair per supplement repo
```

- **`devices-domain`** is the umbrella parent team. The Domain Lead is a Maintainer of it.
- For each supplement repo, two **child teams** are created: `{name} writer` and `{name} maintainer` (where `{name}` is the repo name **without** the `DEV.` prefix — e.g. `DEV.WIA` → team names `WIA writer` and `WIA maintainer`, which GitHub turns into the URL slugs `wia-writer` and `wia-maintainer`).
- **`dev-co-chairs`** is granted **Admin** on every repo (handled by the [repo-creation automation](../playbooks/org-admin-github-app.md)), so the co-chairs always have full control regardless of the per-repo teams.

For new repos, the per-repo child teams are **created and wired up automatically** by the repo-creation workflow. You only do the steps below manually for existing repos, or to fix something up.

---

## 3. The Team-Name → Repo-Role Convention

The last word of the team name **declares the role it is meant to have**, and the repo permission must be set to match. Keeping these aligned is what makes the convention trustworthy — the name tells you the access without having to open settings.

| Team name ending | Intended role | Repo permission to assign |
|------------------|---------------|---------------------------|
| `… writer` | Contributors / lead authors who edit content | **Write** |
| `… maintainer` | People who manage the repo's content workflow | **Maintain** |

> Note: a `… maintainer` team's name refers to the **repo permission** "Maintain" — do **not** confuse it with the **team** role "Maintainer" from [§1](#1-the-mental-model). A maintainer team can still have plain Members and Maintainers of its own.

When you assign a team to a repo ([§6](#6-assigning-a-team-to-a-repo-with-the-right-role)), pick the permission from this table that matches the name. If you ever see a writer team with Admin, or a maintainer team with Read, that's drift — fix it.

---

## 4. Making Someone a Manager (Team Maintainer)

This is the task that's hard to find in the UI. To let another person manage a team (add/remove members, promote others), promote them to **Maintainer**.

> You must already be a **Maintainer** of the team (or an Org Owner) to do this.

1. Go to the team page: **`github.com/orgs/IHE/teams/{team-name}`**
   (e.g. `github.com/orgs/IHE/teams/devices-domain`)
2. Click the **"Members"** tab.
3. Find the person in the list and **tick the checkbox next to their name**.
   - You can select more than one person to change several roles at once.
   - If you don't see checkboxes, you're not a Maintainer of this team — ask an Org Owner or an existing Maintainer.
4. At the **top of the members list**, open the **"change role"** dropdown that appears once someone is selected.
5. Choose **"Maintainer"** (or "Change role to maintainer"). The change takes effect immediately — they now manage the team.

> **If the person isn't on the team yet:** add them first (**"Add a member"** → search username → Add), then follow steps 3–5 to promote them.

To demote: select them the same way → **"change role"** → **"Member"**.

---

## 5. Adding People to a Team (and the Org-Invite Catch)

There's a catch that trips everyone up on our plan:

> **A person can only be on a team if they are a member of the IHE organization — and on the Free plan, only an Org Owner can invite someone to the organization.**

So if you try to add someone who isn't in the IHE org yet, the normal team UI won't let you complete it: you'd first need an Owner to invite them to the org. To get around that bottleneck, we have an **automation** that does the org invite *and* the team add in one step, on behalf of co-chairs.

### Recommended: the "Add People to a Team" automation

Use this for **anyone**, whether or not they're already in the IHE org.

1. Go to **[DEV.tooling → New Issue](https://github.com/IHE/DEV.tooling/issues/new/choose)**
2. Select **"Add People to a Team"**
3. Enter the **team name** (e.g. `WIA writer`, `WIA maintainer`, or `dev-co-chairs` — capitalization and exact spacing don't matter; it's matched against the real teams)
4. List the **GitHub usernames and/or email addresses**, one per line
5. Submit

The workflow checks you're allowed (see below), then for each person:
- **Already an IHE org member** → added to the team immediately.
- **Not yet a member** → sent an **organization invitation**; they join the team automatically once they accept.

It comments back on the issue with a per-person result and closes the issue.

> **Who can use it:** members of **`dev-co-chairs`**, or a **maintainer of the specific team** you're adding to. Anyone else is declined.
>
> **Eligible teams:** any per-repo team (a child of `devices-domain`) and `dev-co-chairs`.

### Manual alternative (existing org members only)

If the person is **already in the IHE org**, a team Maintainer can add them directly:

1. Team page → **Members** tab → **"Add a member"**
2. Search their username → **Add**

This path **cannot** invite someone who isn't in the org yet — for that, use the automation above (or ask an Org Owner to send the org invite first).

---

## 6. Assigning a Team to a Repo with the Right Role

This grants a whole team access to a repository at a chosen permission level. Do it from the **repo** side (it's the most reliable path in the UI).

> You need **Admin** on the repo (Domain Leads and `dev-co-chairs` have this).

1. Go to the repo: **`github.com/IHE/{repo}`**
2. **Settings** tab → left sidebar **"Collaborators and teams"** (under *Access*).
3. Under **Teams**, click **"Add teams"**.
4. Search for the team (e.g. `WIA writer`) → select it.
5. A **"Role"** dropdown appears → set the permission to **match the team name** per the [convention table](#3-the-team-name--repo-role-convention):
   - `… writer` → **Write**
   - `… maintainer` → **Maintain**
6. Click **"Add … to this repository"**.

To change an existing team's permission later: same page → find the team in the list → use its **Role** dropdown → pick the new level.

> **Doing it from the team side instead:** team page → **"Repositories"** tab → **"Add repository"** → pick the repo → set the role. Same result; use whichever you find easier.

---

## 7. Quick Reference

| I want to… | Where | Key step |
|------------|-------|----------|
| Let someone manage a team | `orgs/IHE/teams/{team}` → Members | Tick their checkbox → **"change role"** → **Maintainer** |
| Add someone (new or existing) to a team | [DEV.tooling → New Issue](https://github.com/IHE/DEV.tooling/issues/new/choose) | **"Add People to a Team"** — handles the org invite too |
| Add an **existing org member** to a team | `orgs/IHE/teams/{team}` → Members | **"Add a member"** → search → Add |
| Give a team access to a repo | `IHE/{repo}` → Settings → Collaborators and teams | **"Add teams"** → set Role to match the name |
| Fix a wrong permission | same as above | Use the team's **Role** dropdown |
| Make a child (sub) team | `orgs/IHE/new-team` | Set **"Parent team"** to `devices-domain` when creating |

### Related pages

- [Domain Lead Playbook](../playbooks/domain-lead.md) — day-to-day team and repo management
- [Org Admin Playbook](../playbooks/org-admin.md) — one-time team creation, org-level setup
- [GitHub App Setup & Maintenance](../playbooks/org-admin-github-app.md) — how new repos get their teams automatically
- [Governance](../governance/README.md) — roles and responsibilities
