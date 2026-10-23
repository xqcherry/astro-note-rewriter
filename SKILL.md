---
name: astro-note-rewriter
description: Turn hand-written Markdown notes into Astro content collections — technical posts, 动态 (dynamic), and 每日收获 (diary) — with schema-accurate frontmatter, semantic slugs, and correctly hosted images. Use when publishing, migrating, or tidying notes under src/content/, or when the user says 改写笔记, 整理笔记, 发动态, or 写每日收获.
---

# Astro Note Rewriter

Convert a note into publishable content while keeping its meaning. Structure and accuracy matter; volume does not. Never add commentary, claims, or closing lines the source does not contain.

## 1. Establish ground truth

Read these before writing — they override this skill when they disagree:

1. `src/content/config.ts` (or `src/content.config.ts`) — authoritative schemas: required fields, defaults, types.
2. One recent file in the target collection — match its conventions, not an idealized template.
3. The source note in full, including image links and code fences.

If neither content-config file exists, stop and ask which project and collection to target; the paths below are this blog's defaults, not a universal layout.

## 2. Route the note

| Source note | Collection | Destination | Reference |
| --- | --- | --- | --- |
| Long-form, technical, sections or code | `posts` | `src/content/posts/<topic-dir>/<slug>.md` | [references/posts.md](references/posts.md) |
| Short first-person moment | `dynamic` | `src/content/dynamic/YYYY-MM-DD-<slug>.md` | [references/dynamic.md](references/dynamic.md) |
| One day's list of work or lessons | `diary` | `src/content/diary/YYYY-MM-DD.md` | [references/diary.md](references/diary.md) |
| Genuinely ambiguous | — | — | Ask; do not guess. |

`<topic-dir>` is an existing folder under `src/content/posts/` (e.g. `interview/Redis`, `algorithms`, `cryptography`). It is **not** the frontmatter `category`: folders group by subject, `category` labels the genre (`后端八股`, `算法题解`, ...). Reuse a matching folder; create one only for a new subject.

## 3. Handle media first

Images are hosted on MinIO only; `public/images/` is legacy. Follow [references/images.md](references/images.md). Invariant: every image lives in exactly one place — after upload, no local copy remains — and every reference is a full `https://` URL.

## 4. Write one note at a time

For each note:

1. Derive the date from the filename or the source; never invent one.
2. Choose a lowercase ASCII hyphenated slug that describes the subject (`redis-delay-queue`, not `1` or a pinyin title). Keep the existing slug when editing an existing file.
3. Write frontmatter from the schema. Omit optional fields you cannot ground in the source — do not invent `location`, `series`, `image`, or diary `title`.
4. Write the body per the type reference, preserving every original question, conclusion, caveat, and item order. Fill structural gaps; add no new claims.
5. Validate and report.

## Hard rules

- Code, commands, and config stay verbatim. Rewrite them only when explicitly asked.
- Never publish secrets, private hostnames, IPs, tokens, or internal identifiers.
- Never overwrite diary entries; append and continue the numbering.
- Process notes individually, even in a batch, so one failure does not cascade.
- If a note has no usable body, keep it `draft: true` instead of publishing filler.
- For facts you introduce while rewriting, verify against authoritative docs; leave the author's original claims untouched.

## Validate

From the repo root:

```bash
pnpm check    # schema and type errors
pnpm build    # routes, assets, build-time failures (when files or images were added)
```

Local image references are legacy and must not appear in new or rewritten content:

```bash
grep -rnE '(\]\(|src: |image: )/images/' src/content/{posts,dynamic,diary}
```

Any hit in an entry you touched means the media step is unfinished. Hits in untouched entries are fine until the user requests the full migration.

## Report

After finishing, give a compact table: source note → collection → destination path → images (hosted/linked) → validation result. Flag drafts and unresolved media explicitly.
