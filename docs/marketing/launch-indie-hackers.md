# Launch — Indie Hackers

## Strategy

| | |
|---|---|
| **Platform** | indiehackers.com |
| **Format** | „Launch" post (sub: Show IH) + Milestone-Updates |
| **Tone** | Build-in-Public, ehrlich-vulnerable, Metriken-orientiert |
| **Goal** | Audience-building > direct conversion. IH-Audience sind andere Maker, nicht Restaurants — aber sie teilen weiter und werden Multiplicatoren |

## Main Launch Post

**Title (max 90 char):**
> Launched Tovaglia — menu software for restaurants, built solo over 12 months

Alt-titles:
- „From freelance design pain to SaaS: launched Tovaglia today"
- „Tovaglia: vanilla-stack SaaS for restaurant menus — launched"

**Body draft (~600 words, longer than PH/Reddit because IH-audience reads long-form):**

> Hey IH 👋
>
> Launching **Tovaglia** today — a SaaS that turns restaurant menus into print PDFs, QR menus, and TV boards from a single data source. Built solo over the past 12 months. Wanted to share the origin, stack, pricing, and the open questions I'm sitting on.
>
> ## Origin
>
> I'm a designer. For ~4 years I did seasonal menu updates for one gastronomy client. Each update was 2-3 hours of Photoshop column-balancing for what amounted to 5 price changes and 2 new dishes. The work was creative the first time and routine for every subsequent update. The restaurant paid 80-200€ per update for what felt to them like „you just typed in some numbers".
>
> Realized two things:
> 1. This was systematic across the industry, not unique to my client
> 2. The bottleneck isn't design — it's **layout maintenance under data churn**
>
> Built Tovaglia to remove the maintenance burden from menu design. You enter the menu as structured data, the layout engine handles column balancing, allergen legend (LMIV-compliant, 14 EU mandatory + add-ons), multi-language, and propagates to print + QR menu + TV board from the same source.
>
> ## Stack
>
> - **Frontend:** Vanilla HTML/JS/CSS, no framework, no build step
> - **Backend:** Supabase EU (Postgres + Auth + RLS + Realtime + Storage)
> - **Payments:** Stripe via Cloudflare Worker
> - **AI photo import (Pro tier):** Gemini 2.5 Flash Lite Vision
> - **Deploy:** Cloudflare Pages, push-to-main auto-deploys
> - **CSS:** ~5k LOC across 8 files; design tokens system
> - **JS:** Custom layout engine (~3k LOC) doing block measurement, scale-fit, column balance, validator
>
> No framework was a deliberate call — 12 months ago I started with Next.js and re-wrote to vanilla after realizing the layout-engine logic was the product, and the framework was adding ceremony without value. The bundle is ~120 KB total.
>
> ## Pricing
>
> - **Free:** 1 menu, watermarked print export
> - **Pro:** 14.90€/mo or 134€/yr — 100 menus, unlimited downloads, no watermark, AI photo import (30/mo), custom accent colors
> - **Business:** 19.90€/mo or 179€/yr — Pro + multi-location dashboard, table/order management (Plan-D feature still in pre-release)
>
> All EU-VAT-handled.
>
> ## What I shipped this week (4-day Tovaglia-mini-launch sprint)
>
> - HTML embed feature (iframe-embed of menu in restaurant's own website) — full v1→v3 with print-layout rendering, pagination, hotfixes for cross-domain CSP, modal preview, etc.
> - Mobile produktverwaltung — full mobile editor with drill-down navigation (stack/cats/items/detail)
> - Plan-gate fixes from the April plan-rethink (TV-board access for Pro, not Business-only)
> - 404 page + FAQ route + sitemap updates for SEO surface
> - 12 PRs across two repos in 5 days
>
> ## Open questions I'm sitting on
>
> 1. **Distribution.** Pure SEO + niche-Reddit + this IH post. No paid ads. Will share organic conversion metrics monthly here.
> 2. **English-UI timing.** German-first is genuine market focus (LMIV is EU). English admin UI: priority 2 vs. priority 5? Audience-feedback welcome.
> 3. **AI-quota.** 30/mo on Pro feels generous. Should it be lower (margin-protection) or higher (perceived-value)?
> 4. **Niche-vs-general.** Restaurant-specific. Could go wider (hairdresser-price-lists, gym-class-schedules — same layout-engine). Worth it or focus?
>
> Will share monthly metrics here. Building in public for the next 12 months.
>
> Live at https://tovaglia.io
>
> — [USER-NAME]

## Follow-up — Milestone Posts (monthly cadence)

### Month 1 post-launch (suggested template)

**Title:** „Month 1 metrics: Tovaglia"

**Body skeleton:**
- Sign-ups: X (free) + Y (paid)
- Conversion-rate Free→Paid: Z%
- MRR: € … (or „not yet meaningful — Day 30 too early")
- Top traffic source: …
- Biggest learning: …
- One thing I'd undo: …

### Month 3 post-launch
- Same template + 3-month cohort retention
- Update on the open questions (English-UI, AI-quota)

### Month 6 post-launch
- Decision-point question to community: niche-vs-general from launch-post

## Voice-Notes

- IH audience = peers, not buyers. Talk like talking to another founder.
- Be transparent on stack, pricing, mistakes
- Don't over-claim wins. „Will share monthly" + actually do it = compound trust
- Embrace „I don't know yet" — IH culture rewards honesty over polish

## Cross-promotion logic (IH ↔ Pillar Articles)

- Pillar 1 (Founder-Story) → IH-Launch-Post links it as „longer version of the origin"
- IH-Launch-Post → Pillar 1 cross-link drives long-tail traffic
- Month-1-post: link Pillar 2 (Word-vs-Tovaglia) as „here's what I built around"
- Month-3-post: link Pillar 3 (AI-Foto) when AI-quota data is interesting

## Pre-publish checklist

- [ ] Spell-check, especially company names (Vectron, Lightspeed, orderbird, Stripe, Cloudflare, Supabase)
- [ ] Metrics block accurate, no inflation
- [ ] Link to tovaglia.io works
- [ ] Image (1200×630 OG) attached
- [ ] „Show IH" sub selected
- [ ] Cross-post to Twitter/Mastodon with shortened tldr
