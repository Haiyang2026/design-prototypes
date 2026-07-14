---
agent_created: true
---

# 门户需求文档 — 技能包

> 用途：编写门户页面产品需求文档时使用，确保文档使用业务语言、结构规范、无代码术语。

## 工作模式

本技能采用**苏格拉底式逐节引导**模式。当用户说「写需求文档」时，不一次性生成全文，而是：

1. 从模板的**第一节开始**，依次向用户提问
2. 用户回答后，天宇记录并进入下一节
3. 每个问题只问一次，用户说"过"或"没有"就跳过
4. 全部确认后一次性输出完整文档
5. 后续补加内容可随时说"加XX"进入对应节

**待确认项**统一存放到独立文档 `{需求名称}_待确认.md`，不在主文档中堆积标记。待确认项用单独的任务单跟进（详见 `references/pending-items-management.md`）。

## 路由表

| 触发条件 | 资源 | 预期产出 |
|---------|------|---------|
| 用户说"写需求文档" / "写门户需求" | `references/socratic-flow.md` | 启动苏格拉底式逐节引导，依次确认每个章节 |
| 用户说"加一个页面" / "新增门户" | `templates/portal-page-doc-template.md` | 按 JMBOS 规范模板生成文档框架 |
| 用户说"梳理接口" / "写接口需求" | `references/interface-format.md` | 生成接口总览表（含优先级、已有/新建标注） |
| 用户说"加交互说明" / "加hover" | `references/interaction-rules.md` | 补充交互设计细节 |
| 用户说"加页面清单" / "列页面" | `references/socratic-flow.md` | 跳转到苏格拉底流程的页面范围清单环节 |
| 用户说"画交互矩阵" / "页面关系" | `references/socratic-flow.md` | 跳转到苏格拉底流程的交互矩阵环节 |
| 用户说"登记待确认" / "记一个待办" | `references/pending-items-management.md` | 新增待确认项，写入独立文档并创建任务单 |
| 用户说"查看待确认" / "待办列表" | `references/pending-items-management.md` | 列出当前所有待确认项及状态 |
| 用户说"更新待确认" / "已确认" | `references/pending-items-management.md` | 将某待确认项标记为已确认或已解决 |
| 用户说"检查命名规范" / "格式规范" | `references/format-conventions.md` | 功能ID、文档命名、版本记录、术语规范 |
| 用户说"检查审核标准" / "自检" / "评审准备" | `references/review-standards.md` | 按审核维度和自检清单逐项检查，驳回常见原因提示 |
| 完成修改后自动触发 | `references/document-integrity-check.md` | 执行6项完整性检查，确保结构不乱 |
| 修改 HTML 原型文件（JS/CSS） | `references/html-safety-rules.md` | 改前检查5条规则，改后跑语法验证 |
