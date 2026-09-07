# Agent instructions

Personal academic website for Faozia Fariha, a CUET researcher working on
federated fine-tuning, low-resource NLP, and multimodal AI/AI safety. The
audience is prospective collaborators, graduate programmes, and anyone
following her research, so the site must read as a researcher's page: plain,
quiet, and fast, not a developer portfolio.

Read this before changing anything.

- `DECISIONS.md` explains *why* the site is built this way, including what was
  tried and rejected. Read it before proposing something this file forbids.
- `CONTENT-GUIDE.md` covers routine content edits in more detail.

## Hard rules

1. **No build step, no dependencies, no frameworks.** Plain HTML, one CSS file,
   one small vanilla JS file. Do not add npm, Jekyll, Hugo, Tailwind, React, or
   any CDN `<script>`/`<link>`. The only external request is Google Fonts, and
   every font has a local fallback stack.
2. **Run `python check.py` after every change.** It is the safety net for the
   duplicated markup described below. CI runs it too; a failure blocks deploy.
3. **The `<nav>` and `<footer>` blocks are byte-identical across all pages,
   on purpose.** Injecting them with JavaScript would hide them from search
   engines and break no-JS rendering. If you touch one, touch all of them and
   let `check.py` confirm they match.
4. **`404.html` uses root-absolute paths** (`/assets/css/style.css`,
   `/index.html`), because it can be served from any depth. Root pages use
   relative paths. `check.py` normalises this before comparing navigation.
5. **Colours are defined in three places** in `assets/css/style.css`: the
   `:root` block, the `@media (prefers-color-scheme: dark)` block, and the
   `:root[data-theme="dark"]` block. Changing one without the others breaks
   either the dark theme or the manual toggle.
6. **Keep text contrast at 4.5:1 or better** against its background, in both
   themes. `--text-faint` is already at the floor; do not lighten it.
7. **Never persist a theme to `localStorage` on page load.** Only on an
   explicit click. Writing on load pins the site to whatever the OS happened to
   be on the first visit and stops it following the system afterwards.
8. **Do not publish personal contact details beyond email.** No phone number, no
   home address, and no referees' email addresses on the site. Those stay in the
   PDF CV only.
9. **The theme toggle is a sibling of `.site-nav__links`, not inside it.**
   That is what lets the five links stay on one row at 390px while the toggle
   shares row one with the name. Putting it back inside re-creates a three-row
   header on phones.
10. **No em dashes anywhere.** Use a colon, semicolon, comma, parentheses, or a
   full stop instead. Strings of em-dash asides read as machine-written, which
   is the last impression this site should give. En dashes stay, but only for
   ranges (`2022&ndash;2026`, `pp. 1&ndash;6`) and compounds (`CNN&ndash;LSTM`).
   `grep -c mdash *.html` must return zero.
11. **Write CSS escapes with six hex digits** (`"\0000B7"`). A four-digit form
   generated through a script once produced a literal NUL byte that rendered as
   visible mojibake. `check.py` now fails on NUL bytes and U+FFFD.

## Tone and content

- Understated and factual. No marketing language, no emoji, no exclamation
  marks, no claims the CV does not support.
- British spelling is used throughout the prose.
- Research framing: one question, making large language models efficient,
  safe, and useful outside their best-resourced settings, across three
  threads: Green AI/federated learning, low-resource NLP, and multimodal
  AI/AI safety.
- Competitive-programming ratings and web-development side projects stay on
  `teaching.html` (labelled "Service" in the nav) and `cv.html` only. They must
  not appear on the homepage or the research page; leading with them signals
  "developer" rather than "researcher".

## Layout

```
index.html            About, interests, news, selected publications
research.html         Overview, three threads, current and past work
publications.html     Full list with links and BibTeX; ItemList JSON-LD in <head>
teaching.html         Service, mentoring, and awards (nav label: "Service")
cv.html                Web CV; links to the PDF
404.html               Not-found page (root-absolute paths)
check.py               Consistency checker; also regenerates sitemap.xml
assets/css/style.css   All styling. Numbered sections; tokens at the top
assets/js/main.js      Theme, dates, news collapse, BibTeX copy, back-to-top
.github/workflows/     Check-then-deploy to GitHub Pages
```

There is currently no blog. If one is added later, follow the same pattern
the previous version of this template used (`blog.html` index plus one file
per post under `blog/` with root-absolute paths), and update this file,
`README.md`, and `CONTENT-GUIDE.md` accordingly.

## Reusable components

Defined in `assets/css/style.css`. Prefer these over new CSS:

| Class | Use |
|---|---|
| `.entry` + `.entry__head/__title/__date/__sub` | A dated CV-style item |
| `.timeline` wrapping `.entry` items | Vertical rail with a node per entry |
| `.pub` + `.pub__title/__authors/__venue` | A publication record |
| `.tag`, `.tag--muted` | Journal / Conference / Workshop chips above the publication title |
| `.rows` + `.row` (`<dl>`) | Label-and-value pairs, e.g. skills |
| `.news` | Dated news list on the homepage |
| `.callout` + `.btn` | Boxed row with an action, e.g. CV download |
| `.pub__actions` | Row holding the paper link and the BibTeX disclosure |
| `.news-toggle` | Injected by main.js past 6 news items; do not hand-write |
| `.to-top` | Back-to-top button, injected by main.js on every page |

## Behaviour that lives in main.js, not markup

These are injected at runtime so no page carries duplicate HTML, and so readers
without scripting are never shown a control that could not work:

- the back-to-top button,
- the copy button on each BibTeX panel,
- the "show earlier updates" toggle, which appears only once the news list on
  `index.html` grows past six items. Just keep adding `<li>` entries in
  chronological order; nothing needs pruning,
- the footer year and the "Last updated" date, which reads the page's own
  `Last-Modified` header.

## Verifying

```bash
python check.py                  # links, anchors, nav/footer drift, metadata
python check.py --write-sitemap  # regenerate sitemap.xml after adding a page
python -m http.server 4173       # local preview
```

Check both themes and at least 375px, 780px, and 1100px widths after any layout
change. The navigation collapses to two rows below 780px and must never wrap its
link row.

## Deploying

Push to `main`. `.github/workflows/deploy.yml` runs `check.py`, and publishes to
GitHub Pages only if it passes. Pages source must be set to **GitHub Actions**
(not "deploy from a branch"). No manual step is needed.

## Facts

Do not invent credentials. Current, verified values:

- B.Sc. CSE, Chittagong University of Engineering & Technology (CUET),
  Jan 2022 &ndash; Jun 2026, CGPA 3.84/4.00
- Research Intern, ELITE Research Lab (remote), May 2026&ndash;present
- Graduate Research Member, NLP Lab, CUET, Nov 2025&ndash;present
- Four publications: three 2026 SemEval/LT-EDI workshop papers plus one 2025
  IEEE conference paper. See `publications.html` for exact records
- Email `faoziafarihaaa@gmail.com`
- Scholar `2KRgeSgAAAAJ` &middot; GitHub `bountyhunter12` &middot; LinkedIn
  `faozia-fariha`
- No ORCID on file; do not add an ORCID block until one exists
