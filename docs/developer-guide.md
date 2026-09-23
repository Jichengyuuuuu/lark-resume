# 开发者说明

Lark Resume 是一个面向 Agent 的 Skill 示例，核心不是简历模板，而是“从企业协作上下文生成可信个人材料”的工作流。

## 可以复用的设计

### 1. 生成前确认约束

Skill 在生成前必须确认四类信息：

1. 简历语言；
2. 先生成 brief 还是直接生成完整简历；
3. 任职时间；
4. 最终简历格式。

这能避免 Agent 在上下文不足时直接生成不可用的结果。

### 2. brief 与最终产物分离

brief 是确认叙事方向的对话内容，不需要和最终简历使用同一种格式。用户选择 DOCX、SVG 或 HTML 时，brief 仍然直接返回在对话里。

### 3. 证据边界

Skill 会把以下内容分开处理：

- 个人贡献；
- 团队成果；
- 计划目标；
- 已完成结果；
- 待用户确认的事实。

这比普通简历生成更适合企业工作上下文，因为内部材料里经常同时出现目标、方案、计划和复盘。

### 4. 岗位定制

同一段经历会根据目标岗位重写侧重点。例如：

- 产品岗位强调业务问题、需求判断、流程设计和协同推进；
- 工程岗位强调架构、技术复杂度、稳定性和交付质量；
- 解决方案岗位强调客户场景、约束处理、落地路径和验收结果。

## 如何改造

### 替换数据源

当前 Skill 假设通过 `lark-cli` 获取飞书上下文。你可以保留相同的证据处理规则，替换为其他企业协作数据源，例如 Slack、Google Docs、Notion、Linear 或 Jira。

替换时应保持两个原则：

- 只读取当前用户有权限访问的内容；
- 检索时只传递工作关键词，不传递求职或简历生成目的。

### 增加输出格式

当前最终格式包括 Markdown、DOCX、SVG 和 HTML。新增格式时，建议先补充：

- 格式适用场景；
- 字体、字号和排版约束；
- 可验证的渲染检查；
- 与 brief 的关系。

### 调整 brief 模板

brief 应保持短、可扫读、便于确认。推荐结构：

1. 综合定位；
2. 履历概括；
3. 职业能力概括；
4. 代表项目表格；
5. 一句确认问题。

不要把完整简历 bullets、长篇证据映射和验证状态塞进 brief。

## Validation checklist

- The agent asks for all required choices before generation.
- The brief is returned in chat, not as a file.
- The final resume format only applies to the final resume.
- The retrieval step uses work keywords, not job-search intent.
- Unsupported claims are omitted or marked as needing confirmation.
- Role tailoring changes emphasis and order, not facts.
