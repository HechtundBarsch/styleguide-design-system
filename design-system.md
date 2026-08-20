# Hecht & Barsch – Design-System für Tools

> **Geltungsbereich:** Alle eigenständig gehosteten Tools von Hecht & Barsch — sowohl **interne Tools**
> (Login via Clerk) als auch **Public-Marketing-Apps**. Framework-egal (React, Next, Vite, plain HTML):
> es sind reine CSS-Tokens + Rezepte. **Nicht** für in Shopify eingebettete Theme-Blöcke.
>
> **So benutzt du das:** diesen Block an den Anfang eines neuen Tools geben oder als Referenz im Repo
> ablegen. Die Werte sind aus unserem Marken-Look („Rutenberater") extrahiert.

---

Du baust ein **Tool für Hecht & Barsch** (Raubfisch-Angel-Onlineshop). Alle Tools teilen sich dieses
Design-System. Halte dich strikt daran.

**Grundhaltung:** Der helle „Werkzeug"-Modus (A) ist der Default für Dashboards, Tabellen, Formulare.
Der **blaue Hero-Modus** (B) ist der Aufmerksamkeits-Modus — nur für Login/Landing/Ergebnis-Highlights,
sparsam einsetzen.

## 1. Design-Tokens (immer als `:root` einbinden)

```css
:root {
  /* Marke */
  --brand:      #3185da;  /* Primär-Blau – Buttons, Links, aktive Zustände */
  --brand-dk:   #003b7c;  /* Navy – Überschriften, Eyebrows, Akzentflächen */
  --accent:     #f8b53a;  /* Gold – NUR sparsam: Highlights, Eyebrow auf Blau */
  /* Flächen & Linien */
  --bg:         #f7f8fd;  /* heller Seiten-Hintergrund ("Paper") */
  --surface:    #ffffff;  /* Karten, Panels, Tabellen */
  --line:       #e1e7f0;  /* Rahmen/Hairlines */
  --soft:       #eef2f8;  /* dezente Füllung, Tabellen-Header, Hover-Rows */
  --brand-soft: #e8f1fb;  /* hellblaue Füllung (Info-Bänder, Chips) */
  /* Text */
  --ink:        #10243a;  /* Fließtext / Überschriften */
  --muted:      #56657a;  /* Sekundärtext */
  --dim:        #6b7889;  /* Hinweise/Metadaten – hellste erlaubte Textfarbe auf Weiß (WCAG-AA) */
  /* Status (getrennt vom Marken-Blau/Gold halten) */
  --ok:         #1f9d63;
  --warn:       #b7791f;
  --danger:     #e0533d;
  /* Schrift & Layout */
  --font-heading: "Bree Serif", Georgia, serif;
  --font-body:    "Barlow", -apple-system, "Segoe UI", system-ui, sans-serif;
  --radius:       12px;   /* Karten. Buttons 8–10px · Pills 999px · Hero 16–18px */
}
* { box-sizing: border-box; }
body { background: var(--bg); color: var(--ink); font-family: var(--font-body); line-height: 1.5; margin: 0; }
```

**Schriften laden** (Google Fonts):
```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Barlow:wght@400;500;600;700;800&family=Bree+Serif&display=swap" rel="stylesheet">
```

## 2. Zwei Flächen-Modi

**(A) Hell / „Paper" — Default.** Hintergrund `--bg`, weiße Karten/Panels, dunkler Text.
Für alles Datengetriebene: Dashboards, Tabellen, Formulare, Einstellungen.

**(B) Blauer Hero — Aufmerksamkeit.** Weiße Schrift, **goldene** Eyebrows/Akzente, weiße Karten
„schweben" mit Schatten. Nur für Login/Landing/Ergebnis-Highlights.
```css
.hero-blue {
  background:
    radial-gradient(120% 120% at 85% -10%, rgba(255,255,255,.14), transparent 55%),
    linear-gradient(165deg, #00284f 0%, #073f76 55%, #0f4d86 100%);
  color: #fff; border-radius: 16px;
}
.hero-blue .card { box-shadow: 0 4px 16px rgba(0,12,30,.30); }   /* Karte schwebt auf Blau */
```
Pro Screen **einen** Grundton wählen. Light-first; Dark-Mode nur wenn ausdrücklich gewünscht (dann Marken-Hues beibehalten).

## 3. Typografie

- **Eyebrow** (Kicker): `text-transform:uppercase; letter-spacing:.2em; font-size:11px; font-weight:800; color:var(--brand-dk)` — auf Blau `color:var(--accent)`.
- **Überschriften**: `font-family:var(--font-heading); font-weight:500; text-transform:uppercase`. H1 `clamp(24px,4vw,36px)`, H2 `clamp(18px,3vw,26px)`. (Bree Serif in Versalien = der Marken-Look.)
- **Fließtext**: `--ink`, 14–16px, line-height 1.5–1.6. Sekundär `--muted`, Metadaten `--dim`.
- **Zahlen** (Stats, Tabellen): fett, `font-variant-numeric:tabular-nums`, deutsches Format (`1.414`, `48,2 %`).

## 4. Komponenten

**Buttons** (`border-radius:8px; font-weight:800; text-transform:uppercase; letter-spacing:.03em; transition:.15s`):
```css
.btn-primary { background:var(--brand); color:#fff; padding:12px 26px; font-size:15px; border:none; }
.btn-primary:hover { filter:brightness(1.06); }
.btn-ghost   { background:var(--surface); color:var(--brand-dk); border:1px solid var(--line); padding:11px 18px; }
.btn-sm      { padding:9px 16px; font-size:13px; border-radius:9px; }
.btn-danger  { background:var(--danger); color:#fff; border:none; }   /* destruktive Aktionen */
```
Genau **ein** Primär-Button pro Sicht; alles andere Ghost. Destruktives in `--danger` + Bestätigung.

**Karte / Panel**: `background:var(--surface); border:1px solid var(--line); border-radius:var(--radius); box-shadow:0 2px 8px rgba(20,40,55,.05); padding:16–24px;`

**Hover** für klickbare Kacheln: `transform:translateY(-2px); box-shadow:0 10px 22px rgba(12,30,55,.12); border-color:var(--brand);` (`transition:.15s`).

**Pill / Chip / Status-Badge**: `border-radius:999px; font-size:12px; padding:4px 11px; font-weight:600`.
Neutral = `--brand-soft`/`--brand-dk`; Status = Tint aus `--ok`/`--warn`/`--danger` (helle Füllung + dunkle Schrift derselben Familie). Status **immer** auch in Form/Text, nie nur über Farbe.

**Stat-Kachel** (KPI): kleines Label (`--dim`, 13px) oben, große Zahl (24–42px, weight 800, `tabular-nums`) darunter.

**Tabellen** (Kern interner Tools): weiße Fläche, Kopfzeile `--soft` + `--dim`-Labels, Zeilen durch 1px `--line` getrennt (kein Zebra), Hover-Row `--soft`. Zahlen rechtsbündig + `tabular-nums`. Status als Badge, nicht als Zeilen-Volleinfärbung. Breite Tabellen in eigenem `overflow-x:auto`-Container.

**Inputs/Selects**: 1px `--line`, radius 8px, Höhe ~36–40px, Fokus-Ring `0 0 0 2px var(--brand)` (Default-Outline nie ersatzlos entfernen). Label immer sichtbar, Fehlertext in `--danger` mit konkretem Hinweis.

## 5. Logo & Favicon

Verwende die offiziellen Marken-Assets — **kein Emoji als Logo**.

- **Logo:** `https://cdn.shopify.com/s/files/1/0548/9197/0669/files/doo-logo.png`
  - Im Header ca. 34px Höhe, im blauen Hero ca. 44px Höhe. Auf hellem Grund direkt einsetzbar; auf Blau ggf. mit etwas Weißraum.
  - Optional Wortmarke daneben in Bree Serif (weight 500, `letter-spacing:-.01em`), nur wenn das Logo selbst keine Wortmarke enthält.
- **Favicon:** `https://www.hechtundbarsch.de/cdn/shop/files/hechtundbarsch-de-favicon.png`
- **Icons:** schlichte Outline-SVGs. Keine dekorativen Emoji-Reihen.

## 6. Motion

Dezent: `translateY(-2px)`-Hover, `.15s`-Transitions, ein sanftes Mount-Fade
(`opacity 0→1 + translateY(8px)→0`, `.32s ease`). `prefers-reduced-motion` respektieren.

## 7. UI-Texte

Kurz, klar, funktional — Feld/Aktion beim Namen nennen, was der Nutzer erkennt. Buttons Versalien
(Design), sonst normale Schreibweise. Keine Marketing-Floskeln bei internen Tools. Aktive Sprache:
„Änderungen speichern", nicht „Absenden". Eine Aktion behält ihren Namen durch den ganzen Flow.
Leere Zustände und Fehler geben Richtung: was ist passiert, was tun — in der Stimme der Oberfläche.

## 8. Guardrails

- **Keine Secrets** (API-Keys/Tokens) im Code, in Prompts oder in Artifacts — immer über Env/Secret-Store.
- **Keine erfundenen Personen** (Team, Testimonials) — nur echte Angaben, sonst Platzhalter.
- **Barrierefreiheit**: Kontraste einhalten (`--dim` ist die hellste erlaubte Textfarbe auf Weiß), sichtbare Fokuszustände, semantisches HTML, Tastaturbedienbarkeit.
- *Falls das Tool Shopify-Daten anbindet:* nur lesen/anreichern — **Produktdaten nie löschen** (Shopify ist Single-Source-of-Truth).

---

### Kurzfassung (schnell einfügen)
> Tools von Hecht & Barsch (intern + public). Default = heller Grund `#f7f8fd` + weiße Karten/Panels/Tabellen (Radius 12, 1px `#e1e7f0`, weicher Schatten); Aufmerksamkeits-Screens auf blauem Verlauf `#00284f→#0f4d86`, weiße Schrift + goldene (`#f8b53a`) Eyebrows. Primär-Blau `#3185da`, Navy `#003b7c`. Headings **Bree Serif, UPPERCASE, weight 500**; Body **Barlow**. Buttons uppercase/weight 800/Radius 8, ein Primär je Sicht. Hover `translateY(-2px)`+Schatten. Status-Farben `--ok/--warn/--danger` getrennt vom Marken-Blau. Tabellen: Hairline-Zeilen, Zahlen rechts + tabular-nums, Status als Badge. Echtes Logo + Favicon (offizielle Shop-Assets), kein Emoji. Keine Secrets, keine erfundenen Personen, a11y-Kontraste; Shopify-Produktdaten nie löschen.
