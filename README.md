# Novels-Twines Content

This repository stores the structured content that the Novels-Twines website can display after publication.

## Repository layout

- `catalog/index.json` — list of published novels and ColorNovels.
- `schema/` — JSON Schemas for validating novel and episode metadata.
- `templates/` — starter files for new novels and episodes.
- `novels/<novel-id>/` — standard novel metadata and episode files.
- `color-novels/<novel-id>/` — ColorNovels metadata and episode files.
- `assets/` — artwork references and asset organization guidance.

## Content status

Only content explicitly marked `published` and included in the catalog should be displayed to readers. Drafts and pending submissions should remain in the application/backend until approved.

The catalog starts empty. The website's current demo stories are not copied here and should not be treated as published works.

## Suggested publication flow

1. A Creator drafts a novel or episode in the application.
2. The backend validates metadata and stores the submission.
3. An Admin reviews and approves the submission.
4. The backend's publishing integration writes the approved files and updates the catalog.
5. The website reads the published catalog and content.

Do not place passwords, session tokens, API keys, GitHub App private keys, or other secrets in this repository.

## IDs and file naming

Use stable lowercase IDs with hyphens, such as `the-example-novel`. Episode files should use ordered names such as `episode-001.md`. Avoid changing an existing ID after publication because URLs and references may depend on it.

## Artwork

Store small, optimized assets under `assets/` or use an approved image-storage service. Avoid committing large originals. Record asset paths or trusted URLs in metadata.

## Rights and licensing

The repository currently contains an MIT LICENSE. Review that license before adding original novels, illustrations, or other creative works. Do not assume that the repository's software-oriented license is appropriate for creative content. Obtain the rights holder's decision before publishing or changing licensing terms.
