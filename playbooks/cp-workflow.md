[← Back to Playbooks](README.md) · [Home](../README.md)

# Change Proposal (CP) Workflow

How Change Proposals are created, worked on, balloted, and approved in the Devices domain.

> **Audience:** Anyone who authors or shepherds a Change Proposal. Lead Authors and Domain Leads especially.

---

## What Is a CP?

A **Change Proposal (CP)** is a proposed change to a published or in-progress IHE document. CPs are tracked as files inside the repository they affect, so that the proposal, its history, and its outcome all live alongside the document itself.

**Where CPs live:** every supplement repo — *and* the main repo that holds the technical frameworks — has a top-level **`CP/`** directory:

```
CP/                  ← CPs currently in progress (one file per CP)
CP/Approved/         ← CPs that have passed a ballot
```

- A CP **in progress** lives directly in `CP/`.
- A CP that has **passed its ballot** moves into `CP/Approved/`.

---

## The Big Picture

```
   create CP file ──► work in CP branch ──► PR to main (ready to ballot)
                                                   │
                                              vote happens
                                            (outside GitHub)
                                                   │
                                  ┌────────────────┴────────────────┐
                              PASSED                             DID NOT PASS
                                  │                                  │
                       move file to CP/Approved/            keep working in the
                                                            branch, open a new PR
                                                            when ready → re-ballot
```

Two principles drive this design:

1. **The `main` branch always shows which CPs exist.** Anyone looking at `main` sees every CP that has been started, because the CP *file* is created on `main` up front — even before the work is done.
2. **All the actual CP work happens on a branch** named for that CP, and only merges back when it's ready to be voted on. This keeps `main` clean and gives reviewers a clear PR to look at.

---

## Step 1: Create the CP (make it visible on `main`)

The goal of this step is to get a stub CP file onto `main` so the proposal is visible, then move the real work onto a branch.

There are two ways to do this. **The first is recommended**, because it works cleanly with branch protection on `main`.

### Option A — Branch, then merge the stub in (recommended)

1. Create a short-lived branch from `main`:
   ```bash
   git checkout main
   git pull
   git checkout -b cp/cp-0042-create
   ```
2. Add the new CP file under `CP/` (a stub is fine — title, author, problem statement):
   ```bash
   git add CP/CP-0042-fix-actor-binding.adoc
   git commit -m "Create CP-0042 stub"
   git push -u origin cp/cp-0042-create
   ```
3. Open a PR and **merge it into `main` immediately**. Now the CP file exists on `main` for everyone to see.

> **Why this is recommended:** if `main` is protected (no direct pushes), this is the *only* way to get the file onto `main` — through a PR. It also produces a tidy record that the CP was opened.

### Option B — Create on `main` directly, then branch

1. On `main`, create the CP file and commit it directly.
2. Immediately create the working branch for the CP.

> Only works if `main` is **not** protected against direct pushes. Prefer Option A.

---

## Step 2: Do the Work on the CP's Branch

All work on a Change Proposal happens on **that CP's branch** — never directly on `main`.

```bash
git checkout main
git pull
git checkout -b cp/cp-0042            # the working branch for this CP
# ... edit the CP file and any affected document files ...
git commit -m "CP-0042: draft proposed wording for actor binding"
git push -u origin cp/cp-0042
```

Keep iterating here for as long as it takes. The branch is the CP's workspace.

---

## Step 3: Open a PR When It's Ready to Ballot

When the CP is ready to be voted on, open a **Pull Request from the CP branch into `main`**.

- This is the signal that the CP is ready for a ballot.
- Merging the PR brings the finished proposed text onto `main`, where it's visible to all members ahead of the vote.

---

## Step 4: The Ballot

**Voting does not happen in GitHub.** The ballot is run through the domain's normal process (meetings, IHE balloting tooling, etc.). GitHub's role is only to hold the proposal text and record the outcome.

When the vote concludes, there are two outcomes.

### If the CP PASSES

Move the CP file from `CP/` into `CP/Approved/`:

```bash
git checkout main
git pull
git checkout -b cp/cp-0042-approve
git mv CP/CP-0042-fix-actor-binding.adoc CP/Approved/
git commit -m "Approve CP-0042 — passed ballot"
git push -u origin cp/cp-0042-approve
```

Open a PR for that move and merge it to `main`. The CP now lives in `CP/Approved/`, marking it as accepted.

### If the CP does NOT pass

- The CP file **stays in `CP/`** (still in progress).
- Go back to the CP's branch and keep working — address the feedback that came out of the ballot.
- When it's ready again, open **another PR** to `main` (Step 3) and put it up for **re-ballot**.

**Repeat Steps 3–4 until the CP is approved.**

---

## Naming Suggestions

These aren't enforced, but consistency helps everyone:

| Thing | Suggested pattern | Example |
|-------|-------------------|---------|
| CP file | `CP-{number}-{short-slug}.adoc` | `CP-0042-fix-actor-binding.adoc` |
| Working branch | `cp/cp-{number}` | `cp/cp-0042` |
| Stub/create branch | `cp/cp-{number}-create` | `cp/cp-0042-create` |
| Approve branch | `cp/cp-{number}-approve` | `cp/cp-0042-approve` |

---

## Quick Reference

| Stage | What happens | Where the file is |
|-------|--------------|-------------------|
| **Created** | Stub merged to `main` for visibility | `CP/` |
| **In progress** | Work happens on the CP branch | `CP/` (on `main`); edits on the branch |
| **Ready to ballot** | PR from CP branch → `main` | `CP/` |
| **Passed** | File moved via PR | `CP/Approved/` |
| **Did not pass** | Keep working, re-PR, re-ballot | `CP/` |

### Related pages

- [Contributor & Lead Author Guide](contributor.md) — general branch/PR workflow and AsciiDoc reference
- [Domain Lead Playbook](domain-lead.md) — managing repos, branch protection, and reviews
- [Governance](../governance/README.md) — roles and decision-making
