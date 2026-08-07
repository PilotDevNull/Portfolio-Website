# Jawad Kachbal — Portfolio Website

A home for Jawad Kachbal. Live at [jawadkachbal.com](https://jawadkachbal.com).

A single-page Jekyll site with sliding panels for About Me, a Projects grid,
and a full Detailed CV, plus a standalone CV page (`/cv/`) that doubles as a
downloadable, linkable resume.

---

## Tech stack

- **[Jekyll](https://jekyllrb.com/)** — static site generator, builds the
  site from Markdown/HTML templates + YAML data
- **SCSS/Sass** — compiled to `assets/css/main.css`
- **Vanilla JavaScript** — no frontend framework; panel/carousel/modal logic
  lives inline in `_includes/header.html`
- **Font Awesome (Free)** — icons throughout
- **Cloudflare Web Analytics** — privacy-first visitor analytics (beacon
  script in `_layouts/default.html` and `_layouts/cv-default.html`)
- **GitHub Pages** — hosting, with a custom domain via the `CNAME` file

---

## Adding content (no HTML editing required)

Both the homepage projects and the CV are data-driven. Adding new content
means editing one YAML file — the templates read from it and generate
everything else automatically.

### Adding a homepage project

1. Drop two images into `images/projects/`, named after a slug of your
   choosing (lowercase-with-dashes, must be unique):
   - `<slug>.jpg` — used for the grid tile and the mobile-width hero image
   - `<slug>-inside.jpg` — used for the desktop hero image
2. Open `_data/portfolio.yml`. Copy an existing entry (starts with
   `- slug:`, ends right before the next `- slug:`), paste it below the
   last one, and fill in:

   | Field | What it does |
   |---|---|
   | `slug` | Must match your image filenames and becomes the section id |
   | `title` | Project name, shown as the page heading |
   | `subtitle` | One-line tagline under the title |
   | `meta` | Small text under the subtitle (module code, team size, etc.) — HTML entities like `&middot;` work |
   | `accent` | Hex color, themes the panel's accent (`--accent` CSS variable) |
   | `links` | List of buttons in the top-right of the panel — each has `href`, `icon` (any Font Awesome class, e.g. `fab fa-github`), and `title` (tooltip text) |
   | `technologies` | List shown in the sidebar |
   | `body` | The write-up. Plain HTML — use `<h4 class="cvstyle-subhead">` for section headings and normal `<p>`/`<ul><li>` for the rest to match existing styling |

3. Commit and push. The grid tile, the detail panel, the accent theming,
   and the open/close JS wiring are all generated from that one entry —
   nothing else needs to change.

Everything above is also documented in a comment block at the top of
`_data/portfolio.yml` itself.

### Adding a CV entry (experience, education, skill, project, etc.)

Open `_data/cv-data.yml`. Find the relevant top-level section —
`experiences`, `education`, `projects`, `skills`, `certifications`,
`references`, etc. — copy an existing entry under its `info:` (or
`assignments:` for projects) list, paste a new one below it, and fill in
the fields (they're self-explanatory: `role`, `time`, `company`, `details`
for a job; `title`, `tech`, `details` for a project, and so on).

`details` fields are rendered through Markdown (`| markdownify`), so you
can write things like `- bullet point` or `**bold**` directly instead of
HTML tags.

### How this works under the hood

- `_data/portfolio.yml` is looped over by `_includes/portfolio-projects.html`
  (renders each project's detail panel) and `_includes/portfolio-index.html`
  (renders the homepage grid tiles).
- `_includes/header.html` also loops over the same data to build the
  `portfolioSections` JS lookup table that wires up the open/close panel
  behaviour — so a new project is automatically clickable without editing
  any JavaScript.
- `_data/cv-data.yml` is read by the partials in `_includes/cv/` (one
  partial per CV section) and by `_includes/detailed-cv-panel.html`, which
  embeds the standalone `/cv/` page in an iframe for the in-site popup.

---

## Running locally

Requires Ruby (with DevKit on Windows) and Bundler.

```bash
bundle install       # installs Jekyll + all dependencies from the Gemfile
bundle exec jekyll serve
```

Then open `http://127.0.0.1:4000/`. Jekyll watches for file changes and
rebuilds automatically — just save and refresh. `Ctrl+C` stops the server.

If `bundle install` fails trying to compile a native gem (nokogiri, ffi,
etc.) on Windows, run `ridk install`, choose the MSYS2/MinGW toolchain
option, then retry.

---

## Deploying

Push to `main` — GitHub Pages rebuilds and redeploys automatically, usually
within a minute or two. No manual build/deploy step needed.

```bash
git add -A
git commit -m "Describe what changed"
git push origin main
```

**Windows note:** if you edit files on a machine that hasn't been used with
this repo before, run `git config core.autocrlf true` once, before your
first commit. Without it, Windows' CRLF line endings vs. this repo's LF
endings will make git think every line of every file you touch has
changed, even when it hasn't.

---

## Analytics

Cloudflare Web Analytics is installed via a JS snippet in both layout files
(`_layouts/default.html` and `_layouts/cv-default.html`). Check live stats
at [dash.cloudflare.com](https://dash.cloudflare.com) → Analytics & Logs →
**Web Analytics** (not the "Dashboards" traffic-overview page — that one
tracks proxied zone traffic and won't show anything for this setup).

---

## Credits

Originally based on **jekyll-uno** by Josh Gerdes.

Later adapted by Thomas Zühlke as **jekyll-uno-timeline**.

The overall one-page shell (sliding panel system, video background, layout
grid) started from that base; almost none of the original styling remains
— the panel system, project carousel, colour theming and CV integration are
custom.

The Detailed CV page is built on the **Orbit** resume/CV template by
Xiaoying Riley ([themes.3rdwavemedia.com](http://themes.3rdwavemedia.com),
Creative Commons Attribution 3.0), restructured into the sidebar-plus-main
layout used here and recoloured to match the paper CV.

Icons throughout are **Font Awesome Free**.

The source code in this repository is licensed under the MIT License. See
the LICENSE file for details.

Original content (including text, original photographs, and original
artwork) is © 2026 Jawad Kachbal. All rights reserved.
