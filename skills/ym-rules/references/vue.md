# Vue 3

先确认 Vue 和相关工具版本；不为使用新宏升级依赖，也不把 React 的 Hook 或 JSX 写法套进模板。

## 单文件组件与职责

- 新建 SFC 默认 `template` → `script setup lang='ts'` → `style`，只保留实际需要的区块。局部样式默认 `scoped`，项目支持且确有需要时再用 SCSS，不创建空样式块。
- 简单组件用 `order-summary.vue`；复杂组件可用 `order-summary/index.vue`，按职责拆子组件、组合式函数、类型和工具，不按固定行数拆文件。
- 模板只保留结构、简单表达式和事件绑定；复杂数据转换放 `computed` 或纯函数，多步骤事件放命名处理函数。
- 互斥状态使用相邻的 `v-if` / `v-else-if` / `v-else`；独立区域分别判断。模板禁止嵌套三元，不为单层简单显示条件增加无用函数。

## 响应式与数据流

- 自建状态偏好 `ref`，对象和数组同样适用；脚本按需使用 `.value`，模板遵守 Vue 解包规则。不为个人偏好改写库返回的响应式对象。
- 派生值用 `computed` 或直接计算；`watch` / `watchEffect` 用于必要副作用，不用 watcher 维护本可推导的重复状态，清理订阅、定时器和过期异步结果。
- props 保持单向数据流，不直接修改；通过 emit 或约定的 `v-model` 更新。不随意解构响应式对象丢失追踪；`defineProps` 解构是否保持响应式取决于项目 Vue 版本，不能把新版本行为假定为所有 Vue 3 都支持。
- `v-for` 使用稳定业务 key；同一元素不混用 `v-if` 和 `v-for`，先筛选数据或将条件放到明确的外层。

## 类型与编译器宏

- `defineProps` / `defineEmits` 偏好简短内联对象泛型，只用一次的简单契约不再新建类型文件；共享或复杂契约复用项目类型。命名元组等写法先确认当前版本支持。
- `defineSlots` 使用插槽函数签名；返回类型当前不用于插槽内容检查，示例中的 `any` 是这个 API 的局部例外，不能推广到业务数据。
- `defineModel` 泛型表示绑定值，例如 `defineModel<string>()`；对象值才用对象类型。确认版本支持，并核对默认值是否会让父子状态不同步。
- `defineExpose` 传入真实运行时对象，如 `defineExpose({ focus })`，不能只写泛型替代对象。
- `ref` / `computed` 能推断类型时不加泛型；需要表达空值或未加载状态时使用 `ref<Order | null>(null)` 等完整契约，不靠断言绕过检查。
- 宏由 SFC 编译器提供，不为了调用宏增加无意义 import；其他 Vue API 是否自动导入以项目配置为准。

下面是支持对应宏的项目中的语法片段，不是要求每个组件都声明这些宏：

```ts
// 订单组件入参
const props = defineProps<{ total: number }>();

// 通知父组件选中的订单
const emit = defineEmits<{ select: [order: Order] }>();

// 默认插槽可使用的展示数据
defineSlots<{ default(props: { title: string }): any }>();

// 当前选中订单，初始时尚未选择
const selectedOrder = ref<Order | null>(null);

// 是否存在可展示的订单
const hasSelectedOrder = computed(() => selectedOrder.value !== null);
```

技术依据：[Vue script setup 与编译器宏](https://vuejs.org/api/sfc-script-setup.html)。
