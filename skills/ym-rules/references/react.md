# React 与 TSX

按项目实际 React 版本和渲染方式实现，不引入当前任务未要求的新数据层、服务端组件模式或优化库。

## 组件与状态

- 页面负责场景编排；可独立理解的 UI 放子组件，可复用的状态行为放 Hook，请求和数据转换放项目现有的数据层或纯函数。不要把状态、请求、转换和长 JSX 全塞进页面，也不要把简单内聚逻辑拆成跳转多次才能读懂的碎片。
- 组件用简短中文注释说明职责；局部 Props 放组件函数外，共享业务类型按通用类型规则管理。
- 简单派生数据在渲染时直接计算，不额外创建 state + effect；用户操作触发的流程放事件处理函数。Effect 用于与外部系统同步，保留正确依赖与必要清理。
- 遵守适用的 Hooks 调用规则；常规状态/Effect Hook 不放在条件、循环或早返回后。共享状态放到实际需要它的共同所有者，不重复保存可推导的状态。
- 列表使用稳定业务 key；可重排、增删的列表不使用索引或随机值作为 key。不直接修改 props、state 或它们引用的对象。
- 异步结果需与当前操作对象匹配，处理错误、竞态和必要的取消/过期结果；优先沿用项目已有请求方案，不为此给每个组件写一套基础设施。
- 不无差别添加 `useMemo`、`useCallback` 或 `memo`；有明确计算成本、引用契约或项目要求时再使用。

## 条件渲染

- `loading` / `error` / `success` 等互斥状态，用早返回、`switch`、渲染函数或清晰的组件映射集中表达，不铺一排 `status === ... && ...`。
- 独立的可选区域可用 `&&`；数字条件先转为明确布尔值，例如 `items.length > 0`，避免把 `0` 渲染到页面。
- 简单两选一允许单层三元；禁止嵌套三元。分支复杂时先命名状态或拆出渲染职责，不把业务流程藏在 JSX 表达式里。
- 提前返回不能打乱 Hooks 顺序；需要先调用的 Hooks 放在状态渲染分支之前。

```tsx
// 展示订单加载、失败和成功状态
const OrderContent = ({ status, orders }: OrderContentProps) => {
  switch (status) {
    case 'loading':
      return <Loading />;

    case 'error':
      return <LoadError />;

    case 'success':
      return <OrderList orders={orders} />;

    default:
      return null;
  }
};
```

示例为渲染片段，类型和被引用组件沿用项目定义；不要据此新增空壳组件。

## JSX 与事件

- JSX 只表达结构、简单条件和轻量绑定；分支、校验、请求、多次状态更新等事件逻辑提取为有名称的处理函数。
- `onClick={() => setOpen(false)}`、`onClick={() => handleSelect(item.id)}` 这样的简单绑定可以内联，不必再套一层函数。
- 不在 JSX 中塞大段 `map` 数据转换、立即执行函数或多步骤 callback；先在合适位置准备展示数据。
- 相邻 JSX 同级元素之间默认空一行，服从现有格式化工具。不要为了少一个 DOM 容器留下无效语义，也不要为组件强行增加 wrapper。
- 公共业务组件、shadcn/ui、列表弹层的规则见主入口路由的组件设计资料；元素和样式分别应用对应资料。

技术依据：[React 派生数据与 Effect](https://react.dev/learn/you-might-not-need-an-effect)、[React 状态共享](https://react.dev/learn/sharing-state-between-components)。
