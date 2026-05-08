# LP Login-Button Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Einen sichtbaren „Bereits Mitglied?"-Link in der Top-Nav der Landingpage hinzufügen, der direkt auf `https://app.tovaglia.io/login.html` führt — auf Desktop UND Mobile sichtbar.

**Architecture:** Eine HTML-Edit (neuer `<a>` zwischen „Preise" und Primary-CTA), ein neuer Desktop-CSS-Block (Plain-Text-Stil identisch zu `.nav-a`), ein zusätzlicher Mobile-Override im bestehenden `@media (max-width: 900px)` damit der Link mobile sichtbar bleibt während alle anderen Nav-Items hidden sind. Eigene Klasse `.nav-login` damit das bestehende Mobile-Hide auf `.nav-a`/`.nav-cta` nicht greift.

**Tech Stack:** Statisches HTML mit inline `<style>`-Block — keine Build-Tools, kein JS für dieses Feature. Verifikation über `preview_*` Tools (Snapshot, Screenshot, Inspect).

**Spec:** [docs/superpowers/specs/2026-05-08-lp-login-button-design.md](../specs/2026-05-08-lp-login-button-design.md)

**Branch:** `feat/lp-login-button` (bereits angelegt, Spec committed als `9971417`)

---

## File Structure

| File | Change | Responsibility |
|---|---|---|
| `public/index.html` | Modify (single file, three edits) | LP-Markup + inline CSS — alle Änderungen passieren hier |

Keine neuen Files. Keine externen Assets. Keine JS-Edits.

---

## Pre-flight: Verify Live State (3 min)

**Files:** —

- [ ] **Step 1: Confirm preview server is up**

Run: `mcp__Claude_Preview__preview_list`
Expected: Server `tovaglia-lp-repo` listed with serverId from session start (Port 8766).
If not running: `mcp__Claude_Preview__preview_start` mit `name: tovaglia-lp-repo`.

- [ ] **Step 2: Re-grep `nav-login` to confirm zero collisions**

Use Grep:
- pattern: `nav-login`
- path: `C:/Users/Stef/Desktop/Tavola v1/Landingpage_repo/public/index.html`
- output_mode: `count`

Expected: `0 total occurrences`. If non-zero, STOP and re-evaluate (klassen-Konflikt zu lösen vor Implementation).

- [ ] **Step 3: Re-read current line numbers (in case file changed)**

Read `public/index.html`:
- Z. 3543-3553 (Nav-Block) — confirm structure matches Spec
- Z. 469-473 (Ende `.nav-cta`-Block) — Desktop-CSS-Anker
- Z. 2495-2515 (`@media (max-width: 900px)`) — Mobile-CSS-Anker

Expected: Strukturen unverändert wie im Spec dokumentiert. Falls nicht: Plan-Schritte mit aktuellen Zeilen anpassen.

- [ ] **Step 4: Baseline-Screenshot Desktop**

Run: `mcp__Claude_Preview__preview_resize` mit `preset: desktop`, dann `preview_screenshot`.
Expected: Aktuelle Nav sichtbar (Logo + 3 Anchors + Wein-CTA). Bild als Vergleichsreferenz speichern.

---

## Task 1: HTML — Login-Link in Top-Nav einfügen

**Files:**
- Modify: `public/index.html` — Z. 3551 (zwischen „Preise"-Link und „Kostenlos starten →"-CTA)

- [ ] **Step 1: Edit HTML — neuen `<a>` einfügen**

Use Edit:
- file_path: `C:/Users/Stef/Desktop/Tavola v1/Landingpage_repo/public/index.html`
- old_string:
```
    <a href="#preis" class="nav-a">Preise</a>
    <a href="#cta" class="nav-cta">Kostenlos starten →</a>
```
- new_string:
```
    <a href="#preis" class="nav-a">Preise</a>
    <a href="https://app.tovaglia.io/login.html" class="nav-login" data-cta="member-login" aria-label="Als Mitglied einloggen">Bereits Mitglied?</a>
    <a href="#cta" class="nav-cta">Kostenlos starten →</a>
```

- [ ] **Step 2: Reload preview**

Run: `mcp__Claude_Preview__preview_eval` mit `code: window.location.reload()`.
Wait ~500ms.

- [ ] **Step 3: Verify DOM contains the link**

Run: `mcp__Claude_Preview__preview_snapshot`.
Expected output contains:
- StaticText `"Bereits Mitglied?"`
- Link with that text appears between „Preise" und „Kostenlos starten →"

If missing: Edit hat nicht gegriffen — wiederholen.

- [ ] **Step 4: Verify href + data-cta via DOM-Eval**

Run: `mcp__Claude_Preview__preview_eval` mit code:
```javascript
const a = document.querySelector('.nav-login');
JSON.stringify({
  text: a?.textContent,
  href: a?.href,
  dataCta: a?.dataset.cta,
  parent: a?.parentElement?.className
});
```

Expected (JSON):
```json
{"text":"Bereits Mitglied?","href":"https://app.tovaglia.io/login.html","dataCta":"member-login","parent":"nav-r"}
```

If `href` missing protocol or `dataCta` ≠ `member-login`: zurück zu Step 1.

- [ ] **Step 5: Commit**

```bash
git add public/index.html
git commit -m "feat(nav): add member login link in top-nav"
```

---

## Task 2: Desktop-CSS — `.nav-login` Plain-Text-Stil

**Files:**
- Modify: `public/index.html` — direkt nach Z. 473 (`.nav-cta:hover { ... }`)

- [ ] **Step 1: Edit CSS — Desktop-Stil hinzufügen**

Use Edit:
- file_path: `C:/Users/Stef/Desktop/Tavola v1/Landingpage_repo/public/index.html`
- old_string:
```
.nav-cta:hover { background: var(--wine-dark); transform: translateY(-1px); }

/* ═══ HERO ═══ */
```
- new_string:
```
.nav-cta:hover { background: var(--wine-dark); transform: translateY(-1px); }
.nav-login {
  font-size: 13px; font-weight: 400;
  color: var(--stone);
  text-decoration: none;
  transition: color 0.2s;
}
.nav-login:hover { color: var(--ink); }

/* ═══ HERO ═══ */
```

- [ ] **Step 2: Reload preview at desktop viewport**

Run: `mcp__Claude_Preview__preview_resize` mit `preset: desktop`.
Run: `mcp__Claude_Preview__preview_eval` mit `code: window.location.reload()`.

- [ ] **Step 3: Verify computed styles match `.nav-a`**

Run: `mcp__Claude_Preview__preview_inspect` für selector `.nav-login`, properties:
- `font-size` → expected `13px`
- `font-weight` → expected `400`
- `color` → expected `rgb(...)` (matches `--stone` token)
- `text-decoration-line` → expected `none`

Vergleichen: für `.nav-a` (selector `.nav-r > .nav-a:first-of-type`) dieselben Properties. Werte müssen identisch sein.

- [ ] **Step 4: Verify hover transition**

Run: `mcp__Claude_Preview__preview_eval` mit code:
```javascript
const a = document.querySelector('.nav-login');
const style = getComputedStyle(a);
JSON.stringify({
  transition: style.transition,
  hasNoTextDecoration: style.textDecoration.includes('none')
});
```

Expected: `transition` enthält `color`, `hasNoTextDecoration: true`.

- [ ] **Step 5: Visual-Check — Screenshot Desktop**

Run: `mcp__Claude_Preview__preview_screenshot`.
Visual: Login-Link erscheint zwischen „Preise" und Wein-Button, gleiches visuelles Gewicht wie die anderen `.nav-a`-Anchors. Wein-Button bleibt rechts der dominante Visual-Anchor.

Falls Login-Link visuell „aus der Reihe fällt" (zu groß, zu fett, falsche Farbe): zurück zu Step 1.

- [ ] **Step 6: Commit**

```bash
git add public/index.html
git commit -m "feat(nav): style nav-login as plain text-link on desktop"
```

---

## Task 3: Mobile-CSS — Touch-Target + Sichtbarkeit

**Files:**
- Modify: `public/index.html` — innerhalb des `@media (max-width: 900px)` Blocks, direkt nach Z. 2499 (`.nav-cta { display: none; }`)

- [ ] **Step 1: Edit CSS — Mobile-Override hinzufügen**

Use Edit:
- file_path: `C:/Users/Stef/Desktop/Tavola v1/Landingpage_repo/public/index.html`
- old_string:
```
  .nav { padding: 12px 20px; }
  .nav-a { display: none; }
  .nav-cta { display: none; }
```
- new_string:
```
  .nav { padding: 12px 20px; }
  .nav-a { display: none; }
  .nav-cta { display: none; }
  .nav-login {
    font-size: 14px;
    padding: 13px 8px;
  }
```

- [ ] **Step 2: Resize preview to mobile**

Run: `mcp__Claude_Preview__preview_resize` mit `preset: mobile` (375×812).
Run: `mcp__Claude_Preview__preview_eval` mit `code: window.location.reload()`.

- [ ] **Step 3: Verify other nav items are still hidden**

Run: `mcp__Claude_Preview__preview_eval` mit code:
```javascript
const items = document.querySelectorAll('.nav-r > a');
Array.from(items).map(a => ({
  cls: a.className,
  visible: getComputedStyle(a).display !== 'none'
}));
```

Expected:
- `.nav-a` items → `visible: false` (alle 3)
- `.nav-login` → `visible: true`
- `.nav-cta` → `visible: false`

- [ ] **Step 4: Verify Touch-Target ≥44px**

Run: `mcp__Claude_Preview__preview_eval` mit code:
```javascript
const a = document.querySelector('.nav-login');
const rect = a.getBoundingClientRect();
JSON.stringify({ width: Math.round(rect.width), height: Math.round(rect.height), wcagOk: rect.height >= 44 });
```

Expected: `height >= 44`, `wcagOk: true`. Wenn `height < 44`: padding `13px 8px` → höher setzen (z.B. `14px 8px`) und re-test.

- [ ] **Step 5: Verify Sticky-Bottom-CTA bleibt unverändert**

Run: `mcp__Claude_Preview__preview_eval` mit code:
```javascript
const sticky = document.querySelector('.sticky-cta');
const stickyA = sticky?.querySelector('.cta');
JSON.stringify({
  stickyVisible: getComputedStyle(sticky).display === 'block',
  stickyText: stickyA?.textContent.trim(),
  stickyHref: stickyA?.getAttribute('href')
});
```

Expected: `stickyVisible: true`, `stickyText: "Kostenlos starten →"`, `stickyHref: "#cta"`.

- [ ] **Step 6: Mobile-Screenshot**

Run: `mcp__Claude_Preview__preview_screenshot`.
Visual: Logo links, „Bereits Mitglied?" rechts in der Top-Nav. Andere Items nicht sichtbar. Sticky-Bottom-CTA „Kostenlos starten →" am unteren Rand.

- [ ] **Step 7: Commit**

```bash
git add public/index.html
git commit -m "feat(nav): keep nav-login visible on mobile with WCAG touch target"
```

---

## Task 4: Theme-Mode `notte` Verifikation

**Files:** — (kein Edit, nur Validation)

- [ ] **Step 1: Switch to desktop viewport for clean comparison**

Run: `mcp__Claude_Preview__preview_resize` mit `preset: desktop`.

- [ ] **Step 2: Apply notte-Palette via DOM**

Run: `mcp__Claude_Preview__preview_eval` mit code:
```javascript
document.body.setAttribute('data-palette', 'notte');
'applied';
```

- [ ] **Step 3: Verify nav-login color uses notte-token**

Run: `mcp__Claude_Preview__preview_inspect` für selector `.nav-login`, property `color`.
Expected: Farbe ist hell (vermutlich heller stone-Wert für dunklen Background) und kontrastiert sichtbar gegen den dunklen Nav-Background. Konkreten Hex-Wert mit `.nav-a` (gleicher Selector-Style) abgleichen — sollte identisch sein.

- [ ] **Step 4: Visual-Check Notte-Mode**

Run: `mcp__Claude_Preview__preview_screenshot`.
Visual: Login-Link gut lesbar gegen dunklen Hintergrund, gleiche Farbe wie die anderen Nav-Anchors.

Falls Kontrast schlecht oder Farbe inkonsistent: in Spec dokumentieren und mit User klären (vermutlich Token-Bug der Notte-Palette, kein Login-Button-spezifisches Problem).

- [ ] **Step 5: Reset palette**

Run: `mcp__Claude_Preview__preview_eval` mit code:
```javascript
document.body.removeAttribute('data-palette');
'reset';
```

- [ ] **Step 6: No commit** — reine Verifikation, keine Code-Änderung.

---

## Task 5: Done-Criteria Final Sweep

**Files:** — (Validation only)

- [ ] **Step 1: Click-Test auf Desktop**

Run: `mcp__Claude_Preview__preview_resize` mit `preset: desktop`.
Run: `mcp__Claude_Preview__preview_eval` mit code:
```javascript
const a = document.querySelector('.nav-login');
JSON.stringify({
  href: a.href,
  target: a.target || '_self',
  rel: a.rel || ''
});
```

Expected: `href: "https://app.tovaglia.io/login.html"`, `target: "_self"`, `rel: ""`.
Hinweis: NICHT auf den Link klicken (würde wegnavigieren und Preview-State zerstören) — DOM-Inspect reicht.

- [ ] **Step 2: Hover-Color-Transition manuell verifizieren**

Run: `mcp__Claude_Preview__preview_eval` mit code:
```javascript
const a = document.querySelector('.nav-login');
const before = getComputedStyle(a).color;
a.dispatchEvent(new MouseEvent('mouseover', { bubbles: true }));
const during = getComputedStyle(a).color;
JSON.stringify({ before, during, transitionApplied: getComputedStyle(a).transitionProperty });
```

Hinweis: `:hover`-Pseudoklassen können nicht via JS getriggert werden in vielen Browsern — `before === during` ist normal. Der `transitionProperty`-Check (sollte `color` enthalten) ist hier der primäre Check.

- [ ] **Step 3: Spec-Done-Criteria-Liste durchgehen**

Verifiziere gegen Spec:
- [x] Link erscheint korrekt auf Desktop (Task 1+2)
- [x] Link erscheint korrekt auf Mobile (Task 3)
- [x] Hover-State funktioniert (Task 2 Step 4)
- [x] Klick-Ziel `app.tovaglia.io/login.html` (Task 5 Step 1)
- [x] Sticky-Bottom-CTA Mobile unverändert (Task 3 Step 5)
- [x] `data-palette="notte"` rendert ok (Task 4)

Falls einer fehlt: zurück zur entsprechenden Task.

- [ ] **Step 4: No commit** — reine Verifikation.

---

## Task 6: Code-Review via Subagent

**Files:** —

- [ ] **Step 1: Show pending changes summary**

Run:
```bash
git log --oneline main..HEAD
```
Expected output (nach Tasks 1-3):
```
<sha> feat(nav): keep nav-login visible on mobile with WCAG touch target
<sha> feat(nav): style nav-login as plain text-link on desktop
<sha> feat(nav): add member login link in top-nav
9971417 docs: design spec for LP member login button
```

- [ ] **Step 2: Dispatch code-reviewer subagent**

Use Agent tool with `subagent_type: superpowers:code-reviewer`. Prompt:
> Review the implementation in branch `feat/lp-login-button` against the spec at `docs/superpowers/specs/2026-05-08-lp-login-button-design.md`. The branch contains 4 commits (1 spec + 3 implementation). The implementation only touches `public/index.html`. Validate: (1) Spec D1-D7 decisions are reflected, (2) HTML markup matches the Implementation Sketch, (3) CSS is additive (no removed selectors), (4) Mobile touch-target meets WCAG ≥44px, (5) No `nav-login` collision with existing classes, (6) Done-Criteria from spec are testable. Report blockers vs nits separately.

- [ ] **Step 3: Address any blockers from review**

If review returns blockers: fix them, re-test, commit. Re-dispatch reviewer if non-trivial fix.
If review returns only nits: capture in PR-description, optionally fix inline.

- [ ] **Step 4: No commit** unless blockers found.

---

## Task 7: Push + PR

**Files:** —

- [ ] **Step 1: Identity-Audit**

```bash
gh auth status
```
Expected output enthält: `Active account: true` für `TavolaM`.
Falls falscher User aktiv:
```bash
gh auth switch -h github.com -u TavolaM
```
Re-run `gh auth status`. NICHT pushen wenn falsche Identity.

- [ ] **Step 2: Confirm working tree clean**

```bash
git status
```
Expected: `nothing to commit, working tree clean`. Falls nicht: User fragen welche uncommitted changes sind.

- [ ] **Step 3: Final smoke-test in der Pages-Preview-URL**

Hinweis: Cloudflare Pages erstellt PR-Preview-URLs erst nach Push. Daher: lokaler Final-Smoke ist abgedeckt durch Tasks 1-5; Pages-Preview-Smoke folgt **nach** Step 4 mit dem PR-Link.

- [ ] **Step 4: Push branch**

```bash
git push -u origin feat/lp-login-button
```

- [ ] **Step 5: Create PR**

```bash
gh pr create --title "feat(nav): add member login link 'Bereits Mitglied?' to LP top-nav" --body "$(cat <<'EOF'
## Summary
- Adds a single-state member login link `Bereits Mitglied?` in the top-nav, positioned between „Preise" and the primary „Kostenlos starten →" CTA.
- Links directly to `https://app.tovaglia.io/login.html` (kein Plan-Param, kein Auth-State-Awareness).
- Stays visible on mobile (≤900px) as the only nav item next to the logo, while existing `.nav-a` and `.nav-cta` mobile-hide rules remain untouched.
- Sticky-Bottom-CTA und alle anderen Nav-Items unverändert.

## Spec
- Design: [docs/superpowers/specs/2026-05-08-lp-login-button-design.md](docs/superpowers/specs/2026-05-08-lp-login-button-design.md)
- Plan: [docs/superpowers/plans/2026-05-08-lp-login-button.md](docs/superpowers/plans/2026-05-08-lp-login-button.md)

## Test plan
- [ ] Pages-Preview-URL Desktop: Login-Link sichtbar zwischen „Preise" und Wein-Button, Hover wechselt Farbe, Klick führt zu `app.tovaglia.io/login.html`
- [ ] Pages-Preview-URL Mobile (375×812): Logo links, „Bereits Mitglied?" rechts, andere Nav-Items unsichtbar, Sticky-Bottom-CTA „Kostenlos starten →" funktional
- [ ] Touch-Target des Mobile-Links ≥44px (DevTools-Inspector)
- [ ] `data-palette="notte"` rendert Link mit ausreichendem Kontrast

🤖 Generated with [Claude Code](https://claude.com/claude-code)
EOF
)"
```

Expected: PR-URL wird zurückgegeben.

- [ ] **Step 6: Pages-Preview-Smoke**

PR-Description enthält Cloudflare Pages-Preview-URL (Bot-Comment). User öffnet URL → Smoke gegen Test plan oben. **Erst nach User-Sign-Off mergen.**

- [ ] **Step 7: No commit** in this task — Final-Step ist User-Approval und Merge.

---

## Cache-Bump-Hinweis

Per Spec: **kein expliziter Cache-Bump nötig**. Cloudflare Pages purged HTML automatisch bei Deploy. Keine versionierten Asset-URLs (CSS ist inline im HTML, keine `style.css?v=…` Referenzen). Wenn dieser Plan später CSS in eine separate Datei extrahiert, müsste der Hinweis angepasst werden — aktuell nicht relevant.

---

## Risk-Recap (aus Spec übernommen)

| Risk | Mitigation in diesem Plan |
|---|---|
| `.nav-login` Klassenkollision | Pre-flight Step 2 (re-grep) |
| Touch-Target zu klein | Task 3 Step 4 (≥44px-Assert) |
| Login-Button visuell zu auffällig | Task 2 Step 5 (visual review) + Task 6 (code-reviewer) |
| Theme-Mode Notte-Bug | Task 4 (Verifikation, fail → User-Diskussion) |
| Falsche Git-Identity | Task 7 Step 1 (Identity-Audit) |
| Direkt-zu-main-Push | Branch bereits `feat/lp-login-button` (Pre-flight 0) |
