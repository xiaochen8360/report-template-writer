# Emergency Dispatch Template

## Name

Railway emergency dispatch report / 工班应急出动信息

## Matching Signals

Use this template when the input contains several of these labels:

- 接令时间
- 接令内容
- 作业车号
- 司机
- 轨道车出入库应急时间
- 接触网添乘人员
- 工务添乘

## Output Contract

Output exactly this shape:

```text
松滋西接触网工班应急出动信息:
接令时间：[待确认]
接令内容：[待确认]
作业车号：[待确认]
司机：[待确认]
轨道车出入库应急时间：
[待确认]
接触网添乘人员：[待确认]
工务添乘:[待确认]
```

## Formatting Rules

- One field per line.
- Timeline events are plain lines under `轨道车出入库应急时间：`.
- Do not use tables.
- Do not add `关键时间分析`, `运行记录`, `基本信息`, `图片识别摘要`, or unrelated source fields.
- Keep the colon style close to the user's template.
- `工务添乘:` may use half-width colon if the user template does.
- If a final stabling event includes wheel chock protection, write `入库并设置防溜`.

## Confirmed Example

```text
松滋西接触网工班应急出动信息:
接令时间：5月24日21时41分
接令内容：工班值班员接松滋西站通知传达行调要求供电轨道车应急出库
作业车号：0793021
司机：肖晨，高瑞雪
轨道车出入库应急时间：
21:51联控车站
21:55出库
21:59到达北牵待命
22:02三道出巡
00:11松滋发车
00:47西斋到达
02:27西斋发车
02:56松滋到达
03:09桃子岭到达
04:24桃子岭出发
04:30松滋到达
04:57入库并设置防溜
接触网添乘人员：郝亚飞
工务添乘:张毅然
```

## Pitfalls

- Do not treat old values in the template example as facts for the new report.
- Do not merge image form fields into this report unless they map to these labels.
- Do not add a separate `运行记录` table.
- Do not infer passenger roles if the user has corrected `接触网添乘人员` and `工务添乘`.
- User corrections override OCR and previous assistant output.
