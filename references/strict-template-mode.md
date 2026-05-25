# Strict Template Mode

Strict template mode prevents the assistant from drifting away from the user's report format.

## When To Use

Use this mode when the user says:

- 只需要模板中的信息
- 用模板格式
- 按这个模板
- 一行只单列一处信息
- 不要分析
- 不要表格
- 其他不需要

Also use it when the user provides a filled historical example as a template.

## Rules

- Template labels and line order are the output contract.
- Old values inside the template are examples, not new facts.
- Source material and user corrections fill the template fields.
- Do not add extra fields, extra analysis, tables, duration calculations, summaries, or source notes.
- If the template uses one field per line, output one field per line.
- If the template uses plain timeline lines, keep plain timeline lines.
- If the source has extra records that do not map to a template field, ignore them unless the user asks to include them.
- If a required template field cannot be filled, write `[待确认]`.
- Later user corrections override all earlier content.

## Correction Handling

When the user corrects one field, regenerate the full report in the same template shape.

Example correction:

```text
接令时间：5月24日21时41分
```

Required behavior:

- Update only that field.
- Keep all other previously confirmed fields.
- Output the full corrected report.

## Railway Emergency Dispatch Format

For this shape:

```text
松滋西接触网工班应急出动信息:
接令时间：...
接令内容：...
作业车号：...
司机：...
轨道车出入库应急时间：
...
接触网添乘人员：...
工务添乘:...
```

Do not output:

- 表格
- 关键时间分析
- 运行记录
- 基本信息
- 图片识别摘要
- unrelated fields from a different image form

Do output:

- Same heading.
- Same field labels.
- Same field order.
- One timeline event per line.
