# 前端审查清单

逐项检查，不通过的给出文件与行号及修改建议。按严重程度分组汇报：先硬约束，再默认做法。

## 硬约束

- [ ] 服务端数据通过 TanStack Query（或框架原生加载），没有 `useEffect` 里手写 fetch
- [ ] `queryKey` 包含所有筛选与分页参数；Mutation 后精准 `invalidateQueries`
- [ ] 基础控件来自 `@/components/ui/*`，没有手写高仿控件；类名合并用 `cn()`
- [ ] 没有硬编码色板色值（`bg-gray-*`、`text-zinc-*`、`#xxxxxx`）；状态色用语义 Token
- [ ] 启用 i18n 的项目中，没有硬编码业务文案，zod 错误信息已翻译
- [ ] 破坏性操作有 `AlertDialog` 二次确认和后果说明；高危不可逆操作要求输入名称
- [ ] 纯图标按钮有 `aria-label`
- [ ] 关键操作不只依赖 hover

## 默认做法

- [ ] 所有可交互元素有 hover / focus-visible / active / disabled 状态
- [ ] 表单使用 RHF + zod + Shadcn Form，错误信息贴近字段
- [ ] 选择器类型与选项数量匹配（≤5 / 6–8 / >8）
- [ ] 同一区域只有 1 个 Primary 按钮，外露次要操作不超过 2 个
- [ ] 有空、加载（Skeleton）、失败（含重试）三种状态
- [ ] 异步提交按钮请求期间 disabled + loading
- [ ] 图片与异步区块有固定高度或 `aspect-*`，没有加载时跳动
- [ ] 长文本截断并有 Tooltip；数字列用 `tabular-nums`
- [ ] Dialog/Sheet 内无双重滚动条；未保存修改关闭时会拦截
- [ ] 没有无必要的任意值（`w-[52px]`）、Emoji 图标、装饰性边框与阴影
- [ ] 单文件超过约 250 行时已按职责拆分
- [ ] 动效尊重 `prefers-reduced-motion`
