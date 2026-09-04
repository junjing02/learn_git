# Git & GitHub, visually

A single-page, interactive site that teaches core Git concepts by letting you operate them instead of just reading about them.

**Live:** https://junjing02.github.io/learn_git/

## What's on the page

- **The four Git areas** — drag a file token through working directory → staging → local repo → remote by clicking `git add`, `commit`, `push`, `pull`, `reset`, `fetch`. Only valid moves are enabled.
- **Branching & merging** — grow a real commit graph by clicking buttons *or* typing actual git commands (`git commit`, `git checkout -b feature`, `git merge feature`, etc.) into a simulated terminal. Includes undo and a note about merge conflicts.
- **GitHub workflow stepper** — the 8 stages from fork/clone to merge & cleanup, each with the exact commands.
- **Command reference** — filterable table of common Git commands, with one-click copy.

## Tech

Plain HTML/CSS/JS in a single `index.html` file — no build step, no dependencies (aside from an optional Google Font). Open it directly in a browser, or visit the GitHub Pages link above.
