---
name: jmbos-uiue-form-prototype
description: JMBOS 流程表单 HTML
  原型生成器。当用户需要根据流程需求文档生成可交互的表单原型页面时调用。触发场景：用户说「生成表单原型」「出个表单页面」「画个流程表单」「把这个流程需求文档转成HTML表单」等。支持按文档固定分组顺序渲染、多环节字段权限控制、子表增删改导入导出、审批流程图嵌入、开发备注折叠面板。
agent_created: true
disable: true
---

# JMBOS 流程表单 HTML 原型生成器

基于流程需求文档，生成单个独立 HTML 文件，作为可交互的表单原型页面。

## 核心能力

1. 左侧菜单分组：表单（各环节）→ 流程（流程图）→ 配置表 → 邮件内容
2. 按文档固定分组顺序渲染所有字段，字段权限按环节控制
3. 子表组件：增删行 + 导入 + 导出
4. 审批流程图 SVG 嵌入
5. 开发备注折叠面板
6. 表单控件高度统一、严格 4 列或 2 列布局

## 输出文件

- 单个 `{流程名称}表单.html`，放在需求文档同级目录
- 完整参考模板：`assets/template.html`（CAE分析申请流程的最终版本）

## 页面结构

```
┌──────────────────────────────────────────┐
│ 顶导 42px (品牌色 #0C328C)               │
├────────┬─────────────────────────────────┤
│ 侧边栏 │ 主内容区                         │
│ 180px  │                                 │
│        │  ┌─ 步骤页头 ─────────────────┐  │
│ 📋表单  │  │ 步骤名 / 标签 / 审批人 / 跳过 │  │
│  概览   │  │ 📌 步骤备注                  │  │
│  起草   │  └───────────────────────────┘  │
│  ...    │  ┌─ 表单内容（固定分组顺序）───┐  │
│  确认   │  │ 1. 申请人信息（默认折叠）     │  │
│        │  │ 2. 申请信息                  │  │
│ 📐流程  │  │ 3. 结构材料（子表）           │  │
│ 📊配置表 │  │ 4. 流体属性（子表）           │  │
│ ✉️邮件  │  │ 5. 仿真要求                 │  │
│        │  │ 6. 附件信息                  │  │
│        │  └───────────────────────────┘  │
│        │  ┌─ 开发备注（折叠）───────────┐  │
│        │  │ N 项技术细节                 │  │
│        │  └───────────────────────────┘  │
│        │  [返回] [保存草稿] [提交申请]     │
└────────┴─────────────────────────────────┘
```

## 样式系统（CSS 变量）

所有样式通过 CSS 变量 `:root` 定义，品牌色 `#3685F2`，导航品牌色 `#0C328C`：

```css
:root{
  --primary:#3685F2;--phover:#5E9DF5;--pactive:#2364CC;
  --pbg:#DEEDFF;--lbg:#F3F8FE;--brand:#0C328C;
  --green:#04D999;--glight:#E6FBF5;
  --warn:#FF813B;--red:#FF4D5D;--rlight:#FFEDEF;
  --t1:#141b24;--t2:#7C8493;--t3:#B9BFCB;
  --bg:#EBF1F6;--card:#fff;--hd:#F4F7F9;--bd:#D2D7E1;--dv:#E4E7F0;
  --fxs:11px;--fsm:12px;--fb:13px;--flg:14px;--sb:600;
  --sxs:4px;--ssm:8px;--smd:12px;--sb2:16px;--sxl:24px;
  --r2:4px;--r3:8px;--sh:0 1px 5px rgba(20,27,36,.10);
  --tf:150ms ease-in-out;
}
```

## 布局规范（务必遵守）

### 严格列数
- 每行只用 **4 列** 或 **2 列**，**绝对禁止 3 列**
- 3 个字段时拆分 2+1 两行
- gridRows 用 `curW` 累加宽度（`w:2` 计为 2），满 4 开新行
- 后处理：3 个 w:1 字段的行拆为 2+1

### 宽度控制
- `.fi{flex:1;min-width:0}` — 默认等宽
- `.fi.fw2{flex:2!important}` — 宽字段（`w:2`）
- `.fi.fw{flex:0 0 100%}` — 全宽字段（textarea/附件）
- 合并字段：`t:'双字段'` + `.dc{display:flex;gap:4px}` 横向排两 input，中间 `/` 分隔

### 控件高度
- `.in` / `.sel`：统一 `height:32px;box-sizing:border-box`
- `.ta`：`min-height:80px;box-sizing:border-box`

## 数据模型

### 字段定义
```javascript
const fields = {
  // 基础字段（无 s 属性 = 所有环节只读，如申请人信息）
  applicant: [
    {n:'申请人', t:'文本', r:true},
    {n:'工号 / 岗位', t:'双字段', r:true},  // 合一列
    {n:'部门', t:'文本', r:true, w:2},       // 占两列
  ],
  // 步骤 0 可编辑字段（s:0 或无 s）
  apply: [
    {n:'字段名', t:'单行文本', r:true},
    {n:'下拉字段', t:'下拉选择', r:true, o:['选项1','选项2']},
    {n:'多行字段', t:'多行文本', r:false},    // 非必填
  ],
  // 多环节字段（s:X 表示在步骤 X 可编辑）
  sim: [
    {n:'起草字段', t:'单行文本', r:true, s:0},
    {n:'步骤2字段', t:'日期', r:true, s:2},
    {n:'步骤3字段', t:'下拉多选', r:true, s:3, note:'选项1/选项2/选项3'},
  ],
  attach: [
    {n:'附件', t:'附件上传(多文件)', r:true, s:0},
    {n:'报告', t:'附件上传(单文件)', r:true, s:3},
  ],
  // 子表定义
  structure: {n:'子表名', cols:['列1','列2','列3'], r:false, note:'备注说明'},
  fluid: {n:'流体属性', cols:['列1','列2'], r:true, note:'备注'},
};
```

### 字段属性说明
| 属性 | 说明 |
|------|------|
| `n` | 字段标签名 |
| `t` | 类型：文本 / 单行文本 / 多行文本 / 下拉选择 / 下拉多选 / 多选 / 单选 / 日期 / 附件 / 人员选择器(多选) / 双字段 |
| `r` | 是否必填（`!0`=true, `!1`=false） |
| `s` | 归属步骤 ID（无此属性表示全局只读；有此属性在对应步骤可编辑，其余只读） |
| `w` | 列宽倍数（`w:2` 占两列，默认 `w:1`） |
| `o` | 下拉选项数组 |
| `note` | 开发备注文本 |

### 步骤定义
```javascript
const steps = [
  {id:'overview', name:'概览', sub:'全部字段+开发备注', icon:'📋'},
  {id:0, name:'起草', sub:'提交人填写表单', icon:'✏️', type:'ex',
   approver:'提交人', skipE:'—', skipS:'—', skipR:'—', note:'环节备注'},
  {id:1, name:'审批步', sub:'审批·单审', icon:'①', type:'ap',
   approver:'汇报关系-S3', skipE:'是', skipS:'是', skipR:'纯审批可跳过'},
  // type: 'ap'=审批(绿), 'ex'=执行(蓝); pa:true=并审
];
```

## 渲染函数

### fv(f, stepId) — 字段可视性
```javascript
function fv(f,stepId){
  let s=f.s;
  if(s===undefined||s===null)return 'readonly'; // 无归属 = 全局只读
  if(s===stepId)return 'edit'; // 本环节可编辑
  return 'readonly'; // 所有字段均显示，其他环节只读（本流程无不�见要求）
}
```

### renderRow(arr,state,stepId)
- 每个字段检查 `f._st` 或全局 `state` 决定可编辑/只读
- 下拉类渲染为 `<select>` 标签（含全部 `<option>`），只读时 disabled
- 文本类渲染为 `<input>` 或 `<textarea>`，只读时 disabled + placeholder="本环节只读"
- 附件类：只读时显示"本环节不可上传"

### gridRows(arr,state,stepId)
- 按 `w` 属性累加宽度分行使，满 4 换行
- 后处理：3 个 w:1 字段拆分 2+1

### dataTable(cols,rows,opts) — 子表
- opts：`{n:'表名', rq:是否必填, types:['select','text',...], selOpts:[['选项1','选项2']]}`
- 工具栏：＋ 新增行 / ⬆ 导入 / ⬇ 导出
- 每行末尾：🗑 删除按钮（至少保留 1 行）
- `tc(i,types,selOpts)` 按列类型渲染单元格（select 或 input）

### secHTML(title,body,collapsed)
- 可折叠分组，`collapsed=true` 时默认折叠

## 左侧菜单结构

```javascript
sb.innerHTML = `
  <div class="sg">📋 表单</div>
  ${steps.map(s => `<div class="si" ...>...</div>`)}
  <div class="sg">📐 流程</div>
  <div class="si" ... onclick="selectFlow()">审批流程图</div>
  <div class="sg">📊 配置表<span class="sgn">(暂无)</span></div>
  <div class="sg">✉️ 邮件内容<span class="sgn">(暂无)</span></div>
`;
```

## 流程元数据展示

在 selectFlow() 和 renderOverview() 末尾展示：
- 📋 流程基本信息表
- 📐 审批跳过规则表
- 🔧 步骤详细配置表
- 🔔 通知配置
- 📎 流程角色附录
- 📝 标题生成规则
- 🔗 字段联动关系

## 生成步骤

1. 读取需求文档，提取字段定义、步骤配置、流程图
2. 按 assets/template.html 的结构创建 HTML 文件
3. 替换 fields / steps 数据
4. 调整附属元数据（流程ID、通知配置、角色等）
5. 嵌入审批流程图 SVG（如有）
6. 验证所有环节的字段权限与文档一致
7. 以 `{流程名称}表单.html` 命名，保存在文档同级目录

## 注意事项

- 表单分组顺序严格按需求文档 §1.1 字段分组表顺序
- 申请人信息全局可见、默认折叠
- 所有字段在所有环节均显示，无隐藏逻辑（本流程规则）
- 表头不写下拉选项说明，选项放在开发备注中
- 下拉/多选/单选用 `<select>` 标签而非文本框
- 开发备注放在统一折叠面板底部，不散落在各分组中
