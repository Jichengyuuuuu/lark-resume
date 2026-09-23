# Lark Resume Skill

把飞书群聊、单聊、文档、OKR、任务和会议记录，整理成可信、可核验、面向岗位的简历。

[English README](README.en.md) · [Brief 示例](examples/brief.zh-CN.md) · [简历示例](examples/resume.zh-CN.md)

![Lark Resume 工作流](assets/flow.svg)

大多数简历工具从“你记得什么”开始。**Lark Resume** 从“工作中真实发生过什么”开始。

该 Skill 通过 `lark-cli` 检索用户有权限访问的飞书上下文，从真实工作证据中提炼个人贡献、项目背景、职业定位和岗位匹配价值。它适合把日常工作记录整理成简历 brief、岗位定制简历，或作为“企业协作上下文生成个人材料”的 Agent Skill 示例。

## 为什么做这个

很多真实工作成果不会整齐地出现在周报或绩效材料里，而是分散在：

- 飞书群聊和单聊；
- 产品文档、项目方案、评审记录；
- OKR、任务、会议纪要和交付记录。

写简历时，人很容易只记得最近的事，也容易把团队目标、计划收益或模糊印象写成个人成果。Lark Resume 的目标是让 Agent 从源上下文里重建工作脉络，区分个人贡献和团队结果，再生成更可信、更面向岗位的简历内容。

## 核心能力

- 同时使用飞书群聊、单聊和文档作为一等信息源。
- 检索时只使用项目、产品、人员和日期等必要关键词，不向飞书搜索传递求职或简历生成目的。
- 生成前确认简历语言、生成流程、任职时间和最终格式。
- 支持先输出简洁 brief，再根据用户确认生成完整简历。
- 根据目标岗位调整职业定位、能力维度、经历排序和项目侧重。
- 经历分点采用“加粗概括＋冒号＋具体贡献”的表达方式。
- 区分规划、设计、开发、上线、验收和业务结果，避免将目标写成成果。
- 首版生成后询问是否补充联系方式、教育背景、专业技能、证书、作品集和语言能力。
- 支持 Markdown、DOCX、SVG 和 HTML。

## 支持的工作流

| 工作流 | 输出 | 适用场景 |
| --- | --- | --- |
| 先生成 brief | 对话中的职业定位、能力概括和代表项目表格 | 用户想先确认叙事方向和项目取舍 |
| 直接生成简历 | Markdown / DOCX / SVG / HTML | 用户已经确认范围和最终格式 |
| 岗位定制 | 面向目标岗位重排能力、经历和项目表述 | 投递特定岗位或需要不同版本简历 |
| 证据校验 | 对无法验证的职位、数据、教育或成果进行省略或待确认标记 | 避免把目标、计划和团队成果写成个人事实 |
| 基础信息补全 | 在首版生成后询问联系方式、教育、证书、作品集等 | 用户需要更完整的正式简历 |

## 使用方式

将本仓库安装到支持 Skill 的 Agent 或助手环境中，并确认 `lark-cli` 已安装且在 `PATH` 中可用。

```bash
command -v lark-cli
```

然后在对话中提出类似请求：

```text
请根据我在飞书里的工作记录生成一份简历。
```

Skill 会先确认：

1. 简历使用中文还是英文；
2. 先生成 brief，还是直接生成完整简历；
3. 各段经历的任职时间；
4. 最终需要 Markdown、DOCX、SVG 或 HTML。

如果选择 brief，brief 会直接在对话中返回，不单独生成文件。完整简历生成后，Skill 会询问是否需要补充教育背景、联系方式等基础模块；用户不需要的模块会从最终版本中移除。

## 示例请求

```text
请根据我在飞书里的工作记录生成一份中文简历。
先生成 brief。
任职时间：2025-07 至今。
最终格式：HTML。
```

脱敏示例：

- [中文 brief 示例](examples/brief.zh-CN.md)
- [中文简历示例](examples/resume.zh-CN.md)

## 对开发者有什么参考价值

这个仓库不只是一个简历模板，也可以作为一个 Agent Skill 示例：

- 如何在生成前收集关键约束；
- 如何把企业协作上下文转成结构化个人材料；
- 如何处理证据边界、隐私边界和格式输出；
- 如何让同一个 Skill 支持 brief、Markdown、DOCX、SVG 和 HTML；
- 如何根据目标岗位重写同一段经历，而不是复用固定简历描述。

更多说明见：

- [使用场景](docs/use-cases.md)
- [开发者说明](docs/developer-guide.md)

## 校验清单

使用或改造这个 Skill 时，建议检查：

- 是否在生成前确认语言、流程、任职时间和最终格式；
- brief 是否直接返回在对话中，而不是生成文件；
- 最终格式是否只影响完整简历，不影响 brief；
- 是否只用工作关键词检索飞书上下文；
- 是否避免把求职、简历生成目的传入飞书搜索；
- 是否区分个人贡献、团队成果、计划目标和已完成结果；
- 是否对无法验证的职位、教育、证书、数据和成果保持待确认。

## 依赖

- 已安装并可调用 `lark-cli`；
- 已完成相应飞书身份授权和数据权限配置；
- 生成 DOCX、SVG 或 HTML 时，需要环境中提供相应的文档生成与渲染能力。

`command -v lark-cli` 只能证明 CLI 已安装，不能证明飞书身份、授权范围或数据访问已经可用。

## 隐私与证据边界

- 只读取当前身份有权限访问的内容。
- 不通过变换查询规避权限、审计或风控。
- 不向飞书搜索传递“求职”“简历生成”等目的。
- 未经用户明确要求，不从私人飞书会话中检索联系方式、教育、证书或语言能力等个人信息。
- 私聊原文、同事身份、内部链接、敏感客户信息和非公开运营数据不会直接写入对外简历。
- 无法验证的事实会被省略或标记为待确认，不会为了完整性而补写。

## 仓库结构

```text
lark-resume/
├── SKILL.md                 # Skill 行为规则，使用英文维护
├── README.md                # 中文主说明
├── README.en.md             # English overview
├── README.zh-CN.md          # 中文说明别名
├── LICENSE
├── assets/
│   └── flow.svg
├── docs/
│   ├── developer-guide.md
│   └── use-cases.md
├── examples/
│   ├── brief.zh-CN.md
│   └── resume.zh-CN.md
└── agents/
    └── openai.yaml
```

## English summary

Lark Resume is an AI Agent Skill that generates evidence-backed resumes from Lark / Feishu chats, direct messages, docs, OKRs, tasks, and meeting notes. It uses `lark-cli` with the current user's permissions, asks for required choices before generation, and avoids unsupported resume claims.

See [README.en.md](README.en.md) for the English version.

## License

MIT
