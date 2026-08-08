# aryanp2107.github.io

Personal portfolio site built on [Jekyll](https://jekyllrb.com/) + the [Minimal Mistakes](https://mmistakes.github.io/minimal-mistakes/) remote theme, hosted on GitHub Pages.

Live at [aryanp2107.github.io](https://aryanp2107.github.io).

## About this site

A running project log — computer vision, medical imaging, autonomous perception, teaching videos, and platform work under [arxelos.com](https://arxelos.com). Structured as a reverse-chronological feed with one card per project; each card links to a deeper write-up.

Built to hold real work under one roof rather than scatter it across Colab links, one-off repos, and dead demo pages.

## Structure

    aryanp2107.github.io/
    ├── _config.yml                 # site config: theme, author, plugins
    ├── _data/
    │   └── navigation.yml          # top nav links
    ├── _includes/
    │   └── archive-single.html     # customized to support video-embed cards
    ├── _pages/
    │   ├── about.md                # About Me
    │   └── writing.md              # LinkedIn articles
    ├── _posts/                     # one file per project write-up
    ├── assets/
    │   ├── css/
    │   │   └── main.scss           # SCSS overrides (3-col grid, video container)
    │   └── images/                 # teasers, heroes, avatars
    ├── index.html                  # homepage: reverse-chron post feed
    ├── POST_TEMPLATE.md            # copy this to start a new post
    ├── WORKFLOW.md                 # how to add/edit/delete posts
    └── new-post.ps1                # PowerShell scaffolder for new posts

## Adding a post

Fastest path from repo root:

```powershell
.\new-post.ps1 -Title "Your Post Title"
```

This creates a new dated file in `_posts/` from `POST_TEMPLATE.md`. Fill in the `REPLACE:` sections, drop a teaser image at `assets/images/<slug>-teaser.png`, then:

```powershell
git add .
git commit -m "Add post: <project name>"
git push
```

GitHub Pages rebuilds automatically — live in ~90 seconds.

For the full workflow (drafts, deletions, video embeds, local preview), see [WORKFLOW.md](WORKFLOW.md).

## Video-embed cards

Some cards render an auto-playing muted YouTube loop instead of a static teaser. Set the `youtube:` field in the post's front matter:

```yaml
youtube: "https://www.youtube.com/embed/VIDEO_ID?autoplay=1&mute=1&controls=0&loop=1&playlist=VIDEO_ID"
```

The customized `_includes/archive-single.html` renders it as an iframe on the grid. `.youtube-container` styling lives in `assets/css/main.scss`.

## Customization notes

- **Theme version pinned** in `_config.yml` (`@4.26.2`) — remote-theme updates won't touch the site until deliberately bumped.
- **Skin** left as MM default (light). Alternatives commented in `_config.yml`.
- **3-per-row grid** with a wider max-width container, overriding MM's default 4-per-row float layout — CSS Grid rules in `assets/css/main.scss`.
- **Feed sort** is reverse-chronological by front-matter `date:`.

## Rebuild from scratch

Everything the live site needs is in this repo. No external services or databases.

```bash
git clone https://github.com/aryanp2107/aryanp2107.github.io.git
```

GitHub Pages builds it. Posts are portable Markdown — if MM ever stops working, the content moves cleanly to any other static-site generator.

## Tech

- **Site:** Jekyll + Minimal Mistakes (remote theme)
- **Hosting:** GitHub Pages
- **Content:** Markdown with YAML front matter
- **Media:** Static images in `assets/images/`, YouTube embeds via `youtube:` front matter field

## License

Site content (`_posts/`, `_pages/`, `assets/images/*` I created) is © Aryan Patel. Ask before republishing.

Theme code inherits Minimal Mistakes' MIT license — see [mmistakes/minimal-mistakes](https://github.com/mmistakes/minimal-mistakes).