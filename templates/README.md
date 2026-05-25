# Template Library

This folder stores verified templates and template-specific lessons.

Use these templates before falling back to generic report generation.

## Matching Rules

1. Match by explicit template name if the user provides one.
2. Match by unique headings and field labels.
3. Match by domain keywords.
4. If multiple templates match, choose the one with the closest field structure.
5. If no template matches, use the generic workflow and propose saving the confirmed result as a new template.

## Available Templates

| Template | Use case | Matching signals |
|---|---|---|
| `emergency-dispatch.md` | Railway emergency dispatch reports | `接令时间`, `接令内容`, `作业车号`, `轨道车出入库应急时间`, `接触网添乘人员`, `工务添乘` |

## Template Promotion Rule

When a user confirms a final result for a new recurring format:

1. Ask whether to save it as a reusable template.
2. Save the field structure, not one-time facts.
3. Add common pitfalls and confirmed formatting rules.
4. Register it in this README.
