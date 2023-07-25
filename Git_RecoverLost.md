---
title: Recover Lost Stashes or Commits
date: 2023-7-25
---

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**

- [Recover Lost Stashes or Commits](#recover-lost-stashes-or-commits)
  - [Just Do It](#just-do-it)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Recover Lost Stashes or Commits

## Just Do It

1. Find them from unreachable commit and filter it.
    ```bash
    git fsck --unreachable | grep commit | cut -d ' ' -f3 | xargs git log --merges --no-walk
    ```
2. Put the stash back where it comes from.
    ```bash
    git update-ref refs/stash <hash> -m --create-reflog "poor little stash"
    ```
    or
    Apply the stash to working copy.
    ```bash
    git stash apply <hash>
    ```
