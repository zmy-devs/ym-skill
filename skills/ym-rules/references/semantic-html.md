# 多组件嵌套的语义化 HTML

组件是代码组织单位，标签取决于内容职责。以下为 Vue 组件的模板片段，省略导入、数据和样式；重点检查它们组合后的 DOM。

`section` 可以作为组件根元素；下面的 `order-list.vue` 就是这种用法。也可以在分区内嵌套另一个有主题和相应级别标题的 `section`，但不应按组件层数机械添加。

| 内容职责                         | 根元素选择                           |
| -------------------------------- | ------------------------------------ |
| 有标题的主题分区，如最近订单     | `section`                            |
| 可独立理解的内容，如单张订单卡片 | `article`                            |
| 商品列表                         | `ul` 或 `ol`                         |
| 操作按钮                         | `button`                             |
| 仅用于布局的容器                 | `div`                                |
| 无需额外容器                     | 直接渲染内容，框架允许时使用多根节点 |

## 页面：order-page.vue

页面拥有唯一的 `main`。站点页头和页尾与主体同级。

```vue
<template>
  <site-header />
  <main id="main-content">
    <h1>我的订单</h1>
    <order-list />
    <order-help />
  </main>
  <site-footer />
</template>
```

## 站点页头：site-header.vue

```vue
<template>
  <header>
    <a href="#main-content">跳到主要内容</a>
    <a href="/">商城首页</a>
    <nav aria-label="主导航">
      <ul>
        <li><a href="/products">商品</a></li>
        <li><a href="/orders" aria-current="page">我的订单</a></li>
      </ul>
    </nav>
  </header>
</template>
```

## 订单分区：order-list.vue

这里有明确主题和标题，所以用 `section`；重复订单用列表。示例是静态数据，动态列表应为每个 `li` 提供稳定的业务 key。

```vue
<template>
  <section>
    <h2>最近订单</h2>
    <ul>
      <li><order-card /></li>
    </ul>
  </section>
</template>
```

## 订单卡片：order-card.vue

订单卡片可独立理解，使用 `article`；其页头、页尾属于这张卡片，不增加 `main`。此例在 `h2` 分区下使用 `h3`；复用到其他层级时应调整标题或提供标题级别接口。

```vue
<template>
  <article>
    <header>
      <h3>订单 A2026001</h3>
      <p>已发货</p>
    </header>
    <order-items />
    <footer>
      <p>合计：¥199</p>
      <a href="/orders/A2026001">查看订单详情</a>
    </footer>
  </article>
</template>
```

## 商品明细：order-items.vue

组件可直接以列表作为根元素，不需要额外的 `section` 或 `div`。

```vue
<template>
  <ul aria-label="订单商品">
    <li>机械键盘 × 1</li>
    <li>鼠标垫 × 1</li>
  </ul>
</template>
```

## 补充内容与站点页尾

`order-help.vue`：与订单列表间接相关的帮助信息使用 `aside`；侧边布局本身不是使用 `aside` 的理由。

```vue
<template>
  <aside>
    <h2>订单帮助</h2>
    <a href="/help/shipping">配送说明</a>
  </aside>
</template>
```

`site-footer.vue`：

```vue
<template>
  <footer>
    <p>© 示例商城</p>
    <a href="/contact">联系我们</a>
  </footer>
</template>
```

## 组合后的结构

```text
header                       site-header.vue
  nav
main                         order-page.vue：页面唯一主体
  h1 我的订单
  section                    order-list.vue：订单分区
    h2 最近订单
    ul
      li
        article              order-card.vue：独立订单
          header
            h3 订单 A2026001
          ul                 order-items.vue：商品列表
            li
          footer
  aside                      order-help.vue：补充帮助
    h2 订单帮助
footer                       site-footer.vue
```

参考：[HTML main 规范](https://html.spec.whatwg.org/multipage/grouping-content.html#the-main-element)、[HTML 分区元素规范](https://html.spec.whatwg.org/multipage/sections.html)。
