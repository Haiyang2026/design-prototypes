# 弹性面板布局 · Flexible Panel Layout

## 概述

左图右文弹性面板布局，左侧为视觉卡片（插画/截图+CTA），右侧为可折叠的内容区。**Web 端默认紧凑显示**，用户可点击展开查看详情。

适用于：功能介绍页、流程说明页、状态分支模拟、产品文档原型。

---

## 核心结构

```
┌───────────────┬──────────────────────────────────┐
│               │  📌 主标题区（始终可见）           │
│   视觉面板    │  📝 摘要说明（始终可见）           │
│   (固定宽度)  │                                  │
│   插画/截图   │  ─── 可折叠内容 ───              │
│   + 一句话    │  [展开▼] 步骤流 / 操作区         │
│   + CTA按钮  │  [展开▼] 状态分流规则            │
│               │  [展开▼] 二次进入模拟            │
└───────────────┴──────────────────────────────────┘
```

---

## 实现规范

### HTML 骨架

```html
<div class="flex gap-6 max-w-6xl mx-auto p-6">
  <!-- 左侧：视觉面板 -->
  <div class="w-80 flex-shrink-0">
    <div class="bg-white rounded-xl border overflow-hidden sticky top-6">
      <div class="aspect-[4/3] bg-gray-50 flex items-center justify-center">
        <!-- 插画/截图 -->
        <img src="..." alt="..." class="w-full h-full object-cover" />
      </div>
      <div class="p-4">
        <h3 class="font-medium text-sm">一句话标题</h3>
        <button class="mt-3 w-full py-2 px-4 rounded-lg text-sm font-medium bg-primary text-white">
          CTA 按钮
        </button>
      </div>
    </div>
  </div>

  <!-- 右侧：内容区 -->
  <div class="flex-1 min-w-0">
    <!-- 主标题（始终可见） -->
    <h1 class="text-xl font-bold mb-1">主标题</h1>
    <p class="text-sm text-gray-500 mb-6">摘要说明...</p>

    <!-- 可折叠区块 -->
    <div class="space-y-3">
      <div class="collapse-section">
        <button class="collapse-trigger" onclick="toggleCollapse(this)">
          <span>区块标题</span>
          <i data-lucide="chevron-down" class="w-4 h-4 transition-transform"></i>
        </button>
        <div class="collapse-content hidden">
          <!-- 区块内容 -->
        </div>
      </div>
      <!-- 更多区块... -->
    </div>
  </div>
</div>
```

### CSS 样式

```css
.collapse-section {
  @apply border rounded-xl overflow-hidden;
}
.collapse-trigger {
  @apply w-full flex items-center justify-between px-4 py-3 text-sm font-medium
         bg-gray-50 hover:bg-gray-100 transition-colors;
}
.collapse-content {
  @apply px-4 py-3 text-sm text-gray-600;
}
.collapse-content.open {
  @apply block;
}
.collapse-trigger .lucide-chevron-down {
  transition: transform 0.2s;
}
.collapse-trigger.open .lucide-chevron-down {
  transform: rotate(180deg);
}
```

### JavaScript 交互

```javascript
function toggleCollapse(btn) {
  btn.classList.toggle('open');
  const content = btn.nextElementSibling;
  content.classList.toggle('hidden');
  content.classList.toggle('open');
}
```

---

## 常见区块类型及模板

### 类型A：步骤流 (Step Flow)

横向排列的步骤指示器，适合展示流程阶段。

```html
<div class="flex items-center gap-2 py-3">
  <div class="flex items-center" data-step="current">
    <div class="w-8 h-8 rounded-full bg-primary text-white flex items-center justify-center text-xs font-medium">1</div>
    <span class="ml-2 text-xs font-medium text-gray-700">步骤名称</span>
    <i data-lucide="arrow-right" class="w-4 h-4 mx-3 text-gray-300"></i>
  </div>
  <div class="flex items-center" data-step="todo">
    <div class="w-8 h-8 rounded-full bg-gray-200 text-gray-400 flex items-center justify-center text-xs font-medium">2</div>
    <span class="ml-2 text-xs text-gray-400">步骤名称</span>
  </div>
</div>
```

### 类型B：操作区 (Action Area)

两栏布局：主流程操作 + 状态模拟。

```html
<div class="grid grid-cols-2 gap-4">
  <!-- 主流程操作 -->
  <div>
    <h4 class="text-xs font-medium text-gray-500 mb-2">流程操作</h4>
    <div class="flex gap-2">
      <button class="px-3 py-1.5 rounded-lg text-xs font-medium bg-primary text-white">提交</button>
      <button class="px-3 py-1.5 rounded-lg text-xs font-medium border text-gray-600">取消</button>
    </div>
  </div>
  <!-- 状态模拟 -->
  <div>
    <h4 class="text-xs font-medium text-gray-500 mb-2">二次进入模拟</h4>
    <div class="flex flex-wrap gap-2">
      <button class="px-3 py-1.5 rounded-lg text-xs border border-primary text-primary hover:bg-primary/5"
              onclick="simulateState('midway-exit')">
        中途退出再进
      </button>
      <button class="px-3 py-1.5 rounded-lg text-xs border text-gray-500 hover:border-gray-400"
              onclick="simulateState('not-ordered')">
        未下单再进
      </button>
    </div>
  </div>
</div>
```

### 类型C：状态分流表 (State Routing Table)

三列表格，展示不同用户状态的分流策略。

```html
<table class="w-full text-xs">
  <thead>
    <tr class="border-b text-gray-500">
      <th class="py-2 pr-4 text-left font-medium">用户状态</th>
      <th class="py-2 pr-4 text-left font-medium">入场策略</th>
      <th class="py-2 text-left font-medium">主要目的</th>
    </tr>
  </thead>
  <tbody>
    <tr class="border-b border-gray-50">
      <td class="py-2 pr-4">新用户首次进入</td>
      <td class="py-2 pr-4 text-primary font-medium">引导式</td>
      <td class="py-2">完成首次配置</td>
    </tr>
    <tr class="border-b border-gray-50">
      <td class="py-2 pr-4">中途退出再进</td>
      <td class="py-2 pr-4 text-amber-600 font-medium">断点续传</td>
      <td class="py-2">从上一步继续</td>
    </tr>
    <tr>
      <td class="py-2 pr-4">已完成用户</td>
      <td class="py-2 pr-4 text-green-600 font-medium">快捷模式</td>
      <td class="py-2">查看结果/重新提交</td>
    </tr>
  </tbody>
</table>
```

---

## Web 端折叠规则

| 规则 | 说明 |
|------|------|
| **默认状态** | 所有折叠区块**收起**，只显示标题行 |
| **首个区块** | 首个区块默认**展开**（让用户看到第一步） |
| **展开反馈** | 点击时图标旋转 180° 动画，内容滑入 |
| **同时收起** | 展开新区块时，自动收起其他区块（手风琴模式） |
| **URL 锚点** | 支持 `#section-name` 直接定位到指定区块 |

### 手风琴模式实现

```javascript
let currentOpen = null;

function toggleCollapse(btn) {
  const section = btn.closest('.collapse-section');
  const content = section.querySelector('.collapse-content');
  const isOpen = content.classList.contains('open');

  // 关闭当前打开的其他区块
  if (currentOpen && currentOpen !== section) {
    currentOpen.querySelector('.collapse-trigger').classList.remove('open');
    currentOpen.querySelector('.collapse-content').classList.remove('open', 'hidden');
    // 用 display: none 隐藏
    currentOpen.querySelector('.collapse-content').style.display = 'none';
  }

  // 切换当前区块
  if (isOpen) {
    content.style.display = 'none';
    content.classList.remove('open');
    btn.classList.remove('open');
    currentOpen = null;
  } else {
    content.style.display = 'block';
    content.classList.add('open');
    btn.classList.add('open');
    currentOpen = section;
  }
}
```

---

## 完整示例代码

参见 `templates/flexible-panel-demo.html`（如存在）。也可直接从现有原型中提取布局结构。

---

## 适用场景判断

| 场景 | 推荐使用 | 理由 |
|------|---------|------|
| 功能介绍/产品说明 | ✅ | 左侧截图直观，右侧文字展开 |
| 流程说明文档 | ✅ | 步骤流+状态表，折叠不占空间 |
| 产品方案演示 | ✅ | 高保真外观，客户可直接体验 |
| 纯后台配置页 | ❌ | 太占横向空间，不适合密集操作 |
| 移动端优先场景 | ⚠️ | 需改为上下布局，面板折叠保持不变 |
