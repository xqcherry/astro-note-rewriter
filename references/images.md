# Images

All content images live on MinIO. `public/images/` is legacy being migrated out: do not add new files there, and never keep a local copy of an image you uploaded.

## Upload

```bash
# Article images → images/posts/<slug>/
pnpm image --slug des-feistel-network ./images/a.png ./images/b.png

# Dynamic gallery → images/dynamic/<slug>/
pnpm image --kind dynamic --slug 2026-10-23-walk --alt 傍晚 ./photos/walk.jpg

# Preview without uploading
pnpm image --slug demo ./images/demo --dry-run
```

- `--slug` must equal the content file's slug. For dynamic that is the full `YYYY-MM-DD-<slug>`.
- Paste the printed Markdown or `gallery:` snippet as-is. Put a cover's printed URL in frontmatter `image:`.
- URLs look like `https://<host>/<bucket>/images/{posts,dynamic}/<slug>/<hash>-<name>.ext`.
- Pass a meaningful `--alt` for gallery images; the script falls back to the filename otherwise.
- Credentials live in `.env`; never inline keys, tokens, or endpoints you were not given.

## Legacy local images

A local `/images/...` reference is legacy, not a destination.

- When rewriting a note that references local images, upload them with `pnpm image` first, replace every reference with the printed URL, then delete the now-unused local files.
- Covers stored beside posts (`src/content/posts/**/cover.jpeg`) get the same treatment: upload, switch to the URL, remove the file.
- Delete only after the reference is updated and the remote URL is verified. If upload fails, keep the local file and report it — never leave a broken reference.
- Do not delete untouched `public/images/` content unless the user explicitly asks for the full migration.

## Checks

- New or rewritten content has no `/images/...` references; every image is a full `https://` URL.
- No image file is committed for a MinIO-hosted entry.
- When a source note points at an image that does not exist, report it instead of silently dropping or replacing it.
