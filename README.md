# Haoming Wang's homepage

This is a Jekyll site for the homepage at <https://haomingwang645.github.io/>. The homepage is built directly by `_layouts/default.html`; there is no separate homepage content file. Publication and news data come from Markdown files in `_posts/`.

Homepage order: profile and research directions, optional conference banner, news, recent work (when present), selected publications, others, experience (service and teaching), and visitors.

## Where to make changes

| What to update | Source |
| --- | --- |
| Homepage bio, research directions, section layout, service, teaching, visitors | `_layouts/default.html` |
| Navigation and the CV link | `_includes/navbar.html` |
| News items and publication records | `_posts/*.markdown` |
| Full news archive | `news.md` (renders the same posts as the homepage news feed) |
| Layout for other pages such as news, about, and notes | `_layouts/page.html` |
| Colors, venue badges, layout, and responsive styling | `style.scss` |
| Optional conference travel banner above news | `_data/conferences.yml` |
| About page and notes | `about-me.md`, `notes.md`, `notes/` |
| Publication images and downloadable files | `paper_images/`, `pdfs/` |
| Site metadata, plugins, and build exclusions | `_config.yml` |
| Current English CV source and generated copy | `output/pdf/haoming_wang_cv_2026_09_25.tex` and matching PDF |
| Public CV linked by the navbar | `pdfs/CV_2026_09.pdf` |

`cv_material/` holds older CV drafts, application-specific versions, and research notes. It is excluded from the published site. `cv_material/service.yml` is **not** connected to the homepage; the visible service list is in `_layouts/default.html`.

## How posts appear on the homepage

The `categories` value in each post's YAML front matter controls where it appears:

| Category | Homepage | News feed |
| --- | --- | --- |
| `news_item` | No publication card | Uses `news_text`; optional `news_link` adds a `[link]` |
| `research_pub` | Selected publications | “Paper accepted”; `news_badge` gives a custom acceptance label |
| `research_recent` | Recent work | “New work” |
| `research_arxiv` | Others | “New preprint” |
| `others` | Others | Not included |

For a `research_pub` post, `homepage_group: others` moves its publication card from Selected publications to Others. In the Others section, `hidden: true` suppresses the card. The homepage news table shows the eight newest eligible posts; `news.md` shows the full archive. Both sort by the front matter `date`.

Selected-publication cards read these front matter fields: `title`, `date`, `authors`, `venue`, `venue_badge`, `venue_badge_class`, `image`, and optional links such as `paper`, `arxiv`, `code`, `poster`, `slides`, `website`, and `video`. In Others, `homepage_badge` can override the badge text. The `paper` field drives the **Paper** button and the accepted-paper link in news; `arxiv` drives a separate **arXiv** button. Put local PDFs in `pdfs/` and link them with a root-relative path such as `/pdfs/eccv26_paper.pdf`. Post body text becomes the short abstract shown below a publication card.

When announcing an acceptance, update the existing paper post's category, venue fields, and date. For a same-day post in China, use an explicit `+08:00` timestamp; a future UTC timestamp is omitted from the Jekyll build until that time arrives. See `_posts/2026-09-25-vlm-latent-shaping.markdown` for an accepted-paper example and `_posts/2026-06-17-latentstate.markdown` for a local Paper PDF plus a separate arXiv link.

## CV publishing

The navbar chooses the lexicographically greatest `/pdfs/CV_*.pdf` path among Jekyll static files. To publish a revised CV:

1. Edit or copy the latest LaTeX source in `output/pdf/`.
2. Compile it with `latexmk -pdf`, using a temporary output directory so auxiliary files do not enter the site.
3. Check the page count and rendered pages, then save the generated PDF beside its source and copy it to `pdfs/CV_YYYY_MM.pdf`. Updating the current file works; a new file must have a later name than the current public CV so the navbar selects it.
4. Build the site and verify that the navbar points to the new PDF.

## Local build and checks

```sh
bundle install
bundle exec jekyll serve   # Local preview at http://127.0.0.1:4000/
bundle exec jekyll build   # Generates _site/
git diff --check
```

`_site/` is generated and Git-ignored; edit the source files instead. After changing a post, check the rendered homepage, `/news/`, and any linked file under `_site/`. Keep uploaded PDFs in `pdfs/` with the post change so links work after deployment.
