# Investment Livestream Notes

> 投资直播语音转录 → 结构化 Obsidian 笔记的完整流水线

A [Claude Code](https://claude.ai/code) skill that processes Chinese investment/semiconductor livestream voice-to-text transcripts into a complete 4-file note package:

1. **原文稿.md** — Raw transcript archive with correction notes
2. **分析总结.md** — Three-layer deep analysis (literal / implicit / blind-spot)
3. **分享.html** — Gold-accented Chinese typography share page
4. **分享.pdf** — A4 printable PDF

## Installation

```bash
# Copy to your Claude Code skills directory
cp SKILL.md ~/.claude/skills/investment-livestream-notes/
```

Or install via Claude Code marketplace (if available).

## Usage

Invoke the skill when you have a Chinese investment livestream transcript:

```
/ investment-livestream-notes
```

Then provide the transcript file path or paste the content.

### Trigger Conditions

- Chinese voice-to-text transcripts with timestamps/speakers
- Content about: 投资, 半导体, AI, 存储, 芯片, 宏观, 方法论
- Source: 私域直播, 闭门课, X Space, 访谈

### Not Suitable For

- Short text snippets (< 100 lines)
- Non-Chinese content
- Non-investment topics
- Already structured content

## Pipeline

```
原文稿 → 错别字修正 → 事实核查 → 分析总结 → 分享.html → 分享.pdf
   ↑                        │                          │
   └── Fix errors at source, regenerate downstream ────┘
```

### Phase 1: Clean & Verify (质检阶段)

- **Step 1a** — Build correction map for voice-to-text errors (30-40 items)
- **Step 1b** — Fact-check 6-12 key claims via parallel WebSearch (✅/⚠️/❌/❓)

### Phase 2: Write Analysis (产出阶段)

- **原文稿.md** — Raw transcript + frontmatter + correction table
- **分析总结.md** — Three-layer analysis:
  1. 字面层 (Literal) — Structured extraction with tables
  2. 隐含层 (Implicit) — Unwritten logic, philosophy, psychology (longest section)
  3. 知识盲区层 (Blind Spots) — Missing risks, biases, conflicts of interest

### Phase 3: Generate Share Outputs (产出阶段)

- **分享.html** — Styled page with gold-accented classical Chinese typography
- **分享.pdf** — A4 PDF via Playwright headless Chromium

## Design Principles

- **Single Source of Truth** — Fix errors at origin, never patch only in final output
- **Three-Layer Analysis** — The implicit layer is the core value; anyone can summarize
- **Lightweight Verification** — 6-8 parallel WebSearches (~20K tokens), not deep-research (3M+ tokens)
- **Correction Map** — Document voice-to-text errors, apply fixes to analysis, preserve raw transcript

## File Naming

```
{date}-{topic-slug}-{speaker}-{platform}-{type}.{ext}
```

Example: `2026-06-04-AI基建存储MLCC市场策略-蒋宇飞-私域直播-分析总结.md`

## License

MIT
