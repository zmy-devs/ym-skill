# Tailwind CSS

只在项目已经使用 Tailwind 时应用，先确认主版本和插件；现有 v3 项目保持其配置方式，v4 主题语法仅用于 v4。

## 类名写在使用位置

- 普通静态布局直接写在 JSX / 模板的 `className` / `class` 中，不提取为 `const containerClass = 'flex ...'` 这类单用途字符串常量。
- 类名长本身不是抽取字符串的理由。真实重复的 UI 优先形成组件；有稳定语义的变体表、项目已有 CVA/variants 和组件库样式契约可以保留，不能机械地把所有 class 常量都内联。
- 不在运行时拼接 `bg-${color}-500` 这样的不完整类名；使用完整静态类名映射。必要的动态数值沿用项目的内联样式或 CSS 变量方案。

## 条件类与变体

- 使用项目已有的 `cn`、`clsx`、变体工具或 Vue `:class`；不为了拼一次类名新增依赖或重复实现 `cn`。
- 公共类只写一次，条件分支只放变化部分；相互冲突的工具类从分支源头理清，不依赖字符串顺序碰运气覆盖。
- `cn` 是否合并冲突类取决于项目实现，不把它天然等同于 `tailwind-merge`。

```tsx
<button
  type='button'
  className={cn(
    'flex items-center gap-2 px-4',
    active
      ? 'bg-primary text-primary-foreground'
      : 'bg-muted text-muted-foreground',
  )}
>
  切换视图
</button>
```

示例假设项目已有 `cn` 和对应主题 token；缺少时使用项目现有等价写法，不照搬不存在的类。

## 排序与精简

- 项目已有格式化排序工具时完全服从工具。没有约定时采用个人分组：自定义类 → 尺寸 → 定位 → 布局与对齐 → 其余样式 → 颜色与主题。
- 自定义类包含组件、业务状态和第三方覆写类；尺寸如 `w-*` / `h-*` / `size-*`；定位如 `relative` / `absolute` / `inset-*` / `z-*`；布局如 `flex` / `grid` / `items-*` / `justify-*` / `gap-*`。
- 最后放颜色类及相关主题变体；`text-*`、`border-*` 可能表示字号、边框宽度等非颜色意图，不能只按前缀粗暴归类。同组保持稳定，不拆散同一意图的状态变体。
- 语义等价时合并，如 `px-4 py-4` → `p-4`、`w-full h-full` → `size-full`；确认项目版本支持且没有断点或方向覆盖差异。
- 优先 `flex` / `grid` / `gap` 表达普通布局。绝对定位只用于真实叠层、锚定等需求，不因清理定位而破坏弹层或装饰效果。

## 主题

- 优先复用现有语义主题变量，其次使用符合设计的调色板；`text-white` 合法，不一律禁用。重复设计颜色纳入项目主题，不散落硬编码。
- 不为清理样式新增装饰、改视觉设计或迁移主题配置；保留设计要求的自定义值和必要响应式、焦点、禁用与深色模式样式。

技术依据：[Tailwind 完整类名检测](https://tailwindcss.com/docs/detecting-classes-in-source-files)、[工具类与动态样式](https://tailwindcss.com/docs/styling-with-utility-classes)。
