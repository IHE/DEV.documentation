[← Home](../README.md)

# Governance

Policies, roles, and decision-making for the Devices domain's GitHub presence.

> Many sections below are marked **(TBD)** — these will be filled in as decisions are made.

## Roles and Responsibilities

| Role | Who | Permissions | Responsibilities |
|------|-----|-------------|-----------------|
| **Org Admin** | IHE staff (non-technical) | GitHub Org Owner | Create repos from templates, assign teams, DNS/domain setup |
| **Domain Lead** | Devices domain technical lead | Team Maintainer + Repo Admin | Manage team membership, configure repos, oversee workflows |
| **Lead Author** | Owner of a specific supplement or document | Team Member + write access | Coordinate contributions, review PRs, manage document lifecycle |
| **Contributor** | Anyone writing or editing content | Team Member + write access | Create branches, submit PRs, respond to review feedback |
| **Reviewer** | Anyone reviewing content | Team Member (read access minimum) | Review PRs, provide technical feedback, approve or request changes |

## Decision-Making

**(TBD)** — How are decisions made for the Devices domain's GitHub workflow?

- Who decides whether to create a new repo vs. add to an existing one?
- Who approves changes to the templates or CI/CD workflows?
- Who resolves disagreements about document content?
- How do decisions get documented and communicated?

## Document Lifecycle

IHE documents typically move through these stages:

| Stage | Description | GitHub Representation |
|-------|-------------|----------------------|
| **Draft** | Initial authoring, not yet public | Work happens on branches; PRs merged to `main` |
| **Public Comment** | Published for community review | **(TBD)** — tag/release? separate branch? |
| **Trial Implementation** | Approved for trial use | **(TBD)** — tag/release? |
| **Final Text** | Officially published | **(TBD)** — tag/release? |

**(TBD):** How does the document lifecycle map to GitHub features?
- Do we use GitHub Releases to mark lifecycle transitions?
- Do we use tags (e.g., `v1.0-public-comment`, `v2.0-final-text`)?
- Do we use branches for different lifecycle stages?
- How does the published output reflect the current stage?

## Naming Conventions

### Repositories

All Devices domain repos use the `DEV.` prefix:

```
DEV.{name}
```

Examples: `DEV.tooling`, `DEV.documentation`, `DEV.supplement-template`

### Branches

```
feature/{description}  — new content
fix/{description}      — corrections
editorial/{description} — formatting, typos
```

### Commits

Use imperative mood, describe what was done:
- "Add actor definitions for Device Observer"
- "Fix table numbering in section 3.2"

## Repository Request Process

### Primary method (automated):

1. Domain Lead or Lead Author opens a [New Repository Request](https://github.com/IHE/DEV.tooling/issues/new/choose) issue on DEV.tooling
2. They fill in the repo name (`DEV.{name}`), template, and description
3. A GitHub Actions workflow automatically creates the repo, assigns the `dev-co-chairs` team as Admin, and comments with the repo URL
4. The issue is closed automatically

### Fallback (manual):

If the automation is unavailable, email an Org Admin with the same details. See the [manual repo creation procedure](../playbooks/org-admin-manual-repo-creation.md).

## Archival and Versioning

**(TBD)** — What happens to repos when a document is finalized?

- Are repos archived (read-only)?
- Are final documents tagged with a version?
- Where is the canonical published version hosted?
- How long are draft/working repos kept active?

## Offboarding and Succession

**(TBD)** — What happens when key people leave?

- Who takes over if the Domain Lead steps down?
- How is institutional knowledge preserved? (Answer: in this documentation repo)
- Who has Org Owner access, and is there a backup?
- What happens at the end of the project lead's term?

## Security and Access

**(TBD)** — Baseline security posture:

- Are repos public or private? (Decision pending)
- Are signed commits required?
- Who can merge to `main`? (Currently: anyone with write access + PR approval)
- Are there any secrets or credentials that need rotation?

## Audit and Compliance

**(TBD)** — What audit trail is needed?

- GitHub Free provides limited audit logging
- PR history provides a full change log for all documents
- Team membership changes are visible in the team activity log
- If more audit capability is needed, GitHub Enterprise may be required

## Open Decisions

- [ ] Public vs. private repos
- [ ] License for document content
- [ ] Document lifecycle → GitHub features mapping
- [ ] CP workflow (handled outside templates — TBD)
- [ ] Archival policy for completed documents
- [ ] Succession plan for key roles
