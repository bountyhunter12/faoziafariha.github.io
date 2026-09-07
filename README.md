# bountyhunter12.github.io

Personal academic website of **Faozia Fariha**, researcher at the NLP Lab,
Chittagong University of Engineering &amp; Technology (CUET).

Live at <https://faoziafariha.github.io/> 

## Stack

Plain HTML, one CSS file, and a small vanilla JavaScript file. No build step, no
dependencies, no framework.

```
index.html          About, research interests, news, selected publications
research.html       Research overview, threads, current and past work
publications.html   Full publication list with links and BibTeX
teaching.html        Service, mentoring, awards (nav label: "Service")
cv.html              Web CV, links to the PDF
404.html             Not-found page
check.py             Consistency checker; run after every edit
assets/
  css/style.css      All styling; design tokens at the top
  js/main.js         Theme toggle and footer year
  img/               Profile photo and favicon
  cv/                PDF CV
```

## Working on it

Preview locally:

```bash
python -m http.server 4173
```

Check for broken links, dead anchors, and navigation drift:

```bash
python check.py
```

Regenerate `sitemap.xml` after adding a page:

```bash
python check.py --write-sitemap
```

- [CONTENT-GUIDE.md](CONTENT-GUIDE.md): how to add a publication, news item, or entry.
- [AGENTS.md](AGENTS.md): conventions and hard rules for AI coding agents.
- [DECISIONS.md](DECISIONS.md): why the site is built this way, and what was rejected.

## Deploying

Push to `main`. [`.github/workflows/deploy.yml`](.github/workflows/deploy.yml)
runs `check.py` and publishes to GitHub Pages **only if it passes**, so a broken
edit fails the build instead of taking the live site down. Nothing else is
needed; there is no manual deploy step.

Pages source is set to **GitHub Actions** under *Settings → Pages*.

To publish this at `<your-username>.github.io`, create a repository with
exactly that name, push these files to `main`, and set the canonical URL,
`og:url`, JSON-LD `url`/`sameAs`, `robots.txt`, and `check.py`'s `SITE`
constant to match your actual GitHub Pages URL if it differs from
`https://bountyhunter12.github.io/`.

## Features worth knowing about

- **Dark mode** follows the operating system and can be overridden with the nav
  toggle; the choice persists in `localStorage`, and nothing is written there
  until you actually click, so the site keeps following the OS otherwise.
- **Structured data**: `schema.org` `Person` on the homepage, `ScholarlyArticle`
  on the publications page.
- **Accessible**: semantic landmarks, a skip link, visible focus rings, and
  AA-contrast colours verified in both themes.
- **Prints cleanly**: the CV page drops navigation and chrome when printed.

## Replace the placeholder photo

`assets/img/profile.jpeg` is currently a generated "FF" monogram placeholder,
not a real photo. Replace it with an actual square photo (600&times;600 or
larger) and keep the filename; see [CONTENT-GUIDE.md](CONTENT-GUIDE.md) for
details.
