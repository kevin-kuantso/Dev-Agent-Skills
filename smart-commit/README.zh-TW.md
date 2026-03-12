# Smart Commit

> **[English Version](README.md)**

分析 code changes 並按 feature 拆分為乾淨、有邏輯的 commits，然後生成 PR description。把凌亂的 working directory 變成可 review 的 commit history。

---

## 快速開始

將 skill 資料夾複製到你的 agent skills 目錄，然後呼叫 skill。

```bash
# Claude Code
cp -r smart-commit ~/.claude/skills/smart-commit
claude
/smart-commit

# Cursor — 複製到 rules 目錄
cp -r smart-commit /path/to/your/project/.cursor/rules/smart-commit

# 其他 agent — 將 SKILL.md 作為 system prompt 載入
```

### 指令範例（Claude Code）

```bash
# 將目前的 changes 拆分為邏輯 commits（預設 base: main）
/smart-commit

# 指定不同的 base branch
/smart-commit --base develop
```

---

## 使用方式

當你有一堆 changes — 不論已 commit 或未 commit — 想在 push 或建立 PR 前整理成乾淨的 feature-based commits 時，執行 `/smart-commit`。

### 做了什麼

1. **蒐集** 從 base branch 分出後的所有 changes（committed + uncommitted）
2. **分析** 每個檔案變更，按 feature/concern 分組
3. **呈現** commit plan 等待你確認 — 包含偵測到的 unused files
4. **執行** plan：soft reset 後逐組重新 commit
5. **生成** 基於 final commits 的 PR title 和 description

### 同一檔案拆分到不同 commits

當同一個檔案包含多個 feature 的變更時，skill 會依照 logic blocks 或 functions 拆分 — 每個 commit 只包含與該 feature 直接相關的程式碼。使用 copy-aside 技巧安全地 stage partial changes，不會遺失任何內容。

---

## Workflow

```
蒐集 context → 按 feature 分析分組 → 呈現 commit plan
→ 使用者確認 → Soft reset & re-commit → 驗證 diff 完整性
→ 生成 PR description
```

---

## 核心功能

| 功能 | 說明 |
|------|------|
| **Feature-based splitting** | 按邏輯 feature 分組，而非按檔案 |
| **Partial file splits** | 當同一檔案涉及多個 feature 時，安全地拆分到不同 commits |
| **Unused file detection** | 辨識未被 import/使用的檔案，從 commits 中排除 |
| **Conventional Commits** | `feat`、`fix`、`refactor`、`docs`、`test`、`chore` 等 |
| **Diff integrity check** | 驗證拆分後的 total diff 與原始完全一致 |
| **PR description generation** | 產出 summary、change list、file structure 和 test plan |
| **Recovery guidance** | 出問題時，`git reflog` 可回到 reset 前的狀態 |

---

## 產出內容

### Commits

每個 commit 遵循 Conventional Commits 格式：

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

## 安全機制

- **Reset 前確認** — 執行 `git reset --soft` 前一定會詢問
- **不會 force-push** — 除非你明確要求
- **保留所有 changes** — 拆分後的 total diff 必須與原始一致
- **不刪除 unused files** — 回報給你，由你決定
- **可復原** — 出問題時 `git reflog` 可查看 reset 前的狀態

---

## 檔案結構

```
smart-commit/
├── SKILL.md                       # 核心指令（agent entry point）
└── README.md                      # 本文件
```

`SKILL.md` 為主指令檔，任何 AI coding agent 皆可載入。

---

**維護者：** Kevin Liu
**最後更新：** 2026-03-12