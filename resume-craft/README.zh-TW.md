# Resume Craft

> **[English Version](README.md)**

資深面試官等級的履歷撰寫與優化工具，支援 JD matching 分析。結合科技業 recruiter、engineering hiring manager 和 ATS specialist 三方視角，產出面試即用的履歷。

---

## 快速開始

將 skill 資料夾複製到你的 agent skills 目錄，然後呼叫 skill。

```bash
# Claude Code
cp -r resume-craft ~/.claude/skills/resume-craft
claude
/resume-craft /path/to/resume.pdf /path/to/jd.pdf --lang en,zh-TW

# Cursor — 複製到 rules 目錄
cp -r resume-craft /path/to/your/project/.cursor/rules/resume-craft

# 其他 agent — 將 SKILL.md 作為 system prompt 載入
```

### 指令範例（Claude Code）

```bash
# 優化現有履歷
/resume-craft /path/to/resume.pdf

# 搭配目標 job description 優化
/resume-craft /path/to/resume.pdf /path/to/jd.pdf

# 多語系輸出
/resume-craft /path/to/resume.pdf --lang en,zh-TW,zh-CN
```

---

## 使用方式

### Mode 1：Optimize（預設）

```bash
/resume-craft /path/to/resume.pdf
/resume-craft /path/to/resume.pdf /path/to/jd.pdf
```

讀取履歷，根據內容中模糊或缺漏的部分引導式提問，然後產出優化版本。若提供 JD，會進行 gap analysis 和 ATS keyword matching。

### Mode 2：Deep Analysis（`--analysis`）

```bash
/resume-craft /path/to/resume.pdf --analysis
/resume-craft /path/to/resume.pdf /path/to/jd.pdf --analysis
```

在任何修改前先生成 `analysis_report_<name>_<date>.md` — 包含 6-second scan impression、JD requirements analysis、gap analysis 及 optimization plan。**確認前不會修改任何檔案。**

### Mode 3：Generate from Scratch

```bash
/resume-craft
/resume-craft /path/to/jd.pdf
```

未提供履歷時，進入 generate mode，透過結構化訪談蒐集經歷，從零建立履歷。

### 語言選項

所有模式支援 `--lang` 指定輸出語言。預設：自動偵測履歷語言。

```bash
--lang en              # 僅 English
--lang zh-TW           # 僅繁體中文
--lang en,zh-TW,zh-CN  # 三個版本，自動同步
```

---

## Workflow

```
Input detection → 蒐集關鍵資訊 → 以 XYZ formula 改寫
→ Quality verification loop → 呈現給使用者 → 反覆迭代至滿意
```

### 每個 phase 做了什麼？

1. **Input Detection** — 自動偵測履歷、JD、target company 和語言
2. **Information Collection** — 根據履歷內容引導式提問（generate mode 則進行完整訪談）
3. **Resume Generation** — 套用 Google XYZ formula、anti-inflation rules、ATS 優化及 target company profile
4. **Iteration** — 先修改英文版，自動同步至其他語言版本
5. **Interview Prep**（選用）— 生成逐條 Q&A 及 behavioral questions

---

## 核心功能

| 功能 | 說明 |
|------|------|
| **Google XYZ Formula** | 「Accomplished [X] as measured by [Y] by doing [Z]」— 每條 bullet point 遵循此結構 |
| **Anti-Inflation Rules** | 6 條防護規則，防止 role inflation、metric fabrication、terminology inflation 等 |
| **ATS Optimization** | 標準 section headers、keyword matching（目標 65-75%）、single-column layout |
| **JD Gap Analysis** | Must-have / nice-to-have skills 擷取、缺漏 keyword 辨識 |
| **Target Company Profiles** | TSMC、FAANG、startup、enterprise — 自動調整語氣和重點 |
| **Multi-Language Sync** | English 為 source of truth；修改自動同步至 zh-TW、zh-CN、ja、ko |
| **Never Guess Policy** | 未知 metrics 使用 `[PLACEHOLDER]` 標記 — 絕不捏造資料 |
| **Interview Test** | 每條 bullet point 檢查：「使用者能就此流暢回答 2 分鐘嗎？」 |

---

## 支援的輸入格式

| 輸入 | 格式 |
|------|------|
| 履歷 | `.pdf`、`.docx`、`.md`、`.txt` |
| Job description | URL、檔案或直接貼上文字 |
| Target company | 從 JD 或對話 context 自動偵測 |

---

## 生成的檔案

| 檔案 | 產生時機 | 說明 |
|------|---------|------|
| `resume_<name>_<date>.md` | 每次 | 優化或生成的履歷 |
| `analysis_report_<name>_<date>.md` | `--analysis` mode | 含 gap analysis 的詳細分析報告 |
| `interview_prep_<name>_<date>.md` | 使用者要求時 | 逐條 Q&A 及 behavioral questions |

---

## 檔案結構

```
resume-craft/
├── SKILL.md                       # 核心指令（agent entry point）
└── references/                    # 按需載入的 reference files
    ├── best-practices.md          # 有來源引用的履歷 best practices
    ├── collection-template.md     # 每個職位的結構化訪談 template
    ├── company-profiles.md        # Target company profiles（TSMC/FAANG/startup/enterprise）
    └── locale-rules.md            # 多語系 locale rules（zh-TW/zh-CN/ja/ko）
```

`SKILL.md` 為主指令檔，任何 AI coding agent 皆可載入。Reference files 按需載入，採用 progressive disclosure 模式以控制 context 大小。

---

**維護者：** Kevin Liu
**最後更新：** 2026-03-12