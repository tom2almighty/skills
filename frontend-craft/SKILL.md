---
name: frontend-craft
description: >-
  React + TypeScript 应用型前端（后台、仪表盘、表单、数据密集页面）的工程与 UI 规范：
  技术栈与数据流、Shadcn UI、语义化设计 Token、表单与交互、i18n。
  只要任务涉及新建、修改或审查 React/TSX 页面、组件、表单、列表、弹窗、布局，
  即使用户没提"规范"或"组件库"，也应启用本技能。
  不适用于 Vue/Svelte/纯 HTML 项目；营销页、落地页只适用第 3 节的 Token 规则，其余放宽。
---

# Frontend Craft

## 优先级与适用范围

**仓库现有约定 > 本规范 > frontend-design**

- 仓库已有 UI 库、Tailwind 版本、表单方案、i18n 方案时，沿用它们，只套用本规范中与之不冲突的部分（例如仓库用 Ant Design，就不要引入 Shadcn，但 Query、i18n、状态与交互原则仍适用）。
- 与 `frontend-design` 同时启用时，本规范的技术栈、Token、文案约束优先，视觉创意在 Token 允许的范围内发挥。
- 规则分三档：**硬约束**（违反会造成维护或一致性问题，不得绕过）、**默认做法**（有充分理由可偏离）、**建议**（按场景取舍）。未标注的条目均为默认做法。

## 1. 技术栈与数据流

- 沿用仓库的包管理器与 Lint 配置；新项目默认 `pnpm` + Vite + React + TypeScript + Tailwind CSS v4。
- 状态：局部用 `useState`/`useReducer`，跨组件共享先用 React Context，复杂客户端状态用 Zustand，不为小需求引入 Redux。
- **硬约束｜服务端数据统一用 TanStack Query**，不在 `useEffect` 中手写拉取与竞态处理（理由：缓存、重试、竞态由库处理，手写版本几乎必有边界 bug）。使用框架原生数据加载（Next.js Server Components、React Router loader 等）时除外。
  - `queryKey` 结构化，并包含全部筛选、分页参数。
  - Mutation 成功后用 `invalidateQueries` 精准失效对应缓存，不全量刷新。

## 2. 组件、Shadcn 与表单

- **硬约束｜基础控件来自 Shadcn CLI**，生成到 `@/components/ui/*`，不手写高仿基础控件；类名合并统一用 `cn()`（理由：保证可访问性与主题一致）。
- 按职责拆分组件：单文件超过约 250 行，或同时承载多个独立弹窗/复杂区块时考虑拆成同级 `components/` 子组件。不为凑行数拆出无意义的小文件。
- 表单：`react-hook-form` + `zod` + Shadcn `Form`。默认 `onBlur` 或提交时校验，实时反馈类字段可用 `onChange`；错误信息紧贴字段下方。

## 3. 设计系统 Token

- **硬约束｜只用语义化颜色 Token**：`bg-background`、`text-foreground`、`text-muted-foreground`、`border-border`、`destructive` 等。不硬编码调色板色值（`bg-gray-100`、`text-zinc-600`）（理由：硬编码会破坏暗色模式与换肤）。
- **状态色与图表色**：Shadcn 默认只有 `destructive`。需要成功/警告/信息时，在全局 CSS 中补 `--success`、`--warning`、`--info`（含 `-foreground`）并映射到 Tailwind 主题；图表使用 `--chart-1..5`。不要在组件里临时造色。
- 主题：项目已有主题系统时必须兼容暗色；新项目默认配置亮色/暗色/跟随系统（Shadcn 官方 ThemeProvider 或等价方案）。
- 圆角与阴影用默认 Token（`rounded-md`/`rounded-lg`/`shadow-sm`）；不使用装饰性的边框和阴影，非浮层容器不加重阴影。
- 尺度：优先 Tailwind 刻度与 T-shirt 字号（`text-xs`、`text-sm`）；Tailwind v4 下用动态倍数（`w-13`）而不是 `w-[52px]`。Canvas/SVG 动态坐标等确需任意值的场景除外。
- 图标用 Lucide / Simple Icons，国旗用按需引入的 `country-flag-icons/react/3x2`；不用 Emoji 代替图标。
- 标题、说明、占位符只陈述功能与状态，不用空洞修饰词。

## 4. 不可省略的交互底线

- 所有可交互元素有 `hover`、`focus-visible`、`active`、`disabled` 状态，并尊重 `prefers-reduced-motion`。
- 纯图标按钮必须有 `aria-label`（并配 `Tooltip`）。
- 关键信息和操作不能只依赖 hover，必须支持 click/focus 触发。
- 删除、清空、解绑等破坏性操作用 `destructive` 样式，并经 `AlertDialog` 二次确认。
- 异步提交按钮在请求期间 `disabled` + loading。

## 按需读取

| 场景 | 读取 |
|---|---|
| 项目启用或需要新增多语言、翻译表单校验信息、格式化日期/金额 | `references/i18n.md` |
| 设计分组间距、动效、筛选与选择器、操作层级、危险操作细则、悬浮菜单、空/加载/错误态、弹窗与窄屏 | `references/interaction.md` |
| 用户要求审查或 review 前端代码，或你完成一个页面后自检 | `references/review-checklist.md` |
