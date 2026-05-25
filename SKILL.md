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
- Never invent concrete facts, names, dates, metrics, or results.
- Mark missing or uncertain information as `[待确认]`, `[需补充]`, or `[识别不确定]`.
- Convert rough notes into concise, factual, result-oriented report language.
- Separate facts, judgments, risks, next actions, metrics, and support requests.
- Keep unsupported draft content visibly editable in direct-generation mode.
- The first output is always `汇报初稿` unless the user explicitly asks for final output with no confirmation.
- Every draft must end with a user confirmation or micro-edit request.
- After the user confirms or provides edits, apply the feedback and output a clean `最终汇报`.

## Workflow

1. Identify the input mode.
2. Parse the template structure: title, audience, period, headings, subheadings, fixed wording, table fields, required fields, optional fields, and placeholders.
3. Extract source material:
   - Images: OCR text in image order and flag unclear text.
   - Text notes: split rough notes into atomic items.
   - Generation brief: extract topic, audience, reporting period, emphasis, and tone.
4. Build a structured intermediate record:
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
5. Map extracted items to template sections using the field mapping guidance in `references/field-mapping.md` when needed.
6. Rewrite into polished report language using `references/tone-rules.md` when needed.
7. Produce `汇报初稿`.
8. Add `待确认项`.
9. Ask the user to confirm or micro-edit, following `references/confirmation-rules.md`.
10. After confirmation, produce `最终汇报`.

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

## Template Assets

Use bundled templates when the user names them:

- `assets/templates/daily-report.md`
- `assets/templates/weekly-report.md`
- `assets/templates/project-report.md`

If the user provides their own template, prefer the user's template over bundled assets.
