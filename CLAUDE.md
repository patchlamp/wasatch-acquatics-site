# Wasatch Aquatic Specialties — website (wasatch-acquatics-site)

This repo IS the website for Wasatch Aquatic Specialties. There is no dashboard and no CMS: you
change the site by editing these files and pushing. Live at https://wasatch-acquatics-site.pages.dev/.

How to talk to the client, how texted photos arrive (`incoming/`, one level
up), and what counts as Taylor's work are all in the workspace `CLAUDE.md`
one level above this repo. This file covers only this site: what is in it,
how to change it, and how to check a change is live.

## What this site is

- Plain HTML/CSS/JS static files. **No frameworks, no build step, no npm, no
  external JS libraries.** If a change seems to need one, find the plain
  way or say it's out of scope.
- Five pages, the same five the old site had, at the same paths:
  `index.html` (home: hero, four service cards, about, "Our Promise",
  contact form), `about.html`, `services.html` (intro + the four services,
  each with an id the footer links to), `products.html` (three product
  cards), `contact.html` (the form + phone/email). `404.html` is what the
  host shows for a missing page.
- One stylesheet, `css/style.css`. Colors and fonts are the tokens in `:root`
  at the top; change the look there, not scattered through the file. Fonts
  are self-hosted in `fonts/` (Inter for everything, Cinzel for the
  wordmark only) — nothing loads from a third party.
- One script, `js/main.js` (fade-in on scroll). Motion is transform/opacity
  only, off under `prefers-reduced-motion`, and nothing is hidden with JS off.
- The header, nav, footer, and the hidden SVG icon sprite at the top of
  `<body>` are repeated in every `.html` file (no build step means no shared
  includes). When you edit them, make the same edit in **every page**.
- **The logo** is `images/logo-mark.svg` (the nested-ring drop, remade as
  vector) plus the wordmark as live text (`.brand-name` "WASATCH" in Cinzel,
  `.brand-sub` "Aquatic Specialties"). The look of the logo is Lynn's brand
  and does not change; if he sends the original logo file, replace the
  mark with it and keep the same size. `apple-touch-icon.png` is the mark
  on white.
- `images/` holds the photos. The two pool photos (`hero-pool-lanes.jpg`,
  `pool-tiles-water.jpg`) are Pexels stock (free for commercial use); the
  three product photos are the manufacturers' images carried over from the
  old site. A real photo of Lynn's work should replace stock whenever he
  sends one. `_headers` and `_redirects` are read by the host, not served:
  `_headers` makes every page `no-cache` (so nobody ever sees a stale
  page); `_redirects` is for a page that moved. Leave both alone otherwise.
  With a custom domain every page carries a canonical link to the bare
  domain (www serves the same site).
- **No trackers, no analytics, no cookie banners, no checkout.** The only
  form is the contact form, which posts to
  `https://patchlamp.com/f/wasatch-acquatics/contact` (emailed to Lynn, copy
  kept — `newsletter forms` shows what came in). A newsletter box would
  follow the workspace `PLAYBOOK.md`. Nothing on this site ever stores
  visitor information itself.
- **The words are Lynn's.** The copy was carried over from the old site
  verbatim; change it when he asks, never to "improve" it. Don't invent
  facts, prices, brands, or claims: the products page lists only the three
  brands he listed, with no descriptions, until he says more.

## How to change things

- **Copy** (tagline, service text, about, contact details): edit the text
  in the page it's on. The four service blurbs appear twice — short on
  `index.html`, full on `services.html` — and the "Get in Touch" block is
  on both `index.html` and `contact.html`; keep the pairs consistent. Ask
  for anything you don't have; don't guess an address or a phone number.
- **A new photo**: save it as a JPG in `images/` with a plain name, about
  1200px on the long side (a hero photo 1800px). Texted photos are already
  converted and resized; move them from `incoming/` (one level up) into
  `images/`. `alt` is a short plain description of what's in the photo;
  never leave it empty. The home hero is `img.hero-bg` in `index.html`; the
  about photo is the `figure.feature-photo` on `index.html` / `about.html`.
- **A new product**: copy one `.product-card` in `products.html` (photo on
  white, kicker = the category, h3 = the brand/product name).
- **A new page**: copy `about.html`, change the title, description and
  content, and add it to the nav in **every** page.
- **Colors / fonts**: the `:root` tokens in `css/style.css`.
- **A link** (a supplier, a map): add it to the footer list in every page,
  or as a `.button` in the section it belongs to. Linking out is always
  fine; embedding third-party code is not, except a plain `<iframe>` embed
  from a service the client already uses (a map).

## Every change goes live — deploy check

"Done" means live at https://wasatch-acquatics-site.pages.dev/, not edited on disk. Relay sessions run from the
client workspace one level up, so git takes the form
`git -C repos/wasatch-acquatics-site …`; never `cd` into the repo first (that is always
blocked). After any requested change, without waiting to be asked:

1. Look at your work: `shot repos/wasatch-acquatics-site/<page>.html` (add `--mobile` for
   the phone layout) renders the page from disk and saves a picture under
   `shots/`; open it with Read. For anything to do with layout or style, also
   run `shot check repos/wasatch-acquatics-site/<page>.html`. Fix what's off before you
   push. If you touched HTML, also confirm the tags you edited are balanced.
2. Commit on `main` with a short plain-English message
   (`git -C repos/wasatch-acquatics-site add -A && git -C repos/wasatch-acquatics-site commit -m "…"`).
3. Push: `git -C repos/wasatch-acquatics-site push`. The repo is the record.
4. Publish: `site publish wasatch-acquatics-site`. It sends exactly what is committed to
   the host and prints the live URL once it is serving (seconds, not
   minutes). It refuses if anything is uncommitted or unpushed — that is
   the point, not a bug: fix the git step and run it again.
5. Confirm the live site serves the change, with exactly this shape (no
   pipe, no redirect):

       curl -s https://wasatch-acquatics-site.pages.dev/PAGE.html

   (for the home page, `curl -s https://wasatch-acquatics-site.pages.dev/`). Read the output and look for the
   new content yourself. Only say it's live once you have seen it there.
   There is no cache to wait out: every page is served `no-cache`, so a
   phone that reloads sees the new page.

Undo the last change with `git -C repos/wasatch-acquatics-site revert HEAD`, push, and
publish again. Never force-push, never rewrite history. This is standing
permission from Taylor: don't ask "should I push?" for anything the client
asked for. Do stop if a change would break a rule in this file or delete
something the request didn't clearly ask to delete.

## Hosting (for Taylor)

Cloudflare Pages, one project per site, published by `site publish` from
the committed tree of this repo's `main` (direct upload; no build, no
Cloudflare-side git connection). The repo is on GitHub under `patchlamp`
and stays the record; the host is a mirror of it. A custom domain is
`SITE_ADMIN=1 site domain wasatch-acquatics-site example.com` from the workspace (adds
the domain to the project, puts canonical links on every page, prints the
two DNS lines for the registrar). Handing the repo to the
client at the end is `site transfer wasatch-acquatics-site <their-github-user>`.
