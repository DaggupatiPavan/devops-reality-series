# Git Hands-on Scenarios for DevOps & Platform Engineers (4–8 yrs)

This document is **scenario-driven**. Each section mirrors **real production situations** DevOps / Platform engineers face.

---

## Scenario 1: Cleaning Commit History Before a Pull Request

### Problem

You worked on a feature and ended up with 8 messy commits:

* fix typo
* debug
* try again
* final fix

Reviewers struggle to understand *what actually changed*.

### Goal

Create **clean, atomic commits** before raising a PR.

### Steps

```bash
git rebase -i HEAD~8
```

* Squash related commits
* Edit commit messages

### Outcome

* Clean history
* Faster PR reviews
* Easier rollback

---

## Scenario 2: Hotfix Needed — Without Merging Entire Branch

### Problem

Production is broken.
The fix exists in a feature branch that also contains unfinished work.

### Goal

Move **only the required fix** to `main`.

### Steps

```bash
git checkout main
git cherry-pick <commit-hash>
```

### Outcome

* No risky merges
* Minimal blast radius
* Faster recovery

---

## Scenario 3: Production Bug After Release – Safe Rollback

### Problem

A recent deployment caused failures.
Deleting commits is dangerous and breaks history.

### Goal

Rollback **safely** while preserving audit trail.

### Steps

```bash
git revert <commit-hash>
```

### Outcome

* History preserved
* CI/CD re-triggers cleanly
* Auditable rollback

---

## Scenario 4: Find the Exact Commit That Broke Production

### Problem

Tests were green yesterday. Today production is failing.
No one knows which commit caused it.

### Goal

Identify the **exact breaking commit**.

### Steps

```bash
git bisect start
git bisect bad HEAD
git bisect good <last-known-good-commit>
```

Git will binary-search commits.

### Outcome

* Root cause found in minutes
* Faster RCA
* Less guesswork

---

## Scenario 5: Urgent Context Switch Without Dirty Commits

### Problem

You are mid-work when a critical issue comes in.
You’re not ready to commit.

### Goal

Switch context without losing work.

### Steps

```bash
git stash
git checkout hotfix-branch
```

Later:

```bash
git stash pop
```

### Outcome

* Clean branches
* No half-baked commits

---

## Scenario 6: Protecting Production with Git

### Problem

Developers push directly to `main`.
Incidents happen frequently.

### Goal

Make `main` always production-ready.

### Practices

* Protect `main` branch
* Enforce PR reviews
* Require CI checks
* Disallow force-push

### Outcome

Git becomes a **control system**, not a risk.

---

## Final Takeaway

Normal engineers ask:

> "Did my code push?"

Platform engineers ask:

> "Can we rollback, audit, debug, and ship safely?"

**Git maturity = Delivery maturity**

---

Feel free to fork, share, and reference this in your LinkedIn
