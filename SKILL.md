---
name: astro-note-rewriter
description: Turn rough personal Markdown notes or project drafts into polished Astro posts or project pages, with correct frontmatter, semantic slugs, preserved code/media, and placement in the right content collection.
---

# Astro Note Rewriter

Turn rough writing into well-structured Astro content without changing its intent. This skill has two content modes: article mode for technical notes and project mode for unformatted project drafts. Project mode focuses on content polishing and placement; it does not invent or implement a new project-page UI unless the user separately asks for that.

## Before editing

- Inspect the destination collection schema, existing posts, Markdown extensions, and the source notes before deciding the frontmatter or formatting.
- Determine whether the source is an article or a project description. For project mode, inspect the project collection schema, existing project entries, project detail route, and its card/list conventions before writing the file.
- Inspect the repository’s content directories and naming conventions so the finished file is placed in the collection that actually renders the requested page. Do not leave the polished result only beside the source draft.
- Identify all embedded images, diagrams, local links, and code blocks. Move required media into the destination project and keep every reference valid.
- Treat a note with no meaningful body as a draft to flag rather than publish an empty article.

## Rewrite one article at a time

For every note, individually:

1. Give it a specific, searchable title that retains the original question or subject.
2. Write a concise description, assign a focused category and a small set of meaningful tags, and use a lowercase semantic slug filename. Do not retain opaque names such as `1.md` or `note.md`.
3. Reorganize prose into a clear reading flow: conclusion or context, main explanation, comparisons or steps where useful, and concise follow-up questions. Use headings, tables, callouts, diagrams, and language-tagged code fences when they make the original material easier to scan.
4. Preserve all original questions, material conclusions, caveats, and media. Rephrase for clarity and fill obvious structural gaps, but do not introduce claims that conflict with the source.

## Rewrite a project draft

When the user provides a rough project description without caring about Markdown layout, frontmatter, or presentation:

1. Read the draft as source material, identify the project’s purpose, problem, solution, key features, technology, current status, links, and notable limitations, and keep uncertain or missing details out rather than guessing.
2. Give it a clear project title, concise description, semantic lowercase slug, focused tags, and an appropriate status. Derive these from the draft and repository evidence; do not manufacture metrics, users, dates, features, or links.
3. Organize the body for a project detail page. Prefer a useful sequence such as overview, motivation/problem, implementation, features, usage, lessons learned, and future work, but omit sections that the draft does not support.
4. Preserve technical names, commands, code, API shapes, tradeoffs, and stated limitations. Improve wording and hierarchy, not the underlying implementation claims.
5. Write the finished file into the project collection directory using the destination schema. Keep the original rough draft untouched unless the user asks for replacement. If the project has local images or media, place them according to the repository’s project-content convention and update relative references.
6. Use the project page’s existing fields and rendering behavior. Do not add new UI, styles, components, or dependencies merely to make the content fit; report missing infrastructure separately.
7. Leave genuinely incomplete projects as drafts when the source lacks enough content for a truthful public page, and explain what information is missing.

## Code and factual boundaries

- Preserve code blocks verbatim by default: do not change algorithms, identifiers, implementation choices, or formatting inside a code block. Rewrite code only when the user explicitly asks for it.
- Keep commands and configuration snippets intact unless the user authorizes an operational update. Never carry secrets, private hostnames, IP addresses, tokens, or internal project identifiers into a public post; replace or remove them only after explaining the handling when needed.
- For current, version-sensitive technical facts introduced during rewriting, verify them using authoritative documentation. Clearly separate verified additions from the author’s original notes.

## Validation and handoff

- Validate every rewritten post against the Astro content schema and run the project’s relevant check before handoff.
- Validate every rewritten project entry against its project collection schema, confirm its generated project route, and check that card metadata, detail content, links, and local media resolve correctly.
- Confirm that generated routes, image paths, and Markdown directives work.
- When asked to commit, stage only fully rewritten articles and their required assets. Do not include unfinished migrations or unrelated workspace changes.
- Report which notes were completed, the final slugs, validation result, and anything intentionally left as a draft.
