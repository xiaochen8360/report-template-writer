# Report Template Writer

English | [中文](#中文说明)

Report Template Writer is a Codex skill for turning journal photos, handwritten notes, rough work logs, meeting notes, or short generation briefs into fixed-format report drafts.

It is designed for repetitive reporting workflows where you already have a template, but manually filling it every day or every week costs too much time.

## What It Does

- Parses a user-provided report template.
- Supports three input modes:
  - Template + images
  - Template + text notes
  - Template + generation brief
- Extracts facts, progress, risks, metrics, next actions, and support needs.
- Preserves the original template structure and section order.
- Provides strict template mode for cases where only fields in the template should be output.
- Includes a feedback log and reusable template library for real-world corrections.
- Produces a draft first.
- Marks uncertain or missing information.
- Asks the user to confirm or edit.
- Produces a clean final report after confirmation.

## Why It Exists

Many work reports are not hard because the thinking is hard. They are hard because the same scattered information has to be manually rewritten into the same fixed format again and again.

This skill turns that repeated work into a stable workflow:

```text
Template -> Source material -> Draft -> Confirmation -> Final report
```

## Input Modes

### 1. Template + Images

Use this when you have journal photos, handwritten notes, paper records, whiteboard photos, or screenshots.

The skill will OCR the images, extract information, flag uncertain recognition, and fill the report template.

### 2. Template + Text Notes

Use this when you have rough notes, daily logs, meeting notes, or fragmented records.

The skill will clean the notes, split them into items, classify each item, and rewrite them into polished report language.

### 3. Template + Generation Brief

Use this when you only have a template and a short instruction.

Example:

```text
Use this weekly report template. Draft a report about this week's content operations, customer communication, data review, and next week's plan.
```

The skill will generate an editable draft and mark unsupported details as `[待确认]` or `[需补充]`.

## Default Workflow

1. Identify the input mode.
2. Check the reusable template library.
3. Separate the template from source facts.
4. Parse the template structure and formatting style.
5. Extract source material from images, text notes, or a generation brief.
6. Extract facts, decisions, risks, metrics, next actions, and support needs.
7. Apply user corrections as the highest-priority facts.
8. Map extracted items only to template sections.
9. Rewrite rough notes into report language when needed.
10. Produce a report draft.
11. Add confirmation items when needed.
12. Ask the user to confirm or micro-edit.
13. Capture reusable feedback from corrections.
14. Produce the final report after confirmation.

## Strict Template Mode

Use strict template mode when the user says things like:

```text
Only use the fields in this template.
Use this exact template format.
One item per line.
No analysis.
No tables.
```

In this mode, the skill treats the template as the output contract:

- It outputs only template fields.
- It keeps the original field order and line-break style.
- It does not add analysis, duration calculations, tables, summaries, or extra sections.
- It treats old values in a filled example as examples, not new facts.
- It applies later user corrections over OCR or earlier assistant output.

## Example Prompt

```text
Use report-template-writer.

Template:
[paste my daily report template]

Source:
[attach journal photos]

Requirements:
Make it concise, formal, and suitable for reporting to my manager. Keep risks and next actions separate.
```

## Output Shape

First response:

```markdown
# 汇报初稿

...

# 待确认项

...

# 请确认或微调

...
```

After confirmation:

```markdown
# 最终汇报

...
```

## Installation

Copy this folder into your Codex skills directory:

```bash
cp -R report-template-writer ~/.codex/skills/
```

Then trigger it by name:

```text
Use report-template-writer to fill this report template from my notes.
```

## Repository Structure

```text
report-template-writer/
├── SKILL.md
├── README.md
├── feedback.md
├── references/
│   ├── field-mapping.md
│   ├── tone-rules.md
│   ├── confirmation-rules.md
│   ├── strict-template-mode.md
│   └── continuous-improvement.md
├── templates/
│   ├── README.md
│   └── emergency-dispatch.md
├── assets/
│   └── templates/
│       ├── daily-report.md
│       ├── weekly-report.md
│       ├── project-report.md
│       └── railway-emergency-dispatch.md
└── agents/
    └── openai.yaml
```

## License

MIT

---

# 中文说明

Report Template Writer 是一个 Codex Skill，用来把手帐图片、手写笔记、工作流水账、会议记录、零散文字，或者一句简短生成说明，转换成固定模板格式的汇报初稿。

它适合那些“模板是固定的，但每次都要手动填写”的重复汇报场景。

## 它能做什么

- 解析用户提供的汇报模板。
- 支持三种输入模式：
  - 模板 + 图片
  - 模板 + 文字记录
  - 模板 + 生成说明
- 抽取事实、进展、风险、数据、下一步计划和支持需求。
- 保持原模板结构和栏目顺序。
- 提供严格模板模式，只输出模板中已有的信息项。
- 包含反馈日志和可复用模板库，用真实修正持续优化。
- 先生成初稿。
- 标注不确定或缺失信息。
- 请求用户确认或微调。
- 用户确认后，再输出干净的最终版。

## 为什么需要它

很多汇报不是难在思考，而是难在每天、每周都要把散乱记录重新填进同一套格式里。

这个 Skill 把重复劳动固定成一条流程：

```text
模板 -> 素材 -> 初稿 -> 确认 -> 最终版
```

## 三种输入模式

### 1. 模板 + 图片

适合手帐照片、纸质笔记、白板照片、截图等。

Skill 会先识别图片文字，再抽取信息、标注识别不确定项，并按模板生成汇报初稿。

### 2. 模板 + 文字记录

适合流水账、会议记录、零散事项、工作日志。

Skill 会清洗文字、拆分事项、归类内容，并改写成正式汇报语言。

### 3. 模板 + 生成说明

适合你只有模板和大概主题，需要先起草一版。

例如：

```text
用这个周报模板，生成一份本周运营周报，重点写内容发布、客户沟通、数据复盘和下周计划。
```

Skill 会生成可编辑草稿，并把没有事实依据的内容标记为 `[待确认]` 或 `[需补充]`。

## 默认流程

1. 判断输入模式。
2. 检查可复用模板库。
3. 区分模板结构和素材事实。
4. 解析模板结构和格式风格。
5. 从图片、文字或生成说明中提取素材。
6. 抽取事实、决策、风险、数据、下一步动作和支持需求。
7. 将用户后续纠错作为最高优先级事实。
8. 只把素材映射到模板已有栏目。
9. 必要时把手帐语言改写成汇报语言。
10. 生成汇报初稿。
11. 必要时输出待确认项。
12. 请求用户确认或微调。
13. 从用户修正中记录可复用反馈。
14. 用户确认后输出最终版。

## 严格模板模式

当用户说：

```text
只需要模板中的信息
用模板格式
一行只单列一处信息
不要分析
不要表格
```

Skill 会进入严格模板模式：

- 只输出模板字段。
- 保持字段顺序和换行格式。
- 不添加分析、耗时计算、表格、摘要或额外栏目。
- 用户给的历史填充示例只作为模板，不当作新事实。
- 用户后续纠错优先于 OCR 和前文输出。

## 使用示例

```text
使用 report-template-writer。

模板：
[粘贴我的日报模板]

素材：
[上传手帐图片]

要求：
简洁正式，适合向老板汇报，风险和下一步计划单独列出。
```

## 输出格式

第一次输出：

```markdown
# 汇报初稿

...

# 待确认项

...

# 请确认或微调

...
```

用户确认后：

```markdown
# 最终汇报

...
```

## 安装方式

把这个文件夹复制到 Codex skills 目录：

```bash
cp -R report-template-writer ~/.codex/skills/
```

然后用名称触发：

```text
使用 report-template-writer，把这些记录套进这个汇报模板。
```

## 项目结构

```text
report-template-writer/
├── SKILL.md
├── README.md
├── feedback.md
├── references/
│   ├── field-mapping.md
│   ├── tone-rules.md
│   ├── confirmation-rules.md
│   ├── strict-template-mode.md
│   └── continuous-improvement.md
├── templates/
│   ├── README.md
│   └── emergency-dispatch.md
├── assets/
│   └── templates/
│       ├── daily-report.md
│       ├── weekly-report.md
│       ├── project-report.md
│       └── railway-emergency-dispatch.md
└── agents/
    └── openai.yaml
```

## 许可证

MIT
