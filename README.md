# Miss Amazing Missouri — landing page

A single-page static site giving a brief overview of the Missouri chapter, linking
out to the national site (missamazing.org) and the full chapter site
(mo.missamazing.org) for anything deeper.

```
index.html        the whole page
styles.css        all styling; brand tokens live in :root at the top
assets/           favicon.ico, logo.svg, banner.webp (hero photo)
.nojekyll         tells GitHub Pages to serve files as-is
```

No build step, no dependencies. Open `index.html` in a browser to preview.

## Editing

**Colors and fonts** — the `:root` block at the top of `styles.css`. The brand
purples (`--violet`, `--magenta`) were sampled from `assets/favicon.ico`;
`--header` is the band colour behind the logo. Change a token and the whole
page follows.

**Events** — the `<ul class="events">` block in `index.html`. Each event is one
`<li class="event">`. Delete past events; they do not expire on their own.

**Team** — the `<ul class="team">` block. One `<li class="member">` per person.

**Hero photo** — `assets/banner.webp`, placed on the right of the hero copy.
It is masked with a radial gradient and desaturated so it dissolves into the
violet background rather than sitting in a hard box; `.hero-photo` and
`.hero::after` in `styles.css` control that. Below 1024px it drops behind the
text as a faint texture. To swap the image, keep the subject near the centre
of the frame — the mask fades the edges away.

**Logo** — `assets/logo.svg`, used in the header and the footer. It is a white
wordmark (`fill="currentColor"` with `color="#FFFFFF"` set on the root `<svg>`,
so it stays white when loaded through an `<img>`), which is why both places it
appears sit on a dark band. If you ever need it on a light background, you will
need a dark-ink version of the file.

## Hosting

Squarespace cannot host this. Its hosting is a closed CMS — there is no way to
upload HTML/CSS and have it served. The domain works fine though; you point its
DNS at a static host.

`missourimissamazing.org` is registered at Squarespace, uses Squarespace
nameservers (`nsc1-4.squarespacedns.com`), and currently serves a Squarespace
"under construction" placeholder. Repointing the records below replaces that
placeholder with this site.

**Google Workspace email is live on this domain** (MX → `smtp.google.com`).
Only touch the A and CNAME records described here. Do not change the
nameservers — that would take the MX, SPF, DKIM and DMARC records with them and
break mail until they are rebuilt.

### Deploying to GitHub Pages

The repo must be **public** — Pages on a custom domain requires a paid plan for
private repos. Keep credentials out of it.

1. Push this folder to a GitHub repo.
2. **Settings → Pages**, source = `main` branch, root folder.
3. `CNAME` at the repo root already contains `missourimissamazing.org`. Leave it
   — GitHub reads it to claim the domain, and deleting it unsets the domain.
4. In Squarespace **Domains → missourimissamazing.org → DNS Settings**, delete
   the four existing Squarespace A records on `@` and add:

   | Type  | Host  | Value |
   |-------|-------|-------|
   | A     | @     | 185.199.108.153 |
   | A     | @     | 185.199.109.153 |
   | A     | @     | 185.199.110.153 |
   | A     | @     | 185.199.111.153 |
   | CNAME | www   | `brian-royer.github.io` |

   Replace the existing `www` CNAME (currently `ext-sq.squarespace.com`).
   Leave every MX and TXT record alone.
5. Back in Pages, tick **Enforce HTTPS** once the certificate issues (up to an
   hour). DNS changes can take up to 24 hours to propagate.

Once it is live, the Squarespace *site* subscription is no longer doing
anything — only the domain registration matters. Cancelling the site plan is a
real saving, but check first whether your Google Workspace was bought through
Squarespace, since those billing accounts can be linked.
