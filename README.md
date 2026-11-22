# Novels-Twines Content

This repository is the content library for the Novels-Twines website. Keep the website interface in [Creative-NovelS](https://github.com/Novels-Twines/Creative-NovelS), backend code in [Novels-Twines-Backend](https://github.com/Novels-Twines/Novels-Twines-Backend), and novels/episodes/artwork here.

## 1. What belongs here?

- Novel descriptions and author details.
- Episode titles, summaries, and original episode text.
- ColorNovel metadata and artwork references.
- The public catalogue of approved, published content. Only complete stories with approved artwork (where applicable) belong in the public catalogue.

Do not put website code, backend code, passwords, API keys, session tokens, or GitHub App private keys here.

## 2. Repository folders

```text
catalog/
  index.json                 # Public catalogue (published entries only)
novels/
  <novel-id>/
    novel.json               # Novel details
    episodes/
      episode-001.md         # Episode 1 text and metadata
      episode-002.md         # Episode 2 text and metadata
color-novels/
  <color-novel-id>/
    novel.json               # ColorNovel details
    episodes/
      episode-001.md
assets/
  ...                        # Cover images and other approved artwork
schema/
  novel.schema.json
  episode.schema.json
templates/
  novel.metadata.template.json
  color-novel.metadata.template.json
  episode-001.template.md
```

Use a stable ID made of lowercase letters, numbers, and hyphens. Example: `the-last-star`. Do not change an ID after publishing because links may depend on it.

## 3. Add a new novel on GitHub (step by step)

You can do this in the GitHub website; you do not need Git commands.

1. Open this repository: https://github.com/Novels-Twines/Novels-Twines-Content
2. Select the branch dropdown (usually showing `main`) and create a branch such as `content/add-the-example-novel`. Do not upload directly to `main`.
3. Open the `novels` folder. If it does not exist yet, create it by creating a file named `novels/README.md` first.
4. Choose **Add file → Create new file**.
5. In the filename box, enter `novels/your-novel-id/novel.json`. GitHub creates the folders from the path.
6. Copy the structure from `templates/novel.metadata.template.json` and replace the example values:
   - `id`: same as the folder name, for example `the-example-novel`.
   - `title`: the novel title readers will see.
   - `description`: a short, accurate summary.
   - `author`: the credited author or pen name.
   - `genre`: an array, for example `["Fantasy", "Adventure"]`.
   - `cover`: `null` until an approved cover is added, or a repository-relative asset path.
   - `status`: keep it as `"draft"` while preparing the work.
   - `contentType`: keep it as `"novel"`.
   - `episodeIds`: list the IDs of the episodes you add, such as `["the-example-novel-episode-001"]`.
7. Select **Commit changes** and commit to your feature branch.

Example metadata:

```json
{
  "id": "the-example-novel",
  "title": "The Example Novel",
  "description": "A short description of the story.",
  "author": "Author Name",
  "genre": ["Fantasy"],
  "cover": null,
  "language": "en",
  "status": "draft",
  "contentType": "novel",
  "createdAt": null,
  "updatedAt": null,
  "publishedAt": null,
  "episodeIds": ["the-example-novel-episode-001"]
}
```

## 4. Add an episode

1. Open the same branch.
2. Create a file named `novels/your-novel-id/episodes/episode-001.md`.
3. Copy the format from `templates/episode-001.template.md`.
4. Replace the frontmatter values:
   - `id`: unique episode ID, for example `the-example-novel-episode-001`.
   - `novelId`: the parent novel's ID; it must match the folder and `novel.json`.
   - `title`: episode title.
   - `order`: episode number starting at 1.
   - `summary`: a short episode summary.
   - `status`: `draft` until approved.
   - `publishedAt`: leave `null` until publication.
5. Write the original episode below the second `---` line. Do not paste copyrighted work unless you have permission to publish it.
6. Commit the file to your feature branch.
7. Update `episodeIds` in `novel.json` so it includes the new episode ID.

Example episode:

```markdown
---
id: the-example-novel-episode-001
novelId: the-example-novel
title: Episode 1 — The Beginning
order: 1
summary: The main character discovers an unusual clue.
status: draft
publishedAt: null
---

# Episode 1 — The Beginning

Write your original episode text here.
```

For the next episode, use `episode-002.md`, set `order` to `2`, and give it a new unique ID. Keep numbering continuous.

## 5. Add a ColorNovel

Follow the same process, but create the metadata file at:

`color-novels/your-color-novel-id/novel.json`

Copy `templates/color-novel.metadata.template.json`. Set `contentType` to `"color-novel"`. Put its episode Markdown files in:

`color-novels/your-color-novel-id/episodes/`

Add artwork under `assets/` and use a relative path in the `cover` field when ready. Use descriptive filenames such as `assets/covers/the-example-color-novel.webp`.

## 6. Upload artwork

1. Prepare an optimized image (WebP or JPEG is recommended).
2. In GitHub, open or create the `assets/covers/` folder.
3. Select **Add file → Upload files**.
4. Drag the image into the upload area.
5. Commit it to your feature branch.
6. Set the matching relative path in the novel metadata, for example `"cover": "assets/covers/the-example-novel.webp"`.

Only upload artwork you own or have permission to use. Avoid very large original files.

## 7. Review and publish workflow

The statuses mean:

- `draft`: work in progress; not public.
- `pending`: submitted for review; not public.
- `published`: approved and ready for readers.
- `archived`: no longer actively published.

**Important:** creating or uploading files does not automatically publish a story. Open a pull request from your feature branch to `main`. An authorized reviewer must check the metadata, episode files, artwork rights, and completeness before approving it.

Only after approval should the publishing process:
1. Set the novel and approved episode statuses to `published`.
2. Set publication timestamps.
3. Add the novel entry to `catalog/index.json` under `novels` or `colorNovels`.
4. Merge the approved changes.

The catalogue is the public index. Keep drafts and pending submissions out of it. The episode JSON Schema validates the parsed YAML frontmatter object, not the Markdown source file itself. Never mark work as published just to make it appear on the website.

## 8. Update the catalogue

The catalogue currently has this shape:

```json
{
  "version": 1,
  "updatedAt": null,
  "novels": [],
  "colorNovels": []
}
```

When content is approved for publication, add the published entry to the correct array. Keep IDs unique and make sure each catalogue entry points to a matching metadata file. The website integration reads this catalogue from the Content repository's `main` branch. It accepts an ID string or an object with an `id` field; use the object format below for clarity. Do not add a draft or pending entry.

Example after a novel and at least one episode have been approved:

```json
{
  "version": 1,
  "updatedAt": "2026-11-19T12:00:00Z",
  "novels": [
    { "id": "the-example-novel" }
  ],
  "colorNovels": [
    { "id": "the-example-color-novel" }
  ]
}
```

The public website loads the matching `novel.json` file, then loads the episode Markdown files in episode order. It displays only novels whose metadata status is `published` and only episodes whose frontmatter status is `published`. Keep unpublished episodes out of the public reader by leaving their status as `draft` or `pending`. The catalogue should contain only approved, published novel IDs.

## 9. Create a pull request

1. Open the repository on GitHub and switch to your feature branch.
2. Select **Contribute → Open pull request** (or the **Compare & pull request** button).
3. Set the base branch to `main` and compare branch to your feature branch.
4. Give the PR a clear title, such as `Add The Example Novel and Episode 1`.
5. In the description, summarize the novel, episodes, artwork, and status.
6. Create the pull request and wait for review.
7. Fix requested changes on the same feature branch. Do not merge until the authorized reviewer approves.

## 10. Checklist before requesting publication

- [ ] Novel metadata follows `schema/novel.schema.json`.
- [ ] Each episode follows the episode frontmatter format and `schema/episode.schema.json`.
- [ ] Novel ID, folder name, `novelId`, and episode IDs match.
- [ ] Episode order is continuous and `episodeIds` is up to date.
- [ ] Text is original or you have permission to publish it.
- [ ] Artwork is authorized and its path is correct.
- [ ] Draft/pending content is not in the public catalogue.
- [ ] An authorized reviewer has approved the pull request.

## 11. Current starter records and publication status

The repository includes the starter novel **NEW LIFE IN MINI-CARAVAN** and a planned ColorNovel record. The normal novel's metadata is currently marked `published` and its ID is included in `catalog/index.json`, so the website can list the novel.

Its first episode, `novels/new-life-in-mini-caravan/episodes/episode-001.md`, is still marked `draft` and contains placeholder text, not a complete chapter. The website intentionally hides this episode from the public episode list and reader. This means the novel may appear in the catalogue with **zero published episodes** until a complete chapter is written, reviewed, and approved.

The ColorNovel starter record is not part of the public catalogue. It must have complete original story content and approved visual pages before publication.

Do not change an episode to `published` simply to make it appear on the website. Replace placeholder content with the author's complete original work, review it, and follow the publication workflow above.

## 12. Security, ownership, and licensing

Never commit passwords, API keys, access tokens, session cookies, or GitHub App private keys. The backend must handle privileged publishing credentials server-side.

This repository currently has an MIT LICENSE. That license may not be suitable for creative writing or artwork. Confirm the rights holder's intended license before publishing original content; do not assume the MIT license grants permission to reuse stories or illustrations.
