# Hidden Pace — website

Static site, no build step and no cookies. A small dependency-free `i18n.js` detects the browser language, remembers an explicit choice in `localStorage`, and swaps localized home-page copy and screenshots. Style follows the game's "Alpine '72" tokens (`hiddenpace/Design/Game/GameTheme.swift`).

| Page | File | App Store Connect field |
|---|---|---|
| Home | `index.html` | Marketing URL |
| Support (FAQ) | `support.html` | Support URL (required) |
| Contact | `contact.html` | — |
| Privacy Policy | `privacy.html` | Privacy Policy URL (required) |
| Terms of Use | `terms.html` | — (licence = Apple Standard EULA, linked from the page) |
| Career Packs | `career-packs.html` | English overview of the four story-led packs |
| Ekiden (Japanese) | `ja/ekiden-career-pack.html` | Static, indexable Japanese landing page for 駅伝 searches |
| Not found | `404.html` | — |

Links between pages are relative (`privacy.html`), so the site works on any host and in a sub-path. Canonical URLs, `sitemap.xml` and `robots.txt` assume **https://hiddenpace.github.io** — change them if the site lives elsewhere.

App Store link everywhere is **`https://apps.apple.com/app/id6818750130`** — no country code (`/us/`) and no name slug, so Apple opens the visitor's own storefront and language. Don't paste the `/us/app/hidden-pace-running-career/…` share link. The download badge is drawn in HTML/CSS (`.store-badge`); it can be swapped for Apple's official artwork from the Apple Marketing Tools site, saved locally in `images/`.

Google Search Console: verified with the `google-site-verification` meta tag in `index.html` (URL-prefix property `https://hiddenpace.github.io/`). Keep the tag — removing it un-verifies the site.

`_headers` (security headers, image caching) is used by Cloudflare Pages / Netlify; `.nojekyll` keeps GitHub Pages from running Jekyll. Localized screenshots in `images/<locale>/` come from `appstore/<locale>/screenshots/`; `images/ridges.svg` contains the game's ridge layers.

## Publish

Live on **GitHub Pages**: account `hiddenpace` (hiddenpace.app@gmail.com), repo **`hiddenpace.github.io`** (public), Settings → Pages → Deploy from a branch → `main` → `/ (root)`. The repo name must be exactly `<user>.github.io`, otherwise GitHub serves it under `/<repo>/`.

To update: in that repo, Add file → Upload files → drag the changed files from `site/` (keep `images/` as a folder) → Commit. GitHub republishes in 1–3 minutes (Actions tab). Hidden files (`.nojekyll`, `_headers`) and this README aren't needed there.

Alternative later: Cloudflare Pages with a custom domain — upload `site/`, no build command; `_headers` is used there.

## After publishing

1. Open `/privacy` and `/support` in a browser — App Review checks both.
2. Put the URLs into `Tools/asc/listing.py` → `APP`:
   ```python
   "marketing_url": "https://hiddenpace.github.io",
   "support_url": "https://hiddenpace.github.io/support",
   "privacy_url": "https://hiddenpace.github.io/privacy",
   ```
3. `Tools/asc/deploy_metadata.py` (dry run) → `Tools/asc/deploy_metadata.py --apply`. The URL fields go to all 15 locales and the "Privacy Policy / Support" lines return to the descriptions.

## Keeping it true

The Privacy Policy describes what the app does today: no server, no SDKs, local saves with weekly backups, optional iCloud (private CloudKit + KVS), StoreKit, Game Center, local reminder, share sheet. If any of that changes (analytics, crash reporting, a backend), update `privacy.html`, its effective date and the App Store privacy label before shipping.
