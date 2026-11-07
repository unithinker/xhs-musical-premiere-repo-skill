---
name: xhs-musical-premiere-repo
description: "Reusable workflow for Xiaohongshu (小红书) musical premiere repo/public-opinion work: collecting and triaging premiere-window notes, filtering fan/encore/focus noise, prioritizing genuine show evaluations, analyzing plot/music/staging/performance sentiment, identifying negative feedback and suspected content-infringement posts, and producing Chinese professional repo sentiment reports or Word-ready reports. Use when the user asks to monitor, scrape, summarize, or report 小红书 repo/剧评/观后感/舆情 for a musical, especially 首演, 首轮, 48小时内, 点赞排序, 负面, 侵权, 返场/focus去噪, or repeatable剧目舆情 workflows."
---

# 小红书音乐剧首演 Repo 舆情

## Core Workflow

1. Confirm the monitoring scope:
   - 剧目名, 别名, 常见简称, 主演/卡司关键词.
   - 时间窗, such as 首演日起 48 小时内.
   - Output: quick brief, professional report, Word document, spreadsheet, or local workbench setup.

2. Build keyword groups:
   - Evaluation: `<剧目> repo`, `<剧目> 剧评`, `<剧目> 观后感`, `<剧目> 好看吗`.
   - Content dimensions: `<剧目> 剧情`, `<剧目> 音乐`, `<剧目> 舞美`, `<剧目> 演员`.
   - Negative/risk: `<剧目> 避雷`, `<剧目> 差评`, `<剧目> 难看`, `<剧目> 退票`.
   - Infringement: `<剧目> 全场`, `<剧目> 盗摄`, `<剧目> 录屏`, `<剧目> 网盘`, `<剧目> 资源`.
   - Noise checks: `<剧目> 返场`, `<剧目> focus`, `<剧目> cut`, `<剧目> 谢幕`.

3. Collect with the least brittle available route:
   - Prefer existing logged-in browser state or the user's local workbench when Xiaohongshu login is required.
   - If using automation or CLI, do not bypass login, captcha, rate limits, or platform access controls. When blocked, report the blocker and switch to logged-in-browser assisted collection.
   - If the user explicitly asks to use a browser/computer tool, operate that tool rather than giving generic instructions.

4. Classify samples before summarizing:
   - 有效剧目评价 repo: includes substantive comments on story, music, lyrics, staging, direction, actors, pacing, emotional arc, or overall theatre experience.
   - 粉丝/返场/focus noise: mainly encore clips, curtain call, single-actor focus, beauty photos, ticketing, fan support, or pure cast praise without show evaluation.
   - 侵权疑似: full-show video, bootleg recording, screen recording, cloud drive/resource requests, download links, “私我发你”.
   - 待复核: ambiguous, too short, pure meme, or unclear whether the author attended.

5. Analyze only after de-noising:
   - Separate headline likes from evidence quality. High-like fan content must not dominate show-quality judgment.
   - Report sample size at each stage: visible candidates, opened/read notes, effective show-evaluation samples, excluded fan/noise samples, infringement-risk samples.
   - Track sentiment and topic together: positive/neutral/mixed/negative plus 剧情, 音乐/歌词, 舞美/视觉, 演员/表演, 票务/场馆.

6. Produce outputs:
   - For reports, read `references/report-template.md`.
   - For sampling and exclusion rules, read `references/sampling-rules.md`.
   - If creating a standard Word/internal document, also read `references/word-delivery.md`; use the available documents/docx workflow, apply the 风声 internal header/logo format, render/verify when possible, and deliver the final `.docx`.

## Quality Bar

- Be explicit about limitations: Xiaohongshu ranking is personalized, login state and search terms affect samples, and visible likes are not a complete population metric.
- Do not overclaim “全网舆情” from a small browser sample. Use “小红书可见样本” or “本次采样”.
- Use direct quotes sparingly; summarize and categorize instead.
- If the user asks for “客观” or “去粉丝属性”, increase weighting for content-rich notes and separate fan heat from show reputation.
- If the sample is too thin, say so and recommend expanding keywords/time range before drawing hard conclusions.

## Deliverable Defaults

Unless the user specifies otherwise, deliver in Chinese with:

- 样本与方法
- 核心结论
- 情绪分布
- 主题口碑分析
- 负面与争议点
- 内容侵权风险
- 代表性笔记摘要
- 运营建议
- 限制说明

When the user asks for a “标准 Word 版”, “内部文档格式”, “带风声 logo”, “导出 Word”, or asks for the final result as a deliverable file, create and return a Word document as the final artifact rather than only pasting the report in chat. Follow `references/word-delivery.md`.
