# Multi-Language Locale Rules

## Table of Contents
- English (en)
- Traditional Chinese — Taiwan (zh-TW)
- Simplified Chinese — China (zh-CN)
- Japanese (ja)
- Korean (ko)
- Cross-Locale Quality Rules

---

## English (en)
Default. American English unless target company is UK/AU-based.

---

## Traditional Chinese — Taiwan (zh-TW)

**Technical terms stay in English:** React, Kubernetes, CI/CD, AWS, Sprint, Scrum, API, SDK, PWA, RBAC, Git, Redis, Datadog, Grafana, LCP, CLS, TBT

**Job titles bilingual:** "資深前端工程師 (Senior Frontend Engineer)"

**Section headers bilingual:** "工作經歷 (Experience)"

**Taiwan-standard phrasing (NOT mainland):**

| Use (Taiwan) | NOT (Mainland) | Notes |
|---|---|---|
| 專案 | 項目 | |
| 軟體 | 軟件 | |
| 程式 | 程序 | |
| 網頁 | 网页 | |
| 伺服器 | 服務器 | |
| 使用者 | 用戶 | 常見錯誤 |
| 呼叫 | 調用 | 常見錯誤 |
| 資料 | 數據 | exception: 數據分析 is OK in Taiwan |
| 資訊 | 信息 | |
| 團隊培養 | 導師帶教 | 台灣不用「帶教」 |
| 品牌重塑 | — | 注意不要打成「品牌重塾」(unicode 相似字) |
| 排程 | 調度 | |
| 元件 | 組件 | React component = React 元件 |
| 函式 | 函數 | |
| 迴圈 | 循環 | |

**Abstraction rule:** If using abstract terms like「架構治理」「工程卓越」「技術願景」, you must be able to list 3-5 concrete actions that support it. If you can't, replace with a more direct description. Example: 「架構治理」→「制定共用元件規範、建立 Code Review 流程、統一 API 設計模式」

**Resume-specific conventions:**
- Verb starters: 負責、主導、設計並建置、推動、榮獲 (don't literally translate English past-tense verbs)
- Date format: 2024/09 – 迄今 (use 迄今, not 至今, for formality)
- Education: 國立臺灣大學 (full name), 學士 / 碩士 (not 本科)
- Company names in original language

---

## Simplified Chinese — China (zh-CN)

**Translate most technical terms:**

| English | zh-CN |
|---|---|
| Micro-frontend | 微前端 |
| Design System | 设计系统 |
| Atomic Design | 原子设计 |
| Lazy Loading | 懒加载 |
| Code Splitting | 代码分割 |
| Code Review | 代码审查 |
| Sprint Planning | 迭代规划 |
| Responsive Design | 响应式设计 |
| Prompt Engineering | 提示词工程 |
| Onboarding | 入职引导 |
| Coding Standards | 编码规范 |
| Monitoring | 监控 |
| Observability | 可观测性 |
| Mentoring | 团队培养 (NOT 导师带教) |

**Keep in English:** Vue.js, React, GitHub Actions, CI/CD, API, SDK, HTTP, SQL, Git, PWA, RBAC, MSW, Redis, Datadog, Grafana, Mixpanel, LCP, CLS, TBT

**Mainland-standard phrasing:**

| Use (Mainland) | NOT (Taiwan) |
|---|---|
| 项目 | 專案 |
| 软件 | 軟體 |
| 程序 | 程式 |
| 高级前端工程师 | 資深前端工程師 |
| 用户 | 使用者 |
| 调用 | 呼叫 |
| 数据 | 資料 |
| 信息 | 資訊 |
| 组件 | 元件 |
| 函数 | 函式 |

**Region handling:** If user worked in Taiwan, write "台北，中国台湾" for zh-CN version.
**Education:** 教育背景 (not 學歷), 硕士 (not 碩士)
**Job titles fully translated:** 高级前端工程师

---

## Japanese (ja)
- Technical terms in katakana or English: フロントエンド, Kubernetes, React
- Formal keigo for business documents

---

## Korean (ko)
- Mix of Korean and English technical terms per industry convention
- Formal written style (합니다체)

---

## Cross-Locale Quality Rules

These rules apply when generating or syncing multiple language versions:

1. **Abstraction backing rule:** Any abstract concept (架構治理, Engineering Excellence, 工程卓越) must be supportable by 3-5 concrete actions the user actually did. If it can't be backed up, use a more direct description instead.

2. **Unicode vigilance:** When editing CJK text, watch for visually similar but different characters (塑 vs 塾, 員 vs 貝). Always verify after edits — unicode substitution errors are easy to introduce and hard to spot.

3. **Number consistency:** After any edit, verify all numbers (percentages, team sizes, user counts, dates) are identical across all language versions.

4. **Bullet count parity:** All language versions must have the same number of bullets per role.

---

For other languages, ask about local conventions before proceeding.
