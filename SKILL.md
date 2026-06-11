---
name: investment-livestream-notes
description: Use when processing Chinese investment/半导体 livestream voice-to-text transcripts into structured Obsidian notes — creates raw transcript archive (原文稿), three-layer deep analysis (分析总结), styled HTML share (分享.html), and PDF. Triggered by Chinese transcripts with timestamps, multiple speakers, discussing stocks, AI, semiconductor, or macro topics.
---

# Investment Livestream Notes

## Overview

Process Chinese investment livestream speech-to-text transcripts into a complete 4-file note package: raw archive, three-layer analysis, shareable HTML, and PDF.

## When to Use

- User provides a long Chinese voice-to-text transcript with timestamps/speakers
- Content is about: 投资, 半导体, AI, 存储, 芯片, 宏观, 方法论
- User says "分析总结存档" or "整理成笔记" or "做成分享稿"
- Source is a livestream (私域直播/闭门课/X Space/访谈)

Do NOT use for:
- Short text snippets (< 100 lines)
- Non-Chinese content
- Non-investment topics
- Content already in structured form

## Core Workflow

**CRITICAL: This is a STRICT sequential pipeline. Each step MUST complete and pass verification before the next begins. HTML and PDF are always the LAST outputs, derived from verified, corrected analysis — never created in parallel with it.**

```
                    ┌─ 质检关口 ─┐
原文稿 → 错别字修正 → 事实核查 → 分析总结 → 分享.html → 分享.pdf
   ↑                    │                              │
   └── 发现错误就回到源头改，改完重新生成下游 ──────────┘
```

**Golden rule:** If you find an error in the HTML or PDF, do NOT fix it only there. Fix it in the analysis/transcript FIRST, then regenerate HTML and PDF from the corrected source. Single source of truth.

### Phase 1: Clean & Verify (质检阶段 — MUST complete before any output)

**Step 1a — Fix transcription errors (错别字修正)**

Voice-to-text ALWAYS has errors. Before writing anything else, scan the transcript for these patterns and build a correction map:

| Category | Common Errors → Fix |
|----------|-------------------|
| Company names | 马未都→Marvell, 春田→村田, 卢比→Ruby/Rubin, 陈永泰→陈泰铭 |
| Technical terms | 维拉/Vina/Vela→Vera, 旺卡集群→万卡集群, 诸葛→租给, 免缠→面板 |
| Stock nicknames | 赵王→兆王, 脸长链→两长链, 深藤链→昇腾链, 韩王→寒武纪 |
| People | 黄龄勋→黄仁勋, 简总→蒋总, 戴伟丽→戴伟立 |
| Combined terms | 纯叠→存储+堆叠 |

**Apply corrections to ALL analysis content.** The 原文稿 preserves the raw transcript as-is, but add a "转录修正备注" table at the bottom documenting all corrections. The 分析总结 and 分享.html MUST use corrected terms throughout.

**Step 1b — Fact-check objective claims (事实核查 — 轻量验证)**

Extract ~6-10 key falsifiable claims from the transcript. Run **6-8 parallel WebSearch queries** (NOT deep-research, NOT workflow fan-out) to verify them:
- Numbers: market size, revenue, profit, valuation, percentages
- Dates: IPO dates, product launches, index rebalancing
- Events: stock price movements, rank changes, announcements
- Named entities: CEO names, company rankings, ownership details

For each claim, assign: ✅ 确认 / ⚠️ 部分正确 / ❌ 错误 / ❓ 无法验证.

**Why NOT deep-research:** A 3M+ token, 100-agent workflow for livestream note verification is massive overkill. 13 direct parallel WebSearch calls (~20K tokens total) got 90% of the same answers. The marginal 10% (GTC vs COMPUTEX naming, xAI corporate structure trivia) doesn't justify 150x the cost. Reserve deep-research for open-ended discovery, not checklist verification.

**You MUST have verified results in hand before writing the analysis.** Guessing and fixing later wastes time and erodes trust.

**Quality Gate:** Before proceeding to Phase 2, confirm:
- [ ] All transcription errors identified and correction map built
- [ ] 6-8 WebSearch queries completed (target: <30K tokens total)
- [ ] Fact-check table populated with verified data

### Phase 2: Write Analysis (产出阶段 — based on verified data)

**Step 2a — 原文稿.md**

Save raw transcript with frontmatter. Append "转录修正备注" table at bottom. Do NOT modify the transcript body itself — it's the historical record. Corrections go in the note.

**Step 2b — 分析总结.md**

Write three-layer analysis using ONLY corrected terms and verified facts:
1. 字面层: Structured extraction with corrected data
2. 隐含层: Deeper logic (longest section)
3. 知识盲区层: Missing risks + unverified claims

Insert the "📋 事实核查" table between 隐含层 and 知识盲区层.

**Quality Gate:** Re-read the analysis. Scan for any remaining 错别字 using the correction map from Step 1a. If ANY found, fix them in the analysis content, not just in the correction table.

### Phase 3: Generate Share Outputs (产出阶段 — derived from verified analysis)

**Step 3a — 分享.html**

Create styled HTML page. ALL content copied/adapted from the corrected 分析总结. Never introduce new factual claims in HTML that weren't verified in Phase 1.

**Step 3b — Final error scan (终检)**

Before generating PDF, run a comprehensive grep scan against the correction map:

```
grep -n "错词1\|错词2\|错词3\|..." 分享.html
```

If ANY error pattern found → fix in 分析总结 first, then sync to HTML, then re-scan. Do NOT fix only in HTML.

**Step 3c — 分享.pdf**

Generate PDF from the verified, scanned HTML. Use playwright headless chromium.

### Step 1: Determine metadata

Extract from transcript context:
- **date**: ISO format (YYYY-MM-DD)
- **speaker**: Name + 身份 (e.g., "某主讲人，11年半导体产业经验")
- **platform**: 私域直播 / X Space / 闭门课 / 访谈
- **topics**: Array of topic tags in Chinese
- **duration**: Approximate length

### Step 2: Create 原文稿 (raw transcript)

**File path:** `{vault}/30_Investment/直播笔记/{date}-{speaker}-{platform}-原文稿.md`

**Frontmatter:**
```yaml
---
date: YYYY-MM-DD
type: investment-livestream
source: {speaker}{platform}
tags: [投资, {topics}, 直播转录, 原文稿]
---
```

**Body:** Full transcript with timestamps and speaker labels preserved. Add top-level `# {title} — {speaker} {platform} {date}（原文稿）` heading. Add duration note. Keep ALL spoken content, including Q&A banter — don't edit for clarity.

### Step 3: Create 分析总结 (three-layer analysis)

**File path:** `{date}-{speaker}-{platform}-分析总结.md` (same directory)

**Structure — three layers:**

1. **字面层 (Literal):** Structured summary of all key claims, organized by topic. Use tables for comparisons, data cards for numbers. Capture ALL arguments, not just highlights.

2. **隐含层 (Implicit):** What wasn't said explicitly but runs through the content:
   - The speaker's true investment philosophy (not what they claim, what their actions reveal)
   - Strategic positioning (why they're saying this NOW)
   - Audience psychology patterns they exploit
   - Narrative framing techniques

3. **知识盲区层 (Blind Spots):** What's missing or unverified:
   - Risks not discussed
   - Data claims needing verification
   - Selection bias in examples
   - Business model conflicts of interest
   - Assumptions that could break

End with **可操作提炼 (Actionable Takeaway)** — 4-6 concrete bullets.

### Step 4: Create 分享.html (styled share page)

**File path:** `{date}-{speaker}-{platform}-分享.html`

**Design system:** Gold-accented classical Chinese typography. Reference the template from any existing `分享.html` in the same directory for exact CSS and structure. Key design tokens:

- `--gold: #8a6918`, `--bg: #fbfaf7`
- Fonts: Noto Serif SC (body), Noto Sans SC (UI), JetBrains Mono (numbers)
- Max-width 660px container, centered
- Section headers with `## {num} {title}` format
- Block quotes for key insights (gold left border)
- Data cards with table layout for numbers/comparisons
- Strategy banners for core methodology
- Four-fix grid for methodology cards
- Closing section with numbered key takeaways

**Content structure standard for monologue livestreams:**
1. Header (eyebrow + title + speaker meta)
2. Introduction paragraph
3. Key insight blockquote
4. ~8-12 sections organized by topic
5. Closing with top-N quotes list (`.key-takeaways` numbered list)
6. Footer disclaimer

**For interview/Q&A formats:** Use `.q` (question) and `.a` (answer) blocks instead.

### Step 5: Generate PDF

Use Playwright Chromium headless:

```python
from playwright.sync_api import sync_playwright

with sync_playwright() as p:
    browser = p.chromium.launch()
    page = browser.new_page()
    page.goto(f'file://{html_path}', wait_until='networkidle')
    page.pdf(
        path=pdf_path, format='A4',
        margin={'top': '16mm', 'bottom': '16mm', 'left': '14mm', 'right': '14mm'},
        print_background=True
    )
    browser.close()
```

If playwright not available: `pip3 install playwright && python3 -m playwright install chromium`

## File Naming Convention

```
{date}-{topic-slug}-{speaker}-{platform}-{type}.{ext}

Examples:
2026-06-04-AI基建存储MLCC市场策略-某主讲人-私域直播-原文稿.md
2026-06-04-AI基建存储MLCC市场策略-某主讲人-私域直播-分析总结.md
2026-06-04-AI基建存储MLCC市场策略-某主讲人-私域直播-分享.html
2026-06-04-AI基建存储MLCC市场策略-某主讲人-私域直播-分享.pdf
```

**Topic slug rules:**
- 2-5 keywords, Chinese, separated by hyphens
- Capture the main themes: 存储, MLCC, AI基建, 投资方法论, 大势判断, etc.
- Derived from the transcript's dominant topics, NOT a full title
- Purpose: glance at filename → know what this is about without opening

All four files go in `{vault}/30_Investment/直播笔记/`.

## Three-Layer Analysis Framework

The core differentiator from a simple summary. Always apply all three layers:

| Layer | Question | Method |
|-------|----------|--------|
| 字面层 | What was said? | Structured extraction, tables, data cards |
| 隐含层 | What's really being said? | Read between lines: philosophy, positioning, psychology |
| 知识盲区层 | What's missing? | Identify unverified claims, unmentioned risks, biases |

The 隐含层 should be the longest section. This is where value is created — anyone can summarize, but identifying the unwritten logic is what makes the analysis useful.

## HTML Template Reference

For the complete HTML/CSS template, reference any existing `分享.html` in the vault's `30_Investment/直播笔记/` directory. The template is ~370 lines and should be adapted per-content, not copied verbatim. Key adaptations:

- Replace all text content
- Adjust section count to match content
- Use `.four-fix` grid only when 4-item methodology appears
- Use `.data-card` tables for comparisons and numbers
- Use `.strategy-banner` for core one-liners
- Use `.q`/`.a` blocks for interview formats; standard sections for monologues

## Common Mistakes

| Mistake | Why it happens | Fix |
|---------|---------------|-----|
| **Fixing errors only in final output** — e.g., correcting a typo in HTML but not in 分析总结 | Temptation to "quick fix" the visible file | Always fix at source (分析总结), then regenerate downstream. Single source of truth |
| **Over-investing in verification** — using deep-research workflow (100+ agents, 3M+ tokens, 20+ minutes) for simple fact-checking | "More thorough = better" fallacy | 6-8 parallel WebSearch calls (~20K tokens, <1 minute) covers 90%+ of verifiable claims. The marginal return of deep-research for note-taking is near zero |
| **Adding correction notes without actually fixing the text** — a "转录修正备注" table that documents errors but leaves them in the body | Feels like "I documented it so I'm done" | The table is a MAP, not the fix. Apply corrections to the actual content |
| **Skipping 原文稿** — "transcript is already in chat" | Chat transcript disappears; 原文稿 preserves with frontmatter for Obsidian linking | Always save raw transcript |
| **Shallow summary** — only literal layer | Rushing to output | Three layers mandatory. 隐含层 > 字面层 in depth |
| **Creating HTML/PDF before verification** — generating pretty output with unverified numbers | Excitement to see the finished product | Quality gate: facts verified before HTML is written |
| **Fixing errors in HTML, then forgetting to regenerate PDF** | PDF is "invisible" until opened | After ANY HTML change, always regenerate PDF |
| **No print styles in HTML** | Focused on screen rendering | `@media print` mandatory for PDF quality |

## Red Flags — STOP and Go Back

- "I'll just fix this one typo in the HTML and move on" → Fix in analysis, regenerate both
- "The correction table is enough, I don't need to change the body text" → Wrong. Table = map, not fix
- About to create HTML before finishing fact-check → Quality gate not passed
- "This is a small transcript, I'll skip the error scan" → Small transcripts have typos too
- Found an error in PDF but only fixed it in PDF → Source of truth violation
- "I'll just do a summary, not three layers" → Core value proposition lost

**All red flags mean: Stop. Go back to Phase 1. Follow the pipeline in order.**

## Quick Reference

| Output | Content | Validation |
|--------|---------|------------|
| 原文稿.md | Full transcript + frontmatter | Timestamps preserved, all speakers labeled |
| 分析总结.md | Three layers + fact-check table | 隐含层 > 字面层 in depth, 事实核查 present |
| 分享.html | Gold-accented styled page | Print styles present, 8-12 sections |
| 分享.pdf | A4 PDF from HTML | ~3-4 MB, renders correctly |

**Note on transcript length:** ALL transcripts get the full 4-file treatment regardless of length. Short transcripts (<50 lines) still need HTML + PDF — just with fewer sections. Never skip HTML/PDF because "the transcript is too short."
