---
name: ym-rules
description: 编写、修改或审查 TypeScript、JavaScript、React、TSX、Vue 3 单文件组件、HTML 与 Tailwind CSS v4 时，统一应用个人代码、组件、Vue Composition API、Tailwind 类名和中文技术回复规范。
---

# YM 开发规则

## 适用范围与优先级

- 本技能用于前端开发中的个人约定，不代表所有项目的通用最佳实践。
- 用户当前明确要求优先；尊重目标仓库的开发说明、现有技术栈、框架版本和 lint/格式化配置。个人偏好仅在不冲突时应用，不为满足偏好迁移技术栈或增加依赖。
- 只修改当前任务涉及的代码，不顺带重命名文件、补齐全仓注释或重新格式化无关代码。
- 文件命名、箭头函数、中文注释、JSX 空行、Vue 状态选择和默认类名顺序属于个人偏好；类型正确、HTML 语义和框架 API 用法属于技术要求，优先保证正确性。

## 回复

- 使用中文回复。
- 不写问候语、客套话、无意义语气词或重复总结。
- 直接给出结果、变更说明、风险或待确认事项。

## 通用代码

- 优先使用 TypeScript；无法使用时使用现代 JavaScript。
- 优先函数式编程：保持函数纯粹、数据不可变，使用 `map`、`filter`、`reduce` 等方式转换数据。
- 自建文件和目录统一使用短横线命名，例如 `order-summary.vue`、`order-summary.tsx`、`use-order.ts`；保留框架或工具规定的文件名，例如 `SKILL.md`。变量和函数使用小驼峰，组件标识符和类型使用帕斯卡命名，不能在 JavaScript 标识符中使用短横线。
- 使用表达业务语义的函数名；事件处理函数以 `handle` 开头，例如 `handleSubmitOrder`。
- 默认不用 `var`；普通函数和回调偏好箭头函数与代码块，有返回值时显式 `return`。生成器、重载或需要动态 `this` 等情形按语言要求使用 `function`。
- 不要用 `void` 前缀忽略函数调用结果；异步调用应按调用关系使用 `await`、返回 Promise 或处理 rejection，不能仅删除 `void` 后留下未处理的 Promise。
- 函数返回类型能够由 TypeScript 推断时不要显式标注；仅在公开契约、重载或推断不准确时添加返回类型。
- 需要定义类时使用 `class`，不要用函数模拟类。
- 为所有函数添加简短中文单行注释说明职责，使用 `//`，不要使用 `/** */`。
- 为所有数据声明添加简短中文单行注释，说明数据用途或业务含义，使用 `//`，不要使用 `/** */`。
- 优先使用 `const`，避免使用 `let`。
- 将错误处理、无效输入和退出条件放在函数前部，使用早返回减少嵌套。

### 数据声明注释

- 为所有常量、变量、对象、数组、配置、状态和派生值添加简短中文注释，说明数据用途、业务含义或判定目的。
- 即使是简单字面量或语义明确的直接赋值，也必须添加中文注释。
- 多行表达式、深层属性访问、可选链、空值回退或包含多个逻辑条件的声明，注释应说明数据来源或计算规则。

```ts
// 订单详情接口地址
const orderDetailEndpoint = '/api/orders/detail';

// 优先使用显式宽度，缺省时回退到文本节点默认宽度
const editorWidth = explicitNodeSize.width ?? textNodeConfig.defaultSize.width;

// 根据订单状态返回展示文本
const getOrderStatusText = (status: OrderStatus) => {
  if (!status) {
    return '未知状态';
  }

  return status === 'paid' ? '已支付' : '待支付';
};
```

## React 与 TSX

- 按职责拆分组件、类型、hooks、常量和工具函数，避免将全部内容放入一个 `.tsx` 文件。
- 保持组件短小、单一职责、名称清晰；将复杂界面拆分成可独立理解和测试的子组件。
- 为 React 组件添加简短中文注释说明职责。
- 简单派生数据直接计算，不为了派生值额外添加 state 和 effect；其他 Hook 按其用途及 React 调用规则使用。
- 在相邻 JSX 同级元素之间保留一行空行。

```tsx
// 展示订单金额摘要
export const OrderSummary = ({ total }: OrderSummaryProps) => {
  if (total < 0) {
    return <span>金额无效</span>;
  }

  return <span>订单总额：{total}</span>;
};
```

## Vue 3

- 新建 `.vue` 文件默认采用 `template` → `script setup` → `style` 的区块顺序。按需保留区块；局部样式默认使用 `scoped`，仅当项目已支持 SCSS 且确实需要时使用 `lang="scss"`，不创建空样式区块。

```vue
<template></template>

<script setup lang="ts"></script>

<!-- 需要局部样式时才添加 style scoped 区块 -->
```

- 自建组件状态偏好 `ref`，对象和数组同样适用；脚本中通过 `.value` 读写，模板中按 Vue 解包规则使用。遵循已有接口，不为此改写库返回的响应式对象。
- 简单组件使用 `order-summary.vue`；复杂组件可使用 `order-summary/index.vue`，按职责拆分子组件、类型、常量和工具函数，避免机械拆文件。
- 按项目 Vue 版本使用编译器宏，不为使用新宏擅自升级依赖。
- `defineProps`、`defineEmits` 偏好内联对象泛型，如 `defineProps<{ title: string }>()`、`defineEmits<{ select: [id: string] }>()`，不单独抽取只用一次的 Props/Emits 类型；已有共享契约按项目要求复用。
- `defineSlots` 使用描述插槽函数的类型，如 `defineSlots<{ default(props: { title: string }): any }>()`；这里的 `any` 仅为目前不用于检查的插槽返回类型，不推广到业务数据。
- `defineModel` 的泛型描述绑定值，如 `defineModel<string>()`，对象值才使用对象类型；按需要传入模型名和选项。
- `defineExpose` 传入实际暴露的运行时对象，如 `defineExpose({ focus })`，不能只写泛型代替运行时对象。
- `ref` 和 `computed` 能从初始值或返回值推断类型时不要添加泛型；仅在初始值无法表达完整类型时添加泛型，例如 `ref<Order | null>(null)`。避免以断言或 `any` 绕过类型检查。

```vue
<script setup lang="ts">
// 订单组件入参
const props = defineProps<{
  total: number;
}>();

// 订单操作事件
const emit = defineEmits<{
  select: [order: Order];
}>();

// 当前选中的订单
const selectedOrder = ref<Order | null>(null);

// 是否已选择订单
const hasSelectedOrder = computed(() => {
  return selectedOrder.value !== null;
});

// 处理订单选择
const handleSelectOrder = (order: Order) => {
  selectedOrder.value = order;
  emit('select', order);
};
</script>
```

## 元素结构与样式

### HTML 语义化

- 按内容职责选择标签，不按组件边界套固定结构；检查组件组合后真实 DOM 的语义与层级。
- `section` 可以作为组件根元素，也可以嵌套；前提是每一层都表示真实的主题分区，通常有描述该主题的标题。不要仅因创建了组件就添加 `section`，也不要为了减少嵌套删除有意义的分区。
- `main` 表示页面主体，一个页面最多一个未隐藏的 `main`，由页面层负责；不要嵌套在 `section`、`article`、`aside`、`nav`、`header` 或 `footer` 中。
- `section` 表示有主题的分区，通常配标题；`article` 用于可独立理解的内容；`nav` 用于主要导航；`aside` 用于与主体间接相关的补充内容。仅为布局分组时使用 `div`。
- `header`、`footer` 可属于页面，也可属于 `article` 或 `section`，按实际需要添加；标题按内容层级使用 `h1`、`h2`、`h3`，不能依赖嵌套分区自动调整标题级别。
- 重复项目使用 `ul`/`ol` 与 `li`；跳转使用有 `href` 的 `a`，操作使用 `button`，表单控件关联可访问名称，避免用可点击 `div` 替代原生控件。
- 顺序有意义时使用 `ol`，否则使用 `ul`；名称与值的对应关系可用 `dl`、`dt`、`dd`；具有行列关系的数据使用 `table` 和适当的表头，不为视觉布局使用表格。
- 正文段落使用 `p`，行内无额外语义的分组使用 `span`；不在 `p` 内放分区、列表等块级结构，不嵌套链接或按钮等交互控件。非提交按钮显式设置 `type="button"`。
- 优先原生 HTML 语义，不给原生元素重复添加已有的 ARIA role。输入控件优先关联 `label`，不能仅靠 placeholder 命名；图标按钮提供可访问名称，内容图片提供有意义的 `alt`，装饰图片使用 `alt=""`。

需要多组件嵌套或页面布局示例时，读取 [语义化组件示例](references/semantic-html.md)。

### 容器与布局

- 不要为无布局、无语义、无样式需求的内容增加容器；能直接渲染内容时不要额外嵌套 `section` 或 `div`。
- 优先通过 `flex`、`grid` 和间距工具类减少无用嵌套；单个元素推向右侧可用 `ml-auto`，整体两端分布可用 `justify-between`，按布局意图选择。
- 样式保持最少且服务于当前布局和交互；不要添加无效、重复或仅为装饰而没有设计依据的样式。

## Tailwind CSS v4

- 类名顺序优先服从项目已有格式化配置，例如 `prettier-plugin-tailwindcss`；没有自动排序约定时，采用以下个人默认分组：自定义类名 → 尺寸 → 定位 → 布局与对齐 → 其余样式 → 颜色与主题。下面的分组细则仅用于此默认顺序。
- 将组件样式类、业务状态类和第三方组件覆写类等自定义类名置于最前。
- 尺寸类包括 `w-*`、`h-*`、`size-*`；定位类包括 `relative`、`absolute`、`fixed`、`sticky`、`top-*`、`right-*`、`bottom-*`、`left-*`、`inset-*` 与 `z-*`。
- 布局与对齐类包括 `flex`、`grid`、`block`、`items-center`、`justify-center` 与 `gap-*`。
- 将 `bg-*`、`text-*`、`border-*`、`ring-*`、`fill-*`、`stroke-*`、深色模式和其他主题变体置于最后。
- 同一分组内按语义和阅读顺序保持稳定；不要为了机械排序打散表达同一意图的类名。
- 合并等价的间距和尺寸类，例如使用 `p-4` 代替 `px-4 py-4`，使用 `size-full` 代替 `w-full h-full`。
- 项目使用 Tailwind 时，颜色优先复用已有语义主题变量，其次使用符合设计的内置调色板；`text-white` 本身合法，不一律禁止。避免零散硬编码颜色，重复使用的设计颜色纳入项目主题；仅在 v4 项目采用 v4 主题配置方式。

```tsx
<div className='order-card size-full relative top-0 left-0 flex items-center justify-center bg-slate-100 text-slate-900' />
```

## 交付前检查

- 仅检查本次改动：是否遵循目标项目配置，并在不冲突时应用命名、注释和函数写法等个人偏好。
- 检查类型推断、Promise 错误处理、Vue 宏用法与框架版本是否正确，不用断言掩盖类型错误。
- 检查组件组合后的 HTML 语义、标题层级和原生交互元素，以及样式是否简洁、主题是否统一。
- 按改动范围运行项目已有的 lint、类型检查、测试或构建；报告实际执行结果，无法执行时说明原因。
