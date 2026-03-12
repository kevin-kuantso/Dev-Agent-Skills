# Dev Agent Skills

> **[繁體中文版](README.zh-TW.md)**

A collection of agent skills for automating development workflows. Each skill is a self-contained, markdown-driven instruction set that can be installed into any AI coding agent that supports skill loading.

## 📋 Table of Contents

- [Overview](#-overview)
- [Available Skills](#-available-skills)
- [Getting Started](#-getting-started)
- [Repository Structure](#-repository-structure)
- [Creating New Skills](#-creating-new-skills)
- [Support](#-support)

## 🎯 Overview

**Dev Agent Skills** is a reusable skill library for AI coding agents. Each skill is a self-contained folder with structured markdown instructions that can be loaded by any agent supporting skill/prompt injection — including [Claude Code](https://docs.anthropic.com/en/docs/claude-code), Cursor, Windsurf, and other AI coding assistants.

Skills follow a progressive disclosure pattern — a single `SKILL.md` serves as the entry point, with reference files loaded on-demand to keep context size efficient.

### Tested Platforms

| Platform | Installation Path | Status |
|----------|------------------|--------|
| Claude Code | `.claude/skills/` or `~/.claude/skills/` | Fully tested |
| Cursor | `.cursor/rules/` | Compatible |
| Windsurf | `.windsurfrules/` | Compatible |
| Other agents | Varies — load `SKILL.md` as system prompt | Adaptable |

## 🤖 Available Skills

### 📄 Resume Craft

Senior interviewer-grade resume writer and optimizer with JD matching analysis.

**Quick Start:**
```bash
# 1. Copy the skill to the user skills folder
cp -r resume-craft ~/.claude/skills/resume-craft

# 2. Open Claude Code
claude

# 3. Invoke the skill
/resume-craft /path/to/resume.pdf /path/to/jd.pdf --lang en,zh-TW
```

**Features:**

| Feature | Description |
|---------|-------------|
| **Multi-mode workflow** | Optimize existing resumes, generate from scratch, or deep-analysis mode with `--analysis` |
| **JD matching** | Gap analysis, ATS keyword extraction, and 65-75% keyword match targeting |
| **Anti-inflation guardrails** | 6 rules to prevent AI over-packaging (role inflation, metric fabrication, etc.) |
| **Google XYZ formula** | Every bullet follows "Accomplished [X] as measured by [Y] by doing [Z]" |
| **Multi-language** | English, zh-TW, zh-CN, ja, ko with locale-aware terminology and auto-sync |
| **Target company profiles** | TSMC, FAANG, startup, enterprise — adjusts tone and emphasis automatically |
| **Interview prep** | Optional Phase 5 generates bullet-by-bullet Q&A and behavioral questions |

**Supported inputs:**
- Resume files: `.pdf`, `.docx`, `.md`, `.txt`
- Job descriptions: URL, file, or pasted text
- Figma designs (for portfolio context)

[📖 Full Documentation](resume-craft/SKILL.md)

### 🔀 Smart Commit

Analyze code changes and split them into clean, logical commits by feature, then generate a PR description.

**Quick Start:**
```bash
# 1. Copy the skill to the user skills folder
cp -r smart-commit ~/.claude/skills/smart-commit

# 2. Open Claude Code
claude

# 3. Invoke the skill
/smart-commit
```

**Features:**

| Feature | Description |
|---------|-------------|
| **Feature-based splitting** | Groups changes by logical feature, not by file |
| **Partial file splits** | Safely splits a single file across multiple commits when it touches multiple features |
| **Unused file detection** | Identifies files not imported/used anywhere and excludes them from commits |
| **Conventional Commits** | `feat`, `fix`, `refactor`, `docs`, `test`, `chore`, etc. |
| **Diff integrity check** | Verifies the total diff after splitting matches the original exactly |
| **PR description generation** | Produces a summary, change list, file structure, and test plan |

[📖 Full Documentation](smart-commit/SKILL.md)

---

## 🚀 Getting Started

### Installation

Copy any skill folder to your agent's skills directory:

```bash
# Claude Code — user-wide
cp -r <skill-folder> ~/.claude/skills/

# Claude Code — project-specific
cp -r <skill-folder> /path/to/your/project/.claude/skills/

# Cursor
cp -r <skill-folder> /path/to/your/project/.cursor/rules/

# Other agents — load SKILL.md as system prompt or custom instruction
```

For Claude Code, invoke the skill with `/<skill-name>` or describe the task naturally. For other agents, reference the `SKILL.md` content in your prompt or rules configuration.

## 📁 Repository Structure

```
Dev-Agent-Skills/
├── resume-craft/                  # Resume writing & optimization skill
│   ├── SKILL.md                   # Core instructions (agent entry point)
│   └── references/                # On-demand reference files
│       ├── best-practices.md      # Research-backed resume advice with sources
│       ├── collection-template.md # Structured interview template per role
│       ├── company-profiles.md    # Target company tone profiles
│       └── locale-rules.md        # Multi-language terminology rules
├── smart-commit/                  # Commit splitting & PR description skill
│   └── SKILL.md                   # Core instructions (agent entry point)
└── README.md                      # This file
```

## ✏️ Creating New Skills

Each skill follows this structure:

```
skill-name/
├── SKILL.md                       # Required: main instructions (agent entry point)
├── references/                    # Optional: on-demand reference files
│   └── *.md
├── scripts/                       # Optional: helper scripts
│   └── *.py / *.sh
└── README.md                      # Optional: human-readable documentation
```

**Key conventions:**
- `SKILL.md` frontmatter must include `name`, `description`, and `allowed-tools`
- Keep `SKILL.md` focused — offload detailed reference data to `references/` files
- Skills should be self-contained and copy-pastable into any project

## 📞 Support

**Maintainer:** Kevin Liu