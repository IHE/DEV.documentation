---
layout: page
title: Governance
permalink: /governance/
---

# Governance

Policies, roles, and decision-making for the Devices domain's GitHub presence.

> Many sections below are marked **(TBD)** — these will be filled in as decisions are made.

## Roles and Responsibilities

| Role | Who | Permissions | Responsibilities |
|------|-----|-------------|-----------------|
| **Org Admin** | IHE staff (non-technical) | GitHub Org Owner | Create repos from templates, assign teams |
| **Domain Lead** | Devices domain technical lead | Team Maintainer + Repo Admin | Manage team membership, configure repos, oversee workflows |
| **Lead Author** | Owner of a specific supplement or document | Team Member + write access | Coordinate contributions, review PRs, manage document lifecycle |
| **Contributor** | Anyone writing or editing content | Team Member + write access | Create branches, submit PRs, respond to review feedback |
| **Reviewer** | Anyone reviewing content | Team Member (read access minimum) | Review PRs, provide technical feedback |

## Decision-Making

**(TBD)** — How are decisions made for the Devices domain's GitHub workflow?

- Who decides whether to create a new repo vs. add to an existing one?
- Who approves changes to the templates or CI/CD workflows?
- Who resolves disagreements about document content?

## Document Lifecycle

IHE documents typically move through these stages:

| Stage | Description |
|-------|-------------|
| **Draft** | Initial authoring, not yet public |
| **Public Comment** | Published for community review |
| **Trial Implementation** | Approved for trial use |
| **Final Text** | Officially published |

**(TBD):** How does this lifecycle map to GitHub features (tags, releases, branches)?

## Naming Conventions

All Devices domain repos use the `DEV.` prefix:

```
DEV.{name}
```

Examples: `DEV.tooling`, `DEV.documentation`, `DEV.template-supplement`

### Branches

```
feature/{description}  — new content
fix/{description}      — corrections
editorial/{description} — formatting, typos
```

## Repository Request Process

1. Domain Lead or Lead Author emails an Org Admin with:
   - Repository name (`DEV.{name}`)
   - Which template to use (`DEV.template-supplement`)
   - Short description of the document
   - Lead Author's GitHub username
2. Org Admin creates the repo from the template
3. Org Admin assigns the `Devices-Domain` team as Admin
4. Org Admin confirms back with the repo URL

## Archival and Versioning

**(TBD)** — What happens to repos when a document is finalized?

## Offboarding and Succession

**(TBD)** — What happens when key people leave?

## Security and Access

**(TBD)** — Public vs. private repos, signed commits, merge permissions.

## Open Decisions

- [ ] Public vs. private repos
- [ ] License for document content
- [ ] Document lifecycle → GitHub features mapping
- [ ] CP workflow (handled outside templates — TBD)
- [ ] Archival policy for completed documents
- [ ] Succession plan for key roles
