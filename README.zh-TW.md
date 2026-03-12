# Dev Agent Skills

> **[English Version](README.md)**

AI coding agent skills 集合，用於自動化開發工作流程。每個 skill 都是獨立的 markdown 指令集，可安裝至任何支援 skill 載入的 AI coding agent。

## 📋 目錄

- [概述](#-概述)
- [Available Skills](#-available-skills)
- [快速開始](#-快速開始)
- [Repository 結構](#-repository-結構)
- [建立新 Skill](#-建立新-skill)
- [聯絡資訊](#-聯絡資訊)

## 🎯 概述

**Dev Agent Skills** 是可重複使用的 AI coding agent skill 庫。每個 skill 都是獨立的資料夾，包含結構化的 markdown 指令，可被任何支援 skill / prompt injection 的 agent 載入 — 包括 [Claude Code](https://docs.anthropic.com/en/docs/claude-code)、Cursor、Windsurf 及其他 AI 程式助手。

Skill 採用 progressive disclosure 模式 — 以 `SKILL.md` 作為 entry point，reference files 按需載入，以控制 context 大小。

### 已測試平台

| 平台 | 安裝路徑 | 狀態 |
|------|---------|------|
| Claude Code | `.claude/skills/` 或 `~/.claude/skills/` | 完整測試 |
| Cursor | `.cursor/rules/` | 相容 |
| Windsurf | `.windsurfrules/` | 相容 |
| 其他 agent | 依平台而異 — 將 `SKILL.md` 作為 system prompt 載入 | 可適配 |

## 🤖 Available Skills

### 📄 Resume Craft

資深面試官等級的履歷撰寫與優化工具，支援 JD matching 分析。

**快速開始：**
```bash
# 1. 將 skill 複製到使用者 skills 資料夾
cp -r resume-craft ~/.claude/skills/resume-craft

# 2. 開啟 Claude Code
claude

# 3. 呼叫 skill
/resume-craft /path/to/resume.pdf /path/to/jd.pdf --lang en,zh-TW
```

**功能特色：**

| 功能 | 說明 |
|------|------|
| **多模式 workflow** | 優化現有履歷、從零生成、或使用 `--analysis` 進行深度分析 |
| **JD matching** | Gap analysis、ATS keyword 擷取、目標 65-75% keyword match rate |
| **Anti-inflation guardrails** | 6 條規則防止 AI 過度包裝（role inflation、metric fabrication 等） |
| **Google XYZ formula** | 每條 bullet point 遵循「Accomplished [X] as measured by [Y] by doing [Z]」 |
| **多語系** | English、zh-TW、zh-CN、ja、ko，具備 locale-aware 術語及自動同步 |
| **Target company profiles** | TSMC、FAANG、startup、enterprise — 自動調整語氣和重點 |
| **Interview prep** | 選用 Phase 5 生成逐條 Q&A 及 behavioral questions |

**支援的輸入格式：**
- 履歷檔案：`.pdf`、`.docx`、`.md`、`.txt`
- Job description：URL、檔案或直接貼上文字
- Figma 設計稿（作為 portfolio 參考）

[📖 完整文件](resume-craft/SKILL.md)

---

## 🚀 快速開始

### 安裝方式

將任何 skill 資料夾複製到你的 agent skills 目錄：

```bash
# Claude Code — 全域安裝
cp -r <skill-folder> ~/.claude/skills/

# Claude Code — 專案安裝
cp -r <skill-folder> /path/to/your/project/.claude/skills/

# Cursor
cp -r <skill-folder> /path/to/your/project/.cursor/rules/

# 其他 agent — 將 SKILL.md 作為 system prompt 或自訂指令載入
```

Claude Code 中使用 `/<skill-name>` 呼叫 skill，或直接描述任務即可自動觸發。其他 agent 請在 prompt 或 rules 設定中引用 `SKILL.md` 內容。

## 📁 Repository 結構

```
Dev-Agent-Skills/
├── resume-craft/                  # 履歷撰寫與優化 skill
│   ├── SKILL.md                   # 核心指令（agent entry point）
│   └── references/                # 按需載入的 reference files
│       ├── best-practices.md      # 有來源引用的履歷 best practices
│       ├── collection-template.md # 每個職位的結構化訪談 template
│       ├── company-profiles.md    # Target company profiles
│       └── locale-rules.md        # 多語系術語規則
└── README.md                      # 本文件
```

## ✏️ 建立新 Skill

每個 skill 遵循以下結構：

```
skill-name/
├── SKILL.md                       # 必要：主指令檔（agent entry point）
├── references/                    # 選用：按需載入的 reference files
│   └── *.md
├── scripts/                       # 選用：輔助腳本
│   └── *.py / *.sh
└── README.md                      # 選用：human-readable 文件
```

**主要慣例：**
- `SKILL.md` frontmatter 必須包含 `name`、`description` 和 `allowed-tools`
- `SKILL.md` 保持精簡 — 詳細參考資料放到 `references/` 檔案中
- Skill 應獨立且可直接複製到任何專案使用

## 📞 聯絡資訊

**維護者：** Kevin Liu
