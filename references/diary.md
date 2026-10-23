# 每日收获 (diary)

One file per calendar day: `src/content/diary/YYYY-MM-DD.md`. Never invent a date; ask if it is unknown.

## Frontmatter

| Field | Required | Notes |
| --- | --- | --- |
| `date` | yes | `YYYY-MM-DD`, matching the filename |
| `title` | no | Keep `""` unless the user names a theme for the day |
| `draft` | no | Only when the user wants the day hidden |

## Body

The body is a flat list, not prose.

- Current files use a numbered list (`1. `, `2. `). Match the existing file's marker when appending; new files use numbered lists.
- One source item per line. Preserve wording, order, and item count.
- Do not add an intro, conclusion, heading, commentary, tags, or links. Do not expand a short item into a paragraph.
- Normalize only obvious formatting: `1、` / `•` → `1. `, trim stray whitespace, keep inline code and commands as given.
- Keep vague or incomplete items vague; do not invent details to complete them.

## Existing day

- Append new items after the last entry, continuing the numbering. Do not reorder, rewrite, or renumber existing items.
- If the day's file does not exist, create it with the date frontmatter and the list.
