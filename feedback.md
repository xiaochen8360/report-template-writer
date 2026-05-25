# Feedback Log

This file stores verified issues, user corrections, and template-specific lessons.

Use it as an audit log. Do not silently rewrite history. Append new entries.

## Entry Format

```markdown
## YYYY-MM-DD - short title

- Mode:
- Template:
- Symptom:
- User correction:
- Root cause:
- Rule update candidate:
- Status: open / confirmed / applied
```

## 2026-05-25 - Railway emergency dispatch format drift

- Mode: Template + image/text notes
- Template: `templates/emergency-dispatch.md`
- Symptom: The assistant treated the user's historical filled example as source facts, added unrelated image fields, created tables, added analysis, and failed to keep one field per line.
- User correction: The user repeatedly requested template-only output, one item per line, corrected dispatch time, driver, passenger roles, and final stabling wording.
- Root cause: Missing strict template mode, weak template/source separation, and insufficient correction priority.
- Rule update candidate: Use strict template mode whenever the user says `只需要模板中的信息`, `用模板格式`, `按这个模板`, `一行只单列一处信息`, `不要表格`, or `不要分析`.
- Status: applied

## 2026-05-25 - Template old values copied before image extraction

- Mode: Template + image
- Template: `templates/emergency-dispatch.md`
- Symptom: The assistant copied the user's pasted filled template values, even though the attached image showed a different date and different source record.
- User correction: The user pointed out that the image was `5月17日`, not the pasted template's date.
- Root cause: The workflow did not force Template + images mode when a template and image appear together, and did not define source roles strongly enough.
- Rule update candidate: In Template + images mode, immediately split input into `template_contract`, `image_source`, `user_supplement`, and `template_library`; dynamic facts must come from image extraction or explicit user supplement, never from old template values.
- Status: applied
