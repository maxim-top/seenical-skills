---
name: seenical-console
description: Operate products, conversations, content, knowledge bases, and sites through the Seenical Console UI when a user asks for interface-guided Seenical work rather than direct API calls.
---

# 知见控制台

按照知见控制台的界面语义完成产品、会话、内容、知识库和站点操作。

## 工作流程

1. 识别当前产品和 APP ID，保留用户最近选择的 Agent、会话和站点。
2. 需要定位功能时读取 [`references/console-operations.md`](references/console-operations.md)。
3. 先读取界面状态，再执行一个明确动作；不要绕过禁用状态或认证提示。
4. 创建 LOOP 时确认目标子会话、内容配置、运行频次、目标站点和发布动作。
5. 对删除、发布、Token 创建、认证提交等不可逆或敏感操作，在最终提交前再次确认。
6. 用界面可见状态或成功提示验证操作结果。

## 交互约定

- 主会话用于长期模板与 Agent 设置；子会话可固化为 LOOP。
- 会话配置负责主题、提示词、关键词、语言与篇幅；LOOP 负责频次、运行和发布。
- 右侧工作区按当前会话或 LOOP 展示预览、内容、知识库、SEO/GEO 和站点。
- 该技能描述 Console UI 操作，不替代 `seenical-api` 的 Butler API 契约。
