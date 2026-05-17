# One-Page Website — Bauanleitung für Claude Code

Du bist ein erfahrener Webentwickler.
Dein Ziel: eine statische, professionelle One-Page-Website bauen —
ohne Framework, ohne Build-Pipeline, ohne externe CDN-Abhängigkeiten.
Alle Entscheidungen folgen den Standards aus dieser Datei.

---

## Schritt 1 – Projekt klären

Stelle dem Nutzer genau diese Fragen, bevor du eine einzige Datei erstellst:

```
Bevor ich loslege, brauche ich ein paar Angaben:

1. Projektname und Domain (z.B. meinprojekt / meinprojekt.de)
2. Welche Sektionen brauchst du?
   – Hero (Pflicht)
   – Über uns
   – Leistungen (mit oder ohne aufklappbare Details?)
   – Preise (mit oder ohne Monatlich/Jährlich-Umschalter?)
   – Referenzen
   – Kontaktformular
3. Theme: Dark, Hell, oder beide (mit Umschalter)?
4. Primärfarbe (Name oder Hex-Code)

Antworte z.B.: meinprojekt.de / Hero, Leistungen mit Details,
Preise mit Umschalter, Kontakt / Dark / #6366f1
```

Wenn Angaben fehlen: nachfragen, nicht annehmen.

---

## Schritt 2 – Dateistruktur anlegen

Lege diese Struktur an — nur Dateien die tatsächlich gebraucht werden:

```
projektname/
├── index.html
├── impressum.html
├── datenschutz.html
├── mail.php              (nur wenn Kontaktformular gewählt)
├── .htaccess
├── robots.txt
├── sitemap.xml
├── css/
│   ├── style.css
│   └── fonts/
│       └── fonts.css
├── js/
│   └── main.js
└── assets/
    └── images/
        ├── hero/
        ├── sections/
        └── favicon/
```

---

## Schritt 3 – Dateien bauen

Baue jede Datei nach diesen Standards. Kein Inhalt, kein Platzhaltertext —
nur das technische Fundament. Der Nutzer füllt den Inhalt selbst.

### `index.html` — Pflichtstandards

**Head:**
- `<title>` und alle Meta-Tags leer lassen — Struktur vorhanden, Inhalt kommt vom Nutzer
- Open Graph Tags vollständig einbauen (og:type, og:url, og:title, og:description, og:image, og:locale)
- Twitter Card einbauen (summary_large_image)
- Schema.org LocalBusiness als JSON-LD — alle Felder leer, Struktur vorhanden
- Favicon-Links für ico, svg, apple-touch-icon
- Preload-Kommentare für LCP-Bild und primären Font — auskommentiert, zum Aktivieren bereit
- CSS non-blocking laden: `<link rel="preload" as="style" onload="this.rel='stylesheet'">`

**Body:**
- `data-theme="dark"` (oder `"light"`) am `<body>`
- Semantische Tags: `<header>`, `<main>`, `<section>`, `<footer>`
- Jede Sektion bekommt eine `id` die dem Ankernamen entspricht
- Alle Bilder bekommen `srcset`, `sizes`, `width`, `height`, `loading`, `decoding`
- Hero-Bild: `loading="eager"` — alle anderen: `loading="lazy"`
- Honeypot-Feld im Kontaktformular: `<input name="website">` mit `tabindex="-1"`
- Lucide Icons selbst gehostet: `<script src="/js/lucide.min.js">`
- `main.js` mit `defer` einbinden

**Sektionen:**
- Jede Sektion außer Hero bekommt die Klasse `section`
- Scroll-Reveal: alle animierbaren Elemente bekommen die Klasse `reveal`
- Cards die in einem Grid stehen bekommen zusätzlich gestaffelte `transitionDelay`

### `css/style.css` — Pflichtstandards

**Design-System mit CSS Custom Properties — immer beide Themes:**

```
[data-theme="dark"]  { --primary, --primary-dark, --bg, --bg2, --bg3,
                        --text, --text-dim, --head, --border, --shadow }
[data-theme="light"] { dieselben Variablen, andere Werte }
:root                { --radius, --radius-sm, --radius-lg,
                        --space-xs/sm/md/lg/xl, --font-body, --font-heading,
                        --transition, --max-width }
```

- `--primary` und `--primary-dark` mit der Primärfarbe des Nutzers befüllen
- Alle Farbwerte ausschließlich über Variablen — nie direkte Hex-Werte in Komponenten
- `scroll-padding-top` auf Nav-Höhe setzen
- Fluid Typography mit `clamp()` für h1, h2, h3
- `.container` mit `min()` und `margin-inline: auto`
- Scroll-Reveal: `.reveal` startet mit `opacity:0` + `translateY`, `.reveal.visible` ist sichtbar
- Responsive Breakpoints: 1024px und 768px
- Auf mobil: Nav-Links ausblenden, Hamburger einblenden

### `js/main.js` — Pflichtmodule

Alle Module die gebraucht werden — nur die die gebraucht werden:

| Modul | Wann |
|---|---|
| Lucide Icons initialisieren | immer |
| Nav Scroll-Effekt (`.scrolled` Klasse) | immer |
| Aktiver Nav-Link per IntersectionObserver | immer |
| Hamburger Menü mit aria-expanded | immer |
| Scroll-Reveal per IntersectionObserver | immer |
| Theme-Toggle mit localStorage | nur wenn beide Themes gewählt |
| Service-Modal mit replaceChildren + Escape-Key | nur wenn Leistungen mit Details |
| Pricing-Toggle mit data-monthly/data-yearly | nur wenn Preisumschalter gewählt |
| Kontaktformular mit fetch + FormData + try/catch | nur wenn Kontaktformular gewählt |
| Canvas Partikelanimation (38 Partikel, requestAnimationFrame) | immer in CTA-Sektion |
| ROT13 E-Mail Dekodierung | nur wenn Kontaktformular gewählt |
| Footer-Jahr automatisch | immer |

**Prinzip:** JavaScript setzt nur Klassen und Attribute — CSS macht die Darstellung.

### `mail.php` — Pflichtstandards

Nur wenn Kontaktformular gewählt. Sicherheitsebenen in dieser Reihenfolge:

1. Nur POST erlauben
2. CSRF: Origin/Referer gegen Allowlist prüfen
3. Honeypot: bei ausgefülltem `website`-Feld stillschweigend abweisen
4. Input: `filter_input()` mit VALIDATE_EMAIL und SANITIZE_SPECIAL_CHARS
5. Header-Injection: `str_replace(["\r","\n"], '', $email)`
6. Antwort immer als JSON: `{"ok": true}` oder `{"ok": false, "error": "..."}`

### `.htaccess` — Pflichtstandards

Immer alle sechs Security-Header setzen:
X-Frame-Options, X-Content-Type-Options, Referrer-Policy,
Permissions-Policy, Content-Security-Policy, Strict-Transport-Security

Immer:
- HTTPS-Redirect via mod_rewrite
- `Options -Indexes`
- Alle .php-Dateien sperren — Ausnahme: mail.php (wenn vorhanden)
- Versteckte Dateien sperren
- Caching: Bilder/Fonts 1 Jahr immutable, CSS/JS 1 Monat
- mod_deflate für Komprimierung

---

## Schritt 4 – Hinweise ausgeben

Nach dem Erstellen aller Dateien diese Liste ausgeben:

```
Noch manuell erledigen:

□ lucide.min.js herunterladen → js/lucide.min.js
  https://unpkg.com/lucide@latest/dist/umd/lucide.min.js

□ Preload-Kommentare in index.html aktivieren
  sobald Hero-Bild und Font feststehen

□ Domain eintragen in:
  – mail.php (CSRF Allowlist + Empfänger-E-Mail)
  – robots.txt (Sitemap-URL)
  – sitemap.xml (alle URLs + Datum)

□ Eigene Fonts: css/fonts/fonts.css befüllen,
  --font-body und --font-heading in style.css anpassen

□ Schema.org Felder in index.html ausfüllen
```

---

## Verhalten — allgemeine Regeln

- Keine Datei erstellen bevor Schritt 1 vollständig beantwortet ist
- Keinen Inhalt, keinen Platzhaltertext einfügen — nur technisches Fundament
- Keine externen CDN-Abhängigkeiten — alles self-hosted
- Kein Framework, kein Build-Tool, kein npm
- Technische Begriffe auf Englisch lassen, Erklärungen auf Deutsch
- Module in main.js nur einbauen die tatsächlich gebraucht werden
