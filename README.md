# ym-skill

个人 Codex Skill 集合。目前包含 `ym-rules`，用于在编写、修改或审查前端代码时统一开发习惯与回复规范。

## 包含内容

`ym-rules` 主要约定：

- TypeScript、JavaScript、React、TSX 与 Vue 3 的代码风格
- 组件拆分、Vue Composition API 与类型使用方式
- 语义化 HTML 与无障碍基础要求
- Tailwind CSS v4 类名组织方式
- 简洁、直接的中文技术回复风格

详细规则见 [`skills/ym-rules/SKILL.md`](skills/ym-rules/SKILL.md)。

## 安装

将技能目录复制到 Codex 的个人技能目录：

```powershell
git clone https://github.com/zmy-devs/ym-skill.git
Copy-Item -Recurse -Force .\ym-skill\skills\ym-rules "$env:USERPROFILE\.codex\skills\ym-rules"
```

重新启动 Codex 或新建会话后即可使用。

## 使用

在 Codex 中明确调用技能：

```text
使用 $ym-rules 按我的开发规则实现这项改动。
```

当任务涉及 TypeScript、JavaScript、React、Vue 3、HTML 或 Tailwind CSS v4 时，也可由 Codex 根据技能描述自动选择。

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
    ├── references/semantic-html.md
    └── SKILL.md
```

## 作者与许可

作者：[张铭洋](https://github.com/zmy-devs)

本项目采用 ISC 许可证。
