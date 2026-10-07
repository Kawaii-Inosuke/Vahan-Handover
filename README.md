# Vahan — Marketing Website

Static marketing site for [Vahan](https://vahan.ai), built from the Figma designs
as plain HTML, CSS and vanilla JavaScript. There is no build step, framework or
package manager — every page opens directly in a browser.

## Running locally

Open `index.html` in a browser, or serve the folder so links behave exactly as
they will in production:

```sh
python3 -m http.server 8000
# then visit http://localhost:8000
```

## Deploying

Upload the repository root to any static host (Netlify, Vercel, S3 + CloudFront,
Nginx, …). Do **not** deploy `docs/` or `_archive/` — they are reference
material, not part of the live site.

## Project structure

```
.
├── index.html                  Homepage
├── corporate.html              For Corporates
├── recruitment-agencies.html   For Recruitment Agencies
├── job-seekers.html            For Job Seekers
├── job-listing.html            Individual job page
├── riderhub.html               Rider Hub
├── case-studies.html           Case study listing
├── case-study.html             Case study detail (renders ?id=<slug> from js/case-studies-data.js)
├── partner-stories.html        Partner stories
├── careers.html                Careers
├── career-role.html            Individual open role
├── newsroom.html               Newsroom
├── contact.html                Contact us
├── blog.html                   Blog listing
├── blog-*.html                 Individual blog posts (blog-post.html is the EV delivery article)
│
├── css/
│   ├── styles.css              Global styles: fonts, header, footer, shared components (load first)
│   ├── pages/<page>.css        One stylesheet per page, named after the page
│   └── responsive.css          Tablet/mobile layer (load last on every page)
│
├── js/
│   ├── site.js                 Shared behaviour: canvas scaling + mobile nav (every page)
│   └── case-studies-data.js    Content for every case study
│
├── assets/
│   ├── fonts/season-mix/       Season Mix display typeface (see licence note below)
│   ├── icons/                  Small UI icons shared across pages (chevrons, arrows, socials)
│   └── images/
│       ├── brand/              Vahan logos
│       ├── client-logos/       Client logo strip (Zomato, Swiggy, Uber, …)
│       ├── shared/             Images used by more than one page
│       └── <page>/             Images used by one page, named after that page
│
├── docs/
│   ├── designs/                Exported Figma page designs (SVG) — reference only
│   └── blog-content/           Source copy for the blog posts (.docx)
│
└── _archive/                   Pages taken off the site, with RESTORE.md steps
    ├── events/
    └── our-story/
```

## How the pages are built

- **Desktop (≥ 1920 px):** each page is authored as a fixed 1920 px Figma canvas
  with absolutely positioned sections.
- **1024–1919 px:** `js/site.js` sets `--z` and the whole canvas is scaled down
  proportionally, so laptops get the design intact.
- **≤ 1023 px:** `css/responsive.css` returns sections to normal document flow
  and restyles them for the 430 px mobile frame.

Every page loads, in this order:

```html
<link rel="stylesheet" href="css/styles.css">
<link rel="stylesheet" href="css/pages/<page>.css">
<link rel="stylesheet" href="css/responsive.css">
...
<script src="js/site.js"></script>
```

### Adding a page

1. Copy the closest existing page and rename it (`kebab-case.html`).
2. Create `css/pages/<page>.css` and link it as above.
3. Put the page's images in `assets/images/<page>/`. If an image is reused by
   another page, move it to `assets/images/shared/`.
4. Add the page to the header nav and footer on every page.

### Adding a case study

Add an entry to `window.VAHAN_CASE_STUDIES` in `js/case-studies-data.js`; it is
then available at `case-study.html?id=<key>`.

## Conventions

- File and folder names are lowercase `kebab-case`, with no spaces.
- Asset paths are relative (`assets/...` from HTML, `../../assets/...` from
  `css/pages/`), so the site works from any folder or sub-path.

## Before going live — open items

- **Font licence.** `assets/fonts/season-mix/` contains the *trial* build of
  Season Mix, licensed for **personal use only** (see `LICENSE.txt`). A
  commercial licence must be bought, and the trial files swapped for the
  licensed ones (ideally as `.woff2`), before the site is published.
- **WhatsApp apply links** on `job-seekers.html` and `job-listing.html` use the
  placeholder number `919999999999`.
