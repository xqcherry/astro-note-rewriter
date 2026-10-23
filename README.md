# Astro Note Rewriter

一个用于整理 Astro Markdown 内容的 skill。它把手写笔记整理成符合内容集合约定的三类内容：技术文章（posts）、动态（dynamic）和每日收获（diary），并在改写前先读取目标项目的 schema 与既有约定。

> This repository contains an instruction-driven skill. It adds no runtime dependency to an Astro site.

## Features

- 先读 `src/content/config.ts` 与同类现有文件，再决定 frontmatter、slug 和落盘位置。
- 按内容类型分流：长文进 `posts`，单条片段进 `dynamic`，当日清单进 `diary`。
- 图片统一走 MinIO：上传后引用完整 `https://` URL，不保留本地副本。
- 保留原始问题、结论、限制、条目顺序和代码块，不新增源材料没有的观点。
- 日期、地点、系列等可选字段只从源材料取值，缺失时留空而不是编造。
- 改完后用 `pnpm check` 和 `pnpm build` 验证 schema、路由与资源。

## Requirements

- [Codex](https://openai.com/codex/)、Claude Code 或其他兼容 `SKILL.md` 的客户端
- 一个使用 Astro 内容集合的项目
- 站点若使用 MinIO 托管图片，需要一个可用的上传脚本（示例为 `pnpm image`）

## Installation

将本仓库放到客户端的 skills 目录，并保持目录名为 `astro-note-rewriter`。

Claude Code：

```powershell
git clone git@github.com:xqcherry/astro-note-rewriter.git "$env:USERPROFILE\.claude\skills\astro-note-rewriter"
```

Codex：

```powershell
git clone git@github.com:xqcherry/astro-note-rewriter.git "$env:CODEX_HOME\skills\astro-note-rewriter"
```

Unix-like shell 下把目标路径换成对应目录即可。若客户端使用其他加载约定，按该约定放置本仓库。

## Usage

在包含 Astro 项目的工作区里直接描述任务：

```text
把 notes/redis-delay-queue.md 整理成技术文章，检查 content schema，代码块保持原样。
```

```text
把这段文字整理成动态，日期是 2026-10-23。
```

```text
把今天的工作整理成每日收获，追加到当天文件。
```

技能会先确认目标集合的 schema 和最近一篇同类文件的写法，再按类型参考改写；源笔记默认保持不变。

## Content types

| 源笔记 | 集合 | 落盘位置 | 参考 |
| --- | --- | --- | --- |
| 长文、分节、含代码 | `posts` | `src/content/posts/<topic-dir>/<slug>.md` | [references/posts.md](references/posts.md) |
| 短的第一人称片段 | `dynamic` | `src/content/dynamic/YYYY-MM-DD-<slug>.md` | [references/dynamic.md](references/dynamic.md) |
| 一天的清单式记录 | `diary` | `src/content/diary/YYYY-MM-DD.md` | [references/diary.md](references/diary.md) |
| 难以判断 | — | — | 向用户确认，不猜 |

`<topic-dir>` 是 `src/content/posts/` 下按主题分组的已有目录，与 frontmatter 里的 `category`（文体标签）不是一回事。

## Images

所有内容图片托管在 MinIO，`public/images/` 属于待迁移的旧方案：

- 文章图片 → `images/posts/<slug>/`，动态图集 → `images/dynamic/<slug>/`。
- 上传后直接使用脚本输出的 Markdown；封面 URL 写入 frontmatter `image:`。
- 引用更新并确认可访问后再删除本地文件；上传失败时保留本地文件并报告。
- 新增或改写的内容中不应出现 `/images/...` 本地引用。

详见 [references/images.md](references/images.md)。

## Hard rules

- 代码、命令和配置逐字保留，除非用户明确要求改动。
- 不把密钥、token、私有主机名、IP 或内部标识带进公开内容。
- 只处理单篇笔记，批量时也逐篇进行，避免一处失败牵连其他。
- 每日收获不覆盖既有条目，追加时接续编号。
- 源材料不足以支撑内容时保留 `draft: true`，不写填充文字。

## Validation

在目标站点根目录执行：

```bash
pnpm check    # schema 与类型错误
pnpm build    # 路由、资源与构建期错误
```

检查是否残留本地图片引用：

```bash
grep -rnE '(\]\(|src: |image: )/images/' src/content/{posts,dynamic,diary}
```

本次改动触及的条目若命中，说明图片步骤未完成。

## Repository layout

```text
.
├── SKILL.md              # 主指令：定位、分流、改写与校验流程
├── references/
│   ├── posts.md          # 技术文章 frontmatter 与正文规范
│   ├── dynamic.md        # 动态的写法与示例
│   ├── diary.md          # 每日收获的追加规则
│   └── images.md         # MinIO 上传与迁移流程
├── agents/
│   └── openai.yaml       # 客户端展示元数据
├── README.md
├── LICENSE
└── .gitignore
```

## Development

本仓库只有 Markdown 和 YAML 指令，没有构建步骤。修改后建议：

1. 检查 `SKILL.md` 的 frontmatter、分流表和硬性规则是否仍然自洽。
2. 确认 `agents/openai.yaml` 是有效 YAML，显示名称与 skill 名称一致。
3. 在一个真实 Astro 项目中各跑一次 posts、dynamic、diary 任务。
4. 确认生成内容符合 schema，图片均为远端 URL，`pnpm check` / `pnpm build` 通过。

## Contributing

欢迎提交改进。PR 中请说明：修改解决的具体场景、是否改变了某类内容的默认行为、是否在真实 Astro 项目中验证、是否需要同步更新 README 与 `references/`。

请不要提交个人笔记、私有链接、凭据、生成的站点产物或含敏感信息的示例。

## License

本项目以 [MIT License](LICENSE) 发布。
