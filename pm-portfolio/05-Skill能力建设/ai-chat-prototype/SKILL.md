---
name: ai-chat-prototype
description: 标准 AI 对话框 HTML 原型模板 — 
  当用户需要画 AI 聊天界面原型时使用。包含对话气泡、AI答案卡、评分卡、多版本对比、反馈面板等标准组件。
  配合 ai-product-upgrade-proposal skill 的概要设计文档使用，替代 ASCII 线框原型。
version: 1.0.0
author: 天宇
agent_created: true
---

# AI 对话框 HTML 原型模板

## 技能用途

当用户说「画效果图」「做概念图」「出原型」「聊天框长什么样」时，使用本 skill 的标准 HTML 模板生成 AI 对话框原型。核心特点：
- 统一配色（蓝色主题#1a5fd6 + 浅灰背景）
- 标准组件：对话气泡、快捷pill、评分卡、多版本对比、反馈面板
- 不依赖 mermaid/SVG——纯 HTML+CSS，改样式直接改 class

## 触发条件

- 用户说「画效果图」「做概念图」「出原型」
- 用户在 ai-product-upgrade-proposal 概要设计中要求替换 ASCII 原型
- 用户说「这个聊天框长什么样」

## 路由表

| 触发条件 | 模板 | 说明 |
|---------|------|------|
| 需要基础聊天界面 | `templates/01-标准对话气泡.html` | 蓝头+快捷pill+AI/用户气泡+输入区 |
| AI 答案带操作按钮 | `templates/02-AI答案带操作按钮.html` | 答案卡+内嵌pill按钮（如"诊断这封""调语气"） |
| 需要评分/诊断结果 | `templates/03-诊断评分卡.html` | 数字评分+颜色条+整体判断+建议 |
| 需要多版本对比 | `templates/04-多版本对比卡.html` | 3列并排+选中态（蓝色边框高亮） |
| 反馈面板综合模板 | `templates/05-反馈面板综合.html` | 流程信息+屏幕快照+多轮对话+已解决/未解决 |

## 设计规范

### 配色
```css
--blue: #1a5fd6;       /* 主色-按钮、链接 */
--bg: #f6f8fb;          /* 页面背景 */
--card: #fff;           /* 卡片背景 */
--sub: #5b6675;         /* 辅助文字 */
--line: #e4e8ef;        /* 边框 */
--ok: #1f9d6b;          /* 评分好 */
--warn: #d9822b;        /* 评分中 */
--hot: #e04556;         /* 评分差 */
```

### 气泡规范
- **AI 头像**：蓝色圆形 + "AI" 字样，直径 36px，气泡 `text-align: left`
- **用户头像**：灰色圆形 + 用户初始，直径 36px，气泡 `text-align: right`
- **气泡宽度**：max-width 75%，圆角 12px
- **间距**：气�间 12px

### 卡片规范
- `.card` 基础样式：白底卡片，`border-radius: 14px`，`padding: 16px 18px`
- 带操作按钮的卡：`.action-card` 底部 pil 按钮组
- 评分卡特殊样式：数字+颜色条+`justify-content: space-between`

## 用法

1. 加载本 skill
2. 根据场景选模板
3. 修改卡片内容（文本、按钮标签、评分数字）
4. 直接嵌入概���设计 HTML 的第8章（替换 ASCII）

## 依赖

- 无外部依赖。纯 HTML+CSS，可直接嵌入任何页面。

## 参考案例

- 邮件润色助手功能升级方案（内部评审版）
- 流程智答助手 概要设计 v0.2 第8章
