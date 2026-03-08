# openclaw-save-skill

> OpenClaw 技能：`/save` — AI 对话上下文落盘，配合 `/new` 形成会话管理闭环。

## 这是什么

用 AI 做开发或处理复杂任务，对话开久了上下文必然会满。开新会话，之前的进度就断了。

`/save` 解决的是"怎么干净地结束一段对话"：触发后，AI 自动把当前对话的有价值信息整理进 memory 文件，分层归档，下次开新会话直接接上，不用重新交代背景。

## 使用方式

1. 将 `SKILL.md` 放入你的 [OpenClaw](https://github.com/openclaw/openclaw) skills 目录
2. 在对话中说 `/save`、"落盘" 或 "存一下进度" 即可触发
3. AI 执行完毕后，用 `/new` 开启新会话

## 落盘流程

触发后 AI 会依次执行：

1. **识别 topic 文件** — 回顾对话，找到本次涉及的 `memory/` topic 文件
2. **更新 topic 文件** — 只写有长期保留价值的信息，整合进去，不追加堆砌
3. **写 daily memory** — 追加到 `memory/_daily/YYYY-MM-DD.md`，精炼记录今天做了什么
4. **检查 MEMORY.md 指针** — 有新 topic 或路径变化才更新，通常不动
5. **交接标注** — 进行中但未落地的事，补写"待实施"或"待确认"
6. **文件大小检查** — 实际执行 `wc -c`：topic ≤ 3KB、MEMORY.md ≤ 2KB、daily ≤ 2KB，超限先精简
7. **回复确认** — 告诉你更新了什么、改了什么、下次开工接哪里，然后提示可以 `/new` 了

## 设计原则

**精炼 > 堆砌**。文件有大小硬限制，不是为了卡你，是为了逼你判断什么值得留、什么不值得。落盘的质量不是字数，是判断力。

## 配套使用

- `/sxw` — 上下文水位巡检，知道什么时候该 `/save`（[openclaw-sxw-skill](https://github.com/qingchejun/openclaw-sxw-skill)，如有）
- `/new` — OpenClaw 内置，开启干净的新会话

## 依赖

- [OpenClaw](https://github.com/openclaw/openclaw)

## License

MIT
