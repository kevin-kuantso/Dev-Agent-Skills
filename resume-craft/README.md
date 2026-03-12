# Resume Craft

> **[繁體中文版](README.zh-TW.md)**

Senior interviewer-grade resume writer and optimizer with JD matching analysis. Combines the perspectives of a tech recruiter, engineering hiring manager, and ATS specialist to produce interview-ready resumes.

---

## Getting Started

Copy the skill folder to your agent's skills directory, then invoke the skill.

```bash
# Claude Code
cp -r resume-craft ~/.claude/skills/resume-craft
claude
/resume-craft /path/to/resume.pdf /path/to/jd.pdf --lang en,zh-TW

# Cursor — copy to rules directory
cp -r resume-craft /path/to/your/project/.cursor/rules/resume-craft

# Other agents — load SKILL.md as system prompt
```

### Example commands (Claude Code)

```bash
# Optimize an existing resume
/resume-craft /path/to/resume.pdf

# Optimize with a target job description
/resume-craft /path/to/resume.pdf /path/to/jd.pdf

# Multi-language output
/resume-craft /path/to/resume.pdf --lang en,zh-TW,zh-CN
```

---

## Usage

### Mode 1: Optimize (Default)

```bash
/resume-craft /path/to/resume.pdf
/resume-craft /path/to/resume.pdf /path/to/jd.pdf
```

Reads the resume, asks 3-5 critical questions, then produces an optimized version. If a JD is provided, performs gap analysis and ATS keyword matching.

### Mode 2: Deep Analysis (`--analysis`)

```bash
/resume-craft /path/to/resume.pdf --analysis
/resume-craft /path/to/resume.pdf /path/to/jd.pdf --analysis
```

Generates a detailed `analysis_report_<name>_<date>.md` before any rewriting — includes 6-second scan impression, JD requirements analysis, gap analysis, and optimization plan. **No files are modified until you confirm.**

### Mode 3: Generate from Scratch

```bash
/resume-craft
/resume-craft /path/to/jd.pdf
```

When no resume is provided, enters Generate mode with a structured interview to collect your experience, then builds a resume from scratch.

### Language Flag

All modes support `--lang` to specify output languages. Default: auto-detect from resume language.

```bash
--lang en              # English only
--lang zh-TW           # Traditional Chinese only
--lang en,zh-TW,zh-CN  # All three versions, auto-synced
```

---

## Workflow

```
Input detection → Collect critical info → Rewrite with XYZ formula
→ Quality verification loop → Present to user → Iterate until satisfied
```

### What does each phase do?

1. **Input Detection** — Auto-detect resume, JD, target company, and language
2. **Information Collection** — Ask 3-5 focused questions (or full interview in Generate mode)
3. **Resume Generation** — Apply Google XYZ formula, Anti-Inflation Rules, ATS optimization, and target company profile
4. **Iteration** — Apply changes to EN first, auto-sync to other language versions
5. **Interview Prep** (optional) — Generate bullet-by-bullet Q&A and behavioral questions

---

## Key Features

| Feature | Description |
|---------|-------------|
| **Google XYZ Formula** | "Accomplished [X] as measured by [Y] by doing [Z]" — every bullet follows this structure |
| **Anti-Inflation Rules** | 6 guardrails prevent role inflation, metric fabrication, terminology inflation, etc. |
| **ATS Optimization** | Standard headers, keyword matching (65-75% target), single-column layout |
| **JD Gap Analysis** | Must-have/nice-to-have skills extraction, missing keyword identification |
| **Target Company Profiles** | TSMC, FAANG, startup, enterprise — auto-adjusts tone and emphasis |
| **Multi-Language Sync** | EN is source of truth; changes auto-propagate to zh-TW, zh-CN, ja, ko |
| **Never Guess Policy** | Unknown metrics use `[PLACEHOLDER]` markers — never fabricates data |
| **Interview Test** | Every bullet is checked: "Can the user speak about this for 2 minutes?" |

---

## Supported Inputs

| Input | Format |
|-------|--------|
| Resume | `.pdf`, `.docx`, `.md`, `.txt` |
| Job Description | URL, file, or pasted text |
| Target company | Auto-detected from JD or conversation context |

---

## Generated Files

| File | When | Description |
|------|------|-------------|
| `resume_<name>_<date>.md` | Always | Optimized or generated resume |
| `analysis_report_<name>_<date>.md` | `--analysis` mode | Detailed analysis report with gap analysis |
| `interview_prep_<name>_<date>.md` | On request | Bullet-by-bullet Q&A and behavioral questions |

---

## File Structure

```
resume-craft/
├── SKILL.md                       # Core instructions (agent entry point)
└── references/                    # On-demand reference files
    ├── best-practices.md          # Research-backed resume advice with sources
    ├── collection-template.md     # Structured interview template per role
    ├── company-profiles.md        # Target company tone profiles (TSMC/FAANG/startup/enterprise)
    └── locale-rules.md            # Multi-language terminology rules (zh-TW/zh-CN/ja/ko)
```

`SKILL.md` is the main instruction file that any AI coding agent can load. Reference files are loaded on-demand using a progressive disclosure pattern to keep context size efficient.

---

**Maintainer:** Kevin Liu
**Last Updated:** 2026-03-12