# Confirmation Rules

Every draft must end with a confirmation or micro-edit request.

## Mode A: Template + Images

Ask the user to confirm:

- Whether OCR text was recognized correctly.
- Whether unclear image text should be corrected.
- Whether any item was placed in the wrong section.
- Whether anything visible in the image was omitted.

Default ending:

```text
请确认这版是否可以作为最终版。如果需要微调，可以直接告诉我：
- 哪些图片内容识别错了
- 哪些事项要换栏目
- 哪些模糊内容需要补成真实信息
- 语气是否需要更正式、更简洁，或更有结果感
```

## Mode B: Template + Text Notes

Ask the user to confirm:

- Whether the original meaning was understood correctly.
- Whether any rough-note item was omitted.
- Whether anything should be removed or softened.
- Whether the tone is appropriate.

Default ending:

```text
请确认这版是否符合你的意思。如果要微调，可以直接说：
- 保留或删除哪条内容
- 某个事项换到哪一栏
- 哪些表达需要更正式、更简短，或更有结果感
- 哪些 [待确认] 内容可以补上
```

## Mode C: Template + Generation Brief

Ask the user to confirm:

- Which project names, dates, people, and metrics are real.
- Which draft sections should be replaced with actual facts.
- Which content should be kept, removed, or compressed.
- Whether the report should become a final version.

Default ending:

```text
这版是根据模板和生成说明起草的初稿，其中带有 [待确认] 或 [需补充] 的内容需要你确认。你可以直接告诉我：
- 替换哪些项目名、数据、时间或人物
- 哪些内容不符合实际
- 哪些内容可以保留
- 是否需要压缩成最终汇报版
```

## Finalization

When the user says `确认`, `可以`, `没问题`, `生成最终版`, `按这个改完`, or provides concrete edits:

1. Apply the user's edits.
2. Remove the confirmation prompt.
3. Keep unresolved placeholders if still unsupported.
4. Output only `# 最终汇报` unless the user asks for notes.
