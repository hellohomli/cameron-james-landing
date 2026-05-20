# Cameron James — Landing Page

Single-page landing site for Cameron James (R&B/hip-hop/pop artist on 100 Collective Records). Jordan's brother. Built as a Dispatch Labs portfolio piece.

## Quick facts

- **Repo:** `hellohomli/cameron-james-landing`
- **Local path:** `~/Desktop/Dispatch Labs/CameronJames-website/cameron-james-landing`
- **Hosting:** Vercel (auto-deploys on push to `main`, ~30s)
- **Domain target:** `cameronjamesmusic.com` (Namecheap — DNS not yet wired)
- **Live URL:** `cameron-james-landing-1ofg.vercel.app` (until domain points)

## Stack

- **Single file:** `index.html` with inline CSS + inline JS. No build step, no framework.
- **GSAP 3.12.5** + ScrollTrigger (via cdnjs)
- **Lenis 1.3.23** smooth scroll (via **unpkg** — see Gotchas, cdnjs URL was broken)
- **Google Fonts:** Instrument Serif (display), Figtree (body), JetBrains Mono (labels)

## Edit workflow

Jordan edits via terminal — Python heredocs for multi-line changes, `sed -i ''` for one-liners. Not in an IDE.

```bash
cd ~/Desktop/Dispatch\ Labs/CameronJames-website/cameron-james-landing
# make edits via heredoc / sed
git add -A && git commit -m "..." && git push
```

Hard refresh the browser (`Cmd+Shift+R`) after JS or HTML changes — Chrome/Safari cache the inline script aggressively.

## Design system

Dark cinematic with gold accent. All tokens defined as CSS vars on `:root`:

- `--bg: #0a0908` (near-black)
- `--text: #f5f1ea` (warm off-white)
- `--text-muted` / `--text-faint` (translucent variants)
- `--accent: #c9a961` (gold — italic `<em>` highlights, labels, hover states)
- `--line: rgba(245,241,234,0.12)` (hairline borders)
- `--serif: 'Instrument Serif'` for headlines (italic `<em>` in gold is the visual signature)
- `--mono: 'JetBrains Mono'` for labels (uppercase, wide letter-spacing)
- `--sans: 'Figtree'` for body copy

## Page sections (top → bottom)

1. **Hero** — magazine split, name left, portrait right
2. **Marquee** — rotating "On Time! · July 10 · Trippin' · Out Now"
3. **EP Announcement: "On Time!"** — cover art + live countdown to July 10, 2026 PT
4. **Featured Release: "Trippin'"** — Spotify + Apple Music CTAs
5. **Interstitial** — CJ sticker image (currently rendered in color, not grayscale)
6. **Bio** — word-by-word reveal on scroll
7. **Streaming** — Spotify + Apple Music cards
8. **Label** — 100 Collective Records callout
9. **Connect** — Instagram + YouTube
10. ~~Mailing list~~ — commented out, easy to re-enable
11. **Footer** — wordmark + IG/YT icons + booking email

## Images in project root

- `hero-portrait.jpg` — hero
- `on-time-cover.jpg` — EP cover art
- `cj-sticker.jpg` — interstitial
- Originals live in `../photos/` with messy filenames (spaces, `!`). When pulling new images in, copy with clean kebab-case names.
- All current images are 1–10MB. **TODO:** compress via TinyPNG before site gets real traffic.

## Gotchas (lessons earned, do not repeat)

**1. Lenis CDN path.** The package was renamed from `@studio-freight/lenis` to just `lenis`, and old cdnjs paths 404. Current working URL: `https://unpkg.com/lenis@1.3.23/dist/lenis.min.js`. Bumping versions: confirm on unpkg first.

**2. Lenis + GSAP sync.** Must use `gsap.ticker.add((time) => lenis.raf(time * 1000))` — not a custom requestAnimationFrame loop. Without this, ScrollTrigger doesn't get scroll updates and elements stay invisible past their trigger point.

**3. CSS `transition: all` fights GSAP opacity.** The stream cards hit this — left them stuck at low opacity. Always exclude `opacity` from `transition: all` shorthand on any element GSAP animates. Use explicit transition properties instead.

**4. Bio word-reveal regex.** The word-wrapper for the bio paragraph must handle inline `<em>` tags without splitting them. Current working regex: `/(<[^>]+>)|([^\s<]+)/g`. Don't go back to the older `(<[^>]+>|[^<\s]+)(\s+|$)` form — it eats the `<` and renders `em>` as literal text.

**5. Countdown is hardcoded.** Release date is `new Date('2026-07-10T00:00:00-07:00')` (midnight Pacific). Update — or remove the EP section entirely — when the EP drops.

## Pending TODOs

- [ ] Wire `cameronjamesmusic.com` DNS at Namecheap (A record + CNAME → Vercel)
- [ ] Compress all images (`hero-portrait.jpg` is 2MB, `on-time-cover.jpg` ~1MB, `cj-sticker.jpg` ~3MB)
- [ ] Interstitial caption still reads "In the Room" / "— Est. a decade in" — these fit the old studio shot, not the sticker. Refresh copy.
- [ ] After July 10, 2026: swap EP section from countdown → "Out Now" with streaming links
- [ ] Pre-save links for the EP (Spotify pre-save, Apple Music) — add buttons when available
- [ ] Optional: Plausible or Vercel Analytics for traffic data

## Voice / tone notes

Patient, confident, no hype. Italic gold `<em>` is the visual hook — use it sparingly on a key word per heading, not every sentence. Mono labels are short and ALL CAPS with wide tracking. Avoid emojis, exclamation marks (except in the EP title itself), and salesy language.
