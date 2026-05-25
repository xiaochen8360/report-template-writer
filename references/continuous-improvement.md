# Continuous Improvement

Use this workflow to keep the skill improving from real usage.

## Feedback Capture

When the user corrects the output, capture:

- Original output problem
- User correction
- Template involved
- Root cause
- Proposed rule update
- Whether the correction is one-time or reusable

If filesystem access is available, append the entry to `feedback.md`.

If filesystem access is not available, output a compact feedback block so the user can save it:

```markdown
## Feedback Entry

- Template:
- Symptom:
- User correction:
- Root cause:
- Reusable rule:
```

## Template Matching

Before generating, check `templates/README.md` and matching template files.

Match in this order:

1. Explicit template name from the user.
2. Unique field labels.
3. Domain keywords.
4. Similar line structure.

If a template matches, follow its `Output Contract`, `Formatting Rules`, and `Pitfalls`.

## Template + Image Source Roles

When a user provides both a filled template example and an image, classify the inputs before generating:

- `template_contract`: field labels, order, punctuation, and line-break style.
- `image_source`: dynamic facts visible in the image.
- `user_supplement`: explicit current-report corrections or additions.
- `template_library`: reusable structure, fixed boilerplate, and pitfalls.

Do not copy old filled values from `template_contract` into the new report.

Dynamic fields must come from `image_source` or `user_supplement`; if neither supports them, use `[待确认]`.

## New Template Capture

When a final report format is confirmed and likely recurring:

1. Ask whether to save it as a reusable template.
2. Save the structure, labels, and formatting rules.
3. Remove one-time facts.
4. Add pitfalls learned during correction.
5. Register the file in `templates/README.md`.

## Weekly Optimization

Once a week, review `feedback.md`:

1. Group repeated symptoms.
2. Identify rules that should move into `SKILL.md`.
3. Identify domain-specific lessons that should move into `templates/`.
4. Ask the user to confirm updates before changing core rules.
5. Commit changes with a short message.
