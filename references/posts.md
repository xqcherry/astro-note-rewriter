# Posts

Long-form articles: `src/content/posts/<topic-dir>/<slug>.md`.

## Frontmatter

The schema in `src/content/config.ts` wins. Typical fields:

| Field | Required | Set it when |
| --- | --- | --- |
| `title` | yes | Always; keep the original question or subject, make it searchable |
| `published` | yes | `YYYY-MM-DD` for date-only notes; ISO datetime only when the source gives a time |
| `updated` | no | The source states a revision |
| `description` | no | One sentence on what the reader gets; not a table of contents |
| `category` | no | Genre. Reuse an existing one: `后端八股`, `算法题解`, `运维`, `密码学`, `区块链`, `AI 工程`, `学习`, `生活`, `指南`, `前端` |
| `tags` | no | 2–5 concrete keywords, reusing existing tags when possible |
| `series`, `seriesOrder` | no | The note belongs to a numbered series |
| `image` | no | Cover image URL (MinIO), per [images.md](images.md) |
| `lang` | no | Non-Chinese article |
| `draft` | no | Rewrite is incomplete or the source is a stub; hide demo content with `true` |

List existing values before choosing:

```bash
grep -rh "^category:" src/content/posts | sort | uniq -c | sort -rn
grep -rh "^tags:" src/content/posts | sort -u
```

## Body

- Start with the problem or context in 1–3 sentences. No "本文介绍…" preamble.
- `##` for main sections, `###` for sub-points; never skip levels.
- Use tables for comparisons, callouts for caveats, language-tagged fences for code, and `$...$` / `$$...$$` for math (supported by the theme).
- Keep first-person asides only when they carry meaning; otherwise normalize to neutral prose.
- Preserve the original open questions, caveats, and uncertainties. Do not invent a conclusion, summary, or motivational close.
- Keep code blocks byte-for-byte, including comments and formatting.

## Naming

- File: semantic ASCII slug describing the subject, e.g. `des-feistel-network`, `http-vs-https`, `redis-delay-queue`.
- Folder: subject grouping (`interview/Redis`, `interview/network`, `algorithms`, `hardware`), independent of `category`.
- A series uses the same `series` string and ascending `seriesOrder`.
