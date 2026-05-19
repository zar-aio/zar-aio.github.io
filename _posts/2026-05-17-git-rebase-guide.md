---
layout: post
title: "Understanding Git Rebase Without the Headaches"
date: 2026-05-17
tags: [git, workflow]
description: "A clear, practical guide to git rebase — what it actually does, when to use it, and when not to."
---

`git rebase` has a reputation for being dangerous. That reputation is partly deserved and mostly overstated. Once you understand what it does, it becomes one of the most useful tools in your Git workflow.

## What rebase actually does

Rebase takes the commits on your current branch and **replays them on top of another branch**. Each commit is rewritten with a new hash — same changes, new parent.

```
Before rebase:

  main:    A -- B -- C
                \
  feature:       D -- E

After: git rebase main (from feature)

  main:    A -- B -- C
                      \
  feature:             D' -- E'
```

The result is a clean, linear history. No merge bubbles.

## When to use it

- **Before opening a pull request** — rebase your feature branch onto `main` so the diff is clean and conflicts are resolved before review.
- **Cleaning up local commits** — use `git rebase -i HEAD~n` (interactive rebase) to squash, reorder, or reword commits before sharing them.

## When NOT to use it

**Never rebase commits that have already been pushed to a shared branch.** Since rebase rewrites history, it forces everyone else's local copy out of sync. This is the "dangerous" part people warn about.

A simple rule: rebase freely on branches that only you have pushed; merge on branches others are working on.

## Interactive rebase quick reference

```bash
git rebase -i HEAD~3   # edit the last 3 commits
```

In the editor that opens:

| Command | What it does |
|---------|-------------|
| `pick`  | Keep the commit as-is |
| `reword`| Keep the commit, edit its message |
| `squash`| Combine with the previous commit |
| `drop`  | Remove the commit entirely |

## A practical workflow

```bash
# on your feature branch
git fetch origin
git rebase origin/main   # bring in latest changes

# if there are conflicts, fix them, then:
git add .
git rebase --continue

# push (force needed since history was rewritten)
git push --force-with-lease
```

`--force-with-lease` is safer than `--force` — it refuses to push if someone else has pushed to the same branch in the meantime.

---

Rebase is a scalpel, not a sledgehammer. Used thoughtfully, it keeps your project history readable and your pull requests clean.
