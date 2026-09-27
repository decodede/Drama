# Methodology & Provenance

How this index was built, so you can re-run it and disagree with it.

## Date

All checks: **2026-09-27**. Domains move; re-verify before relying on this.

## Pipeline

1. Cloned `Lebo-20/APKS-SHORT-DRAMA-LOVER` and read `app/provider/__init__.py` to establish the
   provider registry — 65 IDs, 55 distinct sites, of which only 22 modules are real
   implementations and 37 are ~220-byte stubs inheriting one generic scraper.
2. **Fan-out discovery.** Ten research agents in two waves:
   - Wave 1 (broad): official app domains, the Indonesian/DramaFren network, the Narto
     aggregator, GitHub code search for scraper implementations, the "FindDrama"/APK-front-end
     cluster, and an independent 19-domain liveness sweep with two control domains
     (`netflix.com` expected live, a NXDOMAIN control expected dead — both behaved as predicted).
   - Wave 2 (paywall audit): four agents, each assigned a disjoint site group, each asked the
     same question — *can you watch a complete series with no coins, no payment, no VIP, no login,
     and no locked episodes?*
3. Only sites that survived wave 2 with on-site evidence are in `free-sites.json` → `free`.

## The evidence bar for "free"

A site is `FULLY FREE` only if **all** of these hold:

- The **final episode** of a series was fetched and rendered a player (not just the landing page).
- The served HTML/JSON was scanned for lock, currency, and login mechanisms
  (`coin`, `koin`, `kredit`, `vip`, `unlock`, `terkunci`, `premium`, `paywall`, `login`, `top up`).
- A marketing claim alone is **never** sufficient. Every "gratis" / "Free to Watch" /
  "100% free" site in this ecosystem was checked against its own API response.
- If the site was JS-walled, geo-redirected, or Cloudflare-challenged such that the check could not
  be completed, the verdict is `UNVERIFIABLE` — not `FULLY FREE`.

### Verdicts used

| Verdict | Meaning |
|---|---|
| `FULLY FREE` | Whole series watchable, no paywall mechanism found |
| `FREE WITH CAVEAT` | Mostly free, but a paid tier exists or content is incomplete |
| `FREEMIUM` | Free episodes exist; later episodes cost coins or VIP |
| `PAYWALLED` | Cannot complete a series without paying |
| `UNVERIFIABLE` | Could not determine — JS wall, challenge, 502, or no web catalogue |

## Coverage of the false-positive problem

The single most valuable output of this exercise is that **10 sites which advertise themselves as
free are not**. Two clusters account for most of it:

**The D5STUDIO white-label cluster** (`dracinku.site`, `loklok.my.id`) — these run an identical API
stack. Their own `/api/settings/public` returns a `free_episode_limit` field and their own banners
state the cap. `dracinku.site` self-identifies as `site_name: "VIP DramaWave"` while its `<title>`
says *"Gratis"*. The free shares are 20 episodes and 3 episodes respectively.

**The monetised SEO cluster** (`dramafren.org`, `chartdrama.com`, `hoshiyomi.my.id`) — these
publish DMCA policies and are scored 1–2/100 by Gridinsoft, yet genuinely carry no paywall on the
titles sampled. They monetise entirely through ad injection. `hoshiyomi.my.id` is the outlier that
does gate: 5 free episodes of 68.

## Known limitations

1. **Video bytes were never decoded.** On all nine free sites the media URL is resolved
   client-side at play time. Verification established the *absence of a lock mechanism in the page
   markup* — the strongest check possible without a headless browser. A gate implemented purely in
   the player JS would be invisible to this method.
2. **Single egress point.** All fetches came from one network. Geo-redirects are demonstrably
   active (`netflix.com` → `/in/`, `swipedrama.com` → `/ja/`), so results may differ by region.
3. **Ads were identified from HTML source, never executed.** No player was run, so actual ad
   behaviour and any drive-by payloads are unverified.
4. **A 403 behind Cloudflare means live, not dead.** "Just a moment…" is the origin working as
   intended. A `521/523` means the origin is down. A `NXDOMAIN` means the domain is gone.
5. **Tier 1 platform verdicts lean on first-party app listings.** For five JS-walled official sites
   (dramawave, moboreels, flickreels, kalostv, sereal) the coin/VIP evidence came from the
   operator's own App Store / Play listing rather than third-party reviews, because no lock marker
   is exposed in the served HTML.
6. **Content legality is not uniform.** Sites 1–6 and 9 are third-party front-ends with no rights
   behind them. Sites 7 and 8 are the operators' own properties. This index records *what is free*,
   not *what is licensed* — the distinction is called out per-entry in the README.

## Reproducing a single check

```bash
# Is a site's final episode reachable with no gate?
curl -sL "https://<domain>/<series-slug>" \
  | grep -ioE 'coin|koin|kredit|vip|unlock|terkunci|premium|paywall|top up|login' \
  | sort -u
# empty output == no paywall markers in the served markup
```

For JSON APIs, check whether the episode schema even *has* a lock field — `kisskh.is` is free
precisely because `is_locked` / `is_vip` / `price` are absent from the response schema rather than
merely set to `false`.

## Related repositories

Open-source scrapers that document the underlying protocol shapes:

| Repo | Stars | Notes |
|---|---|---|
| `Sansekai/DramaBox-API` | 153 | `sapi.dramaboxdb.com` incl. HLS extraction |
| `Sansekai/SekaiDrama` | 72 | Next.js front-end over 10 providers |
| `giienew/dramabox-scraper` | 30 | DramaBox internals → `sapi.dramaboxvideo.com` |
| `NingRong2/anichin-api` | 20 | 15-source REST/WS aggregator, uniform schema |
| `AmmarrBN/reelshort-api` | 16 | Single-file ReelShort scraper |
| `ElevenZhou/resource-download` | 1 | Deepest protocol documentation in the set |

Note that all of these target the **coin-gated** official APIs from the paywalled list. None of
them is relevant to the nine free sites above, which are all reachable with a plain GET.
