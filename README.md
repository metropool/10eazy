# 10eazy – Metropool dB Meting

Static website that gives Metropool staff a single entry point to the live decibel (dB)
measurement viewers for each stage/zaal across their venues in Hengelo, Enschede and
Almelo. It's plain HTML/CSS (Bootstrap) hosted via GitHub Pages, with no build step and
no backend of its own — each page just embeds an `<iframe>` pointing at a dB meter
device's built-in web viewer on the local network.

Live at: https://10eazy.metropool.nl (see [CNAME](CNAME))

## How it works

- [`index.html`](index.html) — landing page. Shows a "jumbotron" banner per stage with a
  "Start hier" button that links to that stage's page under [`pages/`](pages).
- [`pages/*.html`](pages) — one page per stage (or per combined view). Each embeds an
  `<iframe src="http://<device-ip>/10eazy_webviewer.html">` that loads the live
  measurement UI served directly by the dB meter hardware on the venue's local network.
- Combined pages (`beiden-hengelo.html`, `beiden-enschede.html`) stack two iframes so two
  stages can be watched at once.
- [`css/css.css`](css/css.css) — shared styling (fonts, jumbotron/button colors, iframe
  sizing). [`css/variables.scss`](css/variables.scss) holds Bootstrap variable overrides.
- [`bootstrap/`](bootstrap) — vendored Bootstrap CSS/JS. [`fonts/`](fonts) — vendored
  webfonts (BigNoodle, Circular). [`images/`](images) — logos and jumbotron background
  photos per venue.

Because the iframe sources are local IP addresses on Metropool's own network, the dB
viewers only load when the page is opened from a device on that network (or via VPN);
they will not load over the public internet.

## Pages and their device IPs

| Page | Stage / venue | Viewer IP |
|---|---|---|
| [`pages/jupiler.html`](pages/jupiler.html) | Bud Stage \| Hengelo | `192.168.1.195` |
| [`pages/jackdaniel.html`](pages/jackdaniel.html) | Jack Daniel's Stage \| Hengelo | `192.168.1.196` |
| [`pages/beiden-hengelo.html`](pages/beiden-hengelo.html) | Bud Stage + Jack Daniel's Stage (combined) | `192.168.1.195` + `192.168.1.196` |
| [`pages/hertogjanzaal.html`](pages/hertogjanzaal.html) | Leffe Stage \| Enschede | `172.16.62.201` |
| [`pages/saxionzaal.html`](pages/saxionzaal.html) | Saxion Stage \| Enschede | `172.16.62.202` |
| [`pages/beiden-enschede.html`](pages/beiden-enschede.html) | Leffe Stage + Saxion Stage (combined) | `172.16.62.201` + `172.16.62.202` |
| [`pages/almelo.html`](pages/almelo.html) | Main Stage \| Almelo | `192.168.50.16` |

## Adding a new stage/venue

1. Copy an existing single-stage page (e.g. [`pages/jupiler.html`](pages/jupiler.html)).
2. Update the `<title>`, `<h1>`, and the iframe's `src` to the new device's IP.
3. Add a background image to [`images/`](images) if needed.
4. Add a new jumbotron block in [`index.html`](index.html) linking to the new page.

## Deployment

The site is served as-is by GitHub Pages (see [`CNAME`](CNAME) for the custom domain,
[`robots.txt`](robots.txt) for crawler rules). Pushing to the default branch is enough —
there is no build/compile step.
