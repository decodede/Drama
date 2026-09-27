# Absolutely Free Short-Drama Streaming Sites

An index of short-drama streaming sites that are **100% free: no coins, no payment, no VIP tier,
no subscription, and no locked episodes.**

Every site listed here was checked by fetching the **final episode** of a series and confirming it
renders a working player, then scanning the served HTML for any lock, currency, or login
mechanism. All checks performed **2026-09-27**.

> **Why this list is so short.** The short-drama industry is almost entirely freemium: the official
> apps (DramaBox, ReelShort, NetShort, GoodShort, DramaWave, Sereal, KalosTV, MoboReels,
> FlickReels, MicroDrama) all gate later episodes behind coins or a weekly VIP. See
> [Looks free, isn't](#appendix--looks-free-isnt--verified-paywalled) before you waste a weekend.

Machine-readable: [`free-sites.json`](./free-sites.json) | Method: [`SOURCES.md`](./SOURCES.md)

---

## The 9 verified sites

### 1. `dracinkita.com` — strongest of the set

Indonesian multi-source aggregator (rows labelled dramabox / goodshort / netshort / mydramawave).

- Episode routes are **materialised per episode**: `/dramabox/play/{id}/{n}` — so the last episode
  is a real server-rendered page, not a client-side shell.
- Verified on `EP 58` of 58: correct title, correct canonical URL, full 58-item episode grid,
  and a preconnect to the actual video CDN `hwztakavideoto.dramaboxdb.com`.
- Astro SSR, so the player page is genuinely server-rendered. **0** lock / login / premium flags.
- No account, no currency, no lock state.

### 2. `nunodrama.my.id` — cleanest JSON API

Indonesian. Self-describes as *"gratis tanpa iklan"* (free, no ads).

- Verified **27 of 27** episodes as plain links `/watch/nunomix/429457?ep=1…27`; ep 15 and ep 27
  both return HTTP 200 with distinct proxied sources resolving to real MP4.
- **Zero** occurrences sitewide of `koin` / `kredit` / `vip` / `premium` / `terkunci` /
  `tonton penuh` / `login`.
- The only subscription hit is *"Langganan API Join Grup Telegram"* (Telegram group, unrelated).
- **Caveat:** the `dramabox` platform currently returns **502** (broken upstream). `nunomix` works.
- API: `/api/<platform>/search?q=`, `/api/<platform>/home`, `/api/<platform>/detail?id=`

### 3. `kisskh.is` — no auth at all, JSON-native

*"Asian Dramas & Movies"*. Served from `kisskh.la` / `load.kisskh.co`.

- `/api/DramaList/Drama/10903` (*Against The Current*, 36 episodes) returns **all 36** episode
  objects with the `is_locked` / `is_vip` / `price` fields **absent entirely** — the schema has no
  concept of a gate.
- `/api/DramaList/Show`, `/LastUpdate`, `/Search` all return 200.
- **0** HTML hits for `terkunci` / `lock` / `vip` / `unlock` / `coin` / `koin`. No auth, no wallet.
- Mirrors: `kisskh.space`, `kisskh.nl`, `kisskh.do`, `kisskh9.se`
- Firebase / Cloudflare Turnstile present **for analytics only**.
- Note: this is long-form Asian drama + films, not vertical-only.

### 4. `dracinema.com`

Indonesian short-drama site, China-origin catalogue, Sub Indo.

- Verified on the **final episode 60 of 60**: renders a full player plus the complete 1–60 selector.
- Grep of the episode HTML for `coin|vip|premium|login|paywall` returned **only two false
  positives**, both ordinary Indonesian words — `Daftar` ("list") and `Koin Raja Obat` (a token in
  the story's plot summary).
- No account system, no currency, no lock state in the document.

### 5. `sandiwara.net`

Indonesian + Khmer-sub drama aggregator, Next.js.

- Requested `?ep=105` on a 105-episode title and got a real episode-105 page — **no redirect** to a
  signup or paywall route.
- Its own copy: *"baya ditonton sepenuhnya gratis … tanpa biaya berlangganan dan tanpa perlu
  mendaftar akun"* (fully free, no subscription fee, no account needed).
- **0** coin / vip / unlock markers in the payload.

### 6. `dramafren.org` — the only clean yes in the SEO cluster

WordPress index hub, ~18 player subsites (`dramabox.`, `dramawave.`, `goodshort.`, `netshort.`,
`flickreels.`, `shortmax.`, `starshort.`, `idrama.`, `flextv.`, `kalostv.`, `microdrama.`,
`moboreels.`, `dramabite.`, `reelshort.`, `viglo.`, `tvseries.`).

- No paywall markers on the homepage, series pages, or watch pages.
- Each series collapses the whole drama into **one** entry labelled `EP 1 / FULL EPISODES`.
- Footer: *"here you can watch movies online in high quality for free without annoying of
  advertising."*
- Players used: `VidHide`, `Upnshare`, `Abyss` — third-party ad-supported embeds.
- `Bookmark` / "Followed by N members" is an optional WordPress user feature, not required to watch.

> **Trust warnings, stated plainly.** Gridinsoft scores `dramafren.org` **1/100** and
> `shortmax.dramafren.org` **2/100**, both categorised "Scam Website", citing 3 blacklist hits.
> The site publishes a DMCA policy — evidence of *contested* status, not legitimacy. There is no
> rights holder behind this content. Treat it as untrusted: use an adblocker, and do not submit
> personal or financial information to it.

### 7. `shorttv.live` — ShortMax's official web player, web-only free

- Verified end-to-end: a 69-episode title lists **all 69** episodes with **zero** lock icons;
  EP 1 and EP 40 both load directly. **0** coin/VIP strings across 4 pages fetched.

> **Important:** this is **web-only**. The Android app for the same brand (SHORTMAX LIMITED, whose
> declared website *is* shorttv.live) is coin-gated — its Play listing says *"Ads help unlock
> episodes for free, and you can also complete daily tasks in Rewards to earn free coins."* The
> web player has no coin layer; the app does.

### 8. `myvurt.com` — Vurt

- **No coin system found anywhere.** Site FAQ: *"all of it is completely free"* and *"All content
  is free."*
- Only gate is a **free** account: 3 episodes anonymous, then unlimited with a free sign-up.
- Player chip reads `Beneath the Surface · S1·E14 … Romance · Free`. No price/coin/VIP string on any page.
- India/SEA focused, licensed platform (own API + Firebase + S3/CloudFront).
- Note: the player is JS-rendered, so this was verified from the site's own page text
  (home / login / install / download) rather than by watching playback.

### 9. `layarkeren.digital` — largest catalogue of the free set

WordPress / Muvipro, Indonesian subs and dubs.

- `/the-masked-lover/` shows `EPISODE: 1 2 3 4 5 6 7 8` as plain pagination to `/tv/…/N/`.
- Verified ep 2 and ep 8 — **distinct live AbyssPlayer iframes, no lock**.
- **0** `koin` / `kredit` / `premium` / `terkunci` / `buka` / `tonton penuh` / `langganan` strings.
  (`vip` hits are CSS selectors only.)
- Server-side WordPress, no JS wall. Ships free download mirrors alongside embeds.

---

## Appendix — "looks free, isn't" (verified paywalled)

Kept because every one of these *advertises* free access. Their own API responses contradict the
marketing.

| Domain | Advertises | Actually |
|---|---|---|
| `dracinku.site` | "Gratis" in `<title>` | `site_name: "VIP DramaWave"`, `free_episode_limit: 20`. Own banner: **"UNTUK NON VIP USER SELURUH SERIES HANYA TERUNLOCK 10% DARI TOTAL EPISODE"**. Tier packages Rp 5,000–300,000 |
| `loklok.my.id` | "Gratis" in `<title>` | `free_episode_limit: 3`. Bundle i18n: *"For non VIP users, only 10% of episodes are available"*. 3-day trial, packages to Rp 165,000/yr |
| `m.snssb.com` | — (genuine Loklok H5) | Real rights holder, but: *"Failed to play, lack of Golden Coins"*, *"Top up and redeem to unlock"*, per-episode coin-purchase endpoints |
| `hoshiyomi.my.id` | Freemium | **"Episodes 1–5 free • rest is VIP"** — 63 of 68 episodes redirect to the VIP route. Rp 18,000 / 30 days |
| `narto-drama.com` · `edge.narto-drama.com` · `narto.in` · `nartodrama.com` | *"vast majority free"*, *"No subscription yet"* | Paid **Credits top-up (incl. USDT)** "to unlock premium content", plus a refund policy. Ad-gate endpoints `/player/ads/gate-unlock`. Also **incomplete ingestion** — 29 of 49 episodes return `is_playable: false` with a blank player |
| `chartdrama.com` (FindDrama) | ~20,000 free titles | Ships a real VIP/member tier plus a registration flow, `PREMIUM EPISODES`, `VIP member catalog`, `anti-devtools.js`. The episode API returned 61/61 unlocked on the title sampled — free *in practice*, not free *by design* |
| `teamdl.my.id` | Free Sub Indo | Short-drama rows are **sequentially locked**: `highestUnlockedEp = isShortDrama ? 1 : 999999`, and a "Terkunci" title attribute. Long-form (KissKH) rows are unlocked |
| `drakorid.co` | Browse free | An optional Premium tier exists, with per-episode crown gating CSS (`.episode-pick-num.is-premium`). Currently **dormant** — watching needs no login today, but the gate is built in |
| `dramadizilerim.com` | — | Real `/premium` tier plus `playback-guard.css` and an anti-adblock lock modal |
| `shorts-drama.com` | "Free forever" badge | Its own FAQ: *"two ways to unlock episodes: coins or a subscription… two subscription tiers unlock every story."* Legit French app (Luni/Bordeaux) — but coin-gated |
| `microdrama.id` | "Browser-only, no install" | Browser-only is true, but it is **freemium**: nav has `VIP`, `Unlimited Drama VIP`, `Bonus 80 Koin Harian`, `Tanpa Iklan` |
| `goodshort.com` | "Free to Watch" | Header `TOP UP`; EP7–EP11 carry a lock badge. 6 of 52 free |
| `netshort.com` | — | EP6–EP30 carry lock icons. 5 of 30 free |
| `dramaboxdb.com` | — | Official listing: unlock via free coins from tasks or ads, subscription recommended otherwise |
| `dramawave.tv` · `sereal.com` · `flextv.cc` · `moboreels.com` · `flickreels.net` · `kalostv.com` · `reelshort.com` | — | All coin/VIP freemium. App Store / Play listings quote explicit coin packs and weekly VIP tiers (e.g. Sereal: coins pack USD 9.99, VIP weekly USD 17.99) |

**Not short-drama at all — listed in scraper registries by mistake:**

- `playbox.com` — the page title is *"Playbox - NSFW AI Video Generator"*. No drama catalogue.
- `vigloo.com` — a French free-to-air **live TV network**.
- `drakor21.com` — an **online gambling operation**. Nav is `Slot Online / Live Casino / Sportsbook /
  Arcade / Togel Online / Poker`; catalogue is `Gates of Olympus 1000`, `Starlight Princess 1000`.
  Keyword counts: `nonton` 0, `film` 0, `serial` 0.

## Avoid (hostile)

| Domain | Why |
|---|---|
| `dramaboxdb.org` | Typosquat of the legitimate `.com` |
| `dramaboxplayer.com` | Reproduces DramaBox's official catalog copy **verbatim**; contact address on a non-StoryMatrix domain = phishing pattern. Do not submit personal or financial information |
| `netshortfree.com` · `rfree.reelsfree.com` · `ndrama.nedrama.com` | SEO doorway pages; every CTA points off-site. No player, no catalogue |
| `anichin.club` | Domain-rotation landing page only; lists itself under "old domains", links to a VIP banner |
| `oppa.biz` → bare IP `212.86.121.175` | Served off a **bare IP** with no TLS hostname |

**Verified clone tell:** DramaBox's official blurbs (*Deny Me, Dragon King*, *Tempest: The Last
Mecha*, *Step Back! I'm the Hidden King*) appear verbatim on `dramaboxplayer.com` and
`dramaboxdb.org`. The legitimate `dramaboxdb.com` footer reads
`(c) DramaBox, All Rights Reserved. STORYMATRIX PTE. LTD.`

---

## Honest limitation

On all nine sites the video URL is fetched **client-side at play time**. Verification therefore
established the *absence of any lock mechanism on the final-episode page*, not decoded video bytes.
That is the strongest check available without a headless browser — and for these sites the gate
would have to live in the page markup, where there is none.

`shortflix.net` is the one promising lead left unresolved: 15-source library, live sitemap
(`lastmod 2026-09-27`), 20 locales, and **0** coin/vip/signup strings — but it is fully
client-rendered, its sitemap has only 9 URLs with no title slugs, and no watch page was reachable.
Needs a headless browser before it can be called free.
