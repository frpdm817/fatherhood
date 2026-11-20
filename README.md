# Fatherhood

Source of https://fatherhood.fabiopereira.name, one of three blogs linked from the hub at https://blog.fabiopereira.name. It is a plain Jekyll site built by GitHub Pages (no Gemfile, no custom plugins). The theme is local: `_layouts/`, `_includes/`, `assets/site.css`.

## Writing posts

Name files `YYYY-MM-DD-title.md` and put them in the folder for their language:

| Language   | Folder         | URL                       |
|------------|----------------|---------------------------|
| English    | `_posts/`      | `/YYYY/MM/DD/title/`      |
| Spanish    | `es/_posts/`   | `/es/YYYY/MM/DD/title/`   |
| Portuguese | `pt/_posts/`   | `/pt/YYYY/MM/DD/title/`   |

Front matter needs only `title`; `lang`, `layout` and the permalink come from `defaults` in `_config.yml`. `description` is optional (used for the meta description).

```yaml
---
title: "Hello, Fatherhood"
ref: hello
description: The first post on the Fatherhood blog.
---
```

### Photo and link previews

Every page gets Open Graph and Twitter/X tags (from `_includes/seo.html`), so shared links show a title and description. To add a photo, put it in `assets/images/` and set it in the front matter of each translation:

```yaml
image: /assets/images/my-post.jpg   # or a full URL
image_alt: What the photo shows
```

The photo appears at the top of the post and becomes the preview image. A wide 1200×630 image fits previews best.

### Translations: `ref`

Give every translation of a post the same `ref` value. The language switcher on a post links to the translation with the same `ref`; if a language has none, it links to that language's home page. Posts without `ref` have no translations.

Home pages (`/`, `/es/`, `/pt/`) list only posts in their own language. A language with no posts shows a "no posts yet" note followed by the English posts.

### UI strings and dates

All UI strings, language names, month names and date formats are in `_data/i18n.yml`. To add a language, add its code to `languages`, a block of strings, a home page folder and `defaults` entries.

## Language cookie (shared with the hub)

The hub and the blogs share one cookie, `lang=en|es|pt`, set with `Domain=fabiopereira.name; Secure; SameSite=Lax; Max-Age=31536000`.

- `assets/lang.js` is a copy of the hub's file. It saves the cookie when a language switcher link is clicked (and clears a stale host-only copy).
- The English home (`/`) starts with an inline script (`_includes/lang-redirect.html`, enabled by `lang_redirect: true` in `index.html`). `/?lang=en` saves English and strips that parameter; otherwise a `lang=es|pt` cookie redirects to `/es/` or `/pt/`. It skips the redirect on back/forward navigation. Only the English home has it.
- From es/pt pages, the EN switcher link to the root is `/?lang=en`, as on the hub.
- "All blogs" links back to the hub in the current language.

## Local build

```sh
docker run --rm -v "$PWD":/srv/jekyll jekyll/jekyll:3.8 jekyll build --safe
```

`_site/` is ignored by git.
