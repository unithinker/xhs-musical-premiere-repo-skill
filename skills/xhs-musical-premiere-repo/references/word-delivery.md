# Word Delivery: 风声内部版报告

Use this reference when the user asks to export the 小红书音乐剧首演 repo report as a Word document, especially “标准 Word 版”, “内部文档格式”, “页眉带风声 logo”, or “作为最后输出结果交付”.

## Output Contract

- Deliver a `.docx` as the final artifact.
- Keep the Markdown/report source if useful, but the final user-facing response should link to the `.docx`.
- Use Chinese report text and the same analytical structure from `report-template.md`.
- Do not include raw 小红书 links or long verbatim note text; keep representative notes summarized.

## 风声 Internal Format

Preferred visual system:

- US Letter portrait.
- Margins close to prior 风声 internal reports: top/bottom `1.905 cm`, left/right `2.159 cm`, header/footer distance `1.27 cm`.
- Header: centered 风声 logo, small enough to leave the report title visible on page 1.
- Footer: right-aligned page number in the form `第 N 页`, grey, small type.
- Typography: Microsoft YaHei or a Chinese-safe sans font; dark body text; gold accent for headings and table headers.
- Tables: gold header row, pale alternating rows, thin warm borders, adequate cell padding, repeated header rows, and no row splitting across pages.
- Use tables only for true grids: sample counts, sentiment distribution, dispute/risk matrix, representative notes. Use prose/bullets for analysis and recommendations.

## Locating The Logo / Existing Format

Use a Word template or logo image supplied by the user. If the template contains a logo, identify it in the header and extract the corresponding image from `word/media/`. Reuse the header sizing and footer style when the user requests an exact match.

If no template or logo is supplied, use the report layout described above with a text-only header. For other productions, apply the user's branding when provided.

## Logo Compatibility Note

The extracted 风声 logo may be a transparent PNG that Word can display but LibreOffice headless may render invisibly. Before inserting it into the final DOCX, create a flattened white-background copy for the header:

```python
from PIL import Image
im = Image.open("logo.png").convert("RGBA")
alpha = im.getchannel("A")
bbox = alpha.getbbox() or im.getbbox()
if bbox:
    pad = 60
    l, t, r, b = bbox
    im = im.crop((max(0, l-pad), max(0, t-pad), min(im.width, r+pad), min(im.height, b+pad)))
bg = Image.new("RGBA", im.size, (255, 255, 255, 255))
bg.alpha_composite(im)
bg.convert("RGB").save("logo_flat.png")
```

Use `logo_flat.png` in the header. Keep the original logo only as an intermediate asset; clean it up before final delivery unless the user asks for it.

## DOCX Build Guidance

Use the Documents skill/workflow and bundled Python runtime when available.

Implementation checklist:

- Create title block: `《<剧目>》首演小红书 Repo 舆情报告` plus subtitle `内部舆情监测报告`.
- Include metadata block: collection time, monitoring window, platform/method.
- Preserve sections:
  - 核心结论
  - 样本与方法
  - 情绪分布
  - 主题口碑分析
  - 负面与争议点
  - 内容侵权风险
  - 代表性笔记摘要
  - 运营建议
  - 限制说明
- Use real Word headings and real Word tables, not fake text grids.
- For tables, set explicit widths, cell padding, warm borders, repeated header rows, and row `cantSplit`.
- Avoid over-wide tables. If a table has many prose-heavy columns, shorten the cell text or split the table.

## Render And QA

Always try to render the DOCX to PNG pages with the Documents skill renderer:

```bash
env TMPDIR=/private/tmp python render_docx.py input.docx --output_dir /tmp/report_render --emit_pdf
```

Inspect representative pages:

- Page 1: logo visible, title not crowded, metadata block readable.
- A table page: no clipped text, no table row split, no missing/repeated partial rows, table header visible after page breaks where relevant.
- Final page: footer page number visible, no orphaned heading or large awkward blank gap.

If LibreOffice rendering fails due missing Homebrew libraries on macOS, install the missing dependency when allowed and retry. Known fixes from prior runs:

```bash
brew install little-cms2
brew install fontconfig
```

If rendering remains impossible, structurally inspect the DOCX package and state in the final response that visual render QA could not be completed.

## Final Response

Keep the final response concise:

- Link only the final `.docx` unless the user asks for intermediates.
- Mention that render/visual QA passed, or clearly disclose if it could not be run.
