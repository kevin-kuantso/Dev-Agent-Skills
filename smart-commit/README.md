# Smart Commit

> **[繁體中文版](README.zh-TW.md)**

Analyze code changes and split them into clean, logical commits by feature, then generate a PR description. Turns a messy working directory into a reviewable commit history.

---

## Getting Started

Copy the skill folder to your agent's skills directory, then invoke the skill.

```bash
# Claude Code
cp -r smart-commit ~/.claude/skills/smart-commit
claude
/smart-commit

# Cursor — copy to rules directory
cp -r smart-commit /path/to/your/project/.cursor/rules/smart-commit

# Other agents — load SKILL.md as system prompt
```

### Example commands (Claude Code)

```bash
# Split current changes into logical commits (default base: main)
/smart-commit

# Specify a different base branch
/smart-commit --base develop
```

---

## Usage

Run `/smart-commit` when you have a pile of changes — committed or uncommitted — that you want organized into clean, feature-based commits before pushing or creating a PR.

### What it does

1. **Gathers** all changes since diverging from the base branch (committed + uncommitted)
2. **Analyzes** each file change and groups them by feature/concern
3. **Presents** a commit plan for your approval — including any unused files detected
4. **Executes** the plan: soft-resets and re-commits one group at a time
5. **Generates** a PR title and description based on the final commits

### Splitting a single file across commits

If one file contains changes for multiple features, the skill splits it by logic blocks or functions — each commit only includes the code relevant to that feature. It uses a copy-aside technique to safely stage partial changes without losing anything.

---

## Workflow

```
Gather context → Analyze & group by feature → Present commit plan
→ User confirms → Soft reset & re-commit → Verify diff integrity
→ Generate PR description
```

---

## Key Features

| Feature | Description |
|---------|-------------|
| **Feature-based splitting** | Groups changes by logical feature, not by file |
| **Partial file splits** | Safely splits a single file across multiple commits when it touches multiple features |
| **Unused file detection** | Identifies files not imported/used anywhere and excludes them from commits |
| **Conventional Commits** | `feat`, `fix`, `refactor`, `docs`, `test`, `chore`, etc. |
| **Diff integrity check** | Verifies the total diff after splitting matches the original exactly |
| **PR description generation** | Produces a summary, change list, file structure, and test plan |
| **Recovery guidance** | If something goes wrong, `git reflog` gets you back to the pre-reset state |

---

## Generated Output

### Commits

Each commit follows the Conventional Commits format:

```
type(scope): clear description of the change

Co-Authored-By: Claude <noreply@anthropic.com>
```

### PR Description

```markdown
## Summary
- 1-3 bullet points

## Changes
- Grouped by commit

## File Structure
- Tree of changed files

## Test Plan
- [ ] Checklist for reviewers
```

---

## Safety

- **Confirms before reset** — always asks before running `git reset --soft`
- **Never force-pushes** unless you explicitly ask
- **Preserves all changes** — total diff after splitting must match original
- **Doesn't delete unused files** — reports them, lets you decide
- **Recoverable** — `git reflog` shows pre-reset state if anything goes wrong

---

## File Structure

```
smart-commit/
├── SKILL.md                       # Core instructions (agent entry point)
└── README.md                      # This file
```

`SKILL.md` is the main instruction file that any AI coding agent can load.

---

**Maintainer:** Kevin Liu
**Last Updated:** 2026-03-12