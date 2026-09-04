# Build spec: interactive Git & GitHub learning page

## Goal
Build a single self-contained HTML page that teaches Git and GitHub visually and interactively — not a static poster, but something the learner can click through and *operate*. The core idea: let the user actually move a "file" through the four Git areas, grow a branch graph, and step through the GitHub PR workflow, rather than just reading labeled boxes.

## Deliverable
- One file: `index.html`
- All CSS and JS embedded inline (`<style>` / `<script>` in the same file) — no build step, no npm install, no bundler
- Must open directly in a browser via double-click, no server required
- No external dependencies except optionally a single Google Fonts link — everything else (icons, etc.) should be hand-drawn with CSS/SVG, not an icon library, to keep it a true single file
- Fully responsive: usable on a phone-width screen down to ~375px and up to desktop

## Tech constraints
- Vanilla HTML/CSS/JS only. No React, no framework, no CDN framework imports.
- Use CSS custom properties for the color palette (defined once in `:root`) so theming is consistent throughout.
- Use `<svg>` inline for diagrams/graphs, not canvas.
- Respect `prefers-reduced-motion` — wrap any animation/transition in a media query fallback (instant state change instead of animated transition).
- Support a light/dark toggle (or at minimum respect `prefers-color-scheme`).

## Page structure (four sections, single scrolling page with a sticky nav/progress dots)

### 1. Hero / intro
- One-line title ("Git & GitHub, visually") and one sentence of framing.
- No content requirements beyond that — keep it short.

### 2. The four Git areas (interactive flow)
Recreate this concept as a **working simulation**, not just a diagram:
- Draw four boxes top-to-bottom: **Working directory → Staging area → Local repository → Remote repository (GitHub)**.
- Render one small file icon/token (e.g. a rounded chip labeled `app.js`) that visually sits inside whichever box is "current."
- Below or beside the diagram, show buttons for each command: `git add`, `git commit`, `git push`, `git pull`, `git reset`, `git fetch`.
- Clicking a button:
  - Animates the token moving forward/backward to the correct box (respect real Git semantics — `git add` moves working→staging, `git commit` moves staging→local repo, `git push` moves local→remote, `git reset` moves staging→working, `git pull` moves remote→local).
  - Disables buttons that don't apply to the token's current position (e.g. can't `git push` if the token is still in the working directory) — this is the part that actually teaches the mental model, so don't skip it.
  - Shows a one-line caption below the diagram naming the command that just ran and what it does.
- Include a small "reset simulation" button to put the token back at the working directory.

### 3. Branching & merging (interactive commit graph)
- Draw an SVG commit graph: a horizontal `main` line of circles (commits), with the ability to branch off.
- Provide controls:
  - "New commit on main" — adds a circle to the main line.
  - "Create branch" — spawns a second line (`feature`) diverging from the current last commit on main, in a visually distinct color.
  - "Commit on feature" — adds a circle to the feature line (only enabled once a branch exists).
  - "Merge feature → main" — draws a curved connector from the last feature commit back into a new commit on main, and after merging, disables further feature commits until a new branch is created.
- Use two colors only for this section: one for `main` commits/lines, one for the feature branch — pick from the palette below and keep it consistent with section 2's "forward vs. undo" color logic if possible.
- Below the graph, show the exact command that corresponds to each button when clicked (`git checkout -b feature`, `git commit -m "..."`, `git merge feature`, etc.) so the user connects the visual action to the real syntax.

### 4. GitHub collaboration workflow (stepper)
- A horizontal (desktop) / vertical (mobile) stepper with 8 stages, each clickable to reveal detail:
  1. Fork or clone the repo
  2. Create a branch
  3. Commit your changes
  4. Push the branch
  5. Open a pull request
  6. Review and iterate
  7. Merge the PR
  8. Sync and clean up
- Each stage shows: the plain-English description, and the exact Git command(s) involved.
- Include a progress indicator (dots or numbered pills) showing which stage is selected.

### 5. Command reference (static, always visible)
- A simple two-column table: command → what it does.
- Cover at minimum: `git init`, `git clone`, `git status`, `git diff`, `git add`, `git commit`, `git log`, `git push`, `git pull`, `git fetch`, `git branch`, `git checkout`/`git switch`, `git merge`, `git rebase`, `git stash`, `git reset`, `git revert`.
- Make it filterable with a simple text input (client-side JS filter on keystroke, no backend).

## Visual design guidelines
- Flat design: no gradients, no drop shadows except a very subtle card elevation if needed. No skeuomorphism.
- Color palette (define as CSS variables):
  - Neutral/structural boxes: light gray fill, darker gray border
  - "Forward" actions (add, commit, push): a calm blue or teal
  - "Undo/sync" actions (reset, pull, fetch): a warm coral/orange
  - Main branch: teal; feature branch: coral — keep this pairing consistent across sections 2 and 3
- Typography: one sans-serif font (system font stack or a single Google Font), sentence case everywhere, no ALL CAPS except maybe the hero title.
- Generous whitespace; sections clearly separated; sticky top nav with anchor links to each of the 5 sections.
- Every interactive element needs a visible hover/focus state and must be keyboard-operable (buttons are real `<button>` elements, not styled `<div>`s).

## Accessibility
- Semantic HTML (`<nav>`, `<section>`, `<button>`, headings in order h1→h2→h3).
- All icons/diagrams get `aria-label` or an adjacent visually-hidden text description.
- Color is never the only signal — pair the teal/coral distinction with a label or icon shape difference too.
- Sufficient contrast in both light and dark mode.

## Acceptance checklist (agent should self-check before finishing)
- [ ] Opens correctly as a standalone file with no console errors
- [ ] Token in section 2 only allows semantically valid moves at each state
- [ ] Section 3 graph updates correctly through multiple branch/commit/merge cycles without breaking layout
- [ ] Stepper in section 4 is navigable by click and by keyboard (arrow keys or tab+enter)
- [ ] Command table filter works on every keystroke
- [ ] Page is usable at 375px width and at 1440px width
- [ ] Dark mode (via toggle or `prefers-color-scheme`) doesn't break contrast anywhere
