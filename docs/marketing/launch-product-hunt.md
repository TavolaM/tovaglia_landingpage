# Launch — Product Hunt

## Strategy

| | |
|---|---|
| **Platform** | Product Hunt (producthunt.com) |
| **Format** | Listing + Maker-comments + 24h-Hustle |
| **Hunter** | [USER-DECISION: Self-hunt vs. ask known hunter? Self-hunt OK in 2026, kein PageRank-Boost mehr von „great hunters"] |
| **Launch day** | Dienstag oder Mittwoch (Wochenende-Traffic schlecht für B2B-Tools), 0:01 PST = 09:01 MEZ |
| **Goal** | Top 5 of Day → max ~200 sign-ups; realistic Top 20 → ~30-60 sign-ups |

## Listing

**Name:** Tovaglia

**Tagline (max 60 char):**
> Speisekarten-Software, die das Layout für dich macht

Alternative-tagline-candidates:
- „Restaurant menu software, no design skills needed"
- „You enter the data. Tovaglia handles the layout."
- „Print, QR, and TV menu — from one source"

[USER-DECISION: deutsche oder englische Tagline? PH ist EN-default. Englisch erreicht breitere Audience, aber Tovaglia ist DACH-fokussiert. **Empfehlung:** Englisch für Tagline, Body kann bilingual ankern.]

**Description (max 260 char):**
> Tired of remaking your menu every season? Tovaglia automates the layout. Type your dishes, pick a template, get print-ready PDFs, a QR menu, and a TV board — all from one source. LMIV-compliant allergens, multi-language, no design skills.

**Topics (PH categories — pick 3):**
- Food & Drink
- SaaS
- Design Tools

**Gallery (5 images):**
1. Hero — split-screen vorher (chaotische Word-Karte) / nachher (sauber Tovaglia-Print)
2. Editor-Screenshot — Datenstruktur links, Live-Preview rechts
3. Templates-Übersicht — 6 Templates side-by-side
4. QR-Menü auf Smartphone — Allergen-Filter aktiv
5. TV-Board Screenshot mit Live-Update-Demo

**First-comment (Maker, posted within first 5min):**
> Hey Product Hunt 👋
>
> Tovaglia ist aus persönlicher Frust entstanden. Ich bin Designer und habe Jahre lang Speisekarten für meinen Stammkunden gestaltet — jede saisonale Preisänderung war 2-3 Stunden Photoshop-Tetris. Layer entschlüsseln, Spalten neu ausgleichen, Allergen-Buchstaben verschieben, exportieren, Druckerei. **Für 5 neue Preise.**
>
> Tovaglia trennt Daten vom Layout. Du tippst die Karte als Tabelle, die Layout-Engine baut zusammen, balanciert Spalten, generiert die LMIV-Legende — und propagiert die selbe Quelle in PDF, QR-Menü und TV-Board.
>
> Was es ist:
> - Print-PDFs druckfertig (A4/A5/Bifold/Trifold mit Bleed)
> - QR-Menü mit Auto-Übersetzung + Allergen-Filter
> - TV-Board für Monitor-Anzeigen
> - AI-Foto-Import (Pro): alte Karte fotografieren, struktururiert importiert
>
> Was es nicht ist: Kassen-Integration, Reservierungssystem, Liefer-Plattform. Wir machen einen Job.
>
> Free hat 1 Karte mit Watermark-Druck. Pro ist 14,90€/Monat. Keine Kreditkarte für Free.
>
> Würde mich über euer Feedback freuen — besonders wenn jemand selber Restaurants führt oder dafür baut. AMA in den Comments.
>
> — [USER-NAME]

**Bullet-list-update (10 PH-formatted bullets):**

```
🎯 You enter the data. Tovaglia handles the layout.

📋 What's in the box
• Auto-balanced print PDFs (A4, A5, Bifold, Trifold)
• QR menu with auto-translation (DE/EN/IT/FR/+)
• TV menu board for monitor displays
• AI photo import — snap your old menu, get a structured import
• LMIV-compliant allergen handling (14 EU allergens + add-ons)
• 6 templates × dark/light × 4 accent colors
• Custom hex colors (Pro)
• Multi-language with browser-based auto-switching
• Free tier: 1 menu, watermarked print
• Pro: 14.90€/mo, 100 menus, unlimited downloads, no watermark
```

## Pre-launch (T-7 to T-1)

- T-7: Tweet/Mastodon „Coming next Tuesday on PH" + screenshot
- T-3: DM 5-10 friends/community-members, ask for genuine upvote-on-launch (not buy-upvotes — just „check it out + thoughts")
- T-1: PH listing scheduled draft + assets uploaded
- T-1 evening: Set 9 alarms for next morning (0:01 PST = 09:01 MEZ)

## Launch day (T-0)

- 09:01 MEZ: Listing goes live
- 09:05: First-Maker-Comment posted
- 09:10: DM the 5-10 prepped friends
- 09:30: Post on Twitter „We're live on PH" + link
- 10:00-22:00: Reply within 30 min to EVERY comment. Tone: thankful, technical, anti-hype.
- 16:00: Post in 2-3 relevant Reddit subs (links to Reddit-drafts)
- 18:00: Post on Indie Hackers (link to IH-draft)
- 23:00: Final tweet thanking everyone for the day

## Comment-prep — anticipated questions

**Q: How does this compare to Canva?**
A: Canva is a great general design tool — you draw what you want and move it around. Tovaglia is a *menu-data* tool — you enter what's on the menu, and the layout-engine balances columns, handles widows/orphans, and generates the LMIV allergen legend automatically. Different problem class.

**Q: Why DE-only? When EN?**
A: LMIV is EU-specific, designed for DACH (DE/AT/CH) market first. UI is German-default; QR menu auto-translates to EN/IT/FR/+ for guests. Full English admin-UI is on the roadmap but not the launch priority.

**Q: What's the AI model?**
A: Gemini 2.5 Flash Lite Vision. Open about the stack — no proprietary „secret sauce" claims.

**Q: What happens to my data if I cancel?**
A: PDF export anytime, 30 days read-only after cancellation, then DSGVO-compliant deletion. No lock-in.

**Q: Is there a discount for first 100 users?**
A: [USER-DECISION: launch-discount oder nicht? Spar lieber für Black Friday. Aber ein „Launch Friends"-Promo (~25% off first 3 months) signalisiert Wertschätzung. **Empfehlung:** ja, 20% Code „PHLAUNCH"]

**Q: Is this open source?**
A: No. Vanilla HTML/JS/CSS stack runs on Cloudflare + Supabase. We may open-source parts (e.g. the layout engine) later, but not the full platform.

**Q: Hosting/uptime?**
A: Cloudflare Pages (frontend) + Supabase EU (Postgres + Auth). 99.9% uptime track record, no outages since [USER-DATA: launch date].

## Post-launch follow-up (T+1 to T+7)

- T+1: Thank-you tweet with final placement (Top X of day)
- T+3: PH-update-post (if Top 5: triumph-tone, if Top 20: gratitude-tone, if Top 50+: lessons-learned-tone — be authentic)
- T+7: Blog post „What I learned launching on PH" (becomes IH-share-fodder)

## Voice-Notes

- Anti-hype. „We built X" not „Revolutionary Y"
- Founder-first-person, German-or-English depending on platform-norm
- Reply-to-every-comment is the actual win (not the upvotes)
- Don't beg for upvotes — ever. Reddit-detector triggers.
