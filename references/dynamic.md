# 动态 (dynamic)

One moment per file: `src/content/dynamic/YYYY-MM-DD-<slug>.md`. The date prefix must match the `published` date.

## Frontmatter

| Field | Required | Notes |
| --- | --- | --- |
| `published` | yes | ISO datetime with the site offset, e.g. `2026-10-23T22:00:00+08:00`. Use `T00:00:00+08:00` when the source gives only a date; never invent a time of day. |
| `pinned` | no | Only when the user asks to pin |
| `location` | no | Only when the source states a place (`深夜`, `操场`, `上海`) |
| `gallery` | no | `[{ src, alt }]`; see [images.md](images.md) |

## Body

- First person, present tense, close to the author's original voice. Keep fragments and rhythm; do not polish into an essay.
- Separate paragraphs with a blank line. A single newline collapses when rendered, so do not rely on it for line breaks. For deliberate hard breaks inside a stanza, keep the original grouping in separate paragraphs.
- No headings, lists, or bold section labels unless the source already has them.
- Keep the original ending. Do not add a moral, summary, or poetic closing line.
- Do not add an intro explaining the moment.

## Example shape

```md
---
published: 2026-10-23T22:00:00+08:00
location: 操场
gallery:
  - src: https://minio.xqcherry.top/blog/images/dynamic/2026-10-23-walk/abcd1234-track.jpg
    alt: 傍晚的操场
---

跑完五公里，风比昨天凉。

操场边有人在放很老的歌。
```
