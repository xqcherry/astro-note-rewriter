# Astro Note Rewriter

一个用于整理 Astro Markdown 内容的 Codex skill。它可以把零散的技术笔记或项目草稿，整理成符合 Astro 内容集合约定的文章或项目页面，同时保留原始意图、代码、媒体和链接。

> This repository contains an instruction-driven skill for Codex. It does not add a runtime dependency to an Astro site.

## Features

- 将粗略笔记整理为结构清晰、可发布的技术文章。
- 将项目草稿整理为 Astro 项目详情页内容。
- 根据现有 content schema、路由和命名约定生成 frontmatter 与语义化 slug。
- 保留代码块、图片、图表、本地链接、原始问题、结论和限制条件。
- 在提交前检查内容集合、生成路由、媒体路径和 Markdown 扩展。
- 对信息不足的项目保留为 draft，避免编造指标、用户、日期或链接。

## Requirements

- [Codex](https://openai.com/codex/) 或兼容 skill 的 Codex 客户端
- 一个 Astro 项目，且项目中存在内容集合或 Markdown 内容目录
- Git（仅在希望将整理结果纳入版本控制时需要）

## Installation

### Project-local installation

将本仓库复制或克隆到项目可访问的 skills 目录，并确保目录名为 `astro-note-rewriter`：

```powershell
git clone https://github.com/<owner>/astro-note-rewriter.git $env:CODEX_HOME\skills\astro-note-rewriter
```

在 Unix-like shell 中：

```bash
git clone https://github.com/<owner>/astro-note-rewriter.git "$CODEX_HOME/skills/astro-note-rewriter"
```

如果客户端使用其他 skill 加载目录，请按该客户端的目录约定放置本仓库。

## Usage

在包含 Astro 项目的工作区中直接描述任务，例如：

```text
请把 notes/astro-content.md 整理成可发布的 Astro 文章，检查 content schema，保留代码和图片引用。
```

或：

```text
请把 drafts/my-project.md 整理到项目内容集合，保留原始技术细节，不要编造缺失的信息。
```

skill 会先检查目标项目的内容集合 schema、现有内容、Markdown 扩展、路由和媒体约定，再决定输出位置和 frontmatter。原始草稿默认保持不变。

## Content rules

### Article mode

文章会获得明确、可搜索的标题、简洁描述、聚焦的分类和少量标签，并使用小写语义化 slug。正文按上下文、结论、解释、步骤或对比等结构重排，但不会改变原始问题、结论、限制或代码含义。

### Project mode

项目页会从草稿中提取目的、问题、方案、功能、技术栈、状态、链接和限制，只输出源材料支持的内容。项目集合缺少足够信息时，会建议保留为 draft，并指出缺失信息。

### Safety and fidelity

- 默认逐字保留代码块、命令和 API 形状。
- 不将 token、密码、私有主机名、IP 或内部项目标识带入公开内容。
- 新增的版本敏感技术事实需要查阅权威文档并明确区分来源。
- 不会为了适配内容而新增 UI、样式或依赖。

## Repository layout

```text
.
├── SKILL.md              # Skill instructions loaded by Codex
├── agents/
│   └── openai.yaml       # Display metadata for Codex clients
├── README.md
├── LICENSE
└── .gitignore
```

## Development and validation

本仓库主要由 Markdown 和 YAML 指令组成，没有独立的构建步骤。修改后建议：

1. 检查 `SKILL.md` 的 frontmatter、章节结构和边界条件。
2. 确认 `agents/openai.yaml` 是有效 YAML，并保持显示名称与 skill 名称一致。
3. 在一个真实 Astro 项目中运行一次文章模式和项目模式任务。
4. 检查生成内容是否符合目标项目 schema，且图片、链接和生成路由有效。

## Contributing

欢迎提交改进。请保持 skill 的职责聚焦于 Astro 内容整理，并在 Pull Request 中说明：

- 修改解决了什么具体场景；
- 是否改变了文章模式或项目模式的行为；
- 是否在真实 Astro 项目中验证；
- 是否需要同步更新 README。

请不要提交个人笔记、私有链接、凭据、生成的站点产物或包含敏感信息的示例。

## License

本项目以 [MIT License](LICENSE) 发布。

