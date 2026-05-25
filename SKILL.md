---
name: report-template-writer
description: Use when the user wants to turn journal photos, handwritten notes, work logs, meeting notes, rough text, or a short generation brief into a fixed report template such as daily reports, weekly reports, project updates, reviews, or management-facing summaries. Supports filling templates from images, filling templates from text, and drafting directly from a template plus brief. Always produces a draft first, asks for user confirmation or edits, then produces a final version after confirmation.
metadata:
  short-description: Turn notes or briefs into template-based report drafts
---

# Report Template Writer

## Purpose

Convert user-provided templates plus images, notes, logs, or generation briefs into polished report drafts and final reports.

This skill is for template-based reporting, not generic summarization. The goal is to preserve the user's report template, extract or draft the right content, mark uncertainty, ask for confirmation, and only then produce a clean final version.

## Input Modes

1. **Template + images**
   - OCR journal photos, handwritten notes, whiteboards, screenshots, or paper records.
   - Extract facts and uncertain text.
   - Fill the report template.

2. **Template + text notes**
   - Clean rough work logs, meeting notes, or fragmented records.
   - Extract facts, decisions, risks, metrics, and next actions.
   - Fill the report template.

3. **Template + generation brief**
   - Draft from the user's template and short brief.
   - Mark unsupported facts, names, dates, metrics, and conclusions as `[待确认]` or `[需补充]`.
   - Treat the result as an editable draft, not verified fact.

If the user provides only a template and no source material or brief, return a fillable framework and ask for source material, topic, audience, or reporting period.

## Core Rules

- Preserve the user's template structure, section order, headings, and required fields.
- Treat the template as the output contract, not as source facts. Only output fields that exist in the template unless the user explicitly asks to add fields.
- If the user says "只需要模板中的信息", "用模板格式", "一行只单列一处信息", or similar, switch to strict template mode.
- Never invent concrete facts, names, dates, metrics, or results.
- Mark missing or uncertain information as `[待确认]`, `[需补充]`, or `[识别不确定]`.
- Convert rough notes into concise, factual, result-oriented report language.
- Separate facts, judgments, risks, next actions, metrics, and support requests.
- Keep unsupported draft content visibly editable in direct-generation mode.
- User corrections override OCR, image inference, and earlier assistant output.
- Do not add analysis, duration calculations, tables, summaries, or extra sections unless those fields exist in the template or the user requests them.
- Preserve the user's line-break style when the user provides a concrete template example.
- The first output is always `汇报初稿` unless the user explicitly asks for final output with no confirmation.
- Every draft must end with a user confirmation or micro-edit request.
- After the user confirms or provides edits, apply the feedback and output a clean `最终汇报`.

## Strict Template Mode

Use strict template mode when:

- The user gives a concrete template example and asks to fill it.
- The user corrects the assistant with wording such as `只需要模板中的信息`, `用模板格式`, `按这个模板`, `一行只单列一处信息`.
- The source contains more information than the template needs.

In strict template mode:

- Output exactly the template's fields and field order.
- Do not create tables unless the template uses tables.
- Do not create extra headings such as `关键时间分析`, `运行记录`, `信息来源摘要`, or `待确认项` inside the report body unless they are in the template.
- Put each field or timeline item on its own line if the template does so.
- Keep punctuation, labels, and spacing close to the user's template.
- If a template field is missing from the source, keep that field and use `[待确认]`.
- If the user supplies corrections, immediately regenerate the full report in the same template format rather than explaining the correction.

For railway emergency dispatch templates like:

```text
松滋西接触网工班应急出动信息:
接令时间：5月16日17时40分
接令内容：工班值班员接松滋西站通知传达行调要求供电轨道车应急出库
作业车号：0793021
司机：肖晨，许爽
轨道车出入库应急时间：
17:51联控车站
17:53出库
17:58到达北牵待命
21:23联控入库
21:26入库并设置防溜
接触网添乘人员：袁子傲，占天佐，张恒语
工务添乘:无
```

The output must keep the same shape: one field per line, timeline items as plain lines, no tables, no added analysis, and no unrelated image-template fields.

## Workflow

1. Identify the input mode.
2. Check the reusable template library in `templates/README.md` when a domain or field pattern is recognizable.
3. If a saved template matches, follow that template's output contract, formatting rules, and pitfalls.
4. Separate the template from source facts. If the user provides an example filled with old values, extract its labels, order, and formatting as the template; do not treat old values as facts for the new report unless the user says they still apply.
5. Parse the template structure: title, audience, period, headings, subheadings, fixed wording, table fields, required fields, optional fields, placeholders, line-break style, and whether it uses tables or plain lines.
6. Extract source material:
   - Images: OCR text in image order and flag unclear text.
   - Text notes: split rough notes into atomic items.
   - Generation brief: extract topic, audience, reporting period, emphasis, and tone.
7. Build a structured intermediate record:
   - `date_range`
   - `audience`
   - `projects`
   - `people`
   - `completed_items`
   - `progress`
   - `problems`
   - `risks`
   - `decisions`
   - `metrics`
   - `next_actions`
   - `support_needed`
   - `open_questions`
   - `uncertain_items`
8. Apply user corrections as highest-priority facts.
9. Map extracted items only to template fields using the field mapping guidance in `references/field-mapping.md` when needed.
10. Rewrite into polished report language using `references/tone-rules.md` when needed.
11. Produce `汇报初稿`, or a strict template-only draft when strict template mode is active.
12. Add `待确认项` after the report only when there are unresolved items. Do not insert it into the report body in strict template mode.
13. Ask the user to confirm or micro-edit, following `references/confirmation-rules.md`.
14. When the user corrects the draft, follow `references/continuous-improvement.md`: capture reusable feedback, update the current output, and propose saving recurring formats.
15. After confirmation, produce `最终汇报`.

## Template Library and Feedback

- Check `templates/` before using generic logic.
- Use `templates/emergency-dispatch.md` for railway emergency dispatch reports.
- Append reusable issues and corrections to `feedback.md` when filesystem access is available.
- If filesystem access is not available, output a compact feedback block for manual saving.
- Do not update core rules silently. Summarize proposed rule changes and ask for confirmation before changing `SKILL.md`.
- Review `feedback.md` weekly to decide which repeated issues should become durable rules.

## Default Draft Output

Return these sections:

```markdown
# 汇报初稿

...

# 待确认项

- ...

# 请确认或微调

请确认这版是否可以进入最终版。你可以直接告诉我：
- 哪些内容识别错了
- 哪些事项要换栏目
- 哪些表达要更正式或更简洁
- 哪些 [待确认] 内容可以补上
```

If there are no uncertainties, still ask for confirmation:

```markdown
# 请确认或微调

请确认这版是否可以作为最终版；如果要调整，可以直接指出栏目、语气或具体句子。
```

## Final Output

Only after the user confirms or provides edits, return:

```markdown
# 最终汇报

...
```

Do not include reasoning notes, source extraction details, or confirmation prompts in the final version unless the user asks for them.

If the user is actively correcting field values, output the corrected report in full using the same template format. Do not switch to prose, table format, or analysis mode.

## Template Assets

Use bundled templates when the user names them:

- `assets/templates/daily-report.md`
- `assets/templates/weekly-report.md`
- `assets/templates/project-report.md`
- `assets/templates/railway-emergency-dispatch.md`

If the user provides their own template, prefer the user's template over bundled assets.

Use the verified template library for domain-specific formats:

- `templates/emergency-dispatch.md`
