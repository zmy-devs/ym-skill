# ym-skill

个人 Codex Skill 集合。目前包含 `ym-rules`，按实际技术栈读取代码规范，并在完成前端改动后自动自审、修复和复查。

## 包含内容

`SKILL.md` 只保留工作流程和资料路由，具体规则按需加载：

| 资料                   | 内容                                                |
| ---------------------- | --------------------------------------------------- |
| `code-style.md`        | 通用写法、控制流、命名、中文注释、TypeScript 与清理 |
| `react.md`             | React 组件、Hooks、状态渲染、JSX 和事件             |
| `vue.md`               | Vue 3 单文件组件、响应式、模板与编译器宏            |
| `tailwind.md`          | 类名组织、条件样式、变体、排序与主题                |
| `element-structure.md` | HTML 语义、容器、布局、表单与可访问性               |
| `components.md`        | 业务组件 API、公共组件、shadcn/ui 与 Dialog/Sheet   |
| `review.md`            | 检查表、问题分级、修复闭环、验证和结果输出          |
| `semantic-html.md`     | 多组件组合后的语义化 DOM 示例，仅在需要时读取       |

规则结合前端审查清单整理，既用于编码，也用于交付前检查。尊重项目现有版本、组件库和工具配置，不为应用规则引入新技术栈。

详细规则见 [`skills/ym-rules/SKILL.md`](skills/ym-rules/SKILL.md)。

## 安装

推荐直接在 Codex 中发送：

```text
使用 $skill-installer 从 https://github.com/zmy-devs/ym-skill/tree/main/skills/ym-rules 安装这个 Skill。
```

安装完成后，下一轮对话即可使用。

也可以手动将技能目录复制到 Codex 的个人技能目录：

```powershell
git clone https://github.com/zmy-devs/ym-skill.git
Copy-Item -Recurse -Force .\ym-skill\skills\ym-rules "$env:USERPROFILE\.codex\skills\ym-rules"
```

重新启动 Codex 或新建会话后即可使用。

## 使用

在 Codex 中明确调用技能：

```text
使用 $ym-rules 按我的前端规范实现这项改动，完成后自审并修复。
```

独立审查并修复：

```text
使用 $ym-rules review 本次改动，修复发现的问题，再复查并给出验证结果。
```

只读审查：

```text
使用 $ym-rules 只审查本次改动，不修改文件，按严重程度报告问题。
```

以上是自然语言调用，不是额外的 CLI 子命令。实现任务会自动自审，无须再次发送 review 指令；只要求审查时保持只读。结果简短列出已修复内容、实际验证和遗留问题，不能把未执行的检查标成通过。

当任务涉及 TypeScript、JavaScript、React、Vue 3、HTML 或 Tailwind CSS 时，也可由 Codex 根据技能描述自动选择；不依赖 OO 或专用 MCP 服务。

## 本地维护

项目使用 [pnpm](https://pnpm.io/) 管理开发依赖，并通过 [Prettier](https://prettier.io/) 统一 Markdown、YAML、JSON 等文件的格式。

```bash
pnpm install
pnpm format
pnpm format:check
```

## 目录结构

```text
skills/
└── ym-rules/
    ├── agents/openai.yaml
    ├── references/
    │   ├── code-style.md
    │   ├── react.md
    │   ├── vue.md
    │   ├── tailwind.md
    │   ├── element-structure.md
    │   ├── components.md
    │   ├── review.md
    │   └── semantic-html.md
    └── SKILL.md
```

## 作者与许可

作者：[张铭洋](https://github.com/zmy-devs)

本项目采用 ISC 许可证。
