# 元素结构、布局与可访问性

检查组件组合后真实的 DOM；组件边界不是语义分区，组件库也可能额外渲染容器或交互元素。

## 语义结构

- 页面层拥有主体 `main`，同一页面最多一个未隐藏的 `main`；不在 `article`、`aside`、`footer`、`header`、`nav` 或 `section` 内嵌套 `main`。
- `section` 表示真实主题分区，通常有标题，可以是组件根元素，也可以有合理嵌套；不因新建组件就套 `section`，不为减少层级删除有意义的分区。
- `article` 用于可独立理解的内容；`nav` 用于主要导航；`aside` 用于与主体间接相关的补充内容。侧边位置本身不是使用 `aside` 的理由。
- `header` / `footer` 可属于页面或内容分区；标题按真实内容层级使用 `h1` / `h2` / `h3`，不指望嵌套分区自动调整标题级别。
- 顺序有意义的重复项用 `ol` / `li`，否则用 `ul` / `li`；名称和值可用 `dl` / `dt` / `dd`；真实行列数据用带合适表头的 `table`，不拿表格做视觉布局。
- 正文用 `p`，无额外语义的行内分组用 `span`；不在 `p` 里放列表或分区，不嵌套按钮、链接等交互控件。

## 容器与布局

- 不添加既无语义又无布局、样式或交互职责的 wrapper；React 可用 Fragment，Vue 可按框架规则直接多根输出。
- 纯布局分组使用 `div`，布局容器、滚动区域、定位锚点、过渡或组件库要求的容器可以保留；“少一个 div”不是唯一目标。
- 普通布局优先 `flex` / `grid` / `gap`；单项靠右可用自动外边距，整体两端对齐可用 `justify-between`，按实际需求选择。
- 不用层层容器配合大量 `absolute` 拼普通布局；保留弹层、徽标、叠层和真实锚定需要的定位，检查裁切、滚动和窄屏行为。

## 真实交互

- 跳转用有 `href` 的链接，操作用 `button`；不以可点击 `div` 代替原生控件，不给原生元素重复添加已有 role。
- 原生非提交按钮显式 `type='button'`，尤其在表单内或可能复用到表单的组件中；提交按钮表达提交意图。封装组件先检查本地实现的默认值，不能因为审查“默认属性”就删掉必要按钮类型。
- 输入控件有可访问名称，优先关联可见 `label`，不能只靠 placeholder。图标按钮需要可访问名称；内容图片使用有意义的 `alt`，装饰图片用 `alt=''`。
- 保证核心操作可用键盘完成并有可见焦点；Dialog / Sheet 保留必要标题、说明、焦点约束、Escape 关闭和关闭后焦点恢复。
- 原生语义已充分表达时不叠加 ARIA；需要命名、关系或动态状态时保留恰当属性。不能把 `aria-label`、`aria-expanded`、屏幕阅读器标题等一律视为噪声。

主入口提供多组件语义示例的独立链接；只需判断结构时无需额外读取示例文件。

技术依据：[HTML main](https://html.spec.whatwg.org/multipage/grouping-content.html#the-main-element)、[HTML 分区元素](https://html.spec.whatwg.org/multipage/sections.html)、[HTML button](https://html.spec.whatwg.org/multipage/form-elements.html#the-button-element)。
