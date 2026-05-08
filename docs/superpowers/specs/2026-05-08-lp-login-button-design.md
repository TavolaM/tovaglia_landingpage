# LP Login-Button für Members — Design

**Date:** 2026-05-08
**Repo:** Landingpage_repo (tovaglia.io)
**Status:** Approved (User-Sign-Off 2026-05-08)
**Touched files:** `public/index.html` only

## Problem

Existierende Tovaglia-Mitglieder, die auf `tovaglia.io` landen, haben heute keinen sichtbaren Login-Pfad. Die Top-Nav enthält drei In-Page-Anchors (`So geht's`, `Vorher · Nachher`, `Preise`) und einen Primary-CTA „Kostenlos starten →" zum `#cta`-Anchor — aber keinen Login-Link. Members müssen entweder `app.tovaglia.io/login.html` direkt eintippen oder über die Pricing-Cards den Umweg über `?checkout=…` gehen.

## Goal

Ein gut sichtbarer, aber visuell zurückhaltender Login-Link in der Top-Nav, der Members direkt auf `app.tovaglia.io/login.html` führt. Conversion-Hierarchie der Free-Free-Trial-CTAs darf nicht geschwächt werden.

## Scope

### In Scope
- Neuer Link in der Desktop-Top-Nav (zwischen „Preise" und Primary-CTA)
- Mobile-Sichtbarkeit für den neuen Link (im Gegensatz zu allen anderen Nav-Items, die mobile versteckt sind)
- Konsistentes `data-cta`-Attribut für zukünftige Analytics-Anbindung

### Out of Scope (bewusst deferred)
- Auth-State-Awareness (Cookie-Read/Postmessage zwischen Subdomains zur Anzeige von „Zum Editor →" für eingeloggte User)
- Hamburger-Menü oder Off-Canvas-Drawer für Mobile
- Echtes Analytics-Tracking (DSGVO-Cookie-Banner, Tool-Wahl)
- Änderungen am Footer-Final-CTA Z.4482
- Änderungen an `app.tovaglia.io/login.html` (App-Repo)

## Decisions

### D1 — Single-State-Link statt Auth-State-Aware
Der Button hat genau ein Verhalten: Klick → `https://app.tovaglia.io/login.html`. login.html besitzt bereits Auto-Redirect-Logic für eingeloggte User; ein zweistufiger Button wäre Mehraufwand bei kleinem Mehrwert und blockiert anderen Pre-Launch-Sprint.

### D2 — Label „Bereits Mitglied?"
Frage-Stil, 14 Zeichen. Vermeidet die Sign-Up/Sign-In-Doppeldeutigkeit von „Anmelden" und schließt Erst-Besucher elegant aus, ohne sie zu verschrecken. Passt zum dialogischen Tonfall der LP („Kennst du das?", „Halb elf.").

### D3 — Position Desktop: Nav-Right, vor dem Primary-CTA
Reihenfolge: `Logo  |  So geht's · Vorher · Nachher · Preise · Bereits Mitglied? · [Kostenlos starten →]`. Primary-CTA bleibt rechtsbündig am stärksten Conversion-Slot. Login sitzt zwischen In-Page-Anchors und Primary-Action, was die übliche SaaS-Hierarchie spiegelt.

### D4 — Visual: Plain Text-Link im Stil von `.nav-a`
13px, color `var(--stone)`, hover → `var(--ink)`. Keine Border, kein eigenes Padding, kein Icon. Begründung: visuelles Sekundär-Gewicht, kein Konflikt mit dem Wein-Button daneben, automatische Theme-Kompatibilität (`data-palette="notte"` re-uses dieselben Tokens).

### D5 — Eigene Klasse `.nav-login` (nicht `.nav-a`)
Die existierende `.nav-a` wird mobile via `@media (max-width: 900px) { .nav-a { display: none } }` versteckt. Damit der Login-Link mobil sichtbar bleibt, braucht er eine eigene Klasse — der Stil ist visuell identisch zu `.nav-a` auf Desktop, aber das Mobile-Hide-Override greift nicht.

### D6 — Mobile: Login als einziger Off-Page-Nav-Link sichtbar
Mobile-Nav heute: nur Logo (alle anderen Items hidden). Login bleibt sichtbar via eigene `.nav-login`-Klasse. Schriftgröße auf 14px hochgesetzt, Padding so gewählt dass Touch-Target ≥44px Höhe erreicht. Sticky-Bottom-CTA bleibt unverändert.

### D7 — Tracking: `data-cta="member-login"`, kein neues JS
Konsistent mit bestehenden `data-cta="hero"` und `data-cta="final"` Attributen. Kein Listener heute, aber zukunftssicher falls Analytics nachgerüstet wird.

### D8 — A11y: `aria-label="Als Mitglied einloggen"`
Sichtbarer Text bleibt „Bereits Mitglied?" (D2). Screen-Reader-Nutzer, die mit Link-Listen navigieren, hören aber das `aria-label` und bekommen damit eine eindeutig handlungsorientierte Beschreibung statt einer Frage. Verändert das visuelle Rendering nicht. Ergänzt nach Code-Quality-Review von Task 1.

## Implementation Sketch

### HTML — `public/index.html`, Z. 3543-3553

```html
<nav class="nav">
  <a href="/" class="nav-logo" aria-label="Tovaglia Home">
    <img src="/logo.svg" alt="Tovaglia">
  </a>
  <div class="nav-r">
    <a href="#how" class="nav-a">So geht's</a>
    <a href="#founder" class="nav-a">Vorher · Nachher</a>
    <a href="#preis" class="nav-a">Preise</a>
    <a href="https://app.tovaglia.io/login.html"
       class="nav-login"
       data-cta="member-login"
       aria-label="Als Mitglied einloggen">Bereits Mitglied?</a>
    <a href="#cta" class="nav-cta">Kostenlos starten →</a>
  </div>
</nav>
```

### CSS — `public/index.html` inline, neuer Block direkt nach `.nav-cta:hover` (Z. 473)

```css
.nav-login {
  font-size: 13px; font-weight: 400;
  color: var(--stone);
  text-decoration: none;
  transition: color 0.2s;
}
.nav-login:hover { color: var(--ink); }
```

### CSS — `public/index.html`, im `@media (max-width: 900px)` Block (nach Z. 2499)

```css
.nav-login {
  font-size: 14px;
  padding: 13px 8px;
}
```

(Touch-Target-Höhe: 14px font × line-height ~1.4 ≈ 19.6px + 2× 13px padding = 45.6px → erfüllt WCAG-Empfehlung ≥44px. Live-Smoke-Verifikation via DevTools-Inspector im Plan.)

## Validation

Vor Push:
1. **Desktop ≥900px:** Link erscheint zwischen „Preise" und „Kostenlos starten →" mit gleichem visuellen Gewicht wie die anderen In-Page-Anchors. Hover wechselt auf `--ink`. Klick öffnet `app.tovaglia.io/login.html` (selber Tab).
2. **Mobile ≤900px (375×812):** Logo links, „Bereits Mitglied?" rechts. Andere Nav-Items unsichtbar. Touch-Target ≥44px (gemessen via DevTools-Inspector). Sticky-Bottom-CTA bleibt unverändert.
3. **Theme-Mode `data-palette="notte"`:** Link nutzt die Notte-Token automatisch. Kein Kontrast-Problem.
4. **Cache:** Cloudflare-Pages purged automatisch beim Merge. Kein expliziter Cache-Bump nötig (kein Asset-Hashing-Asset geändert, nur HTML).

## Risks & Mitigations

| Risk | Mitigation |
|---|---|
| `.nav-login` kollidiert versehentlich mit existierender Klasse | Pre-Implementation-Grep durchgeführt: keine Treffer für `nav-login` in `index.html` (verifiziert) |
| Touch-Target zu klein auf Mobile | Padding 10px 4px + 14px font → ≈38px; Live-Smoke-Verifikation via DevTools-Inspector im Plan |
| Member-Pfad öffnet in selbem Tab → User verliert LP-Kontext | Akzeptiert: Members suchen den Editor, nicht die LP. Konsistent mit Pricing-Cards die genauso navigieren |
| Visual-Spannung neben Wein-Button | Bewusste Sekundär-Stilwahl (Plain-Text statt Ghost-Button). Falls in Live-Render zu unauffällig → in PR-Review revidieren |

## Done Criteria

- [ ] Link erscheint korrekt auf Desktop und Mobile
- [ ] Hover-State funktioniert (Color-Transition)
- [ ] Klick navigiert zu `app.tovaglia.io/login.html`
- [ ] Sticky-Bottom-CTA mobile bleibt funktional und unverändert
- [ ] `data-palette="notte"`-Theme rendert ohne Kontrast-Probleme
- [ ] Pages-Preview-Smoke vor Merge bestanden
- [ ] Push erfolgt unter TavolaM-Identity (gh auth-Status verifiziert)
