---
name: smart-commit
description: >
  Analyze current code changes and split them into multiple logical commits
  based on feature implementation, then generate a PR description.
  Use when the user wants to organize messy changes into clean commits,
  split changes by feature, create a PR, or clean up commit history
  before pushing. Triggers on keywords like "split commits", "organize commits",
  "smart commit", "clean commit history", "PR description".
argument-hint: "[--base <branch>]"
allowed-tools:
  - Bash
  - Read
  - Edit
  - Write
  - Glob
  - Grep
---

# Smart Commit

Split code changes into clean, logical commits by feature, then generate a PR description.

The goal is a commit history that's easy to read and lets reviewers track the purpose of each module change at a glance. Think of each commit as a self-contained story: one feature, one fix, or one refactor.

## Arguments

Parse `$ARGUMENTS` for:
- **`--base <branch>`**: base branch to diff against (default: `main`)

Everything else in `$ARGUMENTS` is ignored.

## Workflow

### STEP 1: Gather Context

Collect everything needed to understand the changes in one pass:

1. Confirm this is a git repo and identify the current branch.
2. Determine the base branch (from `--base` or default `main`).
3. Collect the diff — handle two scenarios:
   - **Uncommitted changes only** (no commits ahead of base): use `git diff HEAD` and `git diff --cached`
   - **Commits ahead of base**: use `git diff <base>...HEAD`
   - **Mixed** (commits + uncommitted): include both
4. Read recent commit messages (`git log --oneline -20`) to match the repo's existing style.

### STEP 2: Analyze and Plan

Read the full diff carefully. For each changed file, determine:

1. **Which feature/concern** does this change belong to?
2. **Dependencies** — does it relate to changes in other files?
3. **Is this file actually used?** — check with Grep/Glob if uncertain.

Then group changes into logical commits:

- Each commit = one coherent feature, fix, or refactor
- Each commit should leave the codebase in a buildable state
- Use **Conventional Commits** format: `type(scope): description`
  - Types: `feat`, `fix`, `refactor`, `style`, `docs`, `test`, `chore`, `build`, `ci`, `perf`
- Granularity targets readability — not too coarse (huge grab-bag commits), not too fine (one-liner commits)

**Splitting a single file across commits:** If one file contains changes for multiple features, note which functions/blocks belong to which commit. You'll handle this in STEP 4 using the copy-aside technique.

### STEP 3: Present the Commit Plan

Show the user a numbered plan before touching anything. Include:

1. **Unused files** — list any files that aren't imported/used anywhere, with the reason. These will be excluded from commits (not deleted).
2. **Proposed commits** — in order, with commit message and file list.

**Example:**

```
## Commit Plan

### Unused Files (will be excluded)
- src/old-helper.ts — not imported anywhere

### Proposed Commits

1. **feat(auth): add OAuth2 login flow**
   - src/auth/oauth.ts (new)
   - src/auth/types.ts (lines 12-45)
   - src/config/auth.config.ts (modified)

2. **fix(dashboard): correct data refresh interval**
   - src/dashboard/DataPanel.tsx (modified)

3. **refactor(api): extract shared request helper**
   - src/api/client.ts (modified)
   - src/api/helpers.ts (new)

Proceed? (y/n)
```

**Wait for user confirmation before executing.**

### STEP 4: Execute Commits

After confirmation, soft-reset all changes back to the merge base so everything becomes unstaged, then re-commit one group at a time.

**1. Soft reset (once, at the start):**

```bash
git reset --soft $(git merge-base <base> HEAD)
git reset HEAD .
```

This puts all changes into the working directory. Confirm with the user before running — this rewrites commit history.

**2. For each planned commit, stage and commit:**

- **Whole files**: `git add <file>`
- **Partial files** (same file split across multiple commits):
  ```bash
  # Save the full version
  cp <file> <file>.full
  # Restore the base version
  git checkout $(git merge-base <base> HEAD) -- <file>
  # Apply only this commit's changes using the Edit tool
  # ... edit the file ...
  # Stage it
  git add <file>
  # Restore the full version for subsequent commits
  cp <file>.full <file> && rm <file>.full
  ```
- **Create the commit** using a HEREDOC:
  ```bash
  git commit -m "$(cat <<'EOF'
  type(scope): description

  Co-Authored-By: Claude <noreply@anthropic.com>
  EOF
  )"
  ```
- **Verify**: `git log --oneline -1` after each commit.

If splitting a file is too complex or risky, keep the entire file in the most relevant commit and note it.

**3. After all commits, verify:**

```bash
git status                              # nothing left unstaged
git log --oneline $(git merge-base <base> HEAD)..HEAD  # clean history
git diff <base>...HEAD --stat           # total diff matches original
```

The total diff after splitting must be identical to the original. If anything is missing, investigate before proceeding.

### STEP 5: Generate PR Description

Based on the commits, generate a PR title and description:

```markdown
## PR Title
<short, under 70 characters>

## Summary
<1-3 bullet points — what and why>

## Changes
<grouped by commit, briefly describe each>

## File Structure
<tree or list of changed files, grouped logically>

## Test Plan
- [ ] <checklist for reviewers>

---
🤖 Generated with [Claude Code](https://claude.com/claude-code)
```

Present to the user for review.

## Safety Rules

- **Confirm before reset**: Always get user approval before `git reset --soft`. It rewrites history.
- **Never force-push** unless the user explicitly asks.
- **Preserve all changes**: The total diff after splitting must match the original exactly.
- **Don't delete unused files**: Report them, let the user decide.
- **Partial file splits**: If it's too risky, keep the whole file in one commit. Say so in the plan.
- **Recovery**: If something goes wrong mid-execution, `git reflog` shows the pre-reset state. The user can `git reset --soft <reflog-hash>` to get back to where they started.